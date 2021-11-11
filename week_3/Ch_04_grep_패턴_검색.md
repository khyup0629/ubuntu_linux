# grep 패턴 검색

## grep 이란?

`grep [옵션] [패턴] [파일명]`

입력되는 파일에서 `주어진 패턴 목록`과 매칭되는 `라인`을 검색한 다음 표준 출력으로 `검색된 라인을 복사해서 출력`해 준다.

![image](https://user-images.githubusercontent.com/43658658/141216124-dc37d962-14bd-48b5-a124-4ab2c9c7cf66.png)   
`-b` : 블록 번호를 라인 앞에 보여줍니다.   
![image](https://user-images.githubusercontent.com/43658658/141216159-da03e7e2-cb10-4016-9291-f04dccde5989.png)   
`-n` : 파일 안에서 라인 번호를 앞에 보여줍니다.   
![image](https://user-images.githubusercontent.com/43658658/141216202-34f89e10-fc56-4b7e-8d30-0c4990a7a94c.png)   
`-c` : 매칭되는 라인 수를 보여줍니다.   
![image](https://user-images.githubusercontent.com/43658658/141216242-722ee945-5812-491b-84e9-71add990dd9a.png)   
`-i` : 패턴에 사용되는 문자열의 대소문자를 무시하고 검색합니다.   
![image](https://user-images.githubusercontent.com/43658658/141216320-c52aaf71-ce67-4995-9760-0a5fea5c61a3.png)   
`-l` : 나열된 파일이 패턴과 매칭되는 문자가 있는 경우 파일명을 출력합니다.   
![image](https://user-images.githubusercontent.com/43658658/141216478-dc9a6973-f40b-438f-9c44-f12f7f351053.png)   
`-s` : 에러 메시지를 출력하지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/141216535-265b24b6-3bb1-4b51-84a0-262c4a44d723.png)   
`-v` : 패턴과 매칭되지 않는 라인을 검색합니다.   

> <h3>grep을 이용한 정규표현식 실습</h3>

<testfile 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141217155-df33393c-1b26-4cb1-a49e-c159fb1490a0.png)   

![image](https://user-images.githubusercontent.com/43658658/141300369-cbfd6f56-06fa-49d9-98d5-b1b92ab54075.png)   
단순히 `Kim`과 일치하는 패턴 검색   
![image](https://user-images.githubusercontent.com/43658658/141217329-96184fe7-b0a5-46fa-8f42-f039cf68da19.png)   
제일 첫 글자가 `K` 또는 `k`이고 뒷 글자가 im인 패턴 검색   
![image](https://user-images.githubusercontent.com/43658658/141217383-c5f79213-29fc-4cd6-9f7f-e9005958e179.png)   
첫 글자가 `D`인 라인을 검색   
![image](https://user-images.githubusercontent.com/43658658/141217473-19c5021a-c37c-4c42-94d0-659f421c0958.png)   
마지막 글자가 `8`인 라인을 검색   
![image](https://user-images.githubusercontent.com/43658658/141217686-de4916b6-2233-4c5f-86bf-68b9b315304c.png)   
띄어쓰기가 포함된 패턴을 검색할 때는 작은 따옴표(')로 패턴을 묶어줘야 합니다.   
![image](https://user-images.githubusercontent.com/43658658/141301327-0a701dc1-bc65-4200-b8d2-42ddad4c1d02.png)   
백 슬래쉬는 메타문자를 일반 문자로 인식시킵니다. `5.?`인 단어를 검색   
![image](https://user-images.githubusercontent.com/43658658/141218229-030ba27f-ec67-41cd-9d02-67ab3b9e9735.png)   
`.5`로 끝나는 단어를 검색   
![image](https://user-images.githubusercontent.com/43658658/141218259-fef7da13-98c7-4ab6-84ed-be6ce95348f6.png)   
`S` 또는 `B`로 시작하는 라인을 검색   
![image](https://user-images.githubusercontent.com/43658658/141218279-46981987-9ba7-443c-9d34-e07847d1f441.png)   
`숫자가 아닌` 문자를 검색   
![image](https://user-images.githubusercontent.com/43658658/141218451-ec00501b-012f-47ce-a1d6-3e211e1ae97f.png)   
총 4자리 문자이면서 `[소문자][소문자][공백][대문자]` 형식의 문자열을 검색   
![image](https://user-images.githubusercontent.com/43658658/141218499-913b780a-99b3-4234-9467-5301ab3ac72a.png)   
`n`다음에 `g`가 0번 이상 반복되는 문자열 검색   
![image](https://user-images.githubusercontent.com/43658658/141218531-c3a12ec3-6715-4570-86eb-fea829329e8a.png)   
소문자가 5번 있는 문자열을 검색   
![image](https://user-images.githubusercontent.com/43658658/141218802-30941ded-fd53-495e-8cef-e159c583f073.png)   
`Dae`로 시작하는 단어를 검색   
![image](https://user-images.githubusercontent.com/43658658/141218844-14e7e146-6cdb-48fc-89a8-69c5fd52a996.png)   
대문자로 시작하고, 가운데 아무 문자가 몇 개가 와도 상관 없고, 마지막에 `n`으로 끝나는 단어 검색

![image](https://user-images.githubusercontent.com/43658658/141219199-53de0828-470c-4bf8-82ad-c615bf10a2cb.png)   
`Dae`로 시작하는 라인을 검색   
![image](https://user-images.githubusercontent.com/43658658/141219470-0427662c-69ff-4368-9d89-3ce52868a262.png)   
대소문자를 가리지 않고 `kim`이라는 문자열을 검색   
![image](https://user-images.githubusercontent.com/43658658/141219533-c1085223-513b-40c6-ab8e-6ef55845e849.png)   
`kim`이 들어간 라인을 제외한 라인을 검색   
![image](https://user-images.githubusercontent.com/43658658/141219650-a3dffcfd-47b0-474f-8fcb-caf230fa0a2d.png)   
현재 디렉토리의 전체 파일 중 `Daegu`라는 문자열이 들어간 파일을 검색   
![image](https://user-images.githubusercontent.com/43658658/141219959-72259634-57a2-47d8-99c9-2e93b4e1b1c0.png)   
환경 변수의 값을 통해 grep을 이용할 수도 있습니다.

![image](https://user-images.githubusercontent.com/43658658/141220061-b1664f8b-353f-4eba-9a0c-c6fc444a47e1.png)   
현재 디렉토리의 파일 리스트에서 `d`로 시작하는 라인을 출력합니다.   

## egrep(extended grep)

egrep은 grep의 확장으로서 추가적인 정규표현식 메타문자들을 사용할 수 있습니다.   

![image](https://user-images.githubusercontent.com/43658658/141220546-3656ea91-ea20-4534-92d4-a0b215367f59.png)   
![image](https://user-images.githubusercontent.com/43658658/141220566-46a9f816-48a8-4afd-8558-a8ae9022f980.png)   

![image](https://user-images.githubusercontent.com/43658658/141220803-aa1fbb1a-a260-4564-b4b7-e2bf49fc0e30.png)   
`Kim` 또는 `Kang`이 들어간 라인을 검색   
![image](https://user-images.githubusercontent.com/43658658/141220861-4cf0d218-d79e-4a06-b250-58fd5dc187b3.png)   
`9`가 하나 이상 연속되는 문자열 검색   
![image](https://user-images.githubusercontent.com/43658658/141221209-291bb38c-01b6-4ccf-b81a-6d4aca8b9d61.png)   
`8` 다음 `.`이 없거나 하나 있고, 마지막으로 숫자가 들어가는 문자열 검색   
![image](https://user-images.githubusercontent.com/43658658/141221337-cf3a40c1-f5ac-4edf-ac48-4a6c4608d148.png)   
`Dae` 문자열이 하나 이상 연속되는 문자열 검색   
![image](https://user-images.githubusercontent.com/43658658/141221810-fe9cde91-e4cf-4909-998e-a356b191a577.png)   
`Ki` 또는 `Ka`인 문자열 검색   
![image](https://user-images.githubusercontent.com/43658658/141221858-a36745e2-1563-4ade-b373-12755e33ed83.png)   
`sa` 또는 `u`인 문자열 검색

## fgrep

fgrep에서는 정규표현식 메타문자들을 사용할 수 없기 때문에 특수 문자 및 `$` 문자들은 문자 그대로 출력됩니다.

<fgrep.txt 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141222304-30498310-40e7-4470-8b1c-445dd048727e.png)

![image](https://user-images.githubusercontent.com/43658658/141222421-c2daa304-9979-4383-9675-a5bf1d9b5bed.png)   
`[A-Z]`를 검색하면 정규표현식인 대문자 한 글자로 검색하지 않고 문자 그대로 검색합니다.   

