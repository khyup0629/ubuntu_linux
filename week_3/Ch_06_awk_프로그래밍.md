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

