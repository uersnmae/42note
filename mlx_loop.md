# Synopsys

```c
int mlx_loop(void *mlx_ptr);

int mlx_key_hook(void *win_ptr, int (*funct_ptr)(), void *param);

int mlx_mouse_hook(void *win_ptr, int (*funct_ptr)(), void *param);

int mlx_expose_hook(void *win_ptr, int (*funct_ptr)(), void *param);

int mlx_loop_hook(void *mlx_ptr, int (*funct_ptr)(), void *param);
```

# X-Window 시스템은 양방향(bi-directional)이다.

한편으로는 프로그램이 화면에 픽셀, 이미지 등을 표시하도록 명령을 보내고, 다른 한편으로는 키보드나 마우스 입력을 받을 수 있다. 따라서 이를 위해 프로그램은 X-Window로부터 "이벤트"를 받게 된다.

# Description

이벤트를 받으려면 반드시 `mlx_loop()`함수를 사용해야 한다. 이 함수는 아무것도 반환하지 않는다. 이벤트가 발생하기를 기다리는 무한 루프이며, 이벤트가 발생하면 해당 이벤트와 연관된 사용자 정의 함수를 호출한다. 이 때 필요한 매개변수는 단 하나이며, 이는 연결 식별자(connection identifier)이다.

다음 세 가지 이벤트에 대해 서로 다른 함수를 지정할 수 있다.

- 키가 눌렸을 때: `mlx_key_hook`
- 마우스 버튼이 눌렸을 때: `mlx_mouse_hook`
- 윈도우가 다시 그려져야 할 때(`expose` 이벤트): `mlx_expose_hook`

각 윈도우별로 이벤트를 처리할 서로 다른 함수를 정의할 수 있다.
`mlx_key_hook()` (또는 `mlx_mouse_hook()`, `mlx_expose_hook()`)에 전달되는 함수는 모두 동일한 방식으로 동작한다.

1. 먼저 이벤트 발생 시 호출될 함수의 포인터를 전달한다.
2. 그 다음, 해당 함수에서 사용할 수 있는 주소(포인터)를 전달한다. 예를 들어, 라이브러리 포인터나 윈도우 포인터, 혹은 프로그램의 구조체가 필요하다면 여기서 넘겨주면 된다. 이 함수는 호출되면서, 전달받은 매개변수를 활용하거나 저장하는 데 사용할 수 있다.

# Important

`mlx_loop_hook`은 위의 함수들과 동일한 방식이지만, 이벤트 발생 여부와 관계없ㅂ이 메인 루프가 해당 함수를 계속 호출한다는 점이 다르다. 함수의 원형은 다음과 같다.
```c
int loop_hook(void *param);
```

즉, 특정 이벤트 없이도 이벤트루프에서 주기적으로 이 함수를 호출한다. 이는 애니메이션을 구현하거나 프로그램을 일정 간격으로 업데이트 할 때 자주 사용된다.

# Going further with events

Minilibx는 모든 X-Window 이벤트에 직접 접근할 수 있도록 지원한다. `mlx.h` 헤더 파일에는 처리하고자 하는 이벤트에 대한 매크로 정의가 있으며, 이를 처리하기 위한 함수 원형은 다음과 같다.
```c
int mlx_hook(void *win_ptr, int x_event, int x_mask, int (*funct)(), void *param);
```
이 함수는 `param`과 이벤트를 동일한 방식으로 전달한다. 즉, `mouse_hook`이나 `key_hook` 같은 별도의 훅 함수 없이도, X-Window가 지원하는 모든 이벤트를 직접 처리할 수 있다.