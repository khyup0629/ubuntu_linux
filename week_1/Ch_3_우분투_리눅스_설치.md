본 자료는 「이것이 우분투 리눅스다 (저자:우재남)」를 참조하여 작성되었음을 알립니다.

---

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

> <h3>스냅숏</h3>

![image](https://user-images.githubusercontent.com/43658658/139653665-0bdb85dd-cf14-49db-af8a-7a28554a3f1c.png)   
![image](https://user-images.githubusercontent.com/43658658/139654671-6f612d72-481c-428f-b9aa-312ffd627cd4.png)   
스냅숏을 불러오고 싶을 때는 아래와 같이 한다.   
![image](https://user-images.githubusercontent.com/43658658/139654259-17d040bf-e93d-4813-a7f0-8424f973311a.png)   
![image](https://user-images.githubusercontent.com/43658658/139654157-65d7ceba-e1a7-4823-9875-f93897954691.png)   


