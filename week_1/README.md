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
  * inode : 유닉스 운영체제에서 사용하는 자료 구조로, 파일 시스템 내부에 파일을 유지하는 중요한 정보(소유권, 허가권, 파일 종류 등)를 담고 있습니다. 모든 파일이나 디렉토리는 1개씩 inode가 있으며, 일반적으로 전체 파일 시스템 디스크 용량의 대략 1% 정도가 inode 테이블에 할당됩니다.
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

> <h3>쿠분투 데스크탑 설치</h3>

`Client`의 역할을 하는 `쿠분투 데스크탑`을 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/139432961-995918bd-7f80-449e-b635-5e083c3cbd7b.png)   
kubuntu-20.04-desktop-amd64.iso를 이미지 파일로 넣고 가상머신을 실행합니다.   
![image](https://user-images.githubusercontent.com/43658658/139432991-1053b664-060b-40eb-9ba5-9269f36254be.png)   
![image](https://user-images.githubusercontent.com/43658658/139433025-05468f6f-1b9c-4713-a4a7-dd1de51c3cfa.png)   
![image](https://user-images.githubusercontent.com/43658658/139433033-0d896adb-abb8-4f4f-962c-55bb06a37248.png)   
![image](https://user-images.githubusercontent.com/43658658/139433049-ff3ad0a0-37fd-4f36-b361-8e3fc470afda.png)
![image](https://user-images.githubusercontent.com/43658658/139433062-d4ea8384-9d57-42d9-91c0-ee05f247835c.png)   
![image](https://user-images.githubusercontent.com/43658658/139433072-cf0d3407-588d-40d5-aed9-90f3782ae50b.png)   
이제 설치를 진행합니다. 모든 설치가 진행된 후 재부팅합니다.   
![image](https://user-images.githubusercontent.com/43658658/139433090-93e4574d-9dd2-4b6a-a92e-cb1af2c2ce8a.png)   
패키지를 설치할 때 20.04 LTS가 출시될 때의 패키지만 설치되도록 우분투 서버와 똑같이 설정합니다.

우분투 데스크탑은 GNOME이라는 GUI 환경을 이용하고, 쿠분투 데스크탑은 KDE라는 GUI 환경을 제공합니다.

> <h3>Windows 10 설치</h3>

`Windows 클라이언트`를 위한 Windows 가상머신을 설치할 것입니다. 메뉴얼의 후반부에 `Windows 클라이언트에서 리눅스에 접속하는 상황이 
발생`해서 Windows 클라이언트를 지금 단계에서 설치해놓으면 후에 실습에 있어서 편리하게 작업할 수 있습니다.

[32bit iso 파일 다운로드 사이트](https://www.microsoft.com/ko-kr/evalcenter/evaluate-windows-10-enterprise)
[Windows 10 설치 가이드 메뉴얼](https://dololak.tistory.com/119)

![image](https://user-images.githubusercontent.com/43658658/139446849-e7826571-2578-484c-85e5-e24dff389742.png)   
Windows 10 설치가 완료된 모습입니다.   
이제 `VMware Tools`를 설치합니다. VMware Tools는 가상머신에 여러 드라이버 파일을 설치해서 부드럽게 사용할 수 있도록 도와줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139447205-fc7cd158-e134-4ce5-a4b7-c3f5845daa43.png)   
가상머신을 다시 시작합니다.

# Ch 4. 서버 구축시 알아야 할 필수 개념과 명령어

* 터미널 실행 단축키 :  `[Ctrl] + [Alt] + [T]`

> <h3>시스템 종료 명령</h3>

다양한 `시스템 종료 명령`이 있습니다.   
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

> <h3>런레벨</h3>

리눅스는 시스템이 가동되는 방법을 7가지 런레벨로 나눌 수 있습니다.    
런레벨 모드를 확인하려면 `/lib/systemd/system` 디렉토리 내의 `runlevel?.target` 파일을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/139524283-d8fc4324-e91b-4c00-8b35-20ced0d3394e.png)   
* ls 명령어 : 파일 리스트, -l 옵션 : 자세한 내용(퍼미션(권한), 포함된 파일수, 소유자, 그룹, 파일크기, 수정일자, 파일이름)
* poweroff : 종료 모드
* rescue : 시스템 복구 모드
* multi-user : CLI 모드
* graphical : GUI 모드
* reboot : 재부팅 모드

`우분투 데스크탑`의 경우 런레벨 `5`로 지정되고, `우분투 서버`의 경우 런레벨 `3`으로 지정됩니다.   
init 0, 1, 2, 3, 4, 5, 6 명령으로 런레벨 모드를 변경할 수 있습니다.   
예를 들어, 우분투 데스크탑을 열면 런레벨 5번으로 지정되고, `init 0`을 명령할 경우 `종료 모드`가 되어 시스템이 종료 됩니다.

> <h3>시스템에 설정된 초기 런레벨을 변경하기</h3>

시스템에 설정된 초기 런레벨은 `/lib/systemd/system/default.target`에 연결된 링크를 통해 알 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139524528-a6950a27-baf3-4d90-a4f4-8e16ab62747f.png)   
graphical.target(런레벨 5)에 연결된 것을 확인할 수 있습니다.   
CLI 모드로 부팅되도록 하기 위해 연결 링크를 multi-user.target(런레벨 3)으로 변경합니다.   
![image](https://user-images.githubusercontent.com/43658658/139524565-2c085dde-d0f6-46a9-9d36-72cf898a0720.png)   
`reboot`로 재부팅을 하게 되면 아래와 같이 CLI 환경으로 부팅됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139524711-a0d901e9-805e-4746-a576-ae350c74152b.png)   
`startx` 명령어를 통해 GUI 환경으로 바꿀 수 있습니다.
터미널을 열고 다시 default.target에 연결된 링크를 graphical.target으로 바꿔줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139524760-e2b0d3b5-99a6-49a6-918b-39655252583d.png)   

> <h3>자동 완성 기능과 도스 키</h3>

자동 완성 : 명령을 입력할 때 모두 입력하지 않고 `[Tab]`키를 눌러 자동 완성 시켜주는 기능을 말합니다.   

![image](https://user-images.githubusercontent.com/43658658/139524976-44baef0b-7f4d-43d1-887c-825c3ae7863f.png)   
자동 완성 기능은 가능한 경우가 1가지 일 때만 완성이 됩니다. 두 가지 이상일 때는 완성되지 않고 [Tab]키를 한 번 더 누르게 되면 관련된 리스트가 나타납니다.

도스 키 : 이전에 했던 명령을 화살표 위/아래를 눌러 다시 나타나게 하는 것을 말합니다.   

`history` 명령어는 지금까지 썼던 명령어를 모두 보여줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139524893-0c7731af-e4f6-4ffe-9930-975fdc443979.png)   
`history -c`는 명령어 히스토리를 모두 삭제합니다.   
![image](https://user-images.githubusercontent.com/43658658/139524927-d6c3d401-b944-48be-ad89-fe3ad634e317.png)   

> <h3>vi 에디터 사용</h3>

`vi new.txt`를 입력합니다. vi 에디터는 해당 파일이 있을 경우 수정, 없을 경우 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139525272-4a74fcb0-f0c3-4001-9b2d-f4e43d7328fc.png)   
`i` 키를 누르고 `입력 모드`로 전환해 적절히 수정해 줄 수 있고,   
`[esc]` > `:`키를 눌러 `라인 명령 모드`로 진입할 수 있습니다. 라인 명령 모드에서는 저장(w), 종료(q), 취소(i) 등을 수행할 수 있습니다.   
`:wq!`는 저장하고 종료하라는 의미입니다.   
`:q!`는 수정된 요소를 무시하고 종료하라는 의미입니다.

처음에 `vi`로 생성하고 `:` 라인 명령 모드로 진입해서 `:w test.txt`와 같이 파일 이름을 저장하는 방법도 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139525388-133080de-d92e-426f-9113-af417cdee64b.png)

vi 편집 도중 저장하지 않고 비정상적으로 종료된 경우 아래와 같은 창이 나옵니다.   
![image](https://user-images.githubusercontent.com/43658658/139525513-69f79d5b-6cbc-4884-9495-24b5c1bfcaac.png)   
vi 편집기를 작동시키면 해당 파일에 대한 swap 파일이 생성되고 정상적으로 종료되면 swap 파일이 삭제되는 형식입니다.   
비정상적으로 종료된 파일은 `.파일이름.swp`이 그대로 남아있기 때문에 이때는 swap 파일을 지워주면 해결됩니다.   
vi 편집기를 종료하고, swap 파일을 삭제합니다.   
![image](https://user-images.githubusercontent.com/43658658/139525596-7e70f675-47b7-4f9b-8ca4-017c6b4c4626.png)   
* swap 파일은 숨김 파일이기 때문에 ls 명령어의 `-a`를 사용해서 숨김 파일을 보여줄 수 있도록 합니다.
* rm 명령어의 `-f` 옵션은 강제로 삭제하는 옵션입니다.

편집 모드로 들어가는 방법   
* i : 현재 커서의 위치부터 입력
* a : 현재 커서의 위치 다음 칸부터 입력
* o : 현재 커서의 다음 줄에 입력
* s : 현재 커서의 한 글자를 지우고 입력

명령 모드에서 유용한 키 모음   
![image](https://user-images.githubusercontent.com/43658658/139525828-c4858488-57f0-404e-839e-da331e8c71d9.png)   
![image](https://user-images.githubusercontent.com/43658658/139525839-10f0cbf7-f943-4630-a053-5690ec0792d9.png)   
![image](https://user-images.githubusercontent.com/43658658/139525848-b95e6e40-4a4b-42bc-984c-114c786fe448.png)   

라인 명령 모드에서 유용한 키 모음   
* `:set number` : 행마다 번호가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139527302-2a97e305-ffb5-4728-b3be-18de824f9df2.png)   
* `:%s/기존 문자열/새문자열` : 파일 내용 중 '기존 문자열'을 '새문자열'로 변환합니다.   
![image](https://user-images.githubusercontent.com/43658658/139527329-c33792e7-8bb0-467c-b859-f0c6ead57efe.png)

> <h3>명령어 도움말</h3>

명령어에 대한 옵션 등의 메뉴얼을 보기 위한 명령어는 `man 명령어`입니다(Manual)   
예를 들어, `man ls` 명령어를 입력하면 `ls`에 대한 사용법을 보여줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139527372-fb2464ac-0d50-4a5f-b02a-da2644191f15.png)   
vi 편집기와 같아서 `/단어`를 이용해 단어를 찾을 수 있고, 종료는 `q`입니다. 

> <h3>CD/DVD 마운트</h3>

리눅스에서는 하드디스크의 파티션, CD/DVD, USB 등을 사용하려면 지정한 위치(대게는 폴더)에 연결시키는데, 이 과정을 `마운트`라고 합니다.

![image](https://user-images.githubusercontent.com/43658658/139527994-554250b3-93b8-4cd2-94d3-c78d83e18ca8.png)   
`mount` 명령어로 현재 마운트 정보를 확인할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/139528083-3645c4e3-7336-412b-b7c2-78d1e1998e8f.png)   
`umount` 명령어로 마운트를 해제합니다(기존에 CD/DVD가 마운트되어 있을 수 있으므로)   
* /dev/sr0와 /dev/cdrom은 같은 것을 의미합니다. `ls -l /dev/cdrom`을 통해 살펴보면 /dev/cdrom이 /dev/sr0에 링크되어 있는 것을 확인할 수 있습니다.   

![image](https://user-images.githubusercontent.com/43658658/139528652-0e5165cd-4e95-4790-ba79-b5ea593f86d1.png)   
우측 상단의 CD 모양을 우클릭해서 [settings]에 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/139528452-8f7c7e9a-5741-4690-86b3-08d01e8e18dc.png)   
잠시 후에 잠깐 화면 상단에 20.04 LTS가 연결되었다는 메세지가 나옵니다.   
![image](https://user-images.githubusercontent.com/43658658/139528479-ea7fbcbc-664f-4b66-9393-08e97d99383e.png)   
다시 마운트 명령을 입력하면 위와 같이 CD/DVD 장치인 `/dev/sr0`이 `/media/bllu/Ubuntu 20.04.3 LTS amd64` 디렉토리에 마운트되어 있는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139528534-e45936b4-1098-45d3-a57e-3795eb576cd9.png)   
DVD 패키지가 들어있는 디렉토리로 이동해봅시다.   
![image](https://user-images.githubusercontent.com/43658658/139528829-164cb5a8-8d10-4395-b17c-7f25b9a5a094.png)   
casper > filesystem.squashfs 파일이 우분투 전체가 들어있는 파일입니다. 우분투를 설치하면 이 파일의 압축이 풀리면서 전체 시스템이 구성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139528873-19170ed1-d121-4805-8a01-c21db4af10c2.png)   
DVD를 사용하지 않는다면 마운트를 해제합니다.   
이때, 현재 마운트된 디렉토리에서 명령을 실행하면 아래와 같이 `target is busy`라는 오류 메세지가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139528930-d645081f-d759-466f-a9ed-4fb650eb21c7.png)   
마운트를 완전히 해제하기 위해 [settings]에서 체크를 해제합니다.   
![image](https://user-images.githubusercontent.com/43658658/139529099-9dbfbfaf-c55f-4c93-978c-0e3c340436db.png)   

> <h3>USB 마운트</h3>

Client용 가상머신(Kubuntu)를 실행합니다.   
가상머신 실행 전, Edit에서 USB 호환성을 3.1로 변경합니다(연결할 USB 표준을 확인합니다).   
![image](https://user-images.githubusercontent.com/43658658/139530397-df64e608-7f47-4a23-a1b8-49ae8de694ec.png)   
호스트 컴퓨터(Windows)에 USB를 꽂게 되면 가상 머신 창의 우측 상단에 USB 표시가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139529336-99c1a9cf-cb4a-4a54-ad0a-4fc4176a1533.png)    
[우클릭] > [Connect (Disconnect from host)]를 선택합니다.   
호스트에서 USB 연결이 해제되고, 가상머신 안에 USB를 마운트합니다.   
장치 알리미에서 해당 USB에 대한 연결을 허용해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139530049-7cb571bf-a488-4dcb-a91d-61dbb8e477c3.png)   
`mount`를 통해 확인해보면 USB 장치인 `/dev/sdb1`가 `/media/bllu/USB이름`에 마운트되어 있는 것을 볼 수 있습니다.   
이제 `/media/bllu/DE92-94AE`에 가상머신에 있는 파일을 복사해서 USB에 파일을 넣을 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139530100-a29b763e-825f-4f45-9a85-dcb3e6f6ba4b.png)   
`umount /dev/sdb1`를 통해 마운트를 해제하고, 우측 상단의 USB 아이콘에서 [Disconnected]를 클릭합니다.   

> <h3>CD/DVD, USB 우분투 서버에 마운트</h3>

앞서 진행한 CD/DVD, USB 마운트를 똑같이 진행합니다.   
하지만 mount를 입력해보면 여전히 마운트되지 않은 것을 확인할 수 있습니다.   
우분투 서버에서는 `/media` 하위에 적절한 디렉토리를 만들어주고 수동으로 마운트를 진행해야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/139531085-4824f4bb-f43b-4a92-95cb-f398fb402bf2.png)   
* mkdir로 /media의 하위 디렉토리인 /cdrom과 /usb를 만들어줍니다.
* /dev/cdrom(CD/DVD 장치)를 /media/cdrom에, /dev/sdb1(USB 장치)fmf /media/usb에 각각 마운트합니다.
* /dev/sda : 우분투를 처음 설치할 때의 최초 하드디스크
* /dev/sdb : USB 메모리

`mount`를 실행해보면 아래와 같이 CD/DVD와 USB가 마운트되었음을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139531173-c8933de5-ab82-4fcd-b430-5c3ea065aa56.png)   

![image](https://user-images.githubusercontent.com/43658658/139531567-287ad1da-8574-46c0-a774-867c9572a4be.png)   
/media/usb의 파일 리스트를 확인해보면 USB에 들어있는 파일과 같다는 것을 확인할 수 있습니다.   
마찬가지로 `cp` 명령어를 통해 우분투 서버 내의 파일을 USB에 복사해서 넣을 수도 그 반대로 할 수도 있습니다.

`umount /dev/cdrom`, `umount /dev/sdb1`으로 언마운트 시킵니다.   
앞서 진행한 바와 같이 settings와 disconnected로 확실하게 언마운트 시킵니다.

> <h3>ISO 파일 만들기</h3>

ISO 파일을 생성하는 명령어는 `genisoimage`입니다.   
해당 프로그램이 포함된 패키지가 설치되었는지 확인하기 위해 `dpkg --get-selections genisoimage` 명령을 입력합니다.   
패키지가 설치되어 있다면 `install`이 표시됩니다.
* dpkg : 패키지(프로그램) 설치 명령어

![image](https://user-images.githubusercontent.com/43658658/139532397-81a43785-dd67-4f6d-9f44-f00f0b5e266a.png)

간단히 /boot 디렉토리의 파일들을 boot.iso 파일로 생성해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/139532576-88972921-735e-406d-95c7-8fd39c914734.png)   
* -r, -J : 8글자 이상의 긴 파일 이름을 허용, 대소문자를 구분해서 인식하도록 하는 옵션입니다.
* -o : 출력 파일 이름을 지정하는 옵션입니다.

/media 하위 디렉토리로 /iso를 만들고 boot.iso 파일을 마운트합니다.   
/boot와 /media/iso의 파일 리스트가 동일함을 알 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139533102-e53bead8-53ff-4391-bc4f-7c3adcb56322.png)   
마지막으로 `umount /media/iso`로 언마운트 시킵니다.

> <h3>사용자와 그룹</h3>

리눅스는 다중 사용자 시스템입니다. 1대의 리눅스에 여러 명의 사용자가 동시에 접속할 수 있습니다.   
vi 에디터로 /etc/passwd 파일을 열어봅시다.   
![image](https://user-images.githubusercontent.com/43658658/139561291-4b267417-589c-4b28-9f07-94c1f2a61682.png)   
`사용자 이름:암호:사용자 ID:사용자가 소속된 그룹 ID:추가 정보:홈 디렉토리:기본 셸`   
* 암호는 `/etc/shadow` 파일에 저장되어 있습니다.
* 추가 정보는 '전체 이름, 사무실 호수, 직장 전화번호, 집 전화번호, 기타'로 나타내는데 생략해도 무방합니다.
* 홈 디렉토리는 사용자의 홈 디렉토리를 나타냅니다.
* 기본 셸은 로그인 시 제공되는 셸을 의미합니다.

모든 사용자는 하나 이상의 그룹에 속해 있습니다.   
/etc/group 파일을 살펴봅시다.   
![image](https://user-images.githubusercontent.com/43658658/139561346-a1b6eee4-141f-46f1-8a19-27b6ef43cf10.png)   
`그룹 이름:암호:그룹 ID:그룹에 속한 사용자 이름`
* 그룹에 속한 다른 사용자 이름 : 아무것도 표시되지 않을 수도 있습니다. 그렇다고 소속된 사용자가 없다는 의미가 아닙니다.

![image](https://user-images.githubusercontent.com/43658658/139561404-b21eea0d-ba11-4a7c-9822-7af12bac9c29.png)   
![image](https://user-images.githubusercontent.com/43658658/139561408-e586f0cb-c1c9-4fb5-a51e-e2859d004fd1.png)   
![image](https://user-images.githubusercontent.com/43658658/139561413-9c004378-cfb0-4e7c-9b4a-69d0c284edc0.png)

> <h3>사용자, 그룹 관리 연습</h3>

사용자 이름 : user1 / 비밀번호 : 1 추가   
![image](https://user-images.githubusercontent.com/43658658/139562451-4e68b6da-6048-4c33-ad80-2698b6304c36.png)   
![image](https://user-images.githubusercontent.com/43658658/139562568-a20f4355-cff2-4794-8f5b-c60d3568a41f.png)   
* tail : 마지막 10행을 보여주는 명령어, `tail -5` 형식으로 옵션을 주면 마지막 5행을 보여줍니다.

그룹을 살펴보면 사용자 이름과 동일한 그룹 이름으로 그룹이 생성되어 있음을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139562584-913ad5dd-d7e0-4253-bfa4-c515ffc40c16.png)   

하지만 사용자가 많아질 수록 사용자 이름과 그룹 이름이 같으면 관리하기가 힘들어집니다.   
`userdel user1`을 통해 user1을 지웁니다(그룹도 같이 지워집니다).   

`groupadd ubuntuGroup` 명령어로 그룹을 먼저 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139562685-7320b8d1-e0cb-47fb-a094-62d2ad58f2f4.png)   
`adduser --gid 그룹ID 사용자이름`의 형식으로 명령어를 입력하면 그룹ID에 사용자이름이 소속됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139562755-aa027ef7-bcbf-42bd-ab3d-61b7d3137cf9.png)   

![image](https://user-images.githubusercontent.com/43658658/139562894-3518f7ea-3384-4b2f-b529-a107f1062d17.png)   
새로운 사용자를 생성하면 기본적으로 `/home/사용자이름`으로 디렉토리가 생성되고, `/etc/skel`의 모든 내용을 복사합니다.   
사용자를 생성할 때 특정 파일을 배포하고 싶다면 `/etc/skel`에 해당 파일을 넣어두면 됩니다.

사용자를 변경하려면 `su - 사용자이름`을 입력해서 변경해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139567188-0f5e3391-4aeb-4083-a9cc-7e44f18b1315.png)

> <h3>파일, 디렉터리 소유권과 허가권</h3>

리눅스는 각각의 파일과 디렉토리마다 소유권과 허가권이라는 속성이 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139563034-3232af1c-4169-414e-ad65-3efbe9803106.png)   
* touch : 크기가 0인 새 파일을 생성합니다.

![image](https://user-images.githubusercontent.com/43658658/139563078-f5a9f5fa-00b2-42da-a33f-fb3ae7513649.png)

파일 유형   
* - : 일반 파일
* d : 디렉토리
* b : 블록 디바이스(하드디스크, CD/DVD)
* c : 문자 디바이스(마우스, 키보드, 프린터)
* l : 링크 파일. 실제 파일을 다른 곳에 있다.

파일 허가권   
* 3개씩 끊어서 인식합니다(소유자(User) / 그룹(Group) / 그 외 사용자(Other)).   
![image](https://user-images.githubusercontent.com/43658658/139563199-ae7ddcf4-52bf-4398-9a2c-59854f6403f3.png)   
* 3개의 자리는 rwx로 고정되어 있습니다.
* `r` : read(읽기), `w` : write(쓰기), `x` : execute(실행)
* `rw-` : 읽거나 쓸 수 있지만 실행은 불가. `rwx` : 읽고 쓰고 실행 모두 가능
* `2진수`로 나타낼 수 있습니다(`-` : 0, `r/w/x` : 1).   
* `rwx` : 4 + 2 + 1 = 7, `r--` : 4 + 0 + 0 = 4, `rw-` : 4 + 2 + 0 = 6
* 디렉토리(d)는 해당 디렉토리로 이동하려면 반드시 실행 권한(x)이 있어야 합니다.
* 허가권은 `chmod 2진수 파일이름`으로 변경 가능합니다.

`test.txt` 파일을 하나 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139566670-5a141860-ec29-41aa-a92e-6a80dda0a7aa.png)   
`test.txt` 파일과 관련된 허가권, 소유권은 아래와 같습니다.   
![image](https://user-images.githubusercontent.com/43658658/139566689-5450db66-2058-4182-bd79-a02da7c5878c.png)   
`./test`를 통해 test.txt 파일을 실행하면 `허가 거부`가 나옵니다. 이는 허가권이 `rw-`이기 때문입니다. 
![image](https://user-images.githubusercontent.com/43658658/139566699-fb36d721-4263-4a96-8999-4f7b57817540.png)   
실행할 수 있도록 rwxr-xr-x(755)로 파일 허가를 바꿔줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139566792-e5c2d5cf-caf6-4254-aa5b-0934ba7b000e.png)   
`ls -l test.txt` 파일을 살펴보면 아래와 같이 `rwxr-xr-x`로 변경된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139566826-52cb8aed-c6ac-4242-bc5b-b49e08ed6bca.png)   
다시 `./test`로 파일을 실행해보면 정상적으로 실행이 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139566850-65a5b148-f81f-4db2-adc6-67afb0737c34.png)   
* 명령어가 없으면 오류가 발생하고, 명령어가 있으면 해당 명령어를 실행합니다.

파일 소유권   
* 파일을 소유한 사용자와 그룹을 의미합니다.
* `chown 새로운사용자이름(.새로운그룹이름)`, `chgrp 새로운그룹이름`의 형식 파일의 소유권자를 변경할 수 있습니다.
* `chown user1 sample.txt` : 파일의 소유자를 user1으로 바꿉니다.   
![image](https://user-images.githubusercontent.com/43658658/139563499-a6c38605-9b60-40af-95e5-fc101808c84c.png)   
* `chown user1.ubuntuGroup sample.txt` : 파일의 소유자를 user1, 그룹 ubuntuGroup으로 변경합니다.   
![image](https://user-images.githubusercontent.com/43658658/139563534-5e5b1909-90c9-4fe7-a93f-9b7d53d61406.png)   
* `chgrp ubuntuGroup sample.txt` : 그룹만 ubuntuGroup으로 바꿉니다.
![image](https://user-images.githubusercontent.com/43658658/139563561-839aa8de-a3e5-45e8-b5fd-7cc3c1cfe602.png)   

> <h3>하드 링크와 심볼릭 링크</h3>

linktest 디렉토리 생성 후 디렉토리 내에 `basefile.txt`라는 파일을 생성하고, `cat 파일이름` 명령어를 통해 내용을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/139607343-a7b56812-a74f-4f54-b7d1-02855b16c8a0.png)   
![image](https://user-images.githubusercontent.com/43658658/139607366-0d0dc844-d193-46ce-af11-b36433de9bdd.png)    

`ln 링크원본파일 링크파일`은 링크원본파일로 하는 링크파일을 `하드링크`로 생성합니다. `-s` 옵션을 이용하게 되면, `심볼릭 링크(소프트링크)`를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139607727-d53ec192-6a93-43ba-8eca-ab507320d896.png)   
두 링크 파일의 내용은 같습니다. 하지만 inode의 번호가 하드 링크는 원본 inode 번호와 같은 반면, 심볼릭 링크는 다른 것을 볼 수 있습니다.   
이렇듯 하드 링크는 Data 블록 내에 원본 파일 데이터를 사용하기 때문에 inode도 같고, 용량도 같은 것을 볼 수 있습니다.   
심볼릭 링크는 별도의 원본 파일 포인터를 갖기 때문에 inode도 다르고, 용량이 좀 더 큰 것을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139607997-2a01b871-87fc-437c-a6e2-c345e9e0e39d.png)   
* inode : 유닉스 운영체제에서 사용하는 자료 구조로, 파일 시스템 내부에 파일을 유지하는 중요한 정보(소유권, 허가권, 파일 종류 등)를 담고 있습니다. 모든 파일이나 디렉토리는 1개씩 inode가 있으며, 일반적으로 전체 파일 시스템 디스크 용량의 대략 1% 정도가 inode 테이블에 할당됩니다.
* `-i` : 파일 리스트 맨 앞에 inode 번호를 붙여줍니다.

원본 파일 이동 후 하드 링크와 심볼릭 링크 비교   
![image](https://user-images.githubusercontent.com/43658658/139608214-2c86018d-0693-4c13-8d63-753877823dd0.png)   
원본 파일을 이동시킨 후 하드 링크와 심볼릭 링크를 비교하면 하드 링크는 여전히 링크되어 잘 나타나지만, 심볼릭 링크는 링크가 끊어진 것을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139608865-70b442ae-b90d-4436-8aaf-fcb5507d92d7.png)   
다시 원본 파일의 위치를 원상 복구한 후 심볼릭 링크를 보면 다시 연결된 것을 확인할 수 있습니다.

> <h3>dpkg와 apt</h3>

dpkg와 apt는 우분투에서 패키지를 설치하기 위해 사용되는 명령어입니다.   
둘의 차이점은 `dpkg`는 `의존성`이 있는 패키지의 경우 설치가 `불가능`하고, `apt`는 `가능`하다는 점입니다.   

예를 들어, Firefox의 경우 X 윈도가 설치되어 있어야 가동됩니다.   
X 윈도가 설치되지 않은 상태에서 Firefox를 `dpkg`로 설치하게 되면 의존성 문제 때문에 설치가 되지 않습니다.   
하지만 `apt`로 설치하게 되면, 의존성이 있는 다른 패키지를 자동으로 먼저 설치해줍니다.

패키지 파일은 `.deb` 확장자로 나타납니다.
* i386 : 인텔 또는 AMD 계열의 32비트 CPU -> 구형 CPU
* amd64 : 인텔 또는 AMD 계열의 64비트 CPU -> 보편적 CPU
* all : 모든 CPU 설치 가능

> dpkg 명령 실습

![image](https://user-images.githubusercontent.com/43658658/139611024-dd2df4db-0a1d-4f85-add9-7a8ac78f6228.png)   
![image](https://user-images.githubusercontent.com/43658658/139611044-60204aa3-5be1-4c72-b38a-c34808760b15.png)   
![image](https://user-images.githubusercontent.com/43658658/139611058-abbdd9e4-7277-49b8-b82a-f01b351ac260.png)   

실습을 위해 X 윈도 전용 계산기인 galculator를 설치해봅시다.   
먼저 패키지 파일을 다운 받아줍니다.   
![image](https://user-images.githubusercontent.com/43658658/139609546-253f8b12-c501-4d9f-b8ed-b01c0b658b0a.png)   

웹 사이트에서 다운로드 된 파일은 우분투 현재 계정의 `/다운로드`로 들어갑니다.   
해당 디렉토리로 이동해서 `dpkg --info .[Tab]` 명령어를 통해 해당 패키지 파일에 대한 정보를 알 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139610655-11ccf874-a68b-4515-9232-bb3d44306686.png)

`galculator`를 설치하는 과정에서 `의존성 문제`가 발생하였습니다.   
![image](https://user-images.githubusercontent.com/43658658/139610776-ff9d297b-0eec-481c-94e4-832954e0bf10.png)

`dpkg -l galculator`를 입력해보면 어느 정도는 설치는 된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139610962-59e133c4-1656-4936-93ad-49f361263080.png)

> apt

dpkg와 다르게 `apt`는 의존성 문제를 완벽하게 해결해줍니다.   
`apt`는 일단 패키지 파일(.deb)을 인터넷에서 미리 직접 다운로드 받지 않고도 알아서 우분투 저장소에 있는 패키지 파일을 다운로드하고,   
의존성이 있는 다른 패키지 파일(.deb)까지 알아서 다운로드 받아 설치해줍니다.

우분투 저장소의 URL은 `/etc/apt/sources.list`에 저장되어 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/139612137-1f424d34-c5dd-47eb-9af1-990837481a5b.png)   

![image](https://user-images.githubusercontent.com/43658658/139612198-538f260a-35c7-46b7-a2b5-72509076f294.png)   
* `-y` : 패키지를 설치할 때 `Y/N`라고 묻는 부분을 무조건 YES로 간주하고 넘어갑니다.

![image](https://user-images.githubusercontent.com/43658658/139612285-1b48b2c1-e7fc-4e28-bf4f-4fcc29402a6d.png)   
![image](https://user-images.githubusercontent.com/43658658/139612334-3f6ca413-b06f-496f-a8ad-9c2efc409a03.png)   
![image](https://user-images.githubusercontent.com/43658658/139612350-49dfa54b-fb48-42c7-9258-0b8ac8a06f5e.png)

> apt 명령 실습

과거에 dpkg로 galculator 설치에 실패한 기록이 있어 단순히 `dpkg -r galculator`를 하게 되면, 다시 설치할 때 정상적으로 진행되지 않을 수 있습니다.   

![image](https://user-images.githubusercontent.com/43658658/139612730-ec8c7f21-65a5-42ea-8c9c-0add67753cbf.png)   
* galculator 관련 파일을 임시 폴더로 이동시키고, 패키지를 강제로 제거합니다.

galculator 패키지가 설치되어 있는지 먼저 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/139612831-8829e994-dd2d-4327-973e-0a3378a84521.png)

`apt-cache show 패키지이름`, `apt-cache depends 패키지이름`으로 패키지 정보, 의존성을 미리 확인해봅시다.   
![image](https://user-images.githubusercontent.com/43658658/139612979-9aff65f9-59c8-40e3-af9d-a9a11e9554db.png)   
![image](https://user-images.githubusercontent.com/43658658/139613056-870f6007-c0d5-4a5a-b5dc-30e5e913fb90.png)








---
[우분투 커서, 지우기 관련 단축키](http://egloos.zum.com/ranivris/v/4304292)   

