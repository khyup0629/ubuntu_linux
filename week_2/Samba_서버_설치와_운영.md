# Samba 서버 설치와 운영

Samba : 서로 다른 OS 사이에 자원을 공유할 수 있도록 하는 서비스

<Samba 서버의 역할>   
* 자신의 자원을 사용할 사용자를 추가
* 자원 공유

<Samba 클라이언트의 역할>   
* Samba 클라이언트 패키지 설치
* `smbclient` 명령으로 Samba 서버가 제공하는 자원 확인
* `smbmount` 명령으로 samba 서버가 제공하는 공유 폴더 마운트

## 리눅스에서 윈도우 자원 공유

Samba 서버 : 윈도우   
Samba 클라이언트 : 리눅스   

> <h3>윈도우 폴더 공유</h3>

![image](https://user-images.githubusercontent.com/43658658/140269520-2d2e056a-9584-46e3-b015-ccda362f610c.png)   
`C:\smbShare` 폴더를 만들고 [마우스 우클릭] > [속성]을 클릭합니다.   
![image](https://user-images.githubusercontent.com/43658658/140269693-bfee245d-16d7-473a-a958-23097c99ce4c.png)   
![image](https://user-images.githubusercontent.com/43658658/140269797-d765f7c8-58e3-4351-a3f0-33cd79c5d9ea.png)   
`smbShare` 폴더를 공유합니다.   
![image](https://user-images.githubusercontent.com/43658658/140269888-d2e493fe-4bef-430e-9c23-985130695bf5.png)   
폴더 내에 적당한 파일을 만듭니다.

> <h3>윈도우에 리눅스 사용자 추가</h3>

![image](https://user-images.githubusercontent.com/43658658/140270332-ffed05ef-28ae-4046-a741-080278b608ac.png)   
파워쉘을 관리자 권한으로 실행한 후 `사용자: root/비밀번호: 1234` 를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/140270495-080ea8bb-ff61-4956-b62e-3ded82638ffd.png)   
계정 생성 확인   
![image](https://user-images.githubusercontent.com/43658658/140270564-919ece0a-10f4-4dca-9514-4306005abdd3.png)   
윈도우의 IP 주소를 확인합니다.

> <h3>윈도우에서 공유한 폴더를 리눅스에서 사용하기</h3>

![image](https://user-images.githubusercontent.com/43658658/140276825-14953563-c449-4b96-859c-6ebc700aa4d9.png)   
리눅스에 samba 서버 패키지를 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/140284264-3ad01c17-896f-4980-8155-9443428d00fb.png)   
`smbclient` 명령어로 공유한 폴더를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/140285036-81ed187d-9c37-4eb2-b15e-1b68e319da43.png)   
![image](https://user-images.githubusercontent.com/43658658/140285122-d551bea0-69c2-4916-879d-e6c810a2b0de.png)   
마운트 할 디렉토리를 만들고 윈도우의 공유 폴더에 마운트 시킵니다.   
* -t cifs : 윈도우 공유 폴더를 마운트할 때

마운트 된 폴더에 접속해서 파일 리스트를 확인하면 윈도우 공유 폴더 리스트와 같음을 볼 수 있습니다.   

## 윈도우에서 리눅스 자원 공유

Samba 서버 : 리눅스   
Samba 클라이언트 : 윈도우   

> <h3>리눅스 디렉토리 공유</h3>

![image](https://user-images.githubusercontent.com/43658658/140286626-35985e68-e965-4fbe-9406-3787bcf33589.png)   
`samba 서버` 패키지를 설치해서 리눅스를 samba 서버로 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/140287005-9edecde9-ecb5-4b32-a53b-52897f2d93d5.png)   
![image](https://user-images.githubusercontent.com/43658658/140287057-c34261a8-153e-4fc9-84f3-2719dcf98e11.png)   
`sambaGroup`을 생성하고, `/share` 그룹을 `sambaGroup`으로 바꿔줍니다. 허가는 소유권자와 그룹 사용자 외 다른 사람들은 접근하지 못하게 합니다(770).   
![image](https://user-images.githubusercontent.com/43658658/140287096-c3ed8782-9375-43eb-a2a9-fe9a08524773.png)   
bllu 계정의 두 번째 그룹을 sambaGroup으로 설정합니다.   
* `usermod -G 그룹 사용자` : 사용자 계정의 두 번째 그룹을 추가적으로 설정 하고 싶을 때   

![image](https://user-images.githubusercontent.com/43658658/140287177-e0a2efe6-e088-4198-8c27-3050dc5a617e.png)   
패스워드 지정

`/etc/samba/smb.conf` 설정 파일을 다음과 같이 수정합니다.   
![image](https://user-images.githubusercontent.com/43658658/140288710-7e8ecd32-3673-477f-be1e-cfc7347584dd.png)   
* workgroup = WORKGROUP : 윈도우의 작업 그룹을 써줍니다.
* [제어판] > [시스템 및 보안] > [시스템] > [시스템 속성] > [컴퓨터 이름]에서 윈도우의 작업 그룹을 볼 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/140291051-082fb8b6-7b8c-4a07-940f-49c5172d1076.png)   
* unix charset = UTF-8 : 문자 인코딩
* map to guest = Bad User : 인증 없이 접속 허용

![image](https://user-images.githubusercontent.com/43658658/140289502-686b0a07-0ff2-4e9d-9a76-437b12a26356.png)   
* path = /share : 공유할 폴더
* writable = yes : 쓰기 허용
* guest ok = no : 게스트 거부
* create mode = 0777 : 파일 전체 접근 허용
* directory mode = 0777 : 폴더 전체 접근 허용
* valid users = @sambaGroup : sambaGroup 소속 사용자만 허용

![image](https://user-images.githubusercontent.com/43658658/140291409-8fcdeb4e-53ef-4f9c-a8ef-aefdcd4ec875.png)   
변경한 내용에 오류가 없는지 체크합니다.   
![image](https://user-images.githubusercontent.com/43658658/140291767-42fc5616-4569-4d0c-802c-e0f20e769394.png)   
서비스 재시작, 상시 가동, 작동 확인   
![image](https://user-images.githubusercontent.com/43658658/140291842-9cf2d750-ec7d-45d7-ae7d-b9b03e42a00c.png)   
방화벽을 잠시 끕니다.

> <h3>윈도우 정책 변경</h3>

윈도우 10에서는 보안상 Samba 연결을 막아놓았기 때문에 이를 허용으로 변경해야 합니다.

윈도우 검색에서 `mmc`를 실행 후, [파일] > [스냅인 추가/제거]로 들어갑니다.   
* MMC(Microsoft Management Console) : 윈도우 시스템 관리자, 고급 사용자에게 윈도우를 관리하는데 편리한 인터페이스를 제공하는 콘솔.

![image](https://user-images.githubusercontent.com/43658658/140292373-d8cdd9a1-9080-43fb-a46a-e1b14ae05e1a.png)   
![image](https://user-images.githubusercontent.com/43658658/140345759-23666ab4-2fd0-46c3-9a42-693a55d3529e.png)      
![image](https://user-images.githubusercontent.com/43658658/140292969-5a1d8d95-bc3d-4735-ac07-25fffe1112cb.png)   

> <h3>윈도우에서 리눅스 공유 디렉토리에 접근</h3>

![image](https://user-images.githubusercontent.com/43658658/140293482-deed1a8a-5693-465d-abb6-f2492db09c55.png)   
![image](https://user-images.githubusercontent.com/43658658/140294436-0fbe3c51-e1ee-4fac-b90c-95175b8881fa.png)   
적당한 드라이브 선택 후 `리눅스IP주소\공유디렉토리`를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/140297257-c89dfe77-f24f-4f86-afe2-1a96383cd2cc.png)   
오류 발생!   
[제어판] > [사용자 계정] > [Windows 자격증명 관리]   
![image](https://user-images.githubusercontent.com/43658658/140297432-4420f054-259b-431b-9785-b87b6aab71db.png)   
![image](https://user-images.githubusercontent.com/43658658/140297506-3ccc40ca-8f69-4d7a-af7e-b6bd440c9c6b.png)   
윈도우 자격 증명을 추가해주고 다시 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/140297753-c6ccdfca-d874-4442-afa8-7b46a4b0f587.png)   
정상적으로 연결된 것을 확인할 수 있습니다. 적당한 파일을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/140298069-479bc00d-8ceb-43e4-8fa2-66bfd48db2b8.png)   
리눅스로 돌아와서 공유 디렉토리를 확인해 보면 같은 파일이 있는 것을 확인할 수 있습니다.   
`smbstatus` 명령어를 통해 현재 samba 서버에 접속한 윈도우 IP를 확인할 수 있습니다.





