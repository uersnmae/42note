**AUTOSAR**: 자동차 전제제어장치(ECU)의 소프트웨어 아키텍처를 표준화하여, 특정 마이크로컨트롤러(MCU)에 종속되지 않는 Application Software를 개발하고 **재사용**하는 것을 목표로 한다.

AUTOSAR는 명확하게 정의된 계층 구조를 가진다.

- Application Layer: 실제 자동차 기능을 수행하는 소프트웨어 컴포넌트(SW-C)가 위치하며, 하드웨어로부터 완전히 독립적이다.
- RTE, Runtime Environment: Application layer와 하위 layer를 연결하는 가상 함수 버스(Virtual Function Bus) 역할을 하며, 통신을 중재한다.
- BSW, Basic Software: 하드웨어에 종속적인 부분과 공통 서비스를 제공하는 계층이다. 이는 또 다시 여러 계층으로 나뉜다.
    - Services Layer: OS, 통신, 메모리 관리 등 상위 계층에 서비스를 제공한다.
    - ECU Abstraction Layaer: ECU 보드 상의 외부 주변 장치(예: CAN 트랜시버)에 대한 접근을 추상화한다.
    - MCAL, Microcontroller Abstraction Layer: BSW의 가장 하위 계층으로, MCU 내부의 주변 장치(ADC, PWM, DIO 등) 드라이버를 포함한다. MCAL이 있기 떄문에 상위 소프트웨어는 MCU가 변경되어도 영향을 받지 않는다.
    - Complex Drivers: 특수한 하드웨어나 타이밍 제약이 강한 경우 사용되는 드라이버이다.

![[Screenshot from 2025-07-18 14-41-51.png]]