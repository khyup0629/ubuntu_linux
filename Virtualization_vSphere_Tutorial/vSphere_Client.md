# vSphere Client

## vCenter에 ESXi 호스트 등록하기

vCenter에 ESXi 호스트를 등록합니다.   

![image](https://user-images.githubusercontent.com/43658658/143967825-9d7a778a-c1cc-4c1a-b168-d5a3c8fc2571.png)   
![image](https://user-images.githubusercontent.com/43658658/143968050-e4f43a43-6c75-4cdf-8fc0-4251c92b365e.png)   
![image](https://user-images.githubusercontent.com/43658658/143968085-b4b24f03-45d7-4374-9964-0736a0c78fdb.png)   

ESXi 호스트 정보들을 입력합니다.
![image](https://user-images.githubusercontent.com/43658658/143968125-8e1999ca-bc5e-48d6-907e-572fa97fa21e.png)   
![image](https://user-images.githubusercontent.com/43658658/143968217-03d8b0a4-8df8-430f-bce7-380873e81be5.png)
라이센스 확인 페이지입니다. 여기서 라이센스가 있다면 선택하면 됩니다.   
![image](https://user-images.githubusercontent.com/43658658/143968389-ea90405c-485d-48f4-9982-2826cde0a4fd.png)   
ESXi에 접속이 가능하도록 하기 위해 잠금 모드는 사용하지 않겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/143968529-11024ccb-1fff-42bd-ac4d-e6e59469b9ab.png)   
* `일반` : 로컬 콘솔이나 vCenter Server를 통해서만 ESXi 접속이 가능.
* `엄격` : vCenter Server를 통해서만 ESXi 접속 가능.

VM 위치를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/143968704-099fc970-abcd-4c7f-9380-32a3c72f24d9.png)   
전체 설정을 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/143968740-1a845171-08ef-4035-989f-5abf48ab961f.png)   

조금만 기다리면 ESXi 호스트가 추가된 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/143968820-e36896b0-3490-40bd-bff6-a59bd7b5f062.png)

같은 방법으로 나머지 호스트도 추가합니다.   
![image](https://user-images.githubusercontent.com/43658658/143969301-74db4716-6b9b-4318-b700-7a537cabebcc.png)



클러스터

복수의 ESXi 호스트를 논리적으로 그룹화한 것입니다. vCenter는 동일한 클러스터에 속한 ESXi들의 리소스를 클러스터 단위의 리소스로 관리합니다.


