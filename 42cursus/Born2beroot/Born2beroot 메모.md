# 1. Overview

## 가상머신이란??
## 실재하는 컴퓨터 상에 소프트웨어로 논리적으로 만들어낸 컴퓨터 ##
## 하나의 물리 자원 위에 하나의 환경만 있는 것을 효율화하고자 가상화 층을 만들고 그 위에 OS를 새로 설치하는 기법

## 왜 만들었는가??
실제로 있는 컴퓨터 시스템을 여러 명의 사용자가 동시에 사용할 수 있게 하기 위해서 사용한다. 이로인해 컴퓨터 시스템을 여러 명의 사용자가 동시에 사용할 수 있도록 여러 대의 작은 컴퓨터로 분할 사용하거나, 운영 체제나 하드웨어 등의 구성을 달리하여 운영하고자 할 때 주로 사용된다.

즉, 내 컴퓨터의 자원(CPU, 주 · 보조 기억 장치, 인공지능 연산의 경우 GPU 등등..)을 다른 OS에서도 동시에 쓰고 싶을 때! 가상머신으로 만들어지는 가상의 컴퓨터가 내 컴퓨터의 자원을 일부 공유하면서 빌려 쓰는 것이다. 이거를 서버로 연결하면 내 원래의 컴퓨터 안에 저장되어 있는 데이터는 보호하면서 안정적으로 서버를 구동할 수 있기도 하다!

호스트 OS에서 작업하다가 오류가 발생할 경우 전체 시스템에 장애가 발생하거나, 이전 환경으로 되돌릴 수 없는 상황이 생기는 걸 사전에 막을 수 있다는 점?

## 가상화란??
하나의 실물 컴퓨팅 자원을 마치 여러 개인 것처럼 가상으로 쪼개서 사용하거나, 여러 개의 실물 컴퓨팅 자원들을 묶어서 하나의 자원인 것처럼 사용하는 것.

### **컴퓨팅 리소스를 '추상화'시켜서 하나의 물리 리소스를 여러 개의 논리 리소스처럼 기능 시키거나, 여러 개의 물리 리소스를 하나의 논리 리소스처럼 기능하게 하는 것!


## 가상 머신의 원리
가상 서버를 만들어달라는 요청을 [[하이퍼바이저]]는 소프트웨어에 전달. 요청을 받은 하이퍼바이저 는 새로운 가상 서버를 생성하고 물리서버가 가진 컴퓨팅 리소스를(CPU, 메모리, 스토리지, 네트워크 등) 각 가상 서버에 할당. 리소스 할당 후에는 각 서버에 필요한 운영체제도 설치.

이렇게 생성된 가상 머신을 **게스트 서버**, 가상 머신이 구동되는 서버를 **호스트 서버**라고 한다.

## Rocky와 Debian의 차이점
먼저 제일 큰 차이점은 Rocky는 Debian에 비해 초심자가 설정하기 어렵다는 점이 있다. 또한, 두 운영체제가 겨냥한 주 소비층이 Rocky는 기업, 서버를 주 목표로 하고 Debian은 개인의 PC를 포함한 범용적인 목적으로 설계되었다. 그 외에 무료로 사용할 수 있다는 점, 커뮤니티 기반으로 업데이트가 이뤄져 그 주기가 느리다는 점은 크게 중요하지 않은 것 같다.

이번 subject에서는 Debian으로 진행하기로 했다! (더 쉽다고 하니까)

## apt, aptitude, Apparmor
Debian으로 진행할 경우 apt, aptitude, Apparmor를 설명해야하는데 apt와 aptitude를 같이 설명하고 Apparmor와 Rocky의 SELinux를 같이 설명하는게 나을 것 같다.

## 패키지 관리자 apt와 aptitude
위에 써놨듯이 apt와 aptitude는 **Package Manager(패키지 관리자)** 라고 한다.
직설적으로 설명하면 **[[패키지]]**(vim, ifconfig, git 등등..)를 다루는 작업을 편리하고 안전하게 수행하기 위해 사용되는 툴이라고 한다.

## Advanced Packaging Tool
소프트웨어의 설치와 제거를 처리하는 패키지 관리 툴이다. 초기에는 .deb 패키지를 관리하기 위해 만들었지만 지금은 rpm 패키지 관리자와도 호환된다.

apt는 GUI 없이 명령어로 사용한다. 설치할 패키지 명을 입력하면 '/etc/apt/sources.list'에 저장된 소스 목록에서 해당 패키지 + 종속성 목록화 함께 찾아 자동으로 설치한다. 그래서 apt만으로도 어떤 패키지를 설치할 때에는 종속성 문제를 걱정하지 않아도 된다! 또한 사용자가 직접 새로운 소스 목록을 추가하는 행위도 가능하며 시스템이 업그레이드되도 해당 패키지를 업데이트 하지 않게 해 현재 버전을 계속 사용하는 등의 작업을 할 수 있다.

## Aptitude
사용자 인터페이스를 추가해 사용자가 대화형으로 패키지를 검색해 설치, 제거 할 수 있는 high-level 패키지 관리 도구이다. 이도 apt와 마찬가지로 .deb 패키지를 위해 만들어졌지만 rpm 기반 패키지까지 확장되었다. 텍스트 기반 대화형 인터페이스와 비대화형 Command line 모드에서도 작동한다.

## 차이점
한 줄로 요약하자면 aptitude가 더 많고 방대한 기능을 포함하고 있다. (예를 들어 apt-get, apt-cache 등)

apt-get이 패키지 설치, 업그레이드, 시스템 업그레이드, 종속성 검사를 수행한다면 aptitude는 설치된 패키지 목록 또는 패키지를 자동 혹은 수동으로 설치하도록 표시, 업그레이드에 사용할 수 없는 패키지 보관 등이 있다.

이 외에도 어떤 패키지를 삭제할 때, aptitude는 의존성이 있는 패키지까지 자동으로 삭제되는 반면, apt는 -auto-remove 등을 명시해줘야 한다. 또 aptitude는 why, why-not 등의 명령어를 이용해 어떤 동작이 왜 안되고 되는지를 볼 수 있다고 한다. 제일 큰 차이점이라고 느낀 점은 apt는 설치, 제거 중 충돌이 일어날 경우 그냥 종료되지만, aptitude는 해결 방법을 제시해준다고 한다.

## Mandatory Access Control
Rocky에선 SELinux, Debian에서는 Apparmor가 이에 해당한다.

Mandatory Access Control은 한국말로 **강제적 접근 제어**라고 하는데 무슨 말인지 하나도 안 와닿고 모르겠다. 위키피디아에서는 컴퓨터 보안에서 운영체제나 데이터베이스가 주체(운영체제에서는 프로세스나 스레드)나 개시자가 객체(파일, 디렉터리, TCP/UDP 포트, 공유 메모리 세그먼트, IO 장치 등등..)나 대상에 액세스하거나 일반적으로 일종의 작업을 수행하는 능력을 제한하는 **접근 제어 유형**을 의미한다고 한다.

이 MAC를 사용하게 된다면 그에 따른 보안 정책은 보안 정책 관리자가 중앙에서 관리한다. 사용자는 정책을 무시할 수 없다. 예를 들어, 제한될 파일에 대한 액세스 권한을 부여할 수 없다.

어려운 말을 써놨는데 쉽게 얘기하면 내외부적 위협으로부터 운영체제와 앱을 능동적으로 보호한다는 뜻이다.

## Apparmor

Apparmor의 특징

- **프로파일** 기반 보안: 프로그램에 대해 개별적인 **보안 프로파일**을 정의한다. 이 프로파일은 접근할 수 있는 파일, 네트워크 리소스 그리고 시스템 콜을 구체적으로 명시한다.
- **사용자 친화적**: 보안 정책을 관리하는데 있어 비교적 사용하기 쉬운 구문을 제공한다.
- **상태 모드**: Enforce 모드와 Complain 모드 두 가지 상태 모드를 제공한다. Enforce 모드는 정책이 실제로 위반되면 해당 동작을 차단하며, Complain에서는 위반 로그는 남지만 실제로 차단되지는 않는다. 그래서 Complain 모드에서는 보안 정책을 테스트하고 튜닝할 때 주로 사용한다
- **상태 저장**: 프로파일 변경 시 변경 내용을 즉시 적용하여 시스템 재시작 없이도 보안 정책을 관리할 수 있다.

Apparmor의 주요 기능

- **파일 접근 제어**: 프로그램이 접근할 수 있는 파일과 디렉토리를 제어한다. 불필요한 파일 접근은 차단하고, 프로그램 혹은 사용자 등이 허가된 파일만 다룰 수 있게 한다.
- **네트워크 접근 제어**: 특정 프로그램의 네트워크 사용을 제한할 수 있다. 예를 들어, 프로그램이 특정 포트에만 접근하거나, 특정 IP 주소로의 통신만 허용할 수 있다.
- **권한 제한**: 프로그램이 실행할 수 있는 시스템 콜을 제한하여, 잠재적으로 악의적인 행동을 예방한다.
- **로그 기록**: 모든 정책 위반 사항을 로그에 기록하여, 보안 감사 및 문제 해결에 활용할 수 있다.

SELinux는 이것보다 그냥 훨씬 어렵다고 생각하기로 했다.

aa-status로 Apparmor 상태를 확인하고,
/etc/apparmor.d/ 디렉토리에 보안 프로파일을 작성한다. 예를 들어, usr/bin/ping에 대한 프로파일을 장성하려면

```
vim /etc/apparmor.d/usr.bin.ping
apparmor_parser -r /etc/apparmor.d/usr.bin.ping
```
을 해주면 된다.

그 다음에 aa-enforce / aa-complain 으로 해당 프로파일의 모드를 적용해주면 된다.

Apparmor와 SELinux의 차이점을 표로 그려보면 다음 표와 같다.

|       | Apparmor       | SELinux                                                 |
| ----- | -------------- | ------------------------------------------------------- |
| 접근 제어 | 파일 경로 기반 접근 제어 | 태그 기반 접근 제어                                             |
| 사용 방식 | 간단하고 사용자 친화적   | 파일과 프로세스에 보안 컨텍스트를 할당하며,<br>설정이 더 복잡하지만 더 강력한 보안 기능을 제공 |

평가하면서 Subject에서 명시된 서버 상태에 대한 내용이 10분마다 출력되는지 확인!

# 2. Simple Setup

가상 머신을 부팅할 때 GUI 환경으로 부팅되는지 확인하고, 비밀번호를 입력해야하는지 확인한다.
피평가자의 계정으로 로그인하는데 이 계정은 root 계정이면 안 된다!

시스템을 부팅할 때 UFW, SSH가 돌아가는 중인지 확인한다.

또한, OS가 Rocky인지 Debian인지도 확인한다.

# 3. User

## Create User, Create Group and append it...

피평가자의 계정이 sudo와 user42 그룹에 속해 있는지 확인하고 새로운 유저를 만들어야한다!

```
sudo useradd [USERNAME]
```

이렇게 만든 유저를 evaluating이란 그룹에 추가해야한다!

```
sudo usermod -aG evaluating [USERNAME]
```

## Password Policy

마지막으로 비밀번호 정책을 설정함으로써 생기는 장점을 설명하고 단점 또한 설명해라

비밀번호 정책은 기본적으로 다음과 같은 파일을 수정해 설정할 수 있다.

```
/etc/login.defs
/etc/pam.d/common-passwd
```

예~에전에는 login.defs에서 모든 비밀번호 정책을 설정했다고 하는데 나중에는 비밀번호의 안전성을 이유로 **PAM**을 활용한 비밀번호 정책이 더 실용적이라고 판단해 분리시켰다고 한다.

이번 과제에서는 **PAM**의 여러 모듈 중 **pwquality.so** 모듈을 사용했다.

해당 모듈을 사용하기 위해서 저 모듈의 패키지를 설치해야했다.

```
sudo apt install libpam-pwquality
```

해당 명령어로 설치해주고

```
vim /etc/pam.d/common-passwd
```

로 저 파일을 수정해주면 된다.

과제에서 요구한 비밀번호 정책은 다음과 같다.

- 비밀번호는 30일 마다 폐기된다.
- 한 번 비밀번호를 수정하면 최소 2일을 기다려야한다. (쿨타임 2일)
- 비밀번호가 폐기되기 7일 전에 경고 메시지가 나와야한다.

여기까지는 /etc/login.defs에서 수정하면 된고, 다음부터는 **PAM**을 활용해야한다.

- 비밀번호의 최소 길이는 10글자이다. 대문자, 소문자, 숫자를 최소 1글자씩 포함해야한다. 또한, 같은 문자가 세 번 이상 연속으로 나열되면 안 된다.
- 비밀번호는 user의 ID를 포함해선 안 된다.
- 다음 규칙은 root 유저에겐 적용되지 않는다: 이전 비밀번호의 최소 7글자 이상은 현재 비밀번호에 포함되선 안 된다.
- 위 사항을 제외하곤 똑같은 규칙을 root 유저에게도 적용해야한다.

이것들은 man pam_pwquality 치면 나온다.

나 같은 경우는 다음과 같이 과제를 제출했다.

```
vim /etc/pam.d/common-password

password requisite pam_pwquality.so  minlen=10 ucredit=-1 lcrdit=-1 dcredit=-1 maxrepeat=3 usercheck=1 difok=7

password requisite pam_pwquality.so  minlen=10 ucredit=-1 lcrdit=-1 dcredit=-1 maxrepeat=3 usercheck=1
```

**PAM**이라는 녀석 자체가 비밀번호를 변경하는 과정에서 지금 바꾸려고 하는 주체가 root인지 아닌지, 바꾸려는 비밀번호가 root의 비밀번호인지 아닌지 판단하고, root라면 root에게 더 유리한 정책을 자동으로 적용한다고 한다. 그래서, 다음고 같이 선언해주면 difok은 일반사용자에게는 적용되지만, root에게는 적용되지 않는다!

근데 이게 sudo passwd로 하면 difok 없는 두 번째 규칙으로 들어가버려서.... 어떻게 해야할 지 참 난감;;

## **PAM**

PAM, Pluggable Authentication Moduels,은 리눅스와 유닉스 계열 운영체제에서 사용되는 유연한 인증 프레임워크이다. PAM은 시스템 관리자가 다양한 인증 메커니즘을 쉽게 구성하고 관리할 수 있게 해준다.

## PAM의 특징

1. 모듈화된 설계
	- 각 인증 메커니즘이 독립적인 모듈로 구현되어 있다.
	- 새로운 인증 방식을 쉽게 추가하거나 기존 방식을 수정할 수 있다.
2. 중앙 집중식 인증 정책
	- 시스템 전체의 인증 정책을 한 곳에서 관리할 수 있다.
3. 유연성
	- 다양한 인증 방식(비밀번호, 생체인식, 스마트카드 등)을 지원한다.
	- 애플리케이션별로 다른 인증 정책을 적용할 수 있다.
4. 보안 강화
	- 다단계 인증, 접근 제어 등 고급 보안 기능을 구현할 수 있다.

## PAM 구성 요소

1. PAM 라이브러리: 애플리케이션과 PAM 모듈 사이의 인터페이스 역할을 한다.
2. PAM 모듈: 실제 인증 작업을 수행하는 플러그인이다.
3. PAM 구성 파일: '/etc/pam.d/' 디렉터리에 위치하며 각 서비스의 인증 정책을 정의한다.

## PAM 작동 원리

1. 애플리케이션이 PAM 라이브러리를 호출한다.
2. PAM 라이브러리는 해당 서비스의 구성 파일을 읽는다.
3. 구성 파일에 정의된 **순서대로** PAM 모듈을 호출한다.
4. 각 모듈은 인증, 계정 관리, 세션 관리, 비밀번호 변경 등의 작업을 수행한다.
5. 결과를 애플리케이션에 반환한다.

PAM을 사용함으로써 시스템 보안과 사용자 인증을 유연하고 강력하게 관리할 수 있게 해준다.

# 4. Hostname and Partitions

## Hostname 수정

hostname은 dong-hki42 이런식으로 초기에 설치할 때 만들어놓고

```
/etc/hostname
/etc/hosts
```

이 두 개 파일 안에서 수정할 수 있다

```
hostnamectl set-hostname
```

이거로도 할 수 있다고는 한다.

## Logical Volume Manager

LVM의 정의
- 리눅스 시스템에서 디스크 스토리지를 유연하고 효율적으로 관리할 수 있게 해주는 기술. LVM을 이용해 물리적인 디스크 공간을 논리적인 볼륨으로 추상화하여 관리할 수 있다.

## LVM의 주요 구성 요소

1. 물리 볼륨(Physical Volume, PV)
	- 실제 물리적인 디스크나 파티션을 LVM에서 사용할 수 있도록 초기화한 것
	- ex) /dev/sda1, /dev/sdb1 등의 파티션을 PV로 초기화
2. 볼륨 그룹(Volume Group, VG)
	- 여러 개의 PV를 하나로 묶은 그룹
	- LV를 할당할 수 있는 공간의 풀(Pool) 역할을 한다.
3. 논리 볼륨(Logical Volume, LV)
	- 사용자가 실제로 사용하는 논리적인 스토리지 단위
	- 파일 시스템을 생성하고 마운트하여 사용

**파일 디스크럽터 내용도 추가하기** 
![[Pasted image 20241123131421.jpg]]
## LVM의 특징

1. 유연한 용량 관리
	- 필요에 따라 LV의 크기를 쉽게 확장하거나 축소할 수 있다.
2. 효율적인 스토리지 활용
	- 여러 불리 디스크를 하나의 논리적인 볼륨으로 통합하여 사용할 수 있다.
3. 효율적인 데이터 배치
	- **Striping**이라는 기술을 이용해 여러 PV에 데이터를 분산 저장하여 성능을 향상시킬 수 있다.
	- **Mirroring**을 통해 데이터 복제본을 만들어 안정성을 높일 수 있다.
4. 온라인 데이터 재배치
	- 시스템 운영 중에도 데이터를 이동하거나, 볼륨을 조정할 수 있다.
5. 스냅샷 기능
	- 특정 시점의 데이터 상태를 보존할 수 있어 백업에 유용하다.
6. 추상화 계층 생성
	- LVM은 물리적 저장장치 위에 추장화 계층을 만들어 논리적 볼륨을 생성한다.
	- 파일 시스템은 이 논리적 볼륨에 읽기/쓰기 작업을 수행한다.

## LVM 사용 과정

1. 물리 디스크에 파티션을 생성한다.
2. 파티션을 PV로 초기화한다.
3. PV들을 묶어 VG를 생성한다.
4. VG 내에서 필요한 크기의 LV를 생성한다.
5. LV에 파일 시스템을 생성하고 마운트하여 사용한다.

## 주요 명령어

```
pvcreate  # 파티션을 PV로 초기화한다.
pvdisplay # 모든 PV의 상세 정보를 표시한다.
pvs:      # PV 목록을 간단히 표시한다.
```

```
vgcreate VGNAME /dev/sdb1 /dev/sdc1    # VGNAME이라는 이름의 VG를 생성하고, /dev/sdb1과 /dev/sdc1을 포함시킨다.
vgdisplay                              # 모든 VG의 상세 정보를 표시한다.
vgs                                    # pvs와 동일
vgextend VGNAME /dev/sdd1              # VGNAME에 /dev/sdd1을 추가하여 VG를 확장한다.
vgreduce VGNAME /dev/sdd1              # VGNAME에 /dev/sdd1을 제거하여 VG를 축소한다.
```

```
lvcreate -L 10G -n LVNAME VGNAME              # VGNAME에서 10GB크기의 LVNAME이라는                                                  LV를 생성한다.
lvcreate -l 100%FREE -n LVNAME VGNAME         # VGNAME의 남은 모든 공간을 사용해                                                     LVNAME이라는 LV를 생성한다.
lvdisplay, lvs                                # pv, vg와 동일
lvextend -L +5G /dev/VGNAME/LVNAME            # LVNAME의 크기를 5GB 증가시킨다.
lvextend -r -l +100%FREE /dev/VGNAME/LVNAME   # LVNAME을 VG의 남은 모든 공간을 사용하                                                 여 확장한다.
lvreduce -L -5G /dev/VGNAME/LVNAME            # 말 안 해도 알지?
```

이 외에도 umount(파티션을 언마운트 할 때 사용), mount(파티션을 파일 시스템에 마운트 할 때 사용) 등 다양한 명령어가 있다.

## Encryption

우리 과제에서는 추가로 암호화를 요구하고 있는데 설치 단계에서부터 쉽게 암호화할 수 있지만, 당연히 터미널 상에서도 암호화 할 수 있다.

**LUKS(Linux Unified Key Setup)** 을 사용해 암호화하는 것이 일반적이다.

## LUKS를 사용한 LVM 볼륨 암호화

1. LUKS 초기화
	- cryptsetup luksFormat /dev/volume-groupname/lvolume
	- 위 명령어로 지정된 논리 볼륨에 LUKS 암호화를 설정한다.
2. 암호화된 볼륨 열기
	- cryptsetup open /dev/volume-groupname/lvolume open-lvolume
	- 암호화된 볼륨을 열어 읽기/쓰기가 가능하게 한다.
3. 파일 시스템 생성
	- mkfs.xfs /dev/mapper/open-lvolume
	- 열린 암호화 볼륨에 파일 시스템을 생성한다.
4. 마운트
	- mount /dev/mapper/open-lvolume /mnt
	- 암호화된 볼륨을 마운트 포인트에 연결한다.

**주의사항**
- 암호화 과정은 볼륨의 모든 데이터를 삭제하므로, 기존 데이터가 있다면 백업이 필요하다.
- 암호화된 LVM볼륨은 GUID를 사용할 수 없어 가상 머신 간 마이그레이션이 제한된다.

## 자동 마운트 설정

암호화된 볼륨을 부팅 시 자동으로 마운트하려면

1. LUKS UUID 확인
	- cryptsetup luksUUID /dev/volume-groupname/lvolume
2. /etc/crypttab 파일에 항목 추가
3. /etc/fstab 파일에 마운트 정보 추가

# 5. SUDO

```
dpkg -l | grep sudo
```

위 명령어로 sudo가 잘 설치되었는지 확인!

아까 만든 평가자의 계정을 다음 명령어로 sudo 그룹에 편입

```
sudo usermod -aG EVALUATOR
```

그 다음 sudoers 보여주면서 과제에서 요구한 sudo 정책 설명하기

```
sudo visudo
```

난 대부분의 정보를 man 활용해서 얻었으니까 같이 보면서 설명

# 6. UFW

```
sudo ufw status
```

위 명령어로 ufw 켜져있는거 확인!

## UFW란?
UFW, Unncomplicated Firewall,는 리눅스 시스템에서 사용되는 간단하고 사용하기 쉬운 방화벽 관리 도구이다. 이름부터가 "복잡하지 않은 방화벽"이 적혀있다.

## 특징

- **iptables**를 기반으로 하지만 더 간단하고 직관적인 인터페이스를 제공한다.
- 명령어가 단순하고 적어 사용이 쉽다.
- **IPv4**, **IPv6** 규칙을 자동으로 추가해준다.

## 장점

- 윈도우 방화벽처럼 프로그램을 직접 등록할 수 있어 직관적이다.
- 리눅스 초보자도 쉽게 사용할 수 있다.
- 기본적인 방화벽 설정을 간단하게 할 수 있다.

하지만! 복잡하고 정밀한 방화벽 설정에는 제약이 있을 수 있다.

```
ufw allow 4242
ufw status numbered
ufw delete [POLICYNUM]
ufw status
```

대충 요런 식?

평가표에서는 8080 열었다가 닫으라고 했으니까 그거 함 시연 ㄱㄱ

참고로 deny와 delete는 다르다!!

# 7. SSH

## SSH란?
**Secures SHell**의 약자로 네트워크 상의 다른 컴퓨터에 로그인하거나 원격 시스템에서 명령을 실행하고 파일을 전송할 수 있게 해주는 암호화된 네트워크 프로토콜이다. 주로 유닉스 계열 운영체제에서 사용되며, 윈도우에서도 지원된다.

## SSH의 주요 특징

1. **보안성**
	- 데이터를 암호화하여 전송하므로 중간자 공격이나 도청으로부터 안전하다.
	- 공개키 암호화 방식을 사용하여 인증한다.
2. 다양한 인증 방식
	- 비밀번호 인증
	- 공개키/개인키 인증
	- 호스트 기반 인증
3. **포트 포워딩**
	- 로컬 포트 포워딩
	- 원격 포트 포워딩
	- 동적 포트 포워딩 (SOCKS 프록시)
4. 파일 전송
	- SCP (Secure Copy)
	- SFTP (SSH File Transfer Protocol)

## SSH 작동 원리

1. 연결 초기화
	- 클라이언트가 서버에 연결을 요청한다.
2. 서버 인증
	- 서버가 자신의 공개키를 클라이언트에게 전송한다.
3. 세션 키 교환
	- 대칭 암호화에 사용될 세션 키를 안전하게 교환한다.
4. 사용자 인증
	- 사용자 인증 방식에 따라 인증을 수행한다.
5. 세션 시작
	- 인증이 성공하면 암호화된 SSH 세션이 시작된다.

```
# 원격 로그인
ssh dong-hki@192.168.56.1 -p 4242

# 원격 명령 실행
ssh dong-hki@192.168.56.1 -p 'ls -l'

# SCP를 이용한 파일 전송
scp local_file username@remote_host:/path/to/destination
```


## **포트 포워딩

네트워크에서 외부의 요청을 내부 네트워크의 특정 장치로 전달하는 기술

### 개념 및 작동 원리

- 라우터나 방화벽을 통과하는 네트워크 트래픽을 특정 IP 주소와 포트로 리디렉션한다.
- 공유기가 가진 공인 IP 주소와 특정 포트로 들어오는 요청을 내부 네트워크의 사설 IP 주소로 전달한다.

#### 주요 용도

1. 원격 접속: 외부에서 내부 네트워크의 서버나 장치에 접근할 수 있게 한다.
2. 보안 강화: 내부 네트워크 구조를 숨기고 원하지 않는 트래픽을 차단한다.
3. 온라인 게임: 게임 서버 운영이나 친구와의 연결을 위해 사용된다.
4. 홈 네트워크 관리: 보안 카메라나 원격 데스크톱 접속 등에 활용된다.

## 버추얼 박스에서의 설정

일단 가상 머신의 네트워크 중 기본으로 활성화된 NAT에 더해 Host-only Adapter를 추가로 활성화 시켜준다. 이렇게 하면 외부 네트워크와 더불어 호스트 컴퓨터와 가상 머신간의 네트워크 통신이 가능해진다. 정리하면 다음과 같다.

1. **Host-only Adapter**
	- 호스트 컴퓨터와 가상 머신 간의 네트워크 통신이 가능해진다.
	- 가상 머신과 호스트 간의 전용 네트워크 통신을 제공한다. (vboxnet0)
	- 외부 네트워크와 단절된 상태에서 안전한 개발/테스트 환경을 제공한다.
2. **NAT Adapter**
	- 가상 머신이 외부 네트워크에 연결할 수 있도록 해준다.
	- NAT를 통해 호스트 컴퓨터의 네트워크를 공유한다.
	- 호스트 컴퓨터의 네트워크를 NAT로 공유하기 때문에, 외부에서 가상 머신에 직접 접근할 수 없다.

이 두 개의 어댑터를 함께 활성화하여 호스트와 인터넷에 동시에 접근할 수 있다.

# 8. Cron

Cron은 [[UNIX]]계열 운영 체제에서 사용되는 시간 기반 작업 스케줄러이다. cron을 통해 특정 시간이나 간격으로 명령이나 스크립트를 자동으로 실행할 수 있다.

## cron의 특징

1. 정기적인 작업 실행: 매분, 매시간, 매일, 매주, 매월 등 정해진 시간에 작업을 실행할 수 있다.
2. 사영자별 설정: 각 사용자는 자신만의 cron 작업을 설정할 수 있다.
3. 시스템 전체 설정: 시스템 관리자는 전체 시스템에 적용되는 cron 작업을 설정할 수 있다.
4. 로그 기록: cron 작업의 실행 결과를 로그로 기록할 수 있다.

## crontab

cron 작업은 crontab 파일에 정의된다.

```
sudo crontab -e   # 현재 사용자의 crontab 파일을 편집한다.
sudo crontab -l   # 현재 사용자의 crontab 내용을 표시한다.
sudo crontab -r   # 현재 사용자의 crontab 파일을 삭제한다.
```

위 명령어로 crontab을 편집해야한다.

```
* * * * * command_to_execute
```

와 같은 형태로 되어 있는데 각 필드는 순서대로,

- 분 (0 ~ 59)
- 시간 (0 ~ 23)
- 일 (1 ~ 31)
- 월 (1 ~ 12)
- 요일 (0 ~ 7, 0과 7은 일요일)

## monitoring.sh

```
arch=$(uname -a)
cpu=$(lscpu | grep "Socket(s)" | awk '{print $2}')
vcpu=$(lscpu | grep "^CPU(s)" | awk '{print $2}')
memusage=$(free -m | awk 'NR==2 {printf "%s/%sMB (%.2f%%)\n", $3, $2, $3*100/$2 }')
disktotal=$(df -Bg | grep '^/dev/' | awk '{ft += $2} END {print ft}')
diskusage=$(df -Bm | grep '^/dev/' | awk '{ut += $3} END {print ut}')
diskrate=$(df -Bm | grep '^/dev/' | awk ' {ut += $3} {ft += $2} END {printf("%d"), ut/ft*100} ')
cpuload=$(top -bn1 | grep load | awk '{printf "%.2f%%\n", $(NF-2)}')
lastboot=$(who -b | awk '{print $3, $4}')
lvm=$(lsblk | grep -c "lvm")
lvmuse=$(if [ $lvm -eq 0 ]; then echo no; else echo yes; fi)
connections=$(ss -t | wc -l)
tcp=$(echo "$connections - 1" | bc)
user=$(who | wc -l)
ip=$(hostname -I)
eth=$(ip link show | awk '$1 == "link/ether" {print $2}' | awk 'NR==1 {print}')
cmd=$(journalctl _COMM=sudo | grep -c COMMAND)
wall "
  #Architecture: $arch
  #CPU physical : $cpu
  #vCPU : $vcpu
  #Memory Usage: $memusage
  #Disk Usage: ${diskusage}/${disktotal}Gb ($diskrate%)
  #CPU load: $cpuload
  #Last boot: $lastboot
  #LVM use: $lvmuse
  #Connections TCP : $tcp ESTABLISHED
  #User log: $user
  #Network: IP $ip ($eth)
  #Sudo : $cmd cmd
"
```

# BONUS

1. https://eunbin00.tistory.com/106

2. https://shanepark.tistory.com/398

**리버스 프록시** 찾아보기