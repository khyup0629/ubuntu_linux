본 자료는 「이것이 우분투 리눅스다 (저자:우재남)」를 참조하여 작성되었음을 알립니다.

---

# Ch 1. 실습 환경 구축

> <h3>가상머신(virtual machine, VM)</h3>

![image](https://user-images.githubusercontent.com/43658658/138982840-8637ae05-22ad-493b-aaac-6767b238a40f.png)

하나의 컴퓨터 위에 올라간 여러 개의 운영체제(OS), 그 각각을 가상머신이라고 합니다.   

예를 들어, Windows 운영체제 내에서 리눅스 운영체제를 사용할 수 있고, 리눅스가 바로 VM입니다.

* VM의 장점
  - 1대의 컴퓨터로 실무 환경과 비슷한 네트워크 환경 구성 가능(ex. 서버와 클라이언트 컴퓨터를 만들고 테스트 가능)
  - 운영체제의 특정 시점을 저장하는 `스냅샷` 기능 사용 가능(ex. Git과 비슷)
  - 하드웨어 여러 대를 장착해보며 테스트 가능
  - 현재 PC 상태를 그대로 저장해놓고, 다음에 이어서 사용 가능

> <h3>하이퍼바이저(Hypervisor)</h3>

가상머신을 생성하고 구동하는 소프트웨어입니다.   
호스트 컴퓨터에서 다수의 OS를 동시에 실행하기 위한 논리적 플랫폼입니다.   
컴퓨터에 설치된 운영체제(호스트 OS)에 가상의 컴퓨터를 만들고,   
그 가상의 컴퓨터 안에 또 다른 운영체제(게스트 OS)를 설치/운영할 수 있습니다.

> <h3>가상화 불가</h3>

교재에서는 자신의 컴퓨터에 가상 머신을 설치하여 가상 머신 내에 우분투를 설치하는 것을 가이드하고 있지만,   
`SecurAble 프로그램`이라는 가상화 가능 여부를 알려주는 프로그램을 실행해봤을 때,   
현재의 CPU로는 하드웨어 가상화가 불가능하다고 나왔습니다.   
![image](https://user-images.githubusercontent.com/43658658/138983699-5f46cf43-1453-4040-b644-57396d12ba9c.png)   
BIOS 단계에서 가상화 기술(`Virtualization Technology`) 역시 `[Enable]`로 체크되어 있는 상태였기 때문에   
현재 노트북을 가상화하는 방법이 아닌 다른 방법을 찾아보았습니다.

* 가상화 기술이 불가능하다고 표시되는 것은 사실상 불가능하다는 뜻이 아니라 가상화를 위한 CPU 가속 기능을 최적화 하는 알고리즘을 지원하지 않는다는 것을 의미합니다. 가상화라고 하는 것은 물리 CPU를 잘게 나눠서 순차적으로 쓰는 것을 말하는데, 이것을 더 빠르게 순차적으로 쓰이게 해주는 알고리즘을 지원하지 않는다는 의미입니다. 그래서 가상화 기술을 지원한다는 것은 CPU를 쓰는 속도를 가속시켜 준다는 의미입니다.

> <h3>대안</h3>

`openVPN`을 베이스로 회사의 가상화 서버(`ESXi`)에 접속하여 `Ubuntu` 설치를 진행하였습니다.

* VPN(가상 사설 통신망)을 쓰는 이유?   
`저렴한 비용`으로 `보안 서비스`를 갖춘, 회사만의 네트워크를 구축하고 운영, 통신하기 위함.

> <h3>가상머신 생성</h3>

[가상 시스템] > [VM 생성/등록]을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/139039563-b869a7a6-7ad8-4dc1-af14-068b4b054478.png)

1. 생성 유형 선택

새 가상머신을 생성할 것이므로 새 가상 시스템을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139039851-49ff2230-a112-439d-abfb-4bbba4ecfd2d.png)

2. 이름 및 게스트 운영 체제 선택

우리는 우분투 리눅스를 설치할 것이므로 아래와 같이 설정합니다.    
![image](https://user-images.githubusercontent.com/43658658/139040694-9f1f6df7-b295-46fd-ab21-b1be72b567bc.png)

3. 스토리지 선택

![image](https://user-images.githubusercontent.com/43658658/139041587-1906d9be-7166-4d6d-a36d-4d47a8d4d9ee.png)   
ESXi 물리 서버의 디스크 스토리지를 나타냅니다.   
총 용량 1.9 TB의 디스크에서 사용 가능한 용량은 1.26TB이라는 의미입니다.   
디스크의 파티션을 여러 개 나눠 놓았다면 여러 항목이 있겠지만, 현재 물리 서버에는 하나의 디스크만 있으므로 1개의 항목만 나타나는 것입니다.

4. 설정 사용자 지정

![image](https://user-images.githubusercontent.com/43658658/139048729-00cadf97-0e03-44b1-aba2-7ecbb756330d.png)   
* 가상 하드디스크의 작동 원리
  - 가상 하드디스크의 용량 설정시 VM은 가상 하드디스크를 진짜 하드디스크로 인식하지만, 호스트 입장에서는 하나의 커다란 파일이 됩니다.
  - 예를 들어, 20GB의 가상 하드디스크를 지정했다면, VM은 20GB로 인식하지만, 실제로 파일의 용량은 그보다 작습니다.
  - 가상 머신 내에 OS와 프로그램들이 설치되면서 점점 파일의 용량이 커져가고 최대 20GB까지 커집니다.
  - 가상 하드디스크는 추가할 수 있습니다.
* USB 컨트롤러
  - 실제 물리적으로 USB를 꽂았을 때 VM에서 인식을 할 수 있도록 하기 위해 USB 포트를 뚫어주는 것입니다.
* SCSI   
![image](https://user-images.githubusercontent.com/43658658/139047232-32ce96d9-10d4-4a8c-bc0b-e905fcca10fe.png)
  - 서버나 워크스테이션 등에 쓰이는 고속 인터페이스(연결 규격)
  - 안정성이 높은 것이 큰 장점. 하지만 가격이 비싸다는 단점.
  - SAS 인터페이스가 등장하면서 입지가 줄어들고 있습니다.
* SATA   
![image](https://user-images.githubusercontent.com/43658658/139047629-130261dd-c248-4ae9-8b6b-94388561a44f.png)   
  - 최근에 나온 인터페이스(연결 규격)으로 하드디스크 드라이브의 속도와 연결 방식 등을 개선하기 위해 개발되었습니다.   

[SCSI, SATA 외 인터페이스 설명](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=skyluvtoya&logNo=100120822628)
* 네트워크 어뎁터
  - `VM Network`라는 이름의 가상의 LAN 카드
* CD/DVD 드라이브
  - 설치를 원하는 iso 파일을 선택합니다.
  - iso 파일은 CD와 같고, 원하는 OS를 설치하기 위해 CD/DVD 드라이브에 CD를 넣겠다와 의미가 같습니다.

5. 완료 준비

선택 항목들을 다시 한 번 검토한 후에 [완료] 버튼을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/139049205-47c8bdda-2ca7-4dc1-a426-aae176bf08a3.png)   

선택한 항목들로 VM이 생성됩니다.

> <h3>VMware 설치</h3>

비록, 가상화 테스트에서는 불가능하다고 나왔지만 VMware 설치를 시도해보았습니다.   

VMware Workstation Player 설치 파일을 실행했을 때 아래와 같은 메시지가 출력되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/139246018-6aa6f643-69fa-4a4d-89b0-6acab6069b8b.png)

기술 자료 문서를 클릭해보니 아래와 같았습니다.   
![image](https://user-images.githubusercontent.com/43658658/139246030-9ee678a3-20a8-4c39-96d9-6ea4c12b0fe4.png)

Microsoft VC 재배포 가능 패키지는 Microsoft Visual C++ 2017 Redistributable를 의미했고, 요약하면 Microsoft Visual C++ 2017 Redistributable가 사전에 설치되어 있어야 한다는 것을 의미했습니다.

Microsoft Visual C++ 2017 Redistributable를 구글링 해서 설치했습니다.   
![image](https://user-images.githubusercontent.com/43658658/139246044-9ac67719-9010-444c-bd68-50e35b81297a.png)

재부팅 결과 정상적으로 설치가 가능 해졌습니다.

![image](https://user-images.githubusercontent.com/43658658/139246069-f3fa229f-014f-4c81-85d8-9b20f497828d.png)   
Server라는 이름의 VM을 오른쪽과 같은 환경으로 만들었습니다.

# Ch 2. 우분투 리눅스 소개

> <h3>리눅스의 탄생</h3>

리눅스는 `무료 유닉스`라고 볼 수 있습니다.   
유닉스(Unix)는 리눅스가 탄생하기 이전부터 널리 사용되던 운영체제였는데, 그 비용이 너무 비쌌습니다.   
리누스 토르발스는 유닉스의 작은 버전인 미닉스(Minix)보다 좋은 운영체제를 만드는 것을 목표로 `리눅스 커널`을 만들었습니다.   
리눅스는 유닉스와 거의 동일한 기능과 역할은 하면서도 무료이고, 어떤 면에서는 유닉스보다 더 뛰어나기도 합니다.

> <h3>GNU 프로젝트</h3>

'모두가 공유할 수 있는 소프트웨어'를 만드는 것을 목표로 시작된 `GNU 프로젝트`는 누구든지 소프트웨어를 자유롭게 사용, 수정, 재배포 할 수 있도록 노력했습니다.
GNU 프로젝트를 통해 리눅스가 완성되었으므로 엄밀히 따지면 `GNU/Linux`라고 부르는 것이 맞습니다.

> <h3>커널</h3>

커널은 자동차로 따지면 `엔진`과 같은 핵심 역할을 합니다.   
커널에는 현재 제어하는 하드웨어 장치의 지원 여부 정보, 하드웨어 성능, 하드웨어를 제어하는 코드들이 들어 있습니다.

리눅스 커널의 버전은 `안정 버전`과 `개발 버전`으로 나뉩니다.   
안정 버전은 이미 검증된 개발 완료 코드로 구성되어 있고,   
개발 버전은 현재 개발 중인 코드입니다.   
미리 추가된 기능을 접하고 싶을 때 개발 버전을 이용할 수 있습니다.

> <h3>리눅스 배포판</h3>

일반 사용자들은 리눅스 커널만으로 리눅스를 사용할 수 없습니다.   
이런 이유 때문에 여러 회사나 단체에서 리눅스 커널에 다양한 응용 프로그램을 추가해 쉽게 설치할 수 있도록 `리눅스 배포판`을 만들었습니다.   
리눅스 배포판 = 리눅스 커널 + 응용 프로그램   
`우분투 리눅스`는 이러한 수많은 리눅스 배포판 중에 하나입니다.

우분투 배포판은 기본적으로 `우분투 데스크탑`과 `우분투 서버` 두 가지를 기본적으로 배포합니다.   
![image](https://user-images.githubusercontent.com/43658658/138988132-2d60b48f-e4ac-460c-87b0-08c0f33aa503.png)   
우분투 데스크탑은 `X 윈도` 환경을 통해 GUI(Graphical User Interface) 형태로 제공됩니다.   
우분투 서버는 TUI(Text User Interface) 형태로 제공됩니다.

* TUI는 무엇인가?   
  ![image](https://user-images.githubusercontent.com/43658658/138988540-09f3124e-8024-4963-8a99-afa5402aecd6.png)   
  ![image](https://user-images.githubusercontent.com/43658658/138989078-3adc6d5c-80d9-4e69-8487-14a6b79524c4.png)   
  - TUI는 CLI와는 다릅니다. 문자(Text)로 표현된 그래픽 사용자 인터페이스.
  - GUI보다 훨씬 가벼워서 웬만한 네트워크 환경에서 쾌적하게 코딩, 고급 시스템 관리 가능.
  - 멀티미디어의 표현이 불가능, 그래픽 작업 불가능.

# Ch 3. 우분투 리눅스 설치

> <h3>우분투 데스크탑 설치</h3>

먼저 우분투 데스크탑을 VM에 설치해보겠습니다.   
앞서 VM 생성 당시 CD/DVD 드라이브에 우분투 데스크탑 iso 파일을 선택했었습니다.   

![image](https://user-images.githubusercontent.com/43658658/139168789-96497f81-7c28-49cc-b67c-17ac3e19b577.png)   
생성한 VM을 클릭하고 [편집] > [CD/DVD 드라이브] > 연결에 체크하고 저장합니다.

![image](https://user-images.githubusercontent.com/43658658/139168903-774551be-10db-4bd7-863a-e56904b13686.png)   
상단에 [전원 켜기] 버튼을 클릭하고, 아래의 화면을 클릭하면 우분투 데스크탑 설치가 시작되는 것을 볼 수 있습니다.

[한국어] > [Ubuntu 체험하기]를 눌러줍니다.
* 바로 [Ubuntu 설치]를 클릭하면 화면의 해상도가 낮게 설정되어 설치 진행 버튼들이 화면 밖으로 나가 보이지 않으므로 진행하기 어려워집니다.   
그래서 우선 [Ubuntu 체험하기]를 클릭해서 메모리상에 부팅을 먼저 시킵니다.   
![image](https://user-images.githubusercontent.com/43658658/139226173-38efdd5f-ac17-4632-8955-ef0001ecd811.png)

잠시 기다리면 우분투가 다시 부팅됩니다.

해상도를 바꾸기 위해 [Settings]를 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/139170923-573c7cdb-d6b8-4f2d-9e85-44fec759a271.png)

[Display] > [Resolution]의 해상도를 `1024 x 768`로 맞춰줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139171449-0878bff1-b995-48ec-ad95-fe720ed943c6.png)

바탕화면에서 `Ubuntu 20.04 LTS 설치` 아이콘을 클릭해 설치를 진행합니다.

![image](https://user-images.githubusercontent.com/43658658/139171733-2695d14a-5f25-47b8-a945-ebb4be20a34a.png)   

![image](https://user-images.githubusercontent.com/43658658/139175410-e5483795-6161-4114-99c4-6a1f92c7c4b7.png)   
* Korean-Korean(101/104 key compatible) : 한영키를 사용해 한글을 쓸 수 있게 하는 키보드 레이아웃

![image](https://user-images.githubusercontent.com/43658658/139175786-8426ccd1-46c7-4bb6-af2a-fb36d71d369f.png)   
![image](https://user-images.githubusercontent.com/43658658/139176219-4a6a6744-bef0-45d6-815b-4f098032c91d.png)   
* `디스크를 지우고 Ubuntu 설치`는 전체 디스크를 하나의 파티션으로 설정해서 진행
* `기타`는 디스크를 여러 개의 파티션으로 나누는 진행

![image](https://user-images.githubusercontent.com/43658658/139176986-3337800b-c438-4f3f-b9f2-81dd31fb9349.png)   
알림창이 뜨면 [계속하기] 버튼을 누릅니다.

![image](https://user-images.githubusercontent.com/43658658/139178321-79ac5b1e-953b-4dd7-89db-ad6de9144bd7.png)   
스왑 파티션을 나눕니다.   

* 스왑 파티션이란?   
하드 디스크에서 메모리의 역할을 할 일정 용량을 할당하는 것입니다. 그래서 메모리가 가득 찼을 때 스왑 파티션이 초과되는 메모리의 역할을 담당합니다.   
하지만, 하드 디스크의 용량을 차지하고 있고, 하드 디스크는 계속해서 쓰이게 되므로 성능이 떨어집니다.   
  * 설치된 메모리가 4GB 이하일 때는 스왑 파티션이 사실상 별로 필요하지 않고, 
  * 최소 7200rpm의 하드디스크가 장착되었을 때 메모리 성능을 좀 더 올리고 싶다면 스왑 파티션을 고려해 볼 수 있습니다.
  * 스왑 파티션은 최소, 메모리의 용량에 10~25%의 여유 공간을 더 가지는 용량을, 교재에서는 메모리의 2배의 용량을 권장하고 있습니다.

스왑이라는 개념은 현대에서는 쓰이지 않습니다. 하드웨어의 스팩이 워낙 좋아지면서 스왑의 필요성이 사실상 사라졌기 때문입니다. 설정 상 스왑을 1이라도 잡아야 되는 프로그램이 있습니다. 요즘은 이런 경우에만 스왑 파티션을 나눠줍니다.

![image](https://user-images.githubusercontent.com/43658658/139186279-b31b4125-c597-4a16-ad6e-e43de9566f06.png)   
루트 파티션을 나눕니다.   
디스크 파티션을 스왑 파티션과 루트 파티션으로 나눌 때는 스왑 파티션을 제외한 모든 용량을 루트 파티션으로 나눠줍니다.   
루트 파티션 속에 나머지 파티션(/boot, /etc, /usr, /tmp, /var, /home 등)이 포함되기 때문입니다.   

* EXT4 저널링 파일 시스템   
파일 시스템이 정상적으로 종료(언마운트)되지 않았다면, 저장장치 내에 있는 모든 INode와 Bitmap 등의 Meta Data를 순차적으로 검사하여 일관성 문제를 해결합니다.   
그러나 모두 검사하기에는 시간이 너무 오래 걸리는 이슈가 있습니다.
이런 상황을 해결하고자 파일시스템 Ext3부터 `저널링 방식`을 적용하였습니다.   
`파일을 쓰는 작업`을 할 때 파일시스템의 특정 영역에 `저널(Journal)`이라 불리는 로그를 기록한 뒤,   
시스템이 비정상적으로 종료되었을 때 로그를 보고 종료된 위치를 바로 알 수 있습니다.   
  * inode : 유닉스 운영체제에서 사용하는 자료 구조로, 파일 시스템 내부에 파일을 유지하는 중요한 정보를 담고 있다. 유닉스에서 파일 시스템을 생성할 때, 수 많은 inode 집합을 생성한다. 일반적으로 전체 파일 시스템 디스크 용량의 대략 1% 정도가 inode 테이블에 할당됩니다.
  * 마운트(mount) : 하드디스크와 같은 저장 장치를 사용할 수 있도록 설정하는 작업.

파티션에 대한 설명은 앞으로 자세히 언급할 것이므로 이 정도로 훑고 넘어가겠습니다.

![image](https://user-images.githubusercontent.com/43658658/139188371-4931d41f-3aae-4d39-9d4b-edca058925b2.png)   
![image](https://user-images.githubusercontent.com/43658658/139188461-d2ead4d8-7176-4188-bd3e-9882e385aa6f.png)   
서버의 목적으로 만들 것이므로 [컴퓨터 이름]에는 `server`를 입력하고, [사용자 이름]/[암호]는 ubuntu/ubuntu로 입력합니다.

설치가 모두 완료되면 [지금 다시 시작]을 눌러줍니다.

> <h3>관리자 계정으로 자동 로그인 설정하기</h3>

1. 터미널을 열고 `sudo su`를 통해 관리자 계정으로 접속합니다.
2. `passwd` 명령어로 root 계정의 비밀번호를 설정합니다.
3. [설정] > [사용자] > [잠금 해제]를 통해 계정의 비밀번호를 입력한 후 [자동 로그인]을 켭(보라색)니다.
4. 관리자 계정으로 접속된 터미널에서 `nano /etc/gdm3/custom.conf` 파일을 열어줍니다.
5. 기존에 일반 계정(Ubuntu)로 설정된 것을 관리자 계정(root)으로 바꿔주고, 관리자 계정으로의 접근을 보안적으로 허용해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139196062-29dbcffd-e0ea-489f-a388-0b89197eec54.png)   
6. `nano /etc/pam.d/gdm-password` 파일을 열어줍니다.
7. 계정 로그인 창을 띄우지 않도록 해당 행을 주석 처리(#)를 해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139196352-3e4d8c2c-90df-4d8b-8051-aa3b51084873.png)   
8. `nano /etc/pam.d/gdm-autologin` 파일을 열어줍니다.
9. 마찬가지로 계정 로그인 창을 띄우지 않도록 해당 행을 주석 처리(#) 해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139196534-c0d02e49-5e79-4418-b1c5-43bb292295d1.png)   
10. `reboot`를 통해 재부팅합니다.

*	우분투에서 관리자 계정으로의 접속을 처음부터 막아 놓은 이유는 보안상의 문제 때문입니다. Root는 특별한 경우에만 쓰고, 웬만하면 쓰지 않는 것이 좋습니다. 단적인 예로 Test 서버와 본 서버가 있는데 관리자 권한이 풀려 있으면 실수로 Test 서버에 테스트 해볼 것을 본 서버에 그대로 적용시켜버리는 실수를 할 수도 있습니다.

> <h3>우분투 검은 화면 이슈</h3>

우분투를 재부팅 하였는데 아래와 같은 부팅 시 검은 화면에서 멈추는 증상이 나타났습니다.
![image](https://user-images.githubusercontent.com/43658658/139256167-0e368306-6893-408a-9d34-64e5fd351285.png)   
증상을 해결하기 위해 [VM Edit] > [Display] > [Accelerate 3D graphics]를 체크 해제 하였습니다.   
![image](https://user-images.githubusercontent.com/43658658/139256184-6dced008-aebb-4b6c-93c3-1858a554a7fa.png)   
여전히 문제가 해결되지 않아서 GNU GRUB 화면으로 해결하는 방법을 시도했습니다.
VM을 부팅할 때 Shift를 눌러 GNU GRUB 화면으로 접속하여 Ubuntu에서 편집 모드로 진입했습니다.   
![image](https://user-images.githubusercontent.com/43658658/139256233-392d8a8a-178b-4193-8d26-128de9d9dce5.png)   
해당 부분에 “nomodeset”을 입력하고 우분투를 다시 재부팅한 결과   
![image](https://user-images.githubusercontent.com/43658658/139256257-e5e961b6-f6ad-4693-92d5-55ccaa9251fe.png)   
이곳에서 다시 화면이 멈추었습니다.

> <h3>VMware Tools로 그래픽 드라이버 업데이트 하기</h3>

우분투 검은 화면을 해결하기 위해 VMware Tools를 설치해서 그래픽 드라이버를 업데이트하는 방법을 시도해 보았습니다.   
설치를 하기 위해서는 일단 우분투에 접속이 되어야 하는데 접속조차 불가능한 상황이기에 더 이상 진행할 수가 없어서 새로운 가상 머신을 만들고, 우분투 데스크탑을 새로 설치했습니다.   
![image](https://user-images.githubusercontent.com/43658658/139403055-db2ed2a4-b442-4f7f-bff6-4f64e7d29400.png)   
확인 결과 이미 VMware Toolbox가 가장 최신 버전으로 설치되어 있는 것을 확인할 수 있었습니다.   
![image](https://user-images.githubusercontent.com/43658658/139403077-d9766bdc-cea6-4e59-9077-917d1f1e8941.png)   
VMware Tools 사용 가이드에 의거해 현재 우분투에는 VMware Tools가 설치되면서 VGA 드라이버가 SVGA 드라이버로 교체되었다는 것을 확인할 수 있었습니다.   
원인을 알아보기 위해 관리자 계정으로 자동 로그인 설정을 다시 수행해봤습니다. 재부팅 결과 똑같은 증상이 나타나는 것을 확인할 수 있었습니다.   
![image](https://user-images.githubusercontent.com/43658658/139403017-c3ac9e04-278e-412a-ae79-cc6473cb1a69.png)   
<관리자 계정으로 자동 로그인을 설정> 실습을 수행하지 않으면 우분투가 정상적으로 작동하고 앞으로 관리자 계정으로 자동 로그인을 설정할 경우는 없을 것 같기에 생략하고 진행하기로 했습니다.

> <h3>우분투 서버 설치</h3>

이미지 파일로 `server.iso`를 선택하고, 우분투 데스크탑 설치와 똑같은 방법으로 가상머신을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139410100-8188e587-a856-446c-81df-fa8e640c6911.png)

![image](https://user-images.githubusercontent.com/43658658/139410125-d8413ef7-daa2-4d15-a0e2-ab0318feb311.png)   
우분투 서버에서는 한글을 쓸 필요가 없으므로 언어는 영어로 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/139410144-2495b304-4c6c-446e-a770-fd9ca199a369.png)   
![image](https://user-images.githubusercontent.com/43658658/139410157-2fb38210-3f0d-4c88-b436-fe766dceb72d.png)   
IP 주소가 자동으로 할당되어 있는 것을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139410175-9fac6042-ee3a-4a8b-88fb-aa9779044ee7.png)   
프록시 서버는 클라이언트와 서버 사이에 위치해서 클라이언트가 요청한 서버의 내용을 대신 가져와서 보여주는 역할을 합니다. 프록시를 설정할 필요가 없으므로 넘겼습니다.   
![image](https://user-images.githubusercontent.com/43658658/139410193-1f52db61-1a27-4399-8c9f-87ff961c3c65.png)   
![image](https://user-images.githubusercontent.com/43658658/139410204-2c9ad87b-5cf3-4a57-b9ef-a9e942e90b86.png)   
파티션을 나누지 않고 진행했습니다.   
![image](https://user-images.githubusercontent.com/43658658/139410227-a7b735ec-29a6-4050-ac32-38891000d232.png)   
스토리지와 관련해서는 변경할 사항이 없으므로 그대로 진행했습니다.   
![image](https://user-images.githubusercontent.com/43658658/139410241-6f4cf0b2-2dfb-4781-b4c8-aa313b0d419c.png)   
![image](https://user-images.githubusercontent.com/43658658/139410246-7835bf5c-6cd4-4f88-ab55-fcffc69a9973.png)   
![image](https://user-images.githubusercontent.com/43658658/139410250-48d8056c-f312-4c6a-85cf-92eb3126c95c.png)   
추가할 프로그램은 없으므로 그냥 넘어갑니다.   

# Ch 4. 서버 구축시 알아야 할 필수 개념과 명령어

* 터미널 실행 단축키 :  `[Ctrl] + [Alt] + [T]`

> <h3>시스템 종료 명령</h3>

다양한 시스템 종료 명령이 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139000051-eca7ca9a-34b6-444b-98bb-871a01d6f51f.png)   
![image](https://user-images.githubusercontent.com/43658658/139000287-0b7778e8-c645-466e-932d-db99cc6e619a.png)   
![image](https://user-images.githubusercontent.com/43658658/139000025-f98c6443-6c7e-4a2f-9d25-7f2d5308eb04.png)   
![image](https://user-images.githubusercontent.com/43658658/139000252-74de1b6b-6f87-4dc5-abb5-39fd43c055bc.png)   

* -P or -p : 시스템 종료 옵션

`shutdown` 명령어는 다양한 옵션을 이용해 시스템 종료에 대해 다양한 기능들을 제공합니다.   
* shutdown `-P` +10     : 10분 후 종료`(P : poweroff)`
* shutdown `-r` 22:00   : 22시에 재부팅`(r : reboot)`
* shutdown `-c`         : 예약된 shutdown 취소`(c : cancel)`
* shutdown `-k` +15     : 현재 접속한 사용자에게 15분 후 종료된다는 메세지를 보내지만 실제로는 종료되지 않음.
  * 실제로 다른 가상 콘솔의 접속자에게 로그아웃을 유도하도록 하는 기능입니다.

리눅스에서는 대소문자를 명확하게 구분합니다.
관리자 권한 명령어는 앞에 `sudo`를 붙이면 됩니다.   

![image](https://user-images.githubusercontent.com/43658658/139000597-f2b5cb46-d123-4348-aef2-3f67be9e4396.png)   
일반 사용자는 `$`, root 사용자는 `#`로 나타납니다.

> <h3>재부팅</h3>

![image](https://user-images.githubusercontent.com/43658658/139001210-c88861bf-989b-4ce3-aece-07794c223aa0.png)   
![image](https://user-images.githubusercontent.com/43658658/139001370-e031c966-3f29-42c3-b19d-9fb2b5526dec.png)   
![image](https://user-images.githubusercontent.com/43658658/139001398-08e1cfcb-8577-47ff-97b4-fc3baa464064.png)   

> <h3>로그아웃</h3>

하나의 우분투 OS에 여러 명의 사용자가 동시에 접속해 있을 때 자신의 접속만을 종료하기 위해서 로그아웃을 진행합니다.   

![image](https://user-images.githubusercontent.com/43658658/139003719-5a61e835-533c-4af6-bb1f-d375bfcf3ad4.png)

CLI 환경의 경우 `exit` 또는 `logout`을 입력해서 로그아웃 할 수 있습니다.

> <h3>가상 콘솔</h3>

가상 콘솔이란 `가상의 모니터`라고 생각하면 이해하기 쉽습니다.   
우분투는 총 6개(2번~7번)의 가상 콘솔을 제공합니다. 그래서 총 6개의 모니터가 있다고 볼 수 있습니다.   
2번은 GUI, 3 ~ 7번은 CLI 형태로 제공됩니다.   
가상 콘솔을 옮겨다니는 단축키는 `[Ctrl] + [Alt] + [F1~F7]`입니다.
* `[F1]`의 경우 로그인 화면이 나타납니다. 로그인을 할 시 2번 가상 콘솔로 넘어갑니다.

`[Ctrl] + [Alt] + [F3]`로 CLI 화면을 띄워보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/139005929-55676151-bec8-44f1-8133-24ba138ef0aa.png)   
위 화면과 같이 3번째 가상 콘솔을 의미하는 `tty3`가 써져있고, 로그인을 하는 화면이 나타납니다.

우분투 생성 계정으로 로그인을 진행합니다.

> <h3>root 계정으로 접속하기</h3>

우분투는 초기에 root 계정으로의 접속을 막아놓았기 때문에 처음부터 root 계정으로 접속하려면 추가적인 설정이 필요합니다.

![image](https://user-images.githubusercontent.com/43658658/139008723-4b3ae1a2-df19-408a-bfef-4d1cd6c74295.png)

`sudo su`를 통해 관리자 계정으로 접속해서 `passwd`를 입력해 관리자 계정(root)에 대한 비밀번호를 설정합니다.   
설정이 완료되면 `exit`를 통해 일반 사용자, 다시 한 번 `exit`를 통해 로그아웃을 합니다.   

이제 root 계정으로 바로 접속할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/139009807-da7aa502-1e56-4f00-a617-9b3a4a14110f.png)

로그인을 할 때 ID는 root, 비밀번호는 설정한 비밀번호로 접속합니다.   
바로 관리자 계정으로 접속되는 것을 확인할 수 있습니다.

> <h3>가상 콘솔 사이에 메세지 확인</h3>

서로 다른 가상 콘솔 사이에 시스템 종료 예정/취소 메세지를 확인할 수 있습니다.   
예를 들어, 3번 콘솔에서 `shutdown -P +5`를 입력해서 5분 후에 `종료`하기로 했다면,   
![image](https://user-images.githubusercontent.com/43658658/139013638-4078cca7-f0fe-464d-bcf0-5e49968afd85.png)   
다른 콘솔에서 5분 후에 시스템이 `종료`된다는 메세지가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139013711-88b38f56-aded-4400-be79-66407008a4e5.png)   

마찬가지로 3번 콘솔에서 `shutdown -c`를 입력해 시스템 종료를 `취소`했다면,   
![image](https://user-images.githubusercontent.com/43658658/139013783-19521ae2-b62e-4775-af7f-fd88b0eda847.png)   
다른 콘솔에서 5분 후에 시스템 종료가 `취소`되었다는 메세지가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139013857-a2077c80-5d83-403f-94a3-3cd197438842.png)



---
[우분투 커서, 지우기 관련 단축키](http://egloos.zum.com/ranivris/v/4304292)   

