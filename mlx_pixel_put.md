# Synopsys
```c
int mlx_pixel_put(void *mlx_ptr, void *win_ptr, int x, int y, int color);
int mlx_string_put(void *mlx_ptr, void *win_ptr, int x, int y, int color, char *string);
```

# Description
`mlx_pixel_put()` 함수는 `win_ptr` 윈도우 좌표 (`x`, `y`)에 `color`로 지정된 픽셀 하나를 그린다. 원점 (0, 0)은 윈도우의 좌측 상단이며 x 축과 y 축은 각각 우측, 아래쪽으로 증가한다. 연결 식별자 `mlx_ptr`이 필요하다.

`mlx_string_put()`의 매개변수도 동일하다. 다만, 픽셀 하나를 그리는 대신 (`x`, `y`)에 문자열을 출력한다.

두 함수 모두 정해진 윈도우 바깥으로 출력하는 것은 불가능하며, 선택된 윈도우 앞의 윈도우에 출력하는 것도 불가능하다.

# Color management
`color` 매개변수는 정수형이다. 출력되는 컬러는 정해진 형식으로 암호화 되어야 한다. 모든 출력 가능한 색들은 세 개의 바탕색으로 분리할 수 있다. 빨강, 초록, 파랑이다. 이 세 색의 값을 각각 0~255까지의 값으로 표현하고 조합해 또 다른 색을 만든다. 색을 올바르게 표현하기 위해선 이 세 색들의 값들은 반드시 integer 자료형으로 펴현되어야 한다. 색이 표현되는 방법은 다음과 같다.

##                                | T | R | G | B |    색 정수

interger 값을 채우면서도 시스템의 엔디안도 고려해야한다. "파랑"의 바이트가 그 척도가 되어야 한다.


## 이게 무슨 뜻이냐??

1바이트 짜리 정수형 데이터가 있을 때, 그 데이터의 비트는 위와 같이 구분할 수 있다는 얘기이다. 보통 초기화 할 때 `0xTTRRGGBB`와 같이 16진수 형식으로 초기화 하는데 여기서 각 부분의 의미는 다음과 같다.

- T: 투명도
- R: Red
- G: Green
- B: Blue

그래서 빨강은 `0x00FF0000`, 초록은 `0x0000FF00` 그리고 파랑은 `0x000000FF`로 색을 초기화한다.

여기서 색을 encoding 한다는 말은 비트쉬프트를 이용해 각 RGB값을 초기화 하는 것을 의미한다. 투명도부터 Blue까지의 값을 매개 변수로 받아 하나의 색 value를 반환하는 함수를 만든다고 한다면 이는 다음과 같이 만들 수 있다.

```c
int create_trgb(int t, int r, int g, int b)
{
    return (t << 24 | r << 16 | g << 8 | b);
}
```

거꾸로 각 요소들을 꺼내올 때는 다음과 같이 정의할 수가 있겠다.

```c
int get_t(int trgb)
{
    return (trgb & (0xFF << 24));
}

int get_r(int trgb)
{
    return (trgb & (0xFF << 16));
}

int get_g(int trgb)
{
    return (trgb & (0xFF << 8));
}

int get_b(int trgb)
{
    return (trgb & (0xFF));
}
```

여기서 한 가지 주의해야할 점은, 프로그램을 실행하는 시스템의 [[엔디안 (Endiannness)]]에 유의하여 데이터를 읽어야 한다는 점이다.