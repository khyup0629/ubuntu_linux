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

> <h3>VMnet8의 IP 주소 설정</h3>

![image](https://user-images.githubusercontent.com/43658658/139648734-7e530a2e-47d6-49c4-b0c4-b43d885e41ee.png)   
![image](https://user-images.githubusercontent.com/43658658/139648811-6ede9921-b9a1-4a6d-988a-879c2b1ec9f4.png)   
![image](https://user-images.githubusercontent.com/43658658/139648916-bbfb6416-4a4e-41e0-acf6-438ee31e0a56.png)   
![image](https://user-images.githubusercontent.com/43658658/139649042-aab721f7-220d-4896-9e53-828bfb4eab54.png)   

