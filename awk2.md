# awk description 2
위에는 한번 간단하게 결과를 보고 약간 수정한거고 제가 알아볼만한 결과로 정리 다시 하는걸로 하겠습니다....

awk는 기능에서 나온 단어가 아닌 디자인 사람의 이니셜을 조합하여 만든 이름이다
(**A**ho + **W**einberger + **K**ernighan.)\
파일로부터 레코드를 선택하고 선택된 레코드에 포함된 값을 조작하거나 데이터화 하는 것을 목적으로 사용하는 프로그램.\
awk는 "awk programming language"라는 프로그래밍 언어로 작성된 프로그램을 실행함\

## 명령어의 기본형식
```
awk [Option...] [awk program ...] [ARGUMENT...]
    Option
        -F : 필드 구분 문자 지정
        -f : awk program 파일경로 지정.
        -v : awk program에서 사용될 특정 변수값 지정.
    awk program
        -f : 옵션이 사용되지 않은 경우, awk 가 실행할 코드 지정.
    ARGUMENT
        입력 팡리 지정 또는 variable 값 지정.

```
### awk program 부분의 기본구조는
`'pattern { action }'`\
이렇게 single quotation marks 안에 작성하고, pattern과 action은 생략가능\
생략시 패턴은 모든 레코드가 적용되고 액션은 print가 적용됨\
`awk '{ print } ./file.txt     #모든 레코드 출력`\
`akw '/p/' ./ile.txt           #p를 포함하는 레코드 출력.`


레코드는 $0\
필드는$1,$2,...,$n

## 사용 예시
|awk 사용 예|명령어 옵션|
|:---|:---|
|파일의 전체 내용 출력|awk '{ print }' [FILE]|
|필드 값 출력|awk '{ print $1 }' [FILE]|
|필드 값에 임의 문자열을 같이 출력	|awk '{print "STR"$1, "STR"$2}' [FILE]|
|지정된 문자열을 포함하는 레코드만 출력	|awk '/STR/' [FILE]|
|특정 필드 값 비교를 통해 선택된 레코드만 출력	|awk '$1 == 10 { print $2 }' [FILE]|
|특정 필드들의 합 구하기	|awk '{sum += $3} END { print sum }' [FILE]|
|여러 필드들의 합 구하기	|awk '{ for (i=2; i<=NF; i++) total += $i }; END { print "TOTAL : "total }' [FILE]|
|레코드 단위로 필드 합 및 평균 값 구하기	|awk '{ sum = 0 } {sum += ($3+$4+$5) } { print $0, sum, sum/3 }' [FILE]|
|필드에 연산을 수행한 결과 출력하기	|awk '{print $1, $2, $3+2, $4, $5}' [FILE]|
|레코드 또는 필드의 문자열 길이 검사	|awk ' length($0) > 20' [FILE]|
|파일에 저장된 awk program 실행	|awk -f [AWK FILE] [FILE]|
|필드 구분 문자 변경하기	|awk -F ':' '{ print $1 }' [FILE]|
|awk 실행 결과 레코드 정렬하기	|awk '{ print $0 }' [FILE]|
|특정 레코드만 출력하기	|awk 'NR == 2 { print $0; exit }' [FILE]|
|출력 필드 너비 지정하기	|awk '{ printf "%-3s %-8s %-4s %-4s %-4s\n", $1, $2, $3, $4, $5}' [FILE]|
|필드 중 최대 값 출력	|awk '{max = 0; for (i=3; i<NF; i++) max = ($i > max) ? $i : max ; print max}' [FILE]|

## awk 표현식
```
    (E),    $n,     ++E,    --E,    E++,    E--,    E^E,    !E,     +E,
    -E,     E*E,    E/E,    E%E,    E+E,    E-E,    E E,    E<E,    E<=E,
    E!=E,   E==E,   E>E,    E>=E,   E~E,    E!-E,   E in array,     (n) in array, 
    E&&E,   E||E,   E1?E2:E3        V^=E,   V%=E,   V*=E,   V/=E,   V+=E,
    V-=E,   V=E
```
## 키워드
```
    BEGIN   delete  END     function    in      printf
    break   do      exit    getline     next    return
    continue        else    for         if      print      while
```
## 정의된 변수
```
    ARGC        : ARGV 배열 요소의 갯수.
    ARGV        : command line argument에 대한 배열.
    CONVFMT     : 문자열을 숫자로 변경할 때 사용할 형식. (ex, "%.6g")
    ENVIRON     : 환경변수에 대한 배열.
    FILENAME    : 경로를 포함한 입력 파일 이름.
    FNR         : 현재 파일에서 현재 레코드의 순서 값.
    FS          : 필드 구분 문자. (기본 값 = space)
    NF          : 현재 레코드에 있는 필드의 갯수.
    NR          : 입력 시작 점에서 현재 레코드의 순서 값.
    OFMT        : 문자열을 출력할 때 사용할 형식.
    OFS         : 결과 출력 시 필드 구분 문자. (기본 값 = space)
    ORS         : 결과 출력 시 레코드 구분 문자. (기본 값 = newline)
    RLENGTH     : match 함수에 의해 매칭된 문자열의 길이.
    RS          : 레코드 구분 문자. (기본 값 = newline)
    RSTART      : match 함수에 의해 매칭된 문자열의 시작 위치.
```
## 사용할 수 있는 함수
```
Arithmetic Functions :
    atan2(y,x),     cos(x),     sin(x),     exp(x),     log(x),     sqrt(x),
    int(x),         rand(),     srand([expr])

String Functions :
    gsub(ere, repl[, in]),      index(s, t),            length[([s])],
    match(s, ere),              split(s, a[, fs ]),     sprintf(fmt, expr, expr, ...),
    sub(ere, repl[, in ]),      substr(s, m[, n ]),     tolower(s),
    toupper(s)

Input/Output and General Functions :
    close(expression),          getline                 getline var
    system(expression)
```
---
### sed 설명

sed 명령어는 동작시 내부적으로 두개의 워크스페이스(버퍼)를 사용한다. 둘은 각기 패턴스페이스와 홀드스페이스라고 하며 다음과 같이 동작한다\
<img alt="sed buffer" src="https://snipcademy.com/img/articles/shell-scripting-sed/sed.svg">\
sed 편집기는 원본을 그대로 둔상태에서 명령들을 수행하고 저장하기 전까지는 원본에 아무런 영향을 끼치지 않는 특징이 있다.

기본형태는 아래와 같이 사용됨.
`sed -n -e 'command' [input file]`


## sed subcommand 종류
|subcommand|의미|
|---|:---|
|a\|현재 행에 하나 이상의 새로운 행을 추가|
|c\|현재 행의 내용을 새로운 내용으로 교체|
|d|행 삭제|
|i\|현재 행의 위에 텍스트 삽입|
|h|패턴 스페이스의 내용을 홀드 스페이스에 복사|
|H|패턴 스페이스의 내용을 홀드 스페이스에 추가|
|g|홀드 스페이스의 내용을 패턴 스페이스에 복사(비어 있지 않은 경우 덮어쓰기)|
|G|홀드 스페이스의 내용을 패턴 스페이스에 복사(비어 있지 않은 경우 추가)|
|l|출력되지 않는 특수문자를 명확하게 출력|
|p|행을 출력|
|n|다음 입력 행을 첫 번째 명령어가 아닌 다음 명령어에서 처리|
|r|sed를 종료|
|q|파일로부터 행을 읽음|
|!|선택된 행을 제외한 나머지 전체 행에 명령어를 적용|
|s|문자열을 치환|

## 치환플래그
|플래그|의미|
|---|---|
|g|치환이 행 전체에 대해 이뤄짐|
|p|행을 출력|
|w|파일에 쓴다.|
|x|홀드 버퍼와 패턴 스페이스의 내용을 서로 맞바꿈|
|y|한 문자를 다른 문자로 변환 (정규표현식 메타문자를 사용할 수 없음)|

## 예시

### 전체내용 출력
`sed -n -e '1, $p' file.txt`
`sed -n -e '/$/p' file.txt`

### (start) ~ (end)출력 
`sed -n -e '(start),(end)p' file.txt`

### 다중 커맨드 사용 (첫줄 + 9~끝줄)
`sed -n -e '1p' -e '9,$p file.txt' `

### 특정 문자열이 있는 줄 출력
`sed -n -e '/특정 문자열/p' file.txt

### 단어치환
s/old/new/g(i)
old는 치환할 문자열, new는 치환한 문자열, g는 전체에 적용, i는 대소문자 구분하지 않는다.
`sed -n -e 's/old/new/g' e '2p' file.txt`
