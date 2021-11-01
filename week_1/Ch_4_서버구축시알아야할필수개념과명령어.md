본 자료는 「이것이 우분투 리눅스다 (저자:우재남)」를 참조하여 작성되었음을 알립니다.

---

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

패키지를 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/139620507-43f9654c-65cd-416b-97cc-8d453781026e.png)   

> apt install 작동 원리

![image](https://user-images.githubusercontent.com/43658658/139620662-af35a48a-70a4-4c27-a38a-9b6e1aed9d8a.png)   
1. `apt install 패키지이름` 입력
2. `/etc/apt/sources.list` 파일에서 우분투 저장소 URL 주소를 확인합니다.
3. 설치에 필요한 패키지 파일을 저장소에 요청
4. 패키지 파일 다운로드
5. 의존성 패키지가 있는 경우 `y/n`를 묻습니다.
6. `y`를 눌러 의존성 패키지 파일을 저장소에 요청
7. 의존성 패키지 파일 다운로드

* 우분투 저장소 미러 사이트 : 우분투 저장소는 한 곳만 있는 것이 아니라 동일한 저장소로 대학, 연구소, 기업체 등에서 자발적으로 참여해 구축하고 있다.

> <h3>파일 압축과 묶기</h3>

파일을 압축할 때 사용하는 명령어는 `tar xvfJ 파일이름.tar.xz` 또는 `tar xvfj 파일이름.tar.bz2`

`xz`, `bz2` : 압축 파일 확장자명
`tar` : 파일을 묶어주는 명령어
`xvfj` : 묶인 파일을 풀어주고(x), 푸는 과정을 보여주며(v), 묶음 파일 이름 지정(f-필수), xz 압축 파일 해제(J), bz2 압축 파일 해제(j)

> <h3>파일 위치 검색</h3>

`find`는 파일의 위치를 검색할 때 이용합니다.

* `find /etc -name *.conf` : /etc 디렉토리 하위에서 확장명이 `*.conf`인 파일 검색   
![image](https://user-images.githubusercontent.com/43658658/139622907-b11f503d-9136-42c7-9bd4-a5049ed34d18.png)   
* `find /home -user ubuntu` : /home 디렉토리 하위에서 소유자가 ubuntu인 파일 검색   
![image](https://user-images.githubusercontent.com/43658658/139622977-d1694b96-15c3-4119-b9d5-a53ec146c8ea.png)   
* `find ~ -perm 644` : 현재 사용자의 홈 디렉토리 하위에서 허가권이 644인 파일 검색   
![image](https://user-images.githubusercontent.com/43658658/139623025-23c06b8e-3cfe-4325-ba44-f247d665144b.png)   
* `find /usr/bin -size +10k -size -100k` : /usr/bin 디렉토리 하위에서 파일 크기가 10KB~100KB인 파일 검색   
![image](https://user-images.githubusercontent.com/43658658/139623119-7e4b9302-3f53-4652-b27a-11bc045db399.png)   
* `find ~ -size 0k -exec ls -l {} \;` : 홈 디렉토리 하위에서 파일 크기가 0인 파일 목록들을 상세히 출력    
![image](https://user-images.githubusercontent.com/43658658/139623248-c0313539-719b-4d8f-82bc-f2b963687380.png)   
* `find /home -name *.swp -exec rm {} \;` : 홈 디렉토리 하위에서 확장자가 .swp인 파일들을 모두 제거
* `-exec` : 외부 명령 제거

![image](https://user-images.githubusercontent.com/43658658/139623830-3ad8d4b9-8842-4a91-9ae1-d9ce919501f8.png)   
* `which 실행파일이름` : 디렉토리만 검색
* `whereis 실행파일이름` : 실행파일, 소스, man 파일까지 검색
* `locate 파일이름` : locate 명령어를 실행하려면, `mlocate` 패키지 설치가 선행되어야 합니다. 모든 파일이름을 찾아줍니다.

> <h3>cron과 at</h3>

`cron` : 주기적으로 반복되는 일을 자동으로 실행할 수 있도록 시스템 작업을 예약해놓는 것
`/etc/crontab` 파일에 스크립트로 작성합니다.

cron과 관련 서비스인 cron 프로그램이 동작하고 있는지 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/139624579-6875a723-7e27-4752-a7c1-bec6d2895c26.png)   
* systemctl : 서비스의 시작, 중지, 상태 확인 등을 하는 명령어.

`sudo vi /etc/crontab`으로 들어가 아래와 같이 수정합니다.   
![image](https://user-images.githubusercontent.com/43658658/139625157-2f994b1d-bdd4-4d6c-9e2d-efc67e95bc09.png)   
![image](https://user-images.githubusercontent.com/43658658/139625294-5d7df0a5-b884-4986-a577-3c9ac6d8c51c.png)   
* `분  시 일 월 요일  사용자 실행명령` : `*`는 주기적을 의미

`myBackup.sh` 파일에 명령을 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/139625502-44e22996-4d9b-47f8-82ce-c9dfce495327.png)   
* 현재 날짜를 추출해서 /backup 디렉토리에 "backup-현재날짜.tar.xz" 파일이름으로 전체 백업 파일을 생성하라는 의미.

백업용 디렉토리를 생성하고 `cron`을 재시작합니다.   
![image](https://user-images.githubusercontent.com/43658658/139625726-6d474b71-f66c-4fa4-a81c-60cd0a5371fc.png)

`at` : 일회성 예약, `apt -y install rdate at`으로 관련 패키지를 설치합니다.

내일 새벽 4시에 시스템을 최신 패키지로 업데이트하고 완료되면 재부팅되도록 설정해봅시다.   
![image](https://user-images.githubusercontent.com/43658658/139626878-31f07a57-e625-447a-9e3b-1d113634499b.png)   
* `[ctrl] + [d]`를 통해 at을 빠져 나올 수 있습니다.
* `at -l`을 통해 리스트를 확인할 수 있습니다. 리스트의 맨 앞 번호가 at의 작업번호입니다.
* `atrm 작업번호`를 통해 at 리스트의 작업을 지울 수 있습니다.

> <h3>네트워크 관련 명령어</h3>

* nm-connection-editor : 네트워크 설정과 관련해서 GUI 형식으로 제공
* nmtui : 네트워크 설정과 관련해 TUI 형식으로 제공
  - 자동 IP 주소, 고정 IP 주소 사용 결정
  - IP 주소, 서브넷 마스크, 게이트웨이 정보 입력
  - DNS 정보 입력
  - 네트워크 카드 드라이버 설정
  - 네트워크 장치(ens32 or ens33) 설정

* systemctl start/stop/restart/status networking : 네트워크 설정 변경 후 변경된 내용을 적용시키는 명령어. status 옵션은 현재 작동 or 정지 상태 표시.
* ifconfig 장치이름 : 해당 장치의 IP 주소와 관련 정보를 출력
* nslookup : DNS 서버의 작동을 테스트하는 명령어
* ping IP주소 or ping URL : 컴퓨터가 네트워크 상에서 응답하는지 테스트하는 명령어.

`nm-connections-editor`를 열면 아래와 같이 나타난다.   
![image](https://user-images.githubusercontent.com/43658658/139629477-9b4d2eed-52f1-4ebc-ac9b-1dd3c05e94d6.png)   
`cat /etc/NetworkManager/system-connections/유선[Tab]` 명령으로 설정 파일의 [ipv4]를 확인합니다. 이곳에서 파일을 편집하고 재부팅해도 네트워크가 적용됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139630811-e5a2395a-465a-43b6-b829-c0db4dc1dfa6.png)   
IP 주소, 넷마스크, 게이트웨이   
DNS 주소   
수동 : 수동으로 설정했다는 의미

* 공인된 DNS 서버 : 통신사에서 제공하는 DNS 서버
  - 구글 : 8.8.8.8, 8.8.4.4
  - KT : 168.126.63.1, 168.126.63.2
  - SKT : 219.250.36.130, 210.220.163.82
  - LG U+ : 164.126.101.2, 203.248.252.2

> (실습)DNS 서버에 문제가 생겼을 경우 해결해보자.

임의로 DNS 서버를 바꿔보겠습니다. `/etc/resolv.conf` 파일을 엽니다.   
![image](https://user-images.githubusercontent.com/43658658/139632291-bc3f98fe-287a-4436-a057-8a2da0b0c7b3.png)
* `127.0.0.xx` : `/etc/NetworkManager/system-connections/유선[Tab]` 또는 `/etc/netplan/*.yaml` 파일에 설정된 DNS 서버를 사용한다는 의미
* 이것을 바꾸면 `www.naver.com`를 검색했을 때 정상적으로 접속이 되지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/139632715-e09242e2-0251-43b0-a87b-bc32ae777a7e.png)

`nslookup`을 통해 DNS의 문제인지 확인해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/139632845-d2274b98-f3d8-4303-9da3-f292983b2f2e.png)
* `server` : 현재 DNS 서버 IP 주소를 나타냅니다.
* 도메인 주소를 입력해보면 접속이 되지 않는 것을 확인할 수 있습니다.

`server 새로운DNS서버IP주소`로 DNS 주소를 바꿔보겠습니다.   
공인 DNS 주소로 바꿔서 도메인 주소로 테스트 해보겠습니다.
![image](https://user-images.githubusercontent.com/43658658/139633056-6fae1fbf-9f9b-44b6-9d92-ab071a6857c9.png)

접속이 되는 것을 확인할 수 있습니다.   

이와 같이 `nslookup` 명령어로 DNS 서버 주소를 변경해 네트워크가 제대로 응답한다면, `/etc/resolv.conf` 파일에 설정된 DNS 서버에 문제가 있는 것입니다.   
`exit`으로 nslookup 명령을 종료합니다.

> (실습)CLI 환경에서 네트워크 환경 살펴보기
 
`cat /etc/netplan/00-installer-config.yaml`로 접속해서 우분투 서버의 네트워크 환경을 확인해보자.   
![image](https://user-images.githubusercontent.com/43658658/139634747-88db5c3c-392d-4a23-905c-b126783baf60.png)   
* ens33 : 네트워크 장치 이름
  * dhcp4 : 자동 IP 사용 여부 지정.
  * addresses [IP 주소 / 넷마스크] : 고정 IP 주소, 넷마스크(24 = 255.255.255.0)
  * gateway4 : IP 주소 : 게이트웨이 장치의 주소를 지정
  * nameservers : DNS 서버의 주소를 지정.
    - addresses: [IP 주소] : DNS 서버의 IP 주소를 지정.

`sudo vi /etc/netplan/00-installer-config.yaml`로 파일을 편집해보자.   
![image](https://user-images.githubusercontent.com/43658658/139635177-2cdfc7d9-2f9a-4d15-9671-dcb1cfff02d5.png)   
재부팅한 후 로그인해서 `ifconfig ens33`과 `cat /etc/resolv.conf` 명령으로 변경된 내용을 확인해보자.   
![image](https://user-images.githubusercontent.com/43658658/139635459-53f28a9d-dca9-4793-ae4b-750558e22371.png)   
`/etc/resolv.conf` 파일은 여전히 `127.0.0.xx`입니다. `/etc/netplan/*.yaml`에 적용된 DNS 서버를 사용한다는 의미입니다.   
실제로 작동하는 현재 DNS 서버는 `/run/systemd/resolve/resolv.conf`파일로 확인 가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/139635798-3f1b04e9-db6b-4a01-8186-33659e111c1c.png)

> <h3>프로세스</h3>

프로세스 : '하드디스크에 저장된 실행 코드(프로그램)가 메모리에 로딩되어 활성화된 것`   
* 포그라운드 프로세스 : 프로그램을 실행 했을 때, 화면에 나타나 사용자와 상호 작용하는 프로세스
* 백그라운드 프로세스 : 화면에 보이지 않고 뒤에서 실행되는 프로세스
* 프로세스 번호 : 메모리에 활성화된 프로세스를 구분하는 고유 번호
* 작업 번호 : 백그라운드 프로세스의 순차 번호
* 부모 프로세스와 자식 프로세스 : 모든 프로세스는 혼자서 독립적으로 실행되는 것이 아니라 부모 프로세스의 하위에 종속되어 실행.
  * 예를 들어, Firefox는 X 윈도 프로세스가 구동된 상태에서 실행되어야 하므로 X 윈도는 Firefox의 부모 프로세스, Firefox는 X 윈도의 자식 프로세스.
  * 부모 프로세스를 종료하면 자식 프로세스도 종료.

`yes > /dev/null`을 이용해 y글자를 화면에 무한 출력하는 무한 루프 프로세스를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/139640118-ccdce9b0-1821-4e02-a011-98cec0909597.png)

바탕화면에서 새로운 터미널을 열고, `ps -ef | grep 프로세스이름`를 통해 yes 프로세스를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/139640309-d56a1e34-3e09-431d-977c-a316509d76d7.png)   
* bllu : 소유주 / 8072 : 프로세스 번호 / 2121 : 부모 프로세스 번호

`kill -9 프로세스번호`로 프로세스를 종료시킵니다.   
![image](https://user-images.githubusercontent.com/43658658/139640904-a17bb91e-0ed3-4d22-9b57-ea4d10934abd.png)   
![image](https://user-images.githubusercontent.com/43658658/139640872-9d2fc715-4736-4036-bec6-c720b5614257.png)   
* `[Ctrl]+[C]` : 현재 작동중인 포그라운드 프로세스를 종료

![image](https://user-images.githubusercontent.com/43658658/139641291-5666599c-57a2-40b7-a2b1-2013032c13e5.png)   
* `[Ctrl] + [Z]` : yes > /dev/null
* `bg` : 현재 동작하는 프로세스를 백그라운드 프로세스로 만듭니다.
* `jobs` : 현재 동작하는 백그라운드 프로세스를 보여줍니다.
* `fg` : 프로세스를 포그라운드 프로세스로 만듭니다.

처음부터 백그라운드 프로세스로 만드는 방법   
![image](https://user-images.githubusercontent.com/43658658/139643918-3402838a-aa78-48f5-8900-834fe4d57972.png)   
`fg 1`으로 포그라운드로 당겨오면 vi 편집기가 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139644070-e6700680-01c4-4d29-b747-a4935ab8c254.png)

> <h3>데몬, 서비스, 소켓</h3>

서비스(=데몬(daemon)) : 서버 프로세스. 백그라운드 프로세스의 일종. 웹 서버 데몬, 네임 서버 데몬, DB 서버 데몬 등의 프로세스 지칭. 평상 시에 늘 작동
서비스 실행 스크립트 파일 장소 : `/lib/systemd/system/서비스이름.service`

`systemd`라는 서비스 매니저 프로그램으로 작동시키거나 관리한다.

`systemctl` 명령어로 시스템을 구동합니다(ex, `systemctl restart httpd`).   
* `systemctl start/stop/restart 서비스이름` : 서비스 시작/중지/재시작
* `systemctl status 서비스이름` : 서비스 상태 확인
* `systemctl enable/disable 서비스이름` : 서비스 사용/사용 안 함 설정

소켓 : 필요할 때만 작동하는 서버 프로세스.   
소켓 스크립트 파일 장소 : `/lib/systemd/system/소켓이름.socket`   
![image](https://user-images.githubusercontent.com/43658658/139666036-83941afc-21dd-4c01-92d4-590c3b7c4821.png)

> <h3>(실습)GRUB 부트로더 변경</h3>

![image](https://user-images.githubusercontent.com/43658658/139667056-f360cb86-1be3-4018-a2aa-6c03c38fc490.png)   
* #GRUB_TIMEOUT_STYLE=hidden : 주석처리(#) : GRUB 화면을 보여준다.
* GRUB_TIMEOUT=20 : 20초 동안 부팅 메뉴가 나옴.
* GRUB_DISTRIBUTOR='THIS IS LINUX' : "THIS IS LINUX"가 메뉴에 나타남.

변경한 내용 적용   
![image](https://user-images.githubusercontent.com/43658658/139666997-c8208105-b44b-4e41-97df-1149cbee15df.png)

재부팅하면 초기에 GRUB 화면이 20초 동안 대기한다.   
![image](https://user-images.githubusercontent.com/43658658/139667194-611b4a1e-86d3-4d9d-a823-8eb7c04e0116.png)

> <h3>(실습)GRUB에 비밀번호를 설정</h3>
  
![image](https://user-images.githubusercontent.com/43658658/139668088-734b9207-2419-4f76-aae1-e3d1d9db4749.png)   
* set superusers="grubuser" : 새로운 GRUB 사용자 이름은 grubuser
* password grubuser 1234 : 비밀번호는 1234

![image](https://user-images.githubusercontent.com/43658658/139668334-948dec8b-938d-4945-b96f-d132cf7d395f.png)
편집을 위해 `[E]`를 누르면 사용자와 비밀번호를 입력하는 창이 나옵니다.
![image](https://user-images.githubusercontent.com/43658658/139668520-c1b8a2f1-8ad1-45f8-900e-f3eae62b43c2.png)   
`[Ctrl] + [X]`를 눌러 부팅합니다.

> <h3>모듈</h3>

모듈 : 별도로 보관했다가 필요할 때마다 호출하여 사용되는 코드

---
[우분투 커서, 지우기 관련 단축키](http://egloos.zum.com/ranivris/v/4304292)   

