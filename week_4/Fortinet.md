# Fortinet

## FortiGate 200E

중견기업과 대기업을 위한 차세대 방화벽기능을 제공하는 보안장비입니다.

## 콘솔 접근

![image](https://user-images.githubusercontent.com/43658658/142091243-7afa12ec-8d05-432a-a71e-715edc67ef9c.png)   
![image](https://user-images.githubusercontent.com/43658658/142091279-04b00ff4-0c86-4d47-9c78-371e3205581f.png)   

`PuTTy`로 연결하고, 스위치를 눌러 전원을 켭니다.   

FortiGate 장비는 전원 스위치를 껐다 켜면 항상 `maintainer/bcpb+S/N(유저이름/비밀번호)`로 접속이 가능합니다.   
(장비의 유저와 비밀번호를 모를 경우 사용해서 접속한 후 유저이름과 비밀번호를 변경하는 작업을 거칩니다)   
![image](https://user-images.githubusercontent.com/43658658/142091394-29e4b285-cd0e-42f0-a53d-0adb4f493281.png)   
`admin user` : maintainer   
`password` : bcpb + 시리얼넘버(S/N)   
![image](https://user-images.githubusercontent.com/43658658/142091483-271a3b5d-5019-4efa-9353-f6d5679bda96.png)   

이제 로그인 유저 이름과 비밀번호를 변경해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/142091824-3fca0f5a-17df-4dd9-89bb-e0a0fe1c8213.png)   
`config system admin` : 관리자(admin) 모드로 들어갑니다.   
`edit [사용자명]` : 유저 이름을 [사용자명]으로 변경합니다.   
`set password [패스워드]` : 비밀번호를 [패스워드]로 변경합니다.

이제 변경한 유저 이름과 비밀번호로 접속이 가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/142091938-e810342e-65d8-447a-88a0-53a36f69fd9b.png)

## 공장 초기화

공장 초기화 방법은 간단합니다. 아래의 명령어를 입력해주면 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142092631-27cd6edc-1a8c-4a10-9344-6291a70e6dd7.png)

초기화가 완료되면 호스트네임이 장비명으로 바뀝니다.   
![image](https://user-images.githubusercontent.com/43658658/142092964-db5ed58c-d980-49ed-ba45-f73278b41fdb.png)   
로그인 유저 이름과 비밀번호는 장비에 붙어 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142093084-435a871e-cb61-47de-9b49-288e9329d451.png)

## 방화벽 접속

장비의 `MGMT` 포트와 pc를 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/142118491-9d2aec7a-a1ba-4308-8e79-a73fb03ca5a3.png)

개인 네트워크의 방화벽을 끕니다.   
![image](https://user-images.githubusercontent.com/43658658/142119105-128ff452-5d43-4a65-a0a3-7b1c54be37cc.png)   

웹 브라우저에서 `192.168.1.99`로 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/142119611-0ad957dd-fcc8-4845-8f24-8ada50c55fcb.png)   
이전에 지정했던 `유저이름/비밀번호`를 적고 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/142119807-ea045d96-b584-47ba-ba1a-c3577d7b2559.png)

호스트이름을 지정합니다.   
![image](https://user-images.githubusercontent.com/43658658/142121065-0db7725f-7b18-4214-ab4c-dbfc5fc8de7a.png)

정상적으로 방화벽에 로그인 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142123136-1fdb0eec-83f9-4a01-bd8b-0ea051f6722d.png)

## 방화벽 인터넷 통신 정책 설정

방화벽 정책의 우선순위는 `Top-Down 방식`으로 정책 리스트의 위쪽 정책 우선입니다.



## SSL-VPN

SSL은 웹 브라우저와 서버 간의 통신에서 정보를 암호화함으로써 도중에 해킹을 통해 정보가 유출되더라도 정보의 내용을 보호할 수 있는 기능을 갖춘 보안 솔루션입니다.   
SSL-VPN은 SSL을 기반으로 한 VPN입니다.

SSL-VPN은 IPSec VPN의 방식의 단점을 보완한 방식입니다.   
기본적으로 그냥 VPN보다 IPSec, SSL-VPN이 보안적인 측면에서 뛰어나다는 공통점이 있지만,   
IPSec의 경우 클라이언트 소프트웨어를 설치해야 하기 때문에, 방화벽단에서의 추가적인 설정이 필요하고, 클라이언트가 설치될 하드웨어에 대한 호환성이 문제가 됩니다.   
반면에 SSL-VPN은 웹 브라우저가 클라이언트의 역할을 하기 때문에 클라이언트 소프트웨어를 설치해야 하는 번거로운 문제가 사라지고, 관리와 유지보수에 있어 수월합니다.

> <h3>SSL-VPN 구축</h3>

먼저 VPN을 사용할 로컬 사용자를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142157645-83324f8a-8936-4402-a87f-ad7ff70b3f3c.png)   
![image](https://user-images.githubusercontent.com/43658658/142157791-6288743d-f5e2-4344-b822-d7eb3bf5c00e.png)      
![image](https://user-images.githubusercontent.com/43658658/142147350-e95e237e-ddb8-4d14-9da1-d631e3c698a0.png)   
![image](https://user-images.githubusercontent.com/43658658/142148429-ac7c0a10-b100-44c8-921d-e62e5c2642d9.png)   

이제 방화벽 사용자 그룹 유형을 만듭니다.   
![image](https://user-images.githubusercontent.com/43658658/142148542-38645f0a-be58-4be8-8fb5-69b81f5c0ac4.png)   
![image](https://user-images.githubusercontent.com/43658658/142148603-ec69b7e8-6ad3-4d30-a448-87f65b99b943.png)   

[SSL-VPN 포털]로 들어가면 SSL-VPN 포탈을 편집해서 커스터마이징 할 수 있습니다.   
저희는 `full access`라는 이름의 SSL-VPN을 사용할 것입니다.   
![image](https://user-images.githubusercontent.com/43658658/142148835-600bcce1-109c-47e2-a921-47fb20c7fa96.png)   

[SSL-VPN 설정]으로 들어갑니다.   
![image](https://user-images.githubusercontent.com/43658658/142155537-86f7d0bb-1d67-4ad3-a453-ac42948646e0.png)   
WAN을 통해 나가는 외부 인터페이스 IP의 포트 번호를 할당해주면, `외부 IP:포트번호`의 SSL-VPN의 주소가 나타납니다.

SSL-VPN 주소로 접속해보면 아직 정책을 설정해주지 않았기 때문에 접속이 되지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/142156763-4c10bef3-61c1-47dc-a8a8-bffc2fe70ee8.png)

이전에 생성한 로컬 사용자가 소속된 그룹을 앞으로 사용할 SSL-VPN 포털인 `full-access`로 접근하도록 허용해줍니다.   
![image](https://user-images.githubusercontent.com/43658658/142156325-fc4fb420-6ebb-46ad-a955-f66357af2ac4.png)   
[새로 만들기]를 클릭하고, 이전에 생성한 사용자 그룹과 포털을 지정합니다.
![image](https://user-images.githubusercontent.com/43658658/142156393-d35ab6c7-00af-4328-b48b-011c2ffbf52c.png)   
사용자 그룹과 포털이 지정된 것을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/142156270-0eb84855-fe73-465e-bb1f-94c7122559b1.png)   

> <h3>SSL-VPN 정책 설정</h3>

이제 방화벽 정책을 설정해서 SSL-VPN에 접속이 가능하도록 해보겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/142158615-c43178cd-4d07-4ed2-8ffd-b986d896b802.png)   
![image](https://user-images.githubusercontent.com/43658658/142160098-5c4a3dbe-4f90-4351-937a-3393f2e028e6.png)   
* `Input 인터페이스` : 출발지 인터페이스
* `Output 인터페이스` : 목적지 인터페이스
* `출발지` : 출발지 ip, 접속을 허용할 사용자까지 지정.
* `목적지` : 목적지 ip
* `스케줄` : 얼마를 주기로 반복할 것이냐
* `서비스` : 어떤 포트로의 접근을 허용할 것이냐
* `NAT(Network Address Translation)` : 사설 네트워크에 속한 여러 개의 호스트가 하나의 공인 IP 주소를 사용하여 인터넷에 접속하게 해주는 기술입니다.
  - 방화벽에 적용 되었을 때는 들어오는 쪽(내부) 인터페이스 ip가 정책을 통과 시에 나가는 쪽(외부) 인터페이스 ip로 변환되어 나가도록 해줍니다.
  - 예를 들어, A, B 인터페이스가 있고, A -> B로 통과할 때 A에서 1.1.1.1인 ip를 B쪽으로 통과 시에 B의 ip로 변환되도록 해줍니다.

모두 입력한 후 [승인]을 누르면 아래와 같이 정책이 생성됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142161222-523160a4-0454-4305-b83f-511dc5fc74ab.png)

![image](https://user-images.githubusercontent.com/43658658/142160261-c0395059-be3c-4bbb-ad2d-cc548d7ad151.png)   
`khyup`라는 허가된 사용자가 SSL-VPN에 접근하면 `정책`으로 인해 `wan1` 포트로 빠져나와 원격 사용자의 웹 브라우저에 표시됩니다.   

정책 설정을 모두 완료하게 되면 다시 `외부 IP:포트번호`에 접속 했을 때 `SSL-VPN 포털 클라이언트`에 접속할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142161404-bdc18b38-dbe9-47cb-93a6-d2d9dc9933a8.png)   
앞서 허가한 사용자에 대한 정보를 입력하면 접속이 가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/142161605-612de39c-14db-4a93-b08d-065eea6fbb80.png)


---

[참고 사이트]   
* https://blog.naver.com/PostView.nhn?blogId=mamsmin&logNo=221181438339&parentCategoryNo=&categoryNo=23&viewDate=&isShowPopularPosts=true&from=search
* https://ppocssac.tistory.com/4
* https://m.blog.daum.net/haionnet/705?tp_nil_a=2
