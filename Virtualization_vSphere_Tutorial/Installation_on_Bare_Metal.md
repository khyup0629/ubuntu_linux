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

## DELL사 장비 ESXi 설치

부팅 USB를 꽂고, 모니터와 키보드를 연결합니다.   
![image](https://user-images.githubusercontent.com/43658658/142868581-f480d944-188f-4404-8411-7798dd3cb204.png)

장비를 부팅시키고 `[Ctrl]+[R]` RAID 컨트롤러 BIOS로 진입합니다.   
![image](https://user-images.githubusercontent.com/43658658/142873797-61492100-d6af-4e48-9ecf-b6354a135da0.png)

레이드를 모두 지웁니다.   
![image](https://user-images.githubusercontent.com/43658658/142867591-57db318a-d608-4e3e-b7a6-23063855a08f.png)

새로운 가상 디스크를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142868017-cb9716ef-29cc-45cf-aff4-9d4e0f71087e.png)

레이드를 구성해주고 `초기화` 옵션을 선택한 다음 가상 디스크를 생성합니다.   
![image](https://user-images.githubusercontent.com/43658658/142867974-510adfe0-5639-4f82-81f6-6de9e932f233.png)
* `Force write back with no battery` : 배터리 없어도 강제로 가상 디스크가 `후기입`하겠다는 의미입니다.
  * `write back(후기입)` : 캐시가 모든 데이터를 수신한 후 캐싱된 데이터를 디스크가 가져와 쓰는(write) 방법입니다.
* `Hotspare` : 디스크 장애가 발생하는 경우 장애가 발생한 디스크를 대체하기 위해 `시스템에 저장된` 스페어 디스크 장치입니다.
  - 핫스페어 디스크 장치는 `구성되지 않은 디스크`로 시스템에 저장되고, 디스크 장애가 발생하면 시스템이 핫스페어 디스크 장치를 `장애가 발생한 디스크 장치와 교환`합니다.

BIOS 모드를 빠져나와 `[Ctrl]+[Alt]+[Delete]`를 눌러 재부팅합니다.   

[F11]을 눌러 Boot Manager로 들어 가서 Hard Disk에서 부팅 USB를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142869720-a55a07c2-8d9e-42d1-a351-06cd1823ce99.png)   
![image](https://user-images.githubusercontent.com/43658658/142874506-7a510cf4-0bc1-42ed-8778-d8f63394d6f4.png)

그럼 설치가 진행됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142870477-b9a65847-0111-4cc5-a8ab-c577eb3cb019.png)   
![image](https://user-images.githubusercontent.com/43658658/142870436-beb9d72e-07a8-4f19-961a-0887a67c059a.png)   

라이센스를 동의합니다.   
![image](https://user-images.githubusercontent.com/43658658/142870587-49cf20ba-2357-4c22-a8cf-0f233e46bd4f.png)

ESXi를 설치할 디스크를 선택합니다.   
![image](https://user-images.githubusercontent.com/43658658/142870665-4ac9fe48-2dae-4c4b-96eb-4fe74951c2ab.png)

키보드 레이아웃을 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/142870743-4572c1aa-173a-4e85-ba30-aa840cd1bc8e.png)

패스워드를 입력합니다. 유저 아이디는 자동으로 `root`입니다.   
![image](https://user-images.githubusercontent.com/43658658/142870822-efa7a89e-5437-4f7b-8cab-173a721f04bd.png)

설치를 시작합니다.   
![image](https://user-images.githubusercontent.com/43658658/142870878-21a2ea02-d44b-4a2f-9ec0-d4dd35de4ee4.png)

설치가 진행됩니다.   
![image](https://user-images.githubusercontent.com/43658658/142870923-e7ebe7ff-1469-436a-a7df-a6bd80ccf11b.png)

설치가 완료되면 재부팅합니다.   
![image](https://user-images.githubusercontent.com/43658658/142871045-4b6a3838-5f4e-4b43-8db4-1d360b83ce9c.png)

랜선을 연결하고, [F2]를 눌러 시스템 커스터마이즈 모드에 진입합니다.   
![image](https://user-images.githubusercontent.com/43658658/142871422-249153cc-fff9-4aeb-bd00-f6f0007907ce.png)

Static모드로 IP 주소를 설정합니다.   
![image](https://user-images.githubusercontent.com/43658658/142871492-feadbe75-863c-4908-b941-1f8b444553fa.png)

정적 IP 주소가 설정되었습니다.   
![image](https://user-images.githubusercontent.com/43658658/142871575-7e39ce1f-2312-4a7c-be7f-31006b77cedc.png)

설정한 IP 주소로 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/142873371-18ce738c-a74b-4ec3-888f-a7ece73b70be.png)

유저 아이디(root)와 설정한 비밀번호로 접속합니다.   
![image](https://user-images.githubusercontent.com/43658658/142873441-8f633bde-fa54-4c1e-9b40-26bd8f2eaf90.png)

접속이 정상적으로 되는 것을 확인할 수 있습니다.   
![image](https://user-images.githubusercontent.com/43658658/142873470-93fd344d-e6ae-4e7e-8b6d-7435025f7188.png)

## HP사 장비 ESXi 설치

부팅 USB와 모니터, 키보드를 서버 장비에 연결하고, 서버 장비를 인터넷에 연결합니다.

부팅이 진행되면 `F8`을 열심히 눌러 가상 디스크 설정 화면으로 들어갑니다.   

기존에 존재하는 가상 디스크를 지웁니다.   

새로운 가상 디스크를 만듭니다.   

* `maxmum boot partition` : OS를 설치할 때 설치 파일들이 저장되기 위한 파티션 용량을 설정합니다.   
  - 구형 OS의 경우 4GB 로도 공간이 충분했으나, 점점 설치 파일의 용량이 커지기 시작하면서 8GB를 선택하는 옵션이 추가되었습니다.

부팅 USB에 있는 iso 파일을 선택합니다.   

라이센스를 허용합니다.   

생성한 가상 디스크를 선택합니다.   

키보드 레이아웃을 선택합니다.   

비밀번호를 설정합니다.   

설치를 시작합니다.   

설치가 완료되면 재부팅하고 부팅 USB를 언마운트합니다.   

IP를 설정해야 합니다. `F2`키를 눌러 시스템 커스터마이즈 모드로 접속합니다.   

`Configure Management Network` > `IPv4 Configuration`   

정적 IP를 구성합니다.   

정적 IP가 구성되었습니다.   




---

참고 사이트   
- [rufus 설치 후 부팅 USB 만들기](https://blog.akionz.com/74)
- [MBR과 GPT의 차이](https://m.blog.naver.com/kangyh5/221846708215)
- [가상 디스크 삭제 및 설치 방법](https://www.dell.com/support/kbdoc/ko-kr/000139093/a-a-a-dell-a-a-a-a-a-a-a-a-a-a-a-a-a-a-dell-poweredge)
- [HP 장비 RAID 구성 방법](https://soldier5683.tistory.com/29)
- 
