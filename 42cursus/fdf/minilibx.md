Minilibx는 X-Window 프로그래밍 지식이 없이도 쉽게 그래픽 소프트웨어를 만들 수 있게 도와준다. 간단한 윈도우 생성과 그리기 도구, 이미지와 기초적인 이벤트 관리 도구를 제공한다.

# Synopsys
```c
#include <mlx.h>

void *mlx_init();
```
# Include File
MiniLibx API을 올바르게 사용하기 위해선 **mlx.h** 헤더파일이 include 되어야 한다. 헤더파일은 함수의 prototype만 갖고 있으며 구조는 필요하지 않다.

# Library Functions
먼저 소프트웨어와 디스플레이 사이의 connection을 초기화 해야한다. 연결이 되면 X-server에 요청을 보낼 수 있는 MiniLibx의 함수들을 사용할 수 있다. 예를 들어 "이 윈도우에 노란색 픽셀을 찍어줘" 혹은 "사용자가 키를 눌렀니"와 같은 요청을 보낼 수 있다.

mlx_init 함수가 소프트웨어와 디스플레이 사이의 connection을 초기화한다. 매개변수는 필요하지 않고, 앞으로 라이브러리에 있는 함수들을 호출할 때 쓰일 `void *` 자료형을 반환한다.

다음과 같은 함수들이 있다.

- [[mlx_new_window]]: 윈도우 관리
- [[mlx_pixel_put]]: 윈도우 안에 그리기
- [[mlx_new_image]]: 이미지 관리
- [[mlx_loop]]: 키보드나 마우스 이벤트 처리

# Linking MiniLibx
MiniLibx 함수들을 사용하기 위해선 소프트웨어와 MiniLibx 라이브러리를 포함해 몇 가지 라이브러리들을 link 해야 한다. link 할 때 다음과 같은 인수들을 추가하면 된다.
``` bash
-lmlx -lXext -lX11
```

# Return values
만약 mlx_init()이 X server와의 연결을 실패하면 `NULL`을 반환한다. 그렇지 않으면 연결을 나타내는 non-null 포인터를 나타낸다.