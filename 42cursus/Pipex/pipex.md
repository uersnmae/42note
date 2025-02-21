
`>./pipex file1 cmd1 cmd2 file2` 와 같은 형식으로 실행

- `file1`과 `file2`는 파일의 이름들
- `cmd1`과 `cmd2`는 환경변수를 포함한 쉘 명령어

다음과 같은 쉘 명령어와 정확히 동일하게 동작해야함

```
$> < file1 cmd1 | cmd2 > file2
```

[[pipe]] (`|`)에 대해 다루는 과제이다!

`< file1 cmd1`: `file1`을 열어 표준입력으로 하고 `cmd1`을 실행한다.
`|`: 좌변의 출력 결과를 입력으로 받아 우변으로 넘긴다.
`cmd2 > file2`: 파이프를 통해 좌변의 출력 결과를 입력으로 받아 `cmd2`를 실행하고 그 결과를 `file2`에 저장한다.

그럼?? 먼저 `file1`을 열고 `cmd1`을 실행하는 것까지 해보자


# 처음 보는 함수 정리
## `perror()`

- 용도/동작
    - `<errno.h>` 에 선언된 전역변수를 참조하여, 에러 메시지를 표준 에러에 출력
    - 주로 시스템 콜이나 라이브러리 함수가 실패했을 때 그 원인을 출력하기 위해 사용함
- 예제
```c
#include <stdio.h>
#include <errno.h>

if (open("not_exist.txt", ORDONLY) == -1)
    perror("open failed");

```

## `strerror()`

- 용도/동작
    - `errno`값에 대응하는 에러 문자열을 반환
    - 예: `strerror(ENOENT)` -> `"No such file or directory"`
- 예제
```c
#include <stdio.h>
#include <string.h>
#include <errno.h>

printf("Error: %s\n", strerror(errno));

```

## `access()`

- 용도/동작
    - 특정 경로에 대해 읽기, 쓰기, 실행 권한이 있는지 여부를 검사
    - 성공 시 0, 실패 시 -1을 반환
    - 예: `access("file.txt", R_OK | W_OK)` -> 파일에 대해 읽기·쓰기가 가능한지 확인
- 예제
```c
#include <unistd.h>
#include <stdio.h>

if (access("test.txt", R_OK | W_OK) == 0)
    printf("Read/Write available\n");
else
    perror("access");

```

## `dup()`

- 용도/동작
    - 전달받은 파일 디스크립터를 복제하여 파일 디스크립터를 생성 (가장 낮은 사용 가능한 파일 디스크립터를 반환)
    - 두 파일 디스크립터는 같은 파일(또는 파이프, 소켓)을 가리키고, 동일한 오프셋, 플래그 등을 공유함
- 예제
```c
int new_fd = dup(fd);
if (new_fd == -1) {
    perror("dup");
}
```

## `dup2()`

- 용도/동작
    - 첫 번째 인자로 받은 파일 디스크립터를 두 번째 인자로 받은 파일 디스크립터 번호에 복제한다.
    - 두 번째 인자가 이미 열려 있으면 먼저 닫은뒤 복제
- 예제
```c
if (dup2(fd, STDOUT_FILENO) == -1) {
    perror("dup2")
}
// 이제 printf 같은 표준 출력은 fd에 쓰이게 됨
```

## `execve()`

- 용도/동작
    - 새 프로세스 이미지를 현재 프로세스에 로드하여 실행한다. (기존 코드, 데이터 등은 사라지고 새로운 프로그램으로 대체)
    - `execve()`가 성공하면 호출한 뒤의 코드는 실행되지 않고, 해당 프로그램으로 완전히 덮어씌워진다.
    - 인자로 프로그램 경로, 인자 목록(argv), 환경 변수(envp) 등을 받는다.
    - 성공 시 리턴값이 없으며, 실패시 -1을 반환한다.
- 예제
``` c
#include <unistd.h>
#include <stdio.h>

int main() {
    char *argv[] = {"/bin/ls", "-l", NULL};
    char *envp[] = {NULL};
    if (execve("/bin/ls", argv, envp) == -1) {
        perror("execve");
    }
    // execve 성공 시 이 아래 코드는 실행되지 않음
    return (0);
}
```

## `fork()`

- 용도/동작
    - 현재 프로세스를 복제하여 자식 프로세스를 생성한다.
    - 부모 프로세서에서는 `fork()`의 반환값이 "자식 PID"이고, 자식 프로세서에서는 "0"을 반환한다. 실패 시 -1.
    - 이후 부모·자식 프로세스 모두에서 `fork()` 다음 코드를 각각 실행한다.
- 예제
```c
#include <sys/types.h>
#include <unistd.h>
#include <stdio.h>

pid_t pid = fork();
if (pid == -1)
    perror("fork");
else if (pid == 0)
    // 자식 프로세스
    printf("Child process\n");
else
    // 부모 프로세스
    printf("Parent process, child PID = %d\n", pid);
```

## `pipe()`

- 용도/동작
    - 파이프를 생성하여 두 개의 파일 디스크립터(`pipefd[0]`는 읽기 전용, `pipefd[1]`은 쓰기 전용)를 만든다.
    - 일반적으로 프로세스 간 통신(IPC)에 사용한다.
    - 성공 시 0,  실패 시 -1을 반환한다.
- 예제
```c
#include <unistd.h>
#include <stdio.h>

int pipfd[2];
if (pipe(pipefd) == -1) {
    perror("pipe")
    return 1;
}

// pipefd[0]: read end, pipefd[1]: write end
write(pipefd[1], "Hello", 5);
char buf[10];
read(pipefd[0], buf, 5);
buf[5] = '\0';
printf("Read from pipe: %s\n", buf);
```

## `unlink()`

- 용도/동작
    - 파일 시스템에서 파일 이름을 제거한다(하드 링크를 제거). 파일을 열고 있는 프로세스가 있을 경우, 해당 프로세스가 닫을 때까지 실제 데이터는 남아 있을 수 있다.
    - 성송 시 0, 실패 시 -1을 반환한다.
- 예제
```c
#include <unistd.h>

if (unlink("test.txt") == -1)
    perror("unlink");
```

## `wait()`

- 용도/동작
    - 자식 프로세스가 종료될 때까지 대기하고, 종료 상태 등을 받아온다.
    - 성공 시 종료된 자식의 PID를, 실패 시 -1을 반환한다.
    - `int status`를 통해 자식 프로세스의 종료 코드를 분석할 수 있다.
- 예제
```c
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>

pid_t pid = fork();
if (pid == 0) {
    // 자식 프로세스
    _exit(3); // 임의 종료 코드
} else {
    int status;
    wait(&status);
    if (WIFEXITED(status)) {
        printf("Child exited with cod %d\n", WEXITSTATUS(status));
    }
}
```

## `waitpid()`

- 용도/동작
    - `wait()`와 유사하지만, 특정 자식 PID를 기다릴 수 있고, 논블로킹(non-blocking) 옵션을 설정(`WNOHANG`)할 수도 있다.
    - `wait()`와 달리 여러 옵션을 줘서 보다 정교하게 자식 프로세스를 제어할 수 있다.
    - 성공 시 종료된 자식 PID, 실패 시 -1을 반환한다.
- 예제
```c
#include <sys/types.h>
#include <sys/wait.h>
#include <unistd.h>
#include <stdio.h>

int main(void) {
    pid_t pid = fork();
    if (pid == 0) {
        // 자식
        _exit(5);
    } else {
        int status;
        pid_t w = waitpid(pid, &status, 0);
        if (w == -1) {
            perror("waitpid");}
        } else {
            if (WIFEXITED(status)) {
                printf("Child exited with cod %d\n", WEXITSTATUS(status));
            }
        }
    }
    return 0;
}
```