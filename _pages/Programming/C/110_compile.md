---
title: 컴파일
date: 2025-02-05 134900
---
---
# 컴파일
* 컴파일 -> o file 생성
* o file간 링킹 -> 실행파일 생성

---
# 단순 파일 쪼개기
* 함수 내용만 한 파일에 넣고
* 호출하는 함수에서는 어찌됐든 <font color = 'ff8000'><b>프로토타입 명시</b></font>해줘야함
* 컴파일 시 동시에 컴파일 하면 됨

```c
T1.cpp
#include <stdio.h>
int tt() {
    printf("테스트 \n");
}
```
```c
T2.cpp
int tt();
int main() {
    tt();
}
```
```text
gcc -x c T1.cpp T2.cpp -o A_out
./A_out
테스트
```

---
# 헤더
* 프로토타입을 계속해서 명시하는 건 <font color = 'ff8000'><b>비효율적</b></font>
* 프로토타입을 특정 파일 안에 몰아놓고 그 파일을 \#include
  * \#include는 컴파일 이전에 소스코드 100% 복붙되므로 프로토타입을 나열하는 것과 100% 같은 효과임
  * 안쓰는 함수는 컴파일러에 포함된 링커가 알아서 삭제함

* 좋은 프로그램일수록 main()에서 하는일이 없어짐

* 구조
  * func.c : 함수를 만들어둘 c
    * 상단에 프로토타입 대신에 func.h를 include하는 게 좋다 꼭 필요한 건 아님
  * func.h : func.c의 헤더
    * 굳이 func.c와 같은 이름일 필요는 없지만 관행적으로 같은 이름 사용
    * 헤더에다가 함수를 직접 구현하기도 함
  * main.c : func.h를 include
  * 컴파일 시 함께 컴파일 해야 함
    * 헤더에 직접 코드 구현한 경우는 하나만 하면 됨

* 이렇게 나누는 걸 모듈화 프로그래밍이라고 칭함
* 헤더에는 프로토타입만 넣지 않음
  * 글로벌변수
  * 구조체, 공용체, 열거형
  * 인라인 함수
  * 매크로

```c
T1.c
#include <stdio.h>
#include "T1.h"
int tt() {
    printf("테스트2 \n");
}
```

```c
T1.h
int tt();
```

```c
T2.c
#include "T1.h"
int main() {
    tt();
}
```
```text
gcc -x c T1.cpp T2.cpp -o A_out
./A_out
테스트2
```

---
# 외부 소스
* 미리 만든 함수들의 모임을 라이브러리라고 함
* \#include
  * stdio.h
  * string.h
 
---
# Preprocessor 전처리기
* 컴파일 이전에 정확하게 동일한 코드로 대체됨
* \#으로 시작하며 ;을 붙이지 않는다
* \#include
* \#define : 매크로이름에 해당하는 부분을 값으로 치환

```c
 #define VAR 10 // 값은 안넣어도 된다
 ...
 int arr[VAR];
```
* \#ifdef #endif : 매크로이름이 정의되어 있으면 코드에 포함, 아니면 주석처리됨
  * <font color = 'ff8000'><b>조건부컴파일</b></font>
  * 물리적인 환경이 다를 경우(어떤 cpu는 float, 어떤 건 double) 소스코드 body를 건들지 않고 상단의 define만 컨트롤하여 처리 가능(주석처리 하기 때문)
  * 헤더에 세팅하여 <font color = 'ff8000'><b>헤더가 여러번 include되더라도 한 번만 입력</b></font>되게 처리 가능

```c
 #define A 10
...
 #ifdef A  // ifndef : 정의되지 않은 경우
 printf("AAAA");
 #else
...
 #endif
```
```c
 //myheader.h 헤더파일 내에 아래와 같이 세팅
 #ifndef MY_HEADER_H
 #define MY_HEADER_H
 헤더 내용 여기에 작성
 ...
 #endif
```

* \#pragma
  * 컴파일러에게 말하는 전처리기 명령
  * 전처리기에 의해 처리되지만 명령은 컴파일러에 전달
  * C 기본이라기보다 컴파일러 종속
  * 기능은 다양하다.

```c
#include <stdio.h>
#define VAR 10

int main() {
    char arr[VAR] = {"hi"};
    printf("%s \n", arr);
}
```
```text
hi
```

---
# 매크로 함수
* \#define 함수 함수내용
  * 치환이므로 함수내용은 c문법을 따라야 함

* 단, 어디까지나 단순 치환일뿐이므로 조심해야 함
  * 굉장히 위험함

```c
  #define square(x) x * x

  square(3 + 1) = 3 + 1 * 3 + 1 = 7
```
* 해법 : **괄호**
  * 그럼 이건 문제가 없을까?

```c
  #define square(x) (x) * (x)

  square(3 + 1) = (3 + 1) * (3 + 1) = 16

  1 / square(3 + 1) = 1 / 4 * 4 = 1
```
* 진짜 해법 : 함수와 변수에 괄호

```c
#define square(x) ((x) * (x))

1 / square(3 + 1) = 1 / ((3 + 1) * (3 + 1)) = 1 / 16
```

* \# : 그냥 <font color = 'ff8000'><b>문자열리터럴</b></font>로 바꿈, \#a = "a"
* \#\# : a와 b를 그냥 붙임, a\#\#b = ab
  * 값을 붙이는 게 아니라 그냥 붙이는 거임
  * <font color = 'ff8000'><b>a##b는 ab변수에 접근</b></font>하는 것

```c
#define square(x) x * x
#define ss(x) #x
#define sss(x, y) x ## y

int main() {
    char a = '1', b = '2', ab = 'a';
    printf("square(x) : %d \n", square(3));
    printf("ss(x)     : %s \n", ss(a));
    printf("sss(x)    : %c \n", sss(a, b));
}
```
```text
square(x) : 9 
ss(x)     : a 
sss(x)    : a 
```

---
# 인라인 함수
* 원래 함수는 호출을 통해서 진행됨
  * main에서 함수 호출 -> 해당 <font color ='ff8000'><b>함수의 메모리</b></font>로 이동

* 이동과정에서 시간이 소요되므로 단순 연산의 경우 이동 없이 처리하는 게 효율적

* inline 함수
  * 컴파일러가 호출이 아니라 해당 함수 내용을 <font color = 'ff8000'><b>그대로 구현</b></font>함
  * 인라인함수는 매크로함수와 달리 전처리기가 아닌 컴파일러에 의해서 처리됨
  * 무조건 하는 건 아니고 컴파일러는 **똑똑**하기 때문에 더 효율적일 경우만 inline을 구현함

* 매크로함수보다 inline 함수의 사용을 권장한다.
* inline 함수는 보통 헤더 파일에 직접 구현한다.

* gcc
  * static inline으로 하는 게 좋음
  * gcc 옵션 중 -O2나 O3 옵션을 줘야함(최적화)

```c
static inline int max(int a, int b) {
    if (a > b)
        return a;
    else
        return b;
}
int main() {
    printf("%d", max(4,2));
}
```
```text
4
```

---
# enum 열거형
* <font color = 'ff8000'><b>상수 사전 정의</b></font>해놓기
  * 근데 const int로 하면 메모리가 낭비됨

* enum {RED, BLUD, WHITE, BLACK};
  * 각 순번대로 0, 1, 2, 3, ...이 자동 할당됨

* enum은 코딩시에만 정의되고, 컴파일러가 실제로는 상수로 다 바꿔버리기 때문에 메모리 낭비가 없음
  * 처음 수를 0으로 시작하기 싫다면?
    * enum {RED = 3, BLUD, WHITE, BLACK};
  * enum {RED = 3. BLUD, WHITE = 3, BLACK};
    * 3,4,3,4
    
* <font color = 'ff8000'><b>c언어의 자체기능</b></font>
  * 전처리기와는 다르다

```c
enum {RED, BLUD, WHITE, BLACK};

int main() {
    int i = 3;
    if (i == BLACK) printf("BLACK is 3 \n");
}
```
```text
BLACK is 3
```

---
# 최적화
* gcc에서는 최적화가 옵션으로 존재함
  * 해당 옵션을 주지 않으면 완전한 최적화는 안 됨
* 최적화가 항상 좋을까?
  * 의도치 않은 상황 발생
  * 메모리의 특정 위치에 값이 변할때까지 대기를 타야하는 경우, while문을 사용하게 되는데, 최적화를 하면 메모리의 값이 변할 가능성이 없다고 판단하여 무한루프를 돌지 않고 그냥 끝내버릴 수도 있음

* 변수 선언 시 volatile 명시하면 해결
