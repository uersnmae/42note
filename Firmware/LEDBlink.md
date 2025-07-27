 
# CMSIS-Core를 활용한 펌웨어 개발

## 필요한 툴

- ARM GCC Toolchain: ARM 아키텍처용 C/C++ 컴파일러, 링커, 어셈블러 등이 포함된 툴체인이 필요함. 공식 명칭은 `arm-none-eabi-gcc`
- Make: 그냥 알죠?
- CMSIS (Cortex Microcontroller Software Interface Standard): ARM이 제공하는 표준화된 소프트웨어 인터페이스. 이번 예제에서 핵심 요소는 다음과 같다.
    - CMSIS-Core: NVIC(인터럽트 컨트롤러), SysTick(시스템 타이머) 등 Cortex-M 프로세서 코어의 핵심 기능에 접근하기 위한 API를 제공한다.
    - Device Header File: STMicroelectronics가 제공하는 파일로, 특정 STM32 모델(예: `stm32f446xx.h`)의 모든 레지스터 주소와 비트 필드 정의가 들어있다. 이 파일 덕분에 `RCC->AHB1ENR |= (1 << 0);` 와 같이 직관적인 코드를 작성할 수 있다.
    - 획득 방법: [ST 웹사이트](https://www.arm.com/technologies/cmsis)에서 해당 MCU 제품군dml 'STM32Cube MCU Firmware Package'를 다운로드하면 그 안에 CMSIS 관련 파일들이 모두 포함되어 있다. CubeIDE를 사용하지 않더라도 이 패키지는 필요하다.
- OpenOCD (Open On-Chip Debugger): ST-Link와 통신하여 펌웨어을 보드에 플래시(업로드)하고 디버깅을 가능하게 해주는 오픈소스 소프트웨어이다. ST-Link를 잘 다룰 줄 아는 사람은 해당하는 소프트웨어를 사용하면 된다.

![[Pasted image 20250727082407.png]]

# 필요한 핵심 문서

Low-level 개발에서는 문서를 읽는 능력이 굉장히 중요하다.
- **MCU Datasheet**: 칩의 전기적 특성, 핀 배치, 패키지 정보 등 하드웨어의 전반적인 사양을 다룬다.
- **MCU Reference Manual**: GPIO, RCC(클럭 제어), 타이머 등 칩의 모든 기능(Peripheral)에 대한 상세한 설명과 동작 방식, 그리고 **레지스터(Register) 맵**이 들어있다. LED를 켜려면 GPIO 포트의 클럭을 활성화하고(RCC), 해당 핀은 출력 모드로 설정해야(GPIO) 하는데, 이 모든 정보가 레퍼런스 매뉴얼에 있다.
- **개발 보드 User Manual)**: NUCLEO 보드를 사용한다면 보드의 회로도, 부품 배치, 그리고 우리가 제어할 **사용자 LED가 MCU의 어떤 GPIO 핀에 연결되어 있는지** 알려주는 중요한 정보가 담겨 있다.

# 로드맵

1. 환경 구축: 위에서 설명한 `arm-none-eabi-gcc`, `make`, `OenOCD`를 설치한다.
2. 프로젝트 폴더 생성: 프로젝트를 진행할 폴더를 하나 만든다.
3. 핵심 파일 준비:
    - STM32Cube 펌웨어 패키지에서 **스타트업 파일**(`.s` 확장자)과 **링커 스크립트**(`.ld` 확장자), 그리고 **CMSIS 헤더 파일**들을 프로젝트 폴더로 복사한다.
    - 스타트업 파일: 전원이 켜졌을 때 `main` 함수를 호출하기 전 초기화 작업을 수행한다.
    - 링커 스크립트: 컴파일된 코드와 데이터를 MCU의 플래시 메모리와 RAM에 어떻게 배치할 지 정의한다.
4. Makefile 작성: 소스 파일을 컴파일하고, 모든 오브젝트 파일을 모아 최종 `.elf` 및 `.bin` 펌웨어 파일을 생성하도록 Makefile을 작성한다.
5. C 코드 작성(`main.c`):
    - `stm32f411xe.h` 헤더파일을 포함한다.
    - `main` 함수에서 레지스터를 직접 제어하여 LED를 켠다.
        1. **RCC (Reset and Clock Control)** 레지스터를 설정해 LED에 연결된 GPIO 포트(예: GPIOA)에 클럭을 공급한다.
        2. **GPIO** 레지스터를 설정해 해당 핀을 'General-purpose output mode'로 변경한다.
        3. GPIO의 출력 데이터 레지스터(ODR)나 비트 설정/초기화 레지스터(BSRR)를 조작해 핀의 전압을 HIGH로 만들어 LED를 켠다.
6. 빌드 및 플래시: 터미널에서 `make` 명령어로 코드를 빌드하고, `openocd`를 이용해 생성된 펌웨어를 보드에 업로드한다.