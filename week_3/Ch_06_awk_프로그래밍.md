# awk 프로그래밍

## awk 란?

파일로부터 레코드(record)를 선택하고, 선택된 레코드에 포함된 값을 조작하거나 데이터화하는 것을 목적으로 사용하는 프로그램입니다.

```
awk 'pattern' filename
awk '{action}' filename
awk 'pattern {action}' filename
```

## awk 프로그래밍 

<awkfile 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141226946-8b76825a-0312-4111-aa62-52f13aedf28a.png)   

![image](https://user-images.githubusercontent.com/43658658/141227155-7481869c-f86c-4fa8-807e-5a9b75eaf45d.png)   
`Gildong`을 포함하고 있는 라인 출력   
![image](https://user-images.githubusercontent.com/43658658/141227188-f59ec542-c2f2-4f35-8fad-259a620166c9.png)   
공백 또는 탭을 기준으로 필드를 나누었을 때 왼쪽에서 첫 번째로 나오는 필드(`$1`)를 출력합니다.   
![image](https://user-images.githubusercontent.com/43658658/141227265-597016bc-647c-4cfc-9535-b4b4e1083712.png)   
`Hong`이 들어간 라인의 1, 2번째 레코드를 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141227656-80e7bf70-ab96-48eb-9ea2-ec2fca547811.png)   
`df` 명령으로 나오는 테이블에서 네 번째 필드의 레코드가 1000000을 초과하는 라인 출력

> <h3>print 함수</h3>

print 함수는 `{print}` 형식으로 awk의 액션 부분에 사용될 수 있습니다.   
문자열은 큰 따옴표로 감싸주고, 아규먼트들 사이에는 콤마로 구분지어 줍니다.   
콤마를 사용하지 않으면 아규먼트들은 서로 연결되어버립니다.   

![image](https://user-images.githubusercontent.com/43658658/141228763-2501b19a-e2d1-47be-8c22-d257d498447e.png)   
필드 변수가 연결되어 있으면 콤마를 사용해서 구분지어 주고, 문자열과 변수가 연결되어 있으면 띄어쓰기를 기준으로 구분하면 됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141228936-60f90ec4-5db2-44f3-8f4b-142308178dc2.png)   
![image](https://user-images.githubusercontent.com/43658658/141228921-40245da4-f2d3-4752-ada9-7829a11df534.png)   
`Jung`과 매칭되는 라인 중 1, 2번째 필드값을 이용해 print합니다.   

> <h3>OFMT 변수</h3>

숫자를 출력할 때 숫자의 포맷을 제어하기 위한 역할을 합니다.

![image](https://user-images.githubusercontent.com/43658658/141229454-6c984451-b36e-4b85-8a43-5bc0cabf9df0.png)   
`%.2f` : 소수점 아래 둘째 자리까지 나타내고 그 뒤는 버림   
또한 큰 수는 지수 형태(e)로 나타낼 수 있습니다.   

> <h3>printf 함수</h3>

print 함수와 다르게 printf 함수는 포매팅된 깔끔한 출력을 지원합니다.   

`printf "원하는 출력 형식(%, %)", $1, $2`

![image](https://user-images.githubusercontent.com/43658658/141235167-f895c4f9-c20d-4a53-9f48-7bc574c669ad.png)   
`%-15s`는 좌측에서 15개의 문자열을 출력하는데 %에 들어간 수만큼 15에서 빼고 나머지는 빈 칸으로 출력합니다.   
`%15s`는 우측에서부터 15개의 문자열을 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141304203-33f29234-4e30-44b5-97d1-246f898cd2a6.png)   
![image](https://user-images.githubusercontent.com/43658658/141235620-8c73e4a5-6a79-45d2-8a0d-434ba1f3005f.png)   
1, 2번 필드의 값이 `%-20s`로 들어가고, 3번 필드의 값이 `%4d`로 들어갑니다.   

> <h3>awk -f</h3>

awk 액션과 명령이 파일에 작성되어 있다면 -f 옵션을 사용합니다.

`awk -f [액션이 적힌 파일명] [데이터가 있는 파일명]`

<awkcommand 내용>   
![image](https://user-images.githubusercontent.com/43658658/141235957-716b0a59-9529-4813-9c40-0921fdd41af2.png)   

![image](https://user-images.githubusercontent.com/43658658/141235972-5880803f-2ba3-42bc-b5e3-6db2e2995697.png)

> <h3>레코드와 필드</h3>

![image](https://user-images.githubusercontent.com/43658658/141305870-e0854df9-effd-436f-850a-94c278807dcb.png)   
![image](https://user-images.githubusercontent.com/43658658/141236413-e5b71f1c-abc5-466d-864d-637546bfbbe9.png)   
모든 레코드는 awk에서 `$0`로 참조됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141236610-e4aefeb9-7241-4c41-a60b-63586b483b07.png)   
`NR` : 레코드의 행 번호를 나타내는 빌트인 내장 변수.

![image](https://user-images.githubusercontent.com/43658658/141236737-6ff8ac15-2a9e-4677-b1cd-09d0ea6c2d70.png)   
`NF` : 레코드의 필드 수를 나타내는 빌트인 내장 변수.

> <h3>필드 분리자</h3>

awk는 필드 분리자인 FS의 값을 기준으로 필드를 분리합니다.

명령라인에서 필드 분리자 값을 변경하기 위해서는 `-F`옵션을 사용합니다.   
`awk -F:` : `:`를 기준으로 필드를 분리합니다.   
`awk -F'[ :\t]'` : `공백`, `:`, `Tab`을 기준으로 필드를 분리합니다.

![image](https://user-images.githubusercontent.com/43658658/141237423-b04a3325-e921-4846-aecb-2a72819682bc.png)   
`:`를 기준으로 필드를 분리합니다.   

## awk와 정규표현식

awk에서 패턴을 검색할 땐 슬래시(/)로 문자열을 둘러쌉니다.   
이때 패턴에는 정규표현식을 사용할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/141306650-3d07d5b3-0377-498f-bf9f-1202854d39c2.png)   
![image](https://user-images.githubusercontent.com/43658658/141238059-058d58c8-035c-4f0a-8598-465aaa7f2bd8.png)   
`Jung`으로 시작하는 라인의 1, 2, 3번 필드의 레코드를 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141238242-60f9a31e-3671-4c83-9eb1-00ce05238436.png)   
첫 번째 문자가 대문자로 시작하고, 두 번째 문자부터 소문자가 하나 이상이면서 마지막엔 공백이 들어가는 문자열로 시작하는 라인을 출력합니다.   

> <h3>match 연산자</h3>

틸드(`~`)로 표기되는 match 연산자는 하나의 레코드 또는 필드 안에서 표현식과 매칭되는 것이 있는지 검사하는 연산자입니다.

![image](https://user-images.githubusercontent.com/43658658/141306870-550b905b-2e27-4cc3-8a1d-439f541b0d79.png)   
![image](https://user-images.githubusercontent.com/43658658/141238588-66c603e6-588c-4c99-bff6-52395b33c86f.png)   
두 번째 필드 중에서 레코드가 `Gil` 또는 `gil`이 있는 라인을 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141238711-b1934101-2cdf-4b16-97c5-7a90b74cba80.png)   
두 번째 필드의 레코드가 `g`로 끝나지 않는 라인을 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141238906-a073c0e8-ebe2-4cf1-a057-72fad2f93237.png)   
`소문자 1개 이상 + g + 공백 1칸 이상 + 숫자` 형식을 검색해서 라인을 출력합니다.   

## 스크립트 파일에서 awk

<awkcommand2 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141239332-036036f3-77c7-4613-aac8-78888f6865b0.png)   
![image](https://user-images.githubusercontent.com/43658658/141239440-770b1274-e6bb-4879-aa10-f32d35efd7eb.png)   
액션이 같은 라인에 있을 경우 세미콜론으로 구분지어 줘야 합니다.   

## 비교 표현식

`awk '비교표현식{액션}' [데이터 파일명]`

![image](https://user-images.githubusercontent.com/43658658/141307144-76d84ec0-dade-4324-9d9f-c63169abf87d.png)   
![image](https://user-images.githubusercontent.com/43658658/141239565-31f1cf89-0efc-4e0a-a172-91a84454727e.png)   
3번 필드 값이 7000보다 큰 라인을 출력합니다.

![image](https://user-images.githubusercontent.com/43658658/141239765-aef9a8bc-792f-4473-bf3b-9e9c5e483e57.png)   
3번 필드 값이 7000보다 큰 라인의 1, 2번 필드 레코드를 출력합니다.

> <h3>조건 표현식</h3>

`(조건 표현식1) ? 표현식2 : 표현식3`

`awk '{max=($1 > $2) ? $1 : $2; print max}' awkfile`   
awkfile 파일 내용에서 1번 필드의 레코드와 2번 필드의 레코드를 비교해서 1이 크면 max에 `$1`을 할당하고, 반대면 `$2`를 할당한 후, max를 출력합니다.

> <h3>산술 연산자</h3>

`awk '$3 * $4 > 100' awkfile`   
awkfile 파일 내용에서 3번 필드의 레코드와 4번 필드의 레코드를 곱한 값이 100보다 큰 라인을 출력합니다.

> <h3>논리 연산자</h3>

`&&` : and   
`||` : or   
`!` : not

`awk '$3 > $5 && $3 <= 100 awkfile`   
3번 필드의 레코드가 5번 필드의 레코드보다 크고, 3번 필드의 레코드가 100이하인 라인을 출력합니다.

`awk '!($3 < 100 && $5 < 100)' awkfile`   
3번 필드의 레코드가 100이상이거나 5번 필드의 레코드가 100이상인 라인을 출력합니다.   

## awk 변수

`변수=표현식`

![image](https://user-images.githubusercontent.com/43658658/141241250-4f02a712-6e00-4c1f-8b50-b9ca08e95764.png)   
![image](https://user-images.githubusercontent.com/43658658/141241205-273afadf-feb1-49fa-af73-7b2efecd7dfd.png)

> <h3>증감 연산자(++, --)</h3>

`x++` : x = x + 1   
`x--` : x = x - 1   

![image](https://user-images.githubusercontent.com/43658658/141241431-ea3b349e-40e6-47c3-a87e-53d70ebc0ed2.png)   
두 식의 차이점은 `y=x++`와 `y=++x`입니다.   
`y=x++`는 `y=x`를 먼저 수행하고, `x=x+1`를 수행해 `x = 2, y = 1`의 결과가 나온 것입니다.   
`y=++x`는 `x=x+1`을 먼저 수행하고, 그 값을 `y`에 대입한 것이므로, `x = 2, y = 2`의 결과가 나온 것입니다.

> <h3>명령라인에서 사용자정의 변수</h3>

일반적으로 명령라인에서 사용자정의 변수를 선언할 때는 `-v` 옵션 사용하도록 합니다.   
![image](https://user-images.githubusercontent.com/43658658/141307762-2ef354a4-2e6b-430c-b2a9-47ef9b2b3872.png)   
![image](https://user-images.githubusercontent.com/43658658/141242779-e99f167d-27d0-462c-9423-bdf3e84c5430.png)   
var의 값이 2를 가지게 되면서 `{print $2}`의 액션이 완성되어 2번 필드를 출력하게 됩니다.

> <h3>빌트인 내장 변수 사용</h3>

<빌트인 내장 변수 목록>   
![image](https://user-images.githubusercontent.com/43658658/141287587-439c364b-584c-4a86-a2ef-0e190becfc99.png)

`NR`, `NF`와 같은 빌트인 내장 변수를 이용하게 되면 `변수의 값을 그대로 인식`하게 됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141307945-b3751113-47c6-4af5-a17c-81cf8ba675a6.png)   
![image](https://user-images.githubusercontent.com/43658658/141243541-ee364be6-017c-4546-82a8-b10e7d3da05a.png)   
`$NF`의 경우 내장 변수인 `NF`의 값은 awkfile 파일의 필드의 수인 `5`가 되므로 `$NF = $5`와 같아집니다. 따라서 5번 레코드를 출력합니다.   

> <h3>BEGIN 패턴</h3>

`BEGIN` 패턴은 awk가 입력 파일의 라인들을 처리하기 이전에 실행되며 액션 블록 앞에 놓입니다.   
`BEGIN` 블록은 awk가 BEGIN 액션 블록이 완료될 때까지 입력을 읽어들이지 않기 때문에 `입력 파일 없이 테스트`할 때 쓰일 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/141308377-17440578-6926-432a-bba4-7f3d316cfb60.png)

`BEGIN`은 빌트인 내장 변수(OFS, RS, FS 등)들의 값을 변경하기 위해, 사용자정의형 변수들의 초기값을 할당하기 위해 자주 사용합니다.

![image](https://user-images.githubusercontent.com/43658658/141308511-9c7420cc-eb7c-498c-b4e7-9689ceba4499.png)

> <h3>END 패턴</h3>

BEGIN과 반대로 `END` 패턴은 입력의 모든 라인이 처리되고 난 후에 처리됩니다.   
![image](https://user-images.githubusercontent.com/43658658/141244451-3fc099c7-8862-4dc3-9205-594b79d709a0.png)   
awkfile 파일에서 `Hong`이 들어간 라인이 있으면 `count`가 1씩 증가합니다.   
모든 입력 라인 처리가 끝나면 `END` 블록의 내용이 처리되면서 문자열이 출력됩니다.

BEGIN은 파일명 아규먼트 없이 실행될 수 있지만 END는 파일명 아규먼트가 반드시 필요합니다.

## awk 리다이렉션

> <h3>출력 리다이렉션</h3>

![image](https://user-images.githubusercontent.com/43658658/141308857-85f3b76c-c631-45ad-8ad4-0fcbf3335571.png)   
![image](https://user-images.githubusercontent.com/43658658/141244801-998673a6-f619-470b-acde-965bd57f2d48.png)   
출력할 내용을 `new_file`로 리다이렉션 시키면, awk 명령을 수행했을 땐 모니터에 아무런 결과가 나타나지 않습니다.   
`new_file`의 내용을 보면 출력할 내용이 저장된 것을 확인할 수 있습니다.

> <h3>입력 리다이렉션</h3>

![image](https://user-images.githubusercontent.com/43658658/141245397-b7d593aa-a7ae-4a3f-9f7a-763d55070740.png)   
`date` 명령의 결괏값을 `getline` 함수를 통해 변수 `d`에 저장합니다.   
![image](https://user-images.githubusercontent.com/43658658/141245470-15706b60-984c-4361-a35e-2a7b1e76f5bc.png)   
`date` 명령의 결괏값을 `getline` 함수를 통해 변수 `d`에 저장하고, `d`의 값을 나눠서 `year` 배열을 형성한 후, `year[1]`을 출력합니다.

![image](https://user-images.githubusercontent.com/43658658/141309616-d72914ba-700d-41bb-aaac-d5d973e95edf.png)   
![image](https://user-images.githubusercontent.com/43658658/141246299-8fc2ce2b-42ca-409f-a51f-7c3cdf543baa.png)   
먼저 BEGIN 블록이 실행됩니다. 이름을 물어보고 입력 리다이렉션으로 정보를 입력 받아 `name` 변수에 저장합니다.   
그리고 awkfile을 읽어들이면서 1번 필드 레코드 값 중에 일치하는 라인이 있는지 체크하면서 일치하는 라인이 있다면 문자열과 레코드 번호를 출력합니다.   
마지막으로 END 블록이 실행됩니다. 문자열과 `name` 변수의 값을 출력합니다.

![image](https://user-images.githubusercontent.com/43658658/141247214-1701b059-bd94-4033-b587-73c2c51aae54.png)   
`/etc/passwd`로부터 한 라인씩 읽어들입니다. 각 라인의 끝까지 읽으면 `lc` 변수에 1씩 더해집니다.   
`/etc/passwd` 파일의 총 라인 수를 알 수 있습니다.   

![image](https://user-images.githubusercontent.com/43658658/141310337-e5b261c9-bdb4-4a38-af44-82cca65d5c18.png)      
`ls *`의 결과가 `getline`으로 보내집니다. 한 라인씩 읽으면서 결괏값을 출력합니다.   

## awk 파이프

![image](https://user-images.githubusercontent.com/43658658/141310408-0c2d1b51-58e1-425b-a79d-76e42da4038a.png)   
![image](https://user-images.githubusercontent.com/43658658/141248552-8707911b-f5a0-4cdb-bc87-73808b8141a5.png)   
리눅스 명령어를 입력할 때는 큰따옴표로 묶어주어야 합니다.   

> <h3>파일과 파이프 닫기</h3>

awk 프로그램에서는 파일이나 파이프를 다시 읽고 쓰기 위해서는 첫 번째 파이프를 닫아주어야 합니다.   
왜냐하면 awk가 종료될 때까지 오픈된 상태로 남아있기 때문입니다.   
오픈된 상태로 남아 있으면 END 블록에서 문장들은 파이프에 영향을 받게 됩니다.

![image](https://user-images.githubusercontent.com/43658658/141248970-b1b33f73-f16c-4e4d-baad-97b0bfa81640.png)   
앞에서 오픈된 파이프를 END 블록에서 close를 이용해 닫아주는 모습입니다. 

> <h3>system 명령</h3>

빌트인 내장 함수인 system 함수는 리눅스 시스템 명령들을 실행합니다.

`system("리눅스 명령어")`

리눅스 명령어는 반드시 큰따옴표로 감싸주어야 합니다.

![image](https://user-images.githubusercontent.com/43658658/141249554-14873161-67af-47c5-996a-2a765a2bda5b.png)   
`awkscript` 파일에서 `system`함수가 사용되었습니다. `awktext` 파일의 1번 필드의 레코드들을 `cat`하는 명령인데,   
`awktext` 파일의 1번 필드의 레코드들은 데이터 테이블 파일을 가리키고 있습니다.   
즉, `cat awkfile`과 `cat awkfile_FS`를 명령한 것과 같은 의미가 됩니다.   
따라서, awkfile의 내용과 awkfile_FS의 내용을 보여줍니다.

## 조건문

> <h3>if</h3>

`awk '{if($7 > 100) print $1 "는(은) 100보다 크다"}' filename`   
filename의 7번 필드의 레코드 값이 100보다 크면, "[1번 필드의 레코드 값]는(은) 100보다 크다"라는 문자열을 출력합니다.

`awk '{if($7 > 20 && $8 <= 100){safe++; print "OK"}}' filename`   
filename의 7번 필드의 레코드 값이 20보다 크고, 8번 필드의 레코드 값이 100 이하이면, safe 변수의 값을 1 증가시키고, "OK" 문자열을 출력합니다.

> <h3>if/else</h3>

`awk '{if($7 > 100) print $1 "100보다 크다"; else print "100보다 작다"}' filename`   
filename의 7번 필드의 레코드 값이 100보다 크면, "[1번 필드의 레코드 값] 100 보다 크다" 문자열을 출력하고,   
아니라면, "100보다 작다" 문자열을 출력합니다.

`awk '{if($7 > 100) {count++; print $1} else { x+1; print $2 }}' filename`   
filename의 7번 필드의 레코드 값이 100보다 크면, count를 1 증가시키고, 1번 필드의 레코드를 출력하고,   
아니라면, x를 1 증가시키고, 2번 필드의 레코드를 출력합니다.

> <h3>if/else if/else</h3>

![image](https://user-images.githubusercontent.com/43658658/141252642-b0591dc5-099d-4fa4-b321-351b819159eb.png)

## loop 순환문

> <h3>while 루프</h3>

![image](https://user-images.githubusercontent.com/43658658/141311331-aafaf453-4358-4060-9c6e-5f9389130529.png)   
![image](https://user-images.githubusercontent.com/43658658/141259592-b9ee9535-2433-489a-bae0-e70496722501.png)   
한 라인 씩 읽어들이면서 awkdata에 있는 필드 순서대로 `NF, $i, i++`의 값을 출력합니다.

![image](https://user-images.githubusercontent.com/43658658/141260068-4d1a0639-b649-4703-903d-8bdd37b5ec4f.png)   
위의 while문을 for문으로 바꾼 것입니다.  

## 프로그램 관리 문장

> <h3>next 문장</h3>

`next`는 입력 파일로부터 입력의 다음 라인을 가져오고, awk 스크립트의 시작부터 다시 실행합니다.

![image](https://user-images.githubusercontent.com/43658658/141260433-2e30dc77-c793-4e7c-9c28-e4a8abd8a077.png)   
1번 필드의 레코드값이 `Jane`이라면 awk는 이 라인을 스킵하고 입력 파일로부터 다음 라인을 가져옵니다.

> <h3>exit 함수</h3>

`exit`는 awk 문장을 종료하기 위해 사용합니다. 레코드 처리는 중단하지만 END 문장은 스킵하지 않습니다.   
![image](https://user-images.githubusercontent.com/43658658/141261176-55c8102b-3c5a-4896-b7d2-66cbcd05c57a.png)   
`exit (1)`로 종료하게 되면 종료상태 변수값은 1인 것을 확인할 수 있습니다.

## 배열

> <h3>연관 배열을 위한 서브 스크립트</h3>

![image](https://user-images.githubusercontent.com/43658658/141262241-ab5c9618-7c67-448f-ba11-488411b3274c.png)   
name이라는 배열에 name=(85 91 74)를 저장합니다.   
END 블록에서 for문을 처리합니다. NR은 액션이 진행되면서 NR=2까지 증가된 상태입니다.

![image](https://user-images.githubusercontent.com/43658658/141263585-f6e6e664-134c-46f9-bd82-a52b0d3ae0a5.png)   
id 배열에 id=(88 -1 98)을 할당하고, END 블록에서 for문을 처리합니다.   

> <h3>특수 for문</h3>

![image](https://user-images.githubusercontent.com/43658658/141264797-15af9e6e-05f9-411d-8d9a-5ffebc671bd7.png)   
`for(i in name)`으로 반복문을 돌리면 i에 들어가는 값은 name에 할당된 인덱스를 순서로 출력됩니다.

![image](https://user-images.githubusercontent.com/43658658/141266735-d569e505-8f97-4306-aa3b-4c476096bf8e.png)   
인덱스에는 문자가 들어갈 수도 있습니다. count 배열의 인덱스는 각 이름의 성이 들어가고, 같은 성이 나올 경우 count[성]의 값을 1 증가시킵니다.

> <h3>split</h3>

![image](https://user-images.githubusercontent.com/43658658/141267334-35cbefa4-18f0-40de-9bb1-ad002d4ecb86.png)   
`7/21/2009`를 date라는 배열에 `/`를 기준으로 나눠서 저장합니다.

> <h3>delete</h3>

![image](https://user-images.githubusercontent.com/43658658/141312854-dd6855a8-8843-499a-904b-3b19f888df30.png)   
split으로 `7/21/2009`를 나누고 date[3]을 지웁니다.

> <h3>다차원 배열</h3>

![image](https://user-images.githubusercontent.com/43658658/141268939-68c5bc11-8a9f-481b-a5c2-5c7db3bcb8a5.png)   
NR : 행, NF : 열, NR은 END 블록 전에는 for문이 한 번씩 돌때마다 1씩 증가합니다.   
matrix에 2차원 배열로 저장됩니다.   
END 블록에서 출력을 담당하는데, x는 행, y는 열로 해서 한 행을 모두 출력하면 한 줄을 띄우도록 했습니다.

> <h3>ARGV와 ARGC</h3>

`ARGV` : 명령라인에서 아규먼트를 요소로 가지는 빌트인 내장 배열입니다.   
`ARGC` : 명령라인에서 아규먼트 수를 가지는 빌트인 내장 변수입니다.   

![image](https://user-images.githubusercontent.com/43658658/141270992-f5bcca1a-36c2-493e-8b7e-5066ac68029a.png)   
ARGV[0]에는 항상 `awk`이 들어가고, 1부터 아규먼트들이 들어있는 것을 확인할 수 있습니다.

## awk 빌트인 함수

> <h3>sub, gsub</h3>

`sub (정규표현식, 치환할 문자열);` : 각 라인별로 첫 번째로 정규표현식에 해당하는 문자를 치환할 문자열로 치환합니다.   
`gsub (정규표현식, 치환할 문자열);` : sub와 역할이 같은데 전체 문자를 치환합니다.

`awk '{sub(/Li/, "Linux"); print}' filename`   
![image](https://user-images.githubusercontent.com/43658658/141273029-c8459b9f-c3c2-4df9-b5dd-855607dc42ef.png)   
filename에서 `Li` 문자열을 Linux로 치환합니다.   

`awk '{sub(/Li/, "Linux", $1); print}' filename`   
![image](https://user-images.githubusercontent.com/43658658/141273142-bfd4b3e0-339a-4bc0-a003-86e1dca679a2.png)   
filename의 1번 필드에서 `Li` 문자열을 Linux로 치환합니다.

`awk '{gsub(/Li/, "Linux"); print}' filename`   
![image](https://user-images.githubusercontent.com/43658658/141273258-fff6e585-187e-4834-819b-0f5ef819bbba.png)   
전체 문자열이 바뀐 것을 볼 수 있습니다.

> <h3>index 함수</h3>

`index(string, substring)` : string에서 substring이 나오는 위치를 반환

![image](https://user-images.githubusercontent.com/43658658/141274573-10e5930d-1802-4cfd-b2a2-34730849b018.png)   
Hello에서 lo는 4번째 위치부터 나오므로 4가 반환됩니다.   

> <h3>length 함수</h3>

`length(문자열)` : 문자열의 길이를 반환

![image](https://user-images.githubusercontent.com/43658658/141274765-6594e623-e716-454d-a866-736b9dbf1ed1.png)   

> <h3>substr 함수</h3>

`substr(문자열, 시작 위치)` : 주어진 문자열에서 시작 위치의 앞까지 모두 자른 후 남아있는 문자열을 반환   
`substr(문자열, 시작 위치, 문자열 길이)` : `주어진 문자열`에서 `시작 위치`의 앞까지 모두 자른 후 남아있는 문자열을 `문자열 길이`만큼 반환

![image](https://user-images.githubusercontent.com/43658658/141275162-2d18f7b1-bfa5-4b79-9ce4-3b64023f965f.png)   
7번째 위치인 `C` 전인 공백까지 자르고 `C`부터 길이 2의 문자열인 `Cl`을 반환합니다.

> <h3>match 함수</h3>

`match(문자열, 정규표현식)` : 문자열에서 정규표현식을 만족하는 문자열의 인덱스를 반환

![image](https://user-images.githubusercontent.com/43658658/141275427-f3ac9d35-aa70-463d-b33b-94f6576f882a.png)   
`/[A-Z]+$/`는 라인의 끝이 대문자가 1개 이상인 문자열을 의미합니다. LINUX가 만족하는 문자열이고 위치는 `8번째`입니다.

> <h3>split 함수</h3>

`split(문자열, 배열, 필드분리자)` : 문자열을 필드분리자로 나눠서 배열에 요소로 넣습니다.

![image](https://user-images.githubusercontent.com/43658658/141275965-d732db27-a7b2-4f94-a8d5-638e74bfc46c.png)   
`/`를 기준으로 `7/21/2009`를 나눠서 date 배열에 1부터 집어넣습니다.

> <h3>sprintf 함수</h3>

`변수=sprintf("포맷 형식의 문자열", 표현식1, 표현식2, ... , 표현식n)` : 변수에 포맷 형식으로 출력된 표현식을 반환

![image](https://user-images.githubusercontent.com/43658658/141276597-c07f673d-3c43-4a8e-9fc7-3e47e9ba6697.png)   
`%-10s %6.2f`형식의 문자열을 변수에 할당하고 변수를 출력합니다.

> <h3>toupper와 tolower 함수</h3>

`toupper(문자열)` : 소문자를 대문자로 변경하는 함수   
`tolower(문자열)` : 대문자를 소문자로 변경하는 함수

![image](https://user-images.githubusercontent.com/43658658/141284792-3631c807-161f-4de1-9e9b-a2eb7a38d21f.png)

> <h3>systime 함수</h3>

`systime()` : 1970년 1월 1일부터 현재 시간까지의 초단위 시간을 반환

![image](https://user-images.githubusercontent.com/43658658/141285020-07b92a23-1aec-45ba-8f65-c5e84236ac02.png)

> <h3>strftime 함수</h3>

`strftime(포맷, 시간)` : 시간을 포맷의 형태로 보여줍니다.

<시간 포맷>   
![image](https://user-images.githubusercontent.com/43658658/141285276-028c203b-7328-4f82-8ec5-4c50e434ae08.png)   

![image](https://user-images.githubusercontent.com/43658658/141285546-c50dd661-5e96-4027-834d-45b478fccef5.png)   
시간을 포맷의 형태로 보여줍니다. 시간을 입력하지 않으면 현재 시간으로 포맷합니다.

## awk 수학적 빌트인 함수

> <h3>정수형 함수</h3>

![image](https://user-images.githubusercontent.com/43658658/141279410-3d737a6e-a198-47de-8c63-9806536cec02.png)   
`int`를 쓰면 소수점을 버리고 정수로 만들어줍니다.

> <h3>rand 함수</h3>

![image](https://user-images.githubusercontent.com/43658658/141280069-d45c5da9-6d7b-4200-bcc6-ea2fdd48c2f2.png)   
rand 함수는 난수를 만들어줍니다. 하지만 한 번 실행하게 되면 같은 난수가 발생하게 됩니다.

> <h3>srand 함수</h3>

![image](https://user-images.githubusercontent.com/43658658/141280480-02fefda7-7bef-4d16-9537-244ad6290acf.png)   
srand 함수를 앞에 써주고 뒤에 rand 함수를 써주면 난수가 매번 다르게 발생합니다.

## 사용자정의형 함수

```
function 함수이름 (파라미터, 파라미터, 파라미터, ...) {
  문장
  return 표현식
}
```

> <h3>행의 숫자를 오름차순으로 정렬</h3>

<numbers 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141282103-04f7e432-b0b4-4e91-a7dd-bc61e9cef08a.png)   
<sortnumbers 파일 내용>   
![image](https://user-images.githubusercontent.com/43658658/141314871-be58205e-c873-41ed-a5c3-3205ddadba2e.png)   
![image](https://user-images.githubusercontent.com/43658658/141281514-571398c1-49f6-4f74-9f51-87cbcd268527.png)   
numbers에서 라인 내용을 하나씩 받아서 sortnumbers 스크립트 내용을 수행합니다.

## 기타

> <h3>고정폭 필드</h3>

파일 내용이 고정된 너비를 가지고 있을 때 필드 분리자로 분리할 수 없으면 `substr`를 활용할 수 있습니다.

![image](https://user-images.githubusercontent.com/43658658/141283272-2dd3dec0-f920-4260-92dc-37a64f114897.png)   
`substr`를 이용해 6자리, 6자리, 나머지 자리로 구분해서 문자열을 자르고 중간에 공백을 써넣어서 필드를 생성할 수 있습니다.

> <h3>`$`, 콤마(`,`) 속 숫자 총합 구하기</h3>

![image](https://user-images.githubusercontent.com/43658658/141284355-3b634d3b-c899-426d-b84a-747d49cdc309.png)   
먼저 필드를 `:`를 기준으로 나눠주고, `gsub`를 통해 `$`와 `,`를 null값으로 바꿔준 후, 4번 필드값을 모두 더해줍니다.

> <h3>멀티라인 레코드</h3>

![image](https://user-images.githubusercontent.com/43658658/141286388-805c6787-3e25-4810-ac79-ac81ef4a4af3.png)   
![image](https://user-images.githubusercontent.com/43658658/141286349-780e2354-079a-4b45-bf9f-b1b39c9dd542.png)   
RS(입력 레코드 분리자) : 각 레코드를 무엇을 기준으로 분리할 것인지      
FS(입력 필드 분리자) : 필드를 무엇을 기준으로 분리할 것인지   
QRS(출력 레코드 분리자) : 레코드가 출력될 때 어떻게 분리할 것인지

> <h3>다차원 배열에서 특정 값 뽑아내기</h3>

![image](https://user-images.githubusercontent.com/43658658/141279036-dc83689f-dabf-4193-a447-28103270c64d.png)
















