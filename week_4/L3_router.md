# L3 라우터

## 라우터란?

컴퓨터 네트워크 간에 데이터 패킷을 전송하는 네트워크 장치입니다. 
패킷의 위치를 추출하여, 그 위치에 대한 최적의 경로를 지정하며, 이 경로에 따라 데이터 패킷을 다음 장치로 전달합니다. 
특징은 서로 다른 네트워크 간에 중계 역할을 한다는 것입니다.

## 라우터 콘솔 접근

pc와 라우터를 콘솔로 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/141877073-fc2144c7-f745-410c-9124-7f161d62c2a6.png)   

[장치 관리자]를 열고 포트를 확인합니다.   
![image](https://user-images.githubusercontent.com/43658658/141876619-b54108b4-a7b2-47d2-905a-9aa9cb23d89d.png)    

PuTTy를 열고 `Serial` 방식으로 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/141877145-ae774f10-9f4b-470d-845b-81abf9b44827.png)   

콘솔 창이 뜨면 [Enter]키를 입력합니다.   
![image](https://user-images.githubusercontent.com/43658658/141877268-c3f148b5-1272-4373-926d-05b3c28d7924.png)   

## 라우터 초기화



## AUX 포트

콘솔 포트 밑에 `AUX` 포트라는 것이 있습니다.
이 포트에는 모뎀을 연결할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/141877720-7236accd-bdca-45ed-b79e-25836040c7b9.png)   

모뎀을 연결해 놓으면 원격지에서도 모뎀을 통해 라우터에 명령어를 입력할 수 있습니다.
모뎀을 이용한 라우터 구성은 기존의 네트워크에 문제가 발생해 텔넷으로 접근이 불가능할 때, 그리고 콘솔을 연결하자니 너무 거리가 먼 곳일 때 원격지에서 라우터에 접근을 가능하게 합니다.

