# Synopsys

```c
void *mlx_new_window(void *mlx_ptr, int size_x, int size_y, char *title);
int mlx_clear_window(void *mlx_ptr, void *win_ptr);
int mlx_destroy_window(void *mlx_ptr, void *win_ptr);
```

# Description
`mlx_new_window()` `size_x`와 `size_y` 매개변수를 이용해 화면에 그릴 새로운 윈도우의 크기를 결정하고, `title` 매개변수의 문자열을 윈도우의 제목으로 한다. `mlx_ptr` 매개변수는 `mlx_init()`에서 반환된 connection 식별자이다. `mlx_new_window()`는 `void *` 자료형인 윈도우 식별자를 반환해 다른 MiniLibx 함수들의 호출에 쓰일 수 있다. MiniLibx는 임의의 개수의 독립적인 윈도우를 다룰 수 있다.

`mlx_clear_window()`와 `mlx_destroy_window()`는 각각 윈도우의 화면을 검게 비우고, 주어진 윈도우 식별자의 윈도우를 삭제한다. 이 두 함수 모두 `mlx_ptr` 연결 식별자와 `win_ptr` 윈도우 식별자를 매개변수로 한다.

# Return value
만약 `mlx_new_window()`가 새 윈도우를 생성하는데 실패한다면 `NULL`을 반환한다. 그렇지 않다면 non-null 포인터를 윈도우 식별자로 반환한다. `mlx_clear_window()`와 `mlx_destroy_window()`는 아무 것도 반환하지 않는다.