
## AUTOSAR에서 정의한 공통의 시스템 개발 순서

![[Pasted image 20250721093801.png]]

1) 시스템 구성 단계 (Configure System)
    - 시스템 설정 단계로, 컴포넌트의 구성/연결 등을 정의한다.
    - Software Componenet Description을 개발한다.
2) Implement Component
    - 전 단계인 Configure System 단계에서 구성한 컴포넌트들에 대한 코드 구현 등을 진행한다.
3) Extract ECU-Specific Information
    - 시스템 구성 정보로부터 특정 제어기 소프트웨어를 구현하기 위한 정보만을 추출한다.
4) Configure ECU
    - ECU 관련 설정을 진행한다.
    - ECU Configuration Description을 개발한다.
5) Generate Executable
    - ECU에서 동작하는 실행 파일을 생성한다.
    - 컴파일 및 링크를 통해 실행 이미지를 생성한다.
## Configure System 상세 1

시스템 구성 단계에서 설계한 애플리케이션은 아래와 같은 과성을 통해 ECU 내에서 동작한다.

![[Pasted image 20250721094441.png]]

(1) 애플리케이션 개발
- 서로 연결된 컴포넌트(SW-C)들의 집합을 통해 애플리케이션을 설계한다.
- 이 때, ECU를 고려하지는 않는다.
(2) Virtual Functional Bus (VFB)
- 연결된 컴포넌트들이 교류하는 가상의 통신 구조(통로)를 의미한다.
(3) ECU 할당
- 제어기를 고려하여, 컴포넌트를 특정 제어기에 배치한다. => 가상의 컴포넌트 간의 연결이 실제 연결로 분류된다. 즉, 가상의 컴포넌트 간의 연결이, **제어기 내의 연결**이라던가, (CAN, FlexRay 등 통신 수단을 통한) **제어기 간 연결**이라던가 결정하게 된다.
(4) Run-Time Environment (RTE)
- 컴포넌트 - 컴포넌트 또는 컴포넌트 - BSW(Basic Software)간 구체적인(실제) 인터페이스이다.
- VFB 수준에서 설계한 컴포넌트들 및 컴포넌트 간의 관계를 제어기 상에서 동작할 수 있도록 코드화 한다. ==> **RTE는 VFB를 코드 레벨로 구현한 것이라고 볼 수 있다.**