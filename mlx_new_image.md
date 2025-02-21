
# Synopsys

```c
void *mlx_new_image(void *mlx_ptr, int width, int height);

char *mlx_get_data_addr(void *img_ptr, int *bits_per_pixel, int *size_line, int *endian);

int mlx_put_image_to_window(void *mlx_ptr, void *win_ptr, void *img_ptr, int x, int y);

unsigned int mlx_get_color_value(void *mlx_ptr, int color);

void *mlx_xpm_to_image(void *mlx_ptr, char **xpm_data, int *width, int *height);

void *mlx_xpm_file_to_image(void *mlx_ptr, char *filename, int *width, int *height);

int mlx_destroy_image(void *mlx_ptr, void *img_ptr);
```



# Description
`mlx_new_image()`는 메모리에 새로운 이미지를 생성한다. 이 함수는 해당 이미지를 조작하기 위해 필요한 `void *` 식별자를 반환한다. 이미지를 생성하기 위해서는 오직 이미지의 너비(width)와 높이(height)만 필요하다. 사용자는 이 이미지 공간(캔버스) 안에서 그림을 그릴 수 있지만, 화면에 표시할 때마다 이 함수를 호출해야한다.

다음 3개의 포인터, `mlx_ptr`, `win_ptr`, `img_ptr`는 디스플레이와 연결하고 ,윈도우를 수정하고, 생성된 이미지의 주소를 반환받는 데 필요하다.

`bits_per_pixel`은 픽셀 색상을 표현하기 위해 사용되는 비트 수(이미지의 '깊이')가 채워지고, `size_line`는 메모리에서 한 줄의 크기를 바이트 단위로 나타낸다. 예를 들어 `size_line`이 64라면, 메모리에서 한 줄에서 다음 줄로 이동하기 위해서는 주소에 +64를 해줘야 한다. `endian` 변수는 머신의 [[엔디안 (Endiannness)]]을 나타내며, 일반적으로 0은 리틀 엔디안, 1은 빅 엔디안을 의미한다.
화면(윈도우)에 이미지를 표시하기 위해서는 `mlx_put_image_to_window()`함수를 사용한다.

`mlx_get_data_addr`는 이미지가 저장된 메모리 영역의 시작 위치를 가리키는 `char *`주소를 반환한다. 이 메모리 영역에서 각 픽셀은 4바이트를 차지하며, 처음 3바이트는 픽셀의 RGB 정보를, 마지막 1바이트는 알파(Alpha)채널을 저장한다. 이 주소를 통해 이미지의 픽셀 데이터를 읽거나 쓸 수 있다. 단, 이 주소는 여러 번 함수를 호출했을 때 동일하다는 보장이 없으므로 주의해야 한다.

## 이미지 저장 혹은 복사
이미지를 저장하거나 복사하려면, 단순히 `img_ptr = mlx_new_image()`와 `img_ptr2 = mlx_new_image()`처럼 호출하면 된다!!

# XPM 이미지
XPM 이미지는 조금 특별하다. `mlx_xpm_file_to_image()`함수를 사용하면, XPM 파일에서 저장된 크기와 내부 데이터를 불러와 이미지에 채워 넣는다. 이 함수는 새로운 이미지를 가리키는 포인터를 반환한다. `mlx_ptr`는 디스플레이와의 연결 식별자이고, `xpm_file_to_image()`는 함수 이름이다. 이렇게 반환된 포인터는 일반적인 `mlx_new_image()`로 생성된 포인터와 달리, 내부적으로 데이터를 변환한 뒤에 사용되므로 큰 차이가 있다.

# 반환값
XPM 파일에서 이미지를 생성하는 함수(`mlx_xpm_file_to_image()`, `mlx_new_image()` 등)는 에러가 발생하면 `NULL`을 반환한다. 정상적으로 생성되면 `mlx_pointer` 타입의 이미지를 가리키는 포인터를 반환한다.