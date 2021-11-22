# 베어메탈에 ESXi 설치

## DELL사 장비용 ESXi 부팅 USB 만들기

먼저 부팅 USB로 만들 `USB를 PC에 꽂고` 다음을 진행합니다.

아래의 사이트에 접속해서 `Rufus` 프로그램을 다운로드 받습니다.   
=> https://rufus.ie/ko/   
![image](https://user-images.githubusercontent.com/43658658/142852739-069e7dde-50d8-4ec6-919e-3810557d1e0e.png)

업데이트를 하지 않는다고 선택.   
![image](https://user-images.githubusercontent.com/43658658/142852866-89da15ba-1bd7-4ad0-83f5-5583310e9abe.png)

꽂은 USB를 선택하고, ISO 파일을 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142853352-63aa258e-4896-4e22-8765-e75331bd366a.png)

DELL용 ESXi를 선택하겠습니다.   
![image](https://user-images.githubusercontent.com/43658658/142853537-c1039273-a4da-42f7-bb99-19deb1e31eb7.png)

하단의 시작을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/142855314-8917451e-48b0-415e-925a-a5e8fd3b99f5.png)
* 파티션 형식 
  - MBR : 부팅 시 BIOS 모드와 UEFI 모드에서 모두 지원
  - GPT : 부팅 시 UEFI 모드만 지원. 2020년 이후부터 생산되는 메인보드는 UEFI 모드만 지원.

표시되는 메시지는 모두 `확인`을 누릅니다.   
![image](https://user-images.githubusercontent.com/43658658/142854045-d4c2ffa7-bdcd-412f-b766-0ca9147521b3.png)

설치가 완료되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142854104-febb3846-9b8c-458a-b517-745a4ccb39af.png)

## HP사 장비용 ESXi 부팅 USB 만들기

다른 USB를 꽂고 다시 `Rufus`를 실행합니다.   

이번엔 HP용 ESXi를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142854519-82620ab1-9a5f-4057-a830-c012994c08a7.png)

`시작`을 누르고 설치를 진행합니다.   
![image](https://user-images.githubusercontent.com/43658658/142854668-a462cade-d1f7-4671-a7d5-d3fd18a59435.png)

---

참고 사이트   
- [rufus 설치 후 부팅 USB 만들기](https://blog.akionz.com/74)
- [MBR과 GPT의 차이](https://m.blog.naver.com/kangyh5/221846708215)
- 
