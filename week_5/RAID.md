# RAID

하나의 디스크를 사용하면 안정성의 문제가 떨어지는데, 이를 해결하기 위한 방법.   
`여러 개의 디스크`를 연결해서 `하나의 디스크`로 사용하도록 배열하는 방법입니다.

## RAID 0

데이터를 각각의 디스크에 나눠 담는 방식입니다(속도 중점).   
![image](https://user-images.githubusercontent.com/43658658/143440757-316b2a1c-a267-4f9c-8ce6-f8fdbfeff0df.png)

장점   
* 하드디스크의 개수 만큼 `읽기·쓰기 속도가 증가`합니다(나눠진 만큼 대역폭이 증가하기 때문)   

단점   
* 안정성이 떨어집니다. 
  - 하드 하나라도 고장나면 전체 데이터를 잃어버립니다.

일회성 데이터나 빠른 작업을 위해 사용하고 마치면 바로 백업용 저장 공간으로 옮깁니다.

## RAID 1

각 디스크에 중복으로 데이터를 기록합니다(안정성 중점).   
대부분 `2개`의 디스크로 사용하고 2개를 초과하는 경우는 드뭅니다.   
![image](https://user-images.githubusercontent.com/43658658/143440904-cecc629f-f35f-4b1e-8439-b131bca3a7e5.png)

장점   
* 디스크 하나가 고장나더라도 `데이터가 보존`됩니다.   

단점   
* 하드의 용량을 `반` 밖에 쓰지 못합니다.

## RAID 5

`3개 이상`의 디스크가 요구됩니다(XOR 연산이 가능한 개수가 3개부터이기 때문입니다).   
각 디스크 별로 `패리티`라고 불리는 `복원 정보`를 담습니다.   
`하나의 디스크`가 고장 났을 때에만 복원이 가능합니다.   
![image](https://user-images.githubusercontent.com/43658658/143441154-3842b62f-8e98-4cb0-a938-1a8a69592d49.png)

장점   
* 안정성 증가
  - `복원이 가능`하기 때문입니다. 
* 읽기 속도 증가
  - `대역폭이 커지기` 때문입니다.
* 쓰기 속도 그대로
  - 패리티 연산과 기록의 `처리가 추가`되기 때문입니다.
* 용량 증가
  - `n-1` 배 증가합니다.
  - `패리티 저장 공간` 때문에 하드 1개 만큼의 용량을 뺍니다.

---

[참고 사이트]   
* [RAID 0, 1, 5](https://youtu.be/lP3MlI9aaqY)

