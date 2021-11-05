본 자료는 「이것이 우분투 리눅스다 (저자:우재남)」를 참조하여 작성되었음을 알립니다.

---

# 네임 서버 설치와 운영

네임 서버 : DNS(Domain Name System) 서버. 도메인 주소(ex. www.naver.com)를 IP 주소로 변환 시켜 주는 역할.   
도메인 주소를 IP 주소로 변환 시켜 주는 과정을 `해석`이라고 합니다.   

> <h3>네임 서버 설정 확인</h3>

![image](https://user-images.githubusercontent.com/43658658/139817894-4575c4db-b945-43cd-ac6b-1bc9406c586d.png)   
`nslookup` > `server` 명령어로 현재 서버의 네임 서버 설정 확인   
각 사이트의 IP 주소 확인하기

> <h3>네임 서버에 문제 발생 시 작동</h3>

의도적으로 네임 서버를 고장내자(네임 서버로 가지 못하게 한다).   
![image](https://user-images.githubusercontent.com/43658658/139818847-175cbd24-04c4-4fa0-a1ab-be76a54c20b6.png)   
* `nameserver 네임서버IP주소` : 리눅스에서는 각자 사용하는 네임 서버가 `/etc/resolv.conf` 파일에 `nameserver 네임서버IP주소` 형식으로 설정.   

![image](https://user-images.githubusercontent.com/43658658/139819766-0ed1b721-03f8-4afb-aad4-27ff8904b28b.png)   도메인으론 접속이 불가능   
![image](https://user-images.githubusercontent.com/43658658/139819498-ac535b2c-d444-4650-8a9f-a3aa1d0db824.png)   
IP 주소로는 접속 가능

네임 서버는 편리함을 제공해주는 것일 뿐, 네트워크에는 영향을 미치지 않는 것을 확인할 수 있습니다.   

> <h3>`/etc/hosts`</h3>

![image](https://user-images.githubusercontent.com/43658658/139820325-c05a18b4-e051-4fb3-8b49-bf09bfd53e54.png)   
![image](https://user-images.githubusercontent.com/43658658/139820467-60388506-b507-4ec8-ac15-4736bf2f432e.png)   
* 주소창에 URL을 입력 했을 때, 웹 브라우저는 `/etc/resolv.conf` 파일에 적혀 있는 nameserver를 통해 IP 주소를 얻기 
전에 `/etc/hosts` 파일을 조사해서 URL, IP 정보를 확인한다는 것을 알 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/139822258-76fef367-5b16-4912-b07d-b270a42756db.png)   
만약 엉뚱한 IP 주소를 적어놓게 되면 URL을 입력했을 때 엉뚱한 IP 주소로 접속됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139822421-403dbb06-4ebf-4462-b27c-fe7fcceddef9.png)   
`/etc/hosts` 파일을 먼저 확인하기 때문입니다.

![image](https://user-images.githubusercontent.com/43658658/139822560-e7ce8346-166b-4a94-86f2-9d5b02269ac9.png)

## 네임 서버 구축

로컬 네임 서버 : `/etc/resolv.conf` 파일에 `nameserver` 옆에 적혀 있는 IP주소.

로컬 네임 서버는 모든 도메인과 IP 정보를 가지고 있지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/139824284-7fd99c9d-5296-493c-8ba7-bc510ddfbbd1.png)   
![image](https://user-images.githubusercontent.com/43658658/139824626-37878001-1b8c-4980-8fa0-5984e6faa5c3.png)   

## 캐싱 전용 네임 서버

캐싱 전용 네임 서버 : URL로 IP 주소를 얻고자 할 때, 해당하는 URL의 IP 주소를 알려주는 네임 서버.

> <h3>우분투 데스크탑을 캐싱 전용 네임 서버로 구축하기</h3>

우분투 데스크탑(Server)를 캐싱 전용 네임 서버로 구축해서 우분투 서버(Server-B)와 쿠분투 데스크탑(Client)가 현재 네임 서버(192.168.111.2)이 아닌 우분투 데스크탑(192.168.111.100)을 네임 서버로 이용할 수 있도록 해보겠습니다.

1. 네임 서버와 관련된 패키지를 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/139839570-4a9384ac-0eec-44dc-b2d8-d4fe9dcd50bc.png)   
의존성 문제가 나타났습니다. `bind9-libs=1:9.16.1-0ubuntu2` 패키지를 `-y` 옵션 없이 설치합니다.   
![image](https://user-images.githubusercontent.com/43658658/139839692-852d89ce-1af5-41a8-99c6-4168e49a67fe.png)   
![image](https://user-images.githubusercontent.com/43658658/139839763-63f7208d-9bbb-4727-9b70-717078c1b338.png)   
다시 정상적으로 설치됩니다.   

2. 설정 파일 변경   
![image](https://user-images.githubusercontent.com/43658658/139840150-e9064c65-cc86-47f1-8c97-9d26b94b9687.png)   
vi 편집기로 `/etc/bind/named.conf.options`를 위와 같이 수정합니다.   
VMware가 사용하는 가상머신의 네트워크 주소에 있는 모든 컴퓨터가 네임 서버를 사용할 수 있게 설정하는 것입니다.   

3. 서비스 작동   
![image](https://user-images.githubusercontent.com/43658658/139840499-2a5512f1-b660-41bf-8b3e-e07dc0f38c85.png)   
재시작, 상시 가동, 작동 상태 확인

4. 방화벽 설정   
![image](https://user-images.githubusercontent.com/43658658/139840668-014e8969-fe97-4672-92ab-6e6d2fcad0a5.png)   
네임 서버의 포트인 53번 포트를 허용합니다.

5. 네임 서버 동작 확인   
![image](https://user-images.githubusercontent.com/43658658/139840946-506bcfa9-4e71-4dc4-bd78-fe4d3ad24b8c.png)   
네임 서버가 잘 동작하는지 `dig @네임서버IP 조회할URL` 형식으로 테스트 해봅니다.   
![image](https://user-images.githubusercontent.com/43658658/139841151-6e8850e7-a46f-4a05-afec-0eca0d4a53f1.png)   
`nslookup`을 통해서도 테스트 해 볼 수 있습니다.
`server IP주소` : 현재 서버의 네임 서버를 IP 주소로 설정.

6. 다른 가상머신인 쿠분투 데스크탑(Client)으로 네임 서버 작동 확인   
![image](https://user-images.githubusercontent.com/43658658/139841876-d11bd535-aff8-462e-8e66-7d26931217fc.png)   
정상적으로 작동 되는 것을 확인할 수 있습니다.

7. 쿠분투 데스크탑 네임 서버 고정 지정   
![image](https://user-images.githubusercontent.com/43658658/139842024-f7fb9fb1-b723-499d-90cf-f7a9530b4002.png)   
`/etc/resolv.conf`에서 네임 서버 IP 주소를 수정합니다.

8. 우분투 서버(server-B) 네임 서버 고정 지정   
![image](https://user-images.githubusercontent.com/43658658/139842331-3f4fb681-0e09-4139-bed6-752a045d9c9a.png)   
`/etc/resolv.conf`에서 네임 서버 IP 주소 수정.

9. 우분투 서버 네임 서버 작동 확인   '
![image](https://user-images.githubusercontent.com/43658658/139842501-bbdc0c6d-88ca-4d3b-8930-7fc858f2621b.png)   
정상적으로 동작하는 것을 확인할 수 있습니다.

10. 윈도우(WinClient)에서 네임 서버 고정 지정   
[제어판] > [네트워크 및 인터넷] > [네트워크 상태 및 작업 보기] > [Ethernet()]   
![image](https://user-images.githubusercontent.com/43658658/139843148-f4af81a5-4196-460e-9336-1a2128593bae.png)   

11. 윈도우 네임 서버 작동 확인   
![image](https://user-images.githubusercontent.com/43658658/139844368-2a8a5a78-8a96-43f2-8fa6-34d815586efd.png)   
정상적으로 동작.

12. 윈도우 DHCP 설정으로 되돌리기   
![image](https://user-images.githubusercontent.com/43658658/139844907-b727a054-8317-4bfe-bd68-bfea4d64ed94.png)   
이더넷 어댑터의 이름을 확인하고, `netsh interface ip set dns Ethernet0 dhcp`로 자동으로 DNS 서버를 할당 받는 방식(DHCP)으로 설정합니다.

## 마스터 네임 서버

`john.com`이라는 도메인으로 예를 들어봅시다.   
마스터 네임 서버 : `john.com`과 같은 도메인(www.john.com, ftp.john.com)에 속해 있는 컴퓨터들의 이름을 관리하고 외부에서 컴퓨터 IP 주소를 알고자 할 때 해당 컴퓨터의 IP 주소를 알려주는 네임 서버.

> <h3>우분투 데스크탑(Server)에 john.com의 마스터 네임 서버 구축</h3>

1. 웹 서버 설치   
![image](https://user-images.githubusercontent.com/43658658/139846661-6ae3922b-f725-4460-9aec-0d2a31803f34.png)   
![image](https://user-images.githubusercontent.com/43658658/139846838-3077fa64-5b14-4d05-904a-26e294059cd6.png)   
재시작, 상시 가동, 작동 확인   
![image](https://user-images.githubusercontent.com/43658658/139847113-3b64d8a1-f081-4bf3-ac85-cf27c5418473.png)   
80번 포트 허용   
![image](https://user-images.githubusercontent.com/43658658/139847388-86af3bb4-4e36-4d4b-91e0-414d45d39dc6.png)   
![image](https://user-images.githubusercontent.com/43658658/139847276-c3d51ee6-9507-4a62-9cf9-04b7f99717b2.png)   
`/var/www/html`의 기존 `index.html`를 삭제하고 위의 내용으로 새로운 `index.html` 파일을 만듭니다.

2. 우분투 서버(Server-B)에 FTP 서버 설치   
![image](https://user-images.githubusercontent.com/43658658/139847830-43d6f826-46ac-4519-8f51-edb3d43bff3f.png)   
`sudo apt -y install vsftpd`   
![image](https://user-images.githubusercontent.com/43658658/139848315-4bdb1ae3-6fe3-4b1c-b102-260279c9947f.png)   
21번 포트 허용   
![image](https://user-images.githubusercontent.com/43658658/139850019-632e5b6f-de4c-4dda-9324-54f5bfe364fe.png)   
![image](https://user-images.githubusercontent.com/43658658/139850046-a7b308ba-1018-4bcc-814f-3acbcf86c13b.png)   
외부로 보여지는 페이지 작업   
![image](https://user-images.githubusercontent.com/43658658/139850404-03190c67-f330-4d12-bd7f-575a766db799.png)   
외부에서 anonymous의 사용자로 접속을 허용하고, FTP 서버로 접속 했을 때 `/srv/ftp/welcome.msg` 파일의 내용이 보여지도록 합니다.   
![image](https://user-images.githubusercontent.com/43658658/139850738-398dab34-6491-4c2d-a8c6-69a401f7b55a.png)   
FTP 서버 재시작.

3. `john.com` 도메인 설정   
![image](https://user-images.githubusercontent.com/43658658/139851207-c04731cf-f3ad-479d-9959-869baf8b20ec.png)   
`/etc/bind/named.conf` 파일은 네임 서버 시작 시 제일 먼저 읽는 파일입니다.   
* type master : 마스터 네임 서버
* file : 도메인 주소의 상세 설정 파일

![image](https://user-images.githubusercontent.com/43658658/139851816-0d0e520b-1430-42a4-8fdd-a10664e9f7c4.png)      
설정 파일에 문법 오류가 있는지 체크.   
![image](https://user-images.githubusercontent.com/43658658/139852294-cf01cd03-cdb8-447c-8172-5a72242b4fbc.png)   
* $TTL : 다른 네임 서버가 해당 IP 주소를 캐시에 저장하는 시간. 3H = 3시간
* @ : `/etc/bind/named.conf`에 정의된 john.com을 의미.
* IN : internet을 의미.
* SOA : 권한의 시작. 괄호 안의 숫자는 시간을 의미.
  * 2 : 버전 정보
  * 1D : 상위 네임 서버에 업데이트된 정보를 요청하는 간격
  * 1H : 상위 네임 서버에 문제가 발생했을 때 재접속 간격
  * 1W : 상위 네임 서버에 접속하지 못할 경우 이전의 정보를 파기하는 간격
  * 1H : 이 시간 이후 정보가 삭제됨을 의미.
* NS : 설정된 도메인의 네임 서버 역할을 하는 컴퓨터 지정.
* A : 호스트 이름에 상응하는 IP 주소 지정.

![image](https://user-images.githubusercontent.com/43658658/139853499-687b20c6-4ccb-4df1-9008-da2425ae300c.png)   
포워드 존 파일의 문법 오류 체크.   
![image](https://user-images.githubusercontent.com/43658658/139854266-f7969274-511d-49c5-a771-e64f2fa3af16.png)   
재시작, 작동 확인   

4. 쿠분투 데스크탑(Client)에서 마스터 네임 서버 작동 확인   
앞서 `/etc/resolv.conf`에서 네임 서버 IP 주소를 우분투 데스크탑의 IP 주소로 설정했습니다.   
![image](https://user-images.githubusercontent.com/43658658/139855115-e7821398-2999-438a-a8df-065fb9f17246.png)   
웹 브라우저에서 `www.john.com`로 접속하면 위와 같이 나타납니다.   
![image](https://user-images.githubusercontent.com/43658658/139856346-7c57769d-9388-4e21-b619-45900146f0fd.png)   
터미널에서 우분투 서버에 구축한 FTP 서버로 접속합니다(사용자: anonymous, 암호: 아무거나)   

5. 윈도우(WinClient)에서 마스터 네임 서버 작동 확인   
파워쉘을 관리자 권한으로 실행합니다.   
![image](https://user-images.githubusercontent.com/43658658/139857127-16d7d6f8-835f-4828-8d67-f9d6f6b4cfec.png)   
DNS 서버를 우분투 데스크탑으로 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/139857649-073a48fb-c22f-4eb0-a964-bd1831befb3f.png)   
웹 서버로 정상적으로 접속이 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/139857857-51816583-bf24-4a8e-8a7e-ea21747d3162.png)   
FTP 서버 역시 정상적으로 접속이 됩니다.

## 라운드 로빈 방식의 네임 서버

라운드 로빈 방식 : 여러 대의 웹 서버를 운영해서 웹 클라이언트가 서비스를 요청할 경우 교대로 서비스를 실행해서 서버의 부하를 나누도록 하는 방식.   
![image](https://user-images.githubusercontent.com/43658658/139858470-9ddaf905-c3d5-49a3-b999-1ec7c24a1542.png)   
네이버도 여러 대의 웹 서버를 운영하는 것을 볼 수 있습니다.

> <h3>라운드 로빈 방식의 네임 서버 구현</h3>

1. `/etc/bind/john.com.db` 포워드 존 파일 수정   
![image](https://user-images.githubusercontent.com/43658658/139859090-e74e10e9-978e-471e-8e11-3fbb4dc432aa.png)   
다음과 같이 `/etc/bind/john.com.db` 파일을 수정합니다.
![image](https://user-images.githubusercontent.com/43658658/139859497-78399c71-bee1-4269-a0cb-c8a443a454c1.png)   

2. 라운드 로빈 설정 확인   
![image](https://user-images.githubusercontent.com/43658658/139859845-b34ea719-5df4-49bd-b35d-2ccd2ae9b635.png)   
3개의 웹 서버를 확인할 수 있습니다.

3. 외부에서 테스트   
쿠분투 데스크탑에서 웹 브라우저를 열고 `www.john.com`에 한 번 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/139860254-9b72ebd0-76c2-4cf9-a09a-18711983ecc4.png)   
닫았다가 다시 한 번 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/139860585-ab647ce9-4eda-4bc7-994d-e296fb57d305.png)   
다른 사이트가 나타나는 것을 확인할 수 있습니다.

