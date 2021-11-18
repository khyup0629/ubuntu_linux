# pfSense

pfSense는 개인, 기업에서 무료로 사용할 수 있는 오픈소스 라우터/방화벽입니다. 
정확히는 라우터, 스위치, 무선라우터/스위치, 방화벽으로 설정하여 사용할 수 있고, 대부분의 경우에 내·외부 경계망에서 방화벽으로 사용하고 있습니다.

# pfSense 설치 및 기본 설정

아래의 링크에서 pfSense 이미지 파일을 다운로드 받습니다.   
=> https://www.pfsense.org/download/   
![image](https://user-images.githubusercontent.com/43658658/142332690-b4ecd1c3-f3fa-46fd-881a-020666ea1e93.png)

pfSense를 위한 가상 머신을 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142333129-3f2d714e-0470-4826-8e15-2a8d9eb03bc2.png)   

pfsense `iso` 파일을 넣고 가상 머신을 실행하면 자동으로 설치가 시작됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142333896-f7d78122-6400-4ea9-b827-c7447e9745a6.png)   
Pfsense 최종 사용자 라이선스 계약을 수락합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334010-8585b35a-dd76-4358-ba62-8438733cb074.png)   
설치 옵션을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334054-20dbac44-e47b-4b44-8d23-541875e85ccb.png)   
키보드 레이아웃을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334204-a655f2b9-5377-4dd9-88e5-6bde400dc348.png)   
![image](https://user-images.githubusercontent.com/43658658/142334384-a0163f21-3bf3-4ca1-8c4d-0a31000440d1.png)   
자동 옵션을 선택해서 디스크 분할을 자동으로 수행합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334954-db2f51b0-2bf6-45b9-ab43-fce2da95f237.png)
Pfsense 서버 설치를 시작합니다.   
![image](https://user-images.githubusercontent.com/43658658/142334996-6fbca3a4-f3b4-495a-9b0b-273438763802.png)   
수동 화면 구성을 하지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/142335098-51023807-2bee-471a-b1f8-3b03fb8ddee5.png)   
재부팅합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335125-2a110f9b-d740-4f19-8c33-70ea9c7a0f19.png)   

내부에서 네트워크를 나눌 필요는 없으므로 vlan을 구성하지 않고, 시스템의 외부 인터페이스를 `vmx0`로 구성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335738-65abb620-7461-4ee5-a1ed-653ec7b05d3a.png)   
시스템의 내부 인터페이스를 `vmx1`로 구성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335770-f8bc7c1d-2fe6-4f69-ba90-609a66174e1a.png)   
지정한 인터페이스를 확인하고 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/142335829-cc4f7550-66d6-40d9-95d2-7cc4acd7293d.png)






