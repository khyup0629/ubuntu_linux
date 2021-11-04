# Samba 서버 설치와 운영

Samba : 서로 다른 OS 사이에 자원을 공유할 수 있도록 하는 서비스

<Samba 서버(윈도우)의 역할>   
* 자신의 자원을 사용할 사용자를 추가
* 자원 공유

<Samba 클라이언트(리눅스)의 역할>   
* Samba 클라이언트 패키지 설치
* `smbclient` 명령으로 Samba 서버가 제공하는 자원 확인
* `smbmount` 명령으로 samba 서버가 제공하는 공유 폴더 마운트

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

