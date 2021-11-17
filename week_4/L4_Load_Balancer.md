# L4 로드 밸런서

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



---

[참고 사이트]   
* https://blog.naver.com/PostView.nhn?blogId=mamsmin&logNo=221181438339&parentCategoryNo=&categoryNo=23&viewDate=&isShowPopularPosts=true&from=search
* 
