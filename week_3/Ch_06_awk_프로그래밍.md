# awk 프로그래밍

## awk 란?

데이터를 조작하고 리포트를 생성하기 위해 사용하는 언어입니다.

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

![image](https://user-images.githubusercontent.com/43658658/141235620-8c73e4a5-6a79-45d2-8a0d-434ba1f3005f.png)   
1, 2번 필드의 값이 `%-20s`로 들어가고, 3번 필드의 값이 `%4d`로 들어갑니다.   

> <h3>awk -f</h3>

awk 액션과 명령이 파일에 작성되어 있다면 -f 옵션을 사용합니다.

`awk -f [액션이 적힌 파일명] [데이터가 있는 파일명]`

<awkcommand 내용>   
![image](https://user-images.githubusercontent.com/43658658/141235957-716b0a59-9529-4813-9c40-0921fdd41af2.png)   

![image](https://user-images.githubusercontent.com/43658658/141235972-5880803f-2ba3-42bc-b5e3-6db2e2995697.png)

> <h3>레코드와 필드</h3>

![image](https://user-images.githubusercontent.com/43658658/141236413-e5b71f1c-abc5-466d-864d-637546bfbbe9.png)   
모든 레코드는 awk에서 `$0`로 참조됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141236610-e4aefeb9-7241-4c41-a60b-63586b483b07.png)   
레코드의 번호는 `NR`이라는 변수에 저장됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141236737-6ff8ac15-2a9e-4677-b1cd-09d0ea6c2d70.png)   
각 레코드의 필드의 수는 `NF` 변수에 저장됩니다.

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

![image](https://user-images.githubusercontent.com/43658658/141238059-058d58c8-035c-4f0a-8598-465aaa7f2bd8.png)   
`Jung`으로 시작하는 라인의 1, 2, 3번 필드의 레코드를 출력합니다.   

![image](https://user-images.githubusercontent.com/43658658/141238242-60f9a31e-3671-4c83-9eb1-00ce05238436.png)   
첫 번째 문자가 대문자로 시작하고, 두 번째 문자부터 소문자가 하나 이상이면서 마지막엔 공백이 들어가는 문자열로 시작하는 라인을 출력합니다.   

> <h3>match 연산자</h3>

틸드(`~`)로 표기되는 match 연산자는 하나의 레코드 또는 필드 안에서 표현식과 매칭되는 것이 있는지 검사하는 연산자입니다.

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

`awk '$3 > $5 && $3 <= 100` awkfile`   
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
![image](https://user-images.githubusercontent.com/43658658/141242779-e99f167d-27d0-462c-9423-bdf3e84c5430.png)   
var의 값이 2를 가지게 되면서 `{print $2}`의 액션이 완성되어 2번 필드를 출력하게 됩니다.

> <h3>빌트인 내장 변수 사용</h3>

`NR`, `NF`와 같은 빌트인 내장 변수를 이용하게 되면 `변수의 값을 그대로 인식`하게 됩니다.   

![image](https://user-images.githubusercontent.com/43658658/141243541-ee364be6-017c-4546-82a8-b10e7d3da05a.png)   
`$NF`의 경우 내장 변수인 `NF`의 값은 awkfile 파일의 필드의 수인 `5`가 되므로 `$NF = $5`와 같아집니다. 따라서 5번 레코드를 출력합니다.   

> <h3>BEGIN 패턴</h3>

`BEGIN` 패턴은 awk가 입력 파일의 라인들을 처리하기 이전에 실행되며 액션 블록 앞에 놓입니다.   
`BEGIN` 블록은 awk가 BEGIN 액션 블록이 완료될 때까지 입력을 읽어들이지 않기 때문에 `입력 파일 없이 테스트`할 때 쓰일 수 있습니다.

`BEGIN`은 빌트인 내장 변수(OFS, RS, FS 등)들의 값을 변경하기 위해, 사용자정의형 변수들의 초기값을 할당하기 위해 자주 사용합니다.

> <h3>END 패턴</h3>

BEGIN과 반대로 `END` 패턴은 입력의 모든 라인이 처리되고 난 후에 처리됩니다.   
![image](https://user-images.githubusercontent.com/43658658/141244451-3fc099c7-8862-4dc3-9205-594b79d709a0.png)   
awkfile 파일에서 `Hong`이 들어간 라인이 있으면 `count`가 1씩 증가합니다.   
모든 입력 라인 처리가 끝나면 `END` 블록의 내용이 처리되면서 문자열이 출력됩니다.

BEGIN은 파일명 아규먼트 없이 실행될 수 있지만 END는 파일명 아규먼트가 반드시 필요합니다.

## awk 리다이렉션

> <h3>출력 리다이렉션</h3>

![image](https://user-images.githubusercontent.com/43658658/141244801-998673a6-f619-470b-acde-965bd57f2d48.png)   
출력할 내용을 `new_file`로 리다이렉션 시키면, awk 명령을 수행했을 땐 모니터에 아무런 결과가 나타나지 않습니다.   
`new_file`의 내용을 보면 출력할 내용이 저장된 것을 확인할 수 있습니다.

> <h3>입력 리다이렉션</h3>

![image](https://user-images.githubusercontent.com/43658658/141245397-b7d593aa-a7ae-4a3f-9f7a-763d55070740.png)   
`date` 명령의 결괏값을 `getline` 함수를 통해 변수 `d`에 저장합니다.   
![image](https://user-images.githubusercontent.com/43658658/141245470-15706b60-984c-4361-a35e-2a7b1e76f5bc.png)   
`date` 명령의 결괏값을 `getline` 함수를 통해 변수 `d`에 저장하고, `d`의 값을 나눠서 `year` 배열을 형성한 후, `year[1]`을 출력합니다.

![image](https://user-images.githubusercontent.com/43658658/141246299-8fc2ce2b-42ca-409f-a51f-7c3cdf543baa.png)   
먼저 BEGIN 블록이 실행됩니다. 이름을 물어보고 입력 리다이렉션으로 정보를 입력 받아 `name` 변수에 저장합니다.   
그리고 awkfile을 읽어들이면서 1번 필드 레코드 값 중에 일치하는 라인이 있는지 체크하면서 일치하는 라인이 있다면 문자열과 레코드 번호를 출력합니다.   
마지막으로 END 블록이 실행됩니다. 문자열과 `name` 변수의 값을 출력합니다.

![image](https://user-images.githubusercontent.com/43658658/141247214-1701b059-bd94-4033-b587-73c2c51aae54.png)   
`/etc/passwd`로부터 한 라인씩 읽어들입니다. 각 라인의 끝까지 읽으면 `lc` 변수에 1씩 더해집니다.   
`/etc/passwd` 파일의 총 라인 수를 알 수 있습니다.   

![image](https://user-images.githubusercontent.com/43658658/141248330-c40c22ad-4327-4c4c-8f6f-7f5fb5b58c3e.png)   
"ls *"의 결과가 `getline`으로 보내집니다. 한 라인씩 읽으면서 결괏값을 출력합니다.   

## awk 파이프

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











