---
title: 변수
date: 2025-02-02 180000
---
---
# 변수선언

* c는 변수형에 굉장히 예민, 안 맞으면 바로 에러 또는 사일런트에러 발생
  * float는 입력 시 숫자 뒤에 f를 꼭 써줘야 함
* 선언 시 선언을 읽는 순위는 오른쪽에서 왼쪽
  * 타입이 제일 마지막에 정해진다
* char는 연산에 포함되면 그냥 정수형으로 취급되기 때문에 주의할 것
  * char도 unsigned 지정이 가능함
* float, double, long double은 _Complex와 같이 사용하면 복소수형으로 할당 가능

|Name       |byte  |Range             |
|:---------:|:----:|------------------|
|char       |1     |signed : -128 \~ 127 <br> unsigned : 0 \~ 255|
|short (int)|2     |signed : -32768 \~ 32767 <br> unsigned : 0 \~ 65535|
|int        |4     |signed : -2147483648 \~ 2147483647 <br> unsigned : 0 \~ 4294967295|
|long (int) |8(4)  |32bit면 4 bytes|
|long long int||
|bool       |1     | T F |
|float      |4     |±3.4e{±38}(7digits)|
|double     |8     | ±1.7e{±308}(15digits)|
|long double|유동  | ±1.7e{±308}(15digits)|

---
# 포맷 지정자

* int  : %{n}d
* long int : %ld, d로 작동 안 한다
  * **<font color ='ff8000'>int -> long int</font>**로 바꾸더라도 printf를 전부 신경써야 한다
* float, double : %{n.m}f


---
# const 상수

* 최초 선언 이후 변수를 바꿀 수 없고 바꾸면 에러 발생
  * const (변수형) (변수명) = (값);

* 정수형 상수는 10, 8, 16진법으로 작성 가능하다
  * 작성법만 다를뿐이지 똑같은 진법으로 기록됨
  * 10진법은 0으로 시작하면 안 됨
  * 8진법은 0으로 시작해야 함
  * 16진법은 0x로 시작해야 함 :영어 대소문자 구분 x
  * 섞어서 쓸 수도 있음 10 + 015 + 0x20

```c
int main() {
    int int_v = 123;
    int int_v2 = 12345;
    float float_v = 123.123123f; //float임을 명시하기위해 f를 붙임
    double double_v = 123.123123;
    printf("%%5d using int means pad 5 : %5d \n", int_v);
    printf("%%5d using int means pad 5 : %5d \n", int_v2);
    printf("%%5.2f using double        : %5.2f \n", double_v);
    printf("%%.25f using double        : %.25f \n", double_v);
}
```
```text
%5d using int means pad 5 :   123 
%5d using int means pad 5 : 12345 
%5.2f using double        : 123.12 
%.25f using double        : 123.1231230000000067548171501  <- 이건 부동소수점문제 
```

---
# 음수의 개념

* 1bit를 부호로 사용할 경우 +0과 -0이 존재하여 확인연산 필요 = 자원 낭비
  * 부동소수점은 부호비트를 도입
* int는 2의 보수 표현을 사용
  * a에 대한 N의 보수 = $N^{m}$ - a where $m = min(m) \ where \ N^{m} \ > \ a$
  * A와 B를 더했을 때 0이되게끔 하는 숫자(비트 늘어나면 맨 앞비트 버림)
  * 비트를 반전시킨 후 1을 더하면 됨(각 비트를 1<>0 교환)
* 2의 보수 표현을 통한 부호 표기는 정해진 비트체계 하에서만 성립되며, 비트체계가 달라지면 그 수치또한 변함
  * 따라서 2의 보수 표현을 따른 숫자는 실제 2진법과도 다르다
  * int와 unsigned int의 값이 다름
* 맨 앞비트가 1이면 음수임
* 이런 특수성 때문에 <font color='ff8000'> **Overflow** </font>가 발생
  * 2,147,483,647 + 1 = <font color = 'ff8000'>**-2,147,483,648**</font>
  * unsigned int에 -1 넣으면 4,294,967,295

```c
int main() {
    int a = 2147483647;
    unsigned int b = 4294967295;
    printf("a : %d \n", a++);
    printf("a : %d \n", a);

    printf("b : %u \n", b++);
    printf("b : %u \n", b--);
    printf("b : %u \n", b);
}
```

```text
a : 2147483647 
a : -2147483648 
b : 4294967295 
b : 0 
b : 4294967295 
```

---
# overflow 오버플로우

* 메모리 내에서 표현 가능한 수치를 넘어섰을 때 잘못된 수치가 표기되는 것
* <font color = 'ff8000'><b>잡을 수 있는 방법이 없으므로 코딩을 잘해야 함</b></font>
* 오버플로우 : 수치연산에서 최대값이나 최소값을 초과(버림 발생)
* 버퍼 오버플로우 : 할당한 메모리공간을 초과하였을 경우, 다른 메모리 영역에 데이터를 덮어씌워 시스템 크래시 등 오류를 야기
  * 보안상 취약점이며 공격자들이 의도적으로 조작 가능

---
# Character

* 문자와 숫자를 매칭하여 문자를 표현
* char = 1 byte = -128 ~ 127
  * 이 체계는 ASCII임
  * ASCII로 모자라서 나중에 4byte인 Unicode 도입

---
# 데이터 세그먼트

* 프로그램이 실행되면 모든 프로그램 내용이 RAM에 올라온다
* 아래로 갈수록 메모리 주소는 낮아진다.

|메모리|
|:------:|
|Stack (Local)|
|...|
|Heap|
|Data Segment (Global, Static)|
|Read Only Data (const, literal)|
|Code Segment|


|특징|스택(Stack)|힙(Heap)|
|:----:|:-------:|:------:|
|할당 방식|자동 할당 (함수 호출 시, 함수 종료 시 해제)|동적 할당 (프로그래머가 직접 할당하고 해제)|
|메모리 크기|일반적으로 고정된 크기|제한된 크기 없이 필요할 때마다 동적으로 할당|
|속도|빠르다 (자동으로 관리됨)|상대적으로 느리다 (동적 관리가 필요함)|
|관리 방법|시스템이 자동으로 관리|프로그래머가 직접 할당 및 해제해야 함|
|할당되는 위치|스택 영역에 할당됨|힙 영역에 할당됨|


* Stack은 지역변수가 늘어나면 영역이 아래로 커지다가 다시 줄어듦

---
# Lifecycle 변수의 생명주기 

* <font color = 'ff8000'><b>로컬 변수</b></font>는 중괄호 \{\}기준
  * 함수고 뭐고 상관없이 그냥 중괄호 안에 있으면 로컬변수임
    * 괄호 안과 밖의 변수 이름이 같더라도 가리키는 메모리 위치가 다름(바깥의 변수는 임시로 숨겨짐)
  * 영역이 끝나면 파괴됨
* 글로별 변수는 main 바깥에 생성하면 그게 글로벌
  * 글로벌 변수는 정의 시 자동으로 0으로 초기화
  * 글로벌변수는 메모리의 <font color = 'ff8000'><b>데이터세그먼트</b></font>에 할당됨
  * main 함수 호출보다 먼저 진행

* <font color = 'ff8000'><b>정적 변수 static variable<b></font>
  * 로컬에서 선언해도 파괴되지 않는 변수
  * 함수 내에 생성하면 함수를 실행하기 이전부터 메모리에 존재하며 글로벌 변수처럼 작동함
    * <font color = 'ff8000'><b>static int a = 2;를 함수에 넣어 함수를 여러번 실행하더라도, 변수 초기화는 한 번만 진행됨</b></font>
    * 글로벌 변수처럼 정의 시 자동으로 0으로 초기화
https://blog.naver.com/speciallive?Redirect=Log&logNo=98372211

```c
int count_call;

void func(){
    static int static_counter = 0;
    static_counter++;
    count_call++;
    printf("static counter : %d \n", static_counter);
}
/* 파괴될 변수를 return 하기 때문에 에러 발생
int* pfunt() { int a = 3; return &a; }        */
int* pfunt() {
    static int a = 3;
    return &a;
}
int main() {
    int a = 3;
    int i;
    int *ptr;
    {
        int a = 4;
        printf("local  a : %d \n", a);
    }
    printf("global a : %d \n", a);
    for (i = 0; i < 5; i++) {
        func();
    }
    printf("global counter : %d \n", count_call);
    ptr = pfunt();

    printf("ptr  : %p \n", ptr);
    printf("*ptr : %d \n", ++*ptr);
}
```
```text
local  a : 4 
global a : 3 
static counter : 1 
static counter : 2 
static counter : 3 
static counter : 4 
static counter : 5 
global counter : 5 
ptr  : 0x55dcf36c7010 
*ptr : 4 
```
---
# Void

* void : 공허
  * 타입이 없음을 의미
  * return이 없어야 함
  * 보통 return이 필요없는 함수에 사용
  * 그 외 변수 할당은 불가능

* void 포인터
  * void*
  * 없는 것의 포인터지만, 정의되어있지 않은 데이터형이므로, <font color = 'ff8000'><b>어떤 데이터형의 포인터라도 담을 수 있음</b></font>
  * 온전히 주소값의 <font color = 'ff8000'><b>보관 역할</b></font>만 수행
  * 보관만 하므로 *를 통해 값에 접근은 불가함(몇 바이트인지 모름)
  * 값에 접근하려면 형변환 필요
  * 인자를 포인터로 받는데, 받을 타입이 명확치 않은 경우 void*를 사용

```c
void aa(int* p) { (*p)+=10; }
int read_char(void *p, int byte) { // 그냥 단순히 1바이트씩 읽는 함수
    do {
        printf("%x \n", *(char *)p);
        byte--;
        p = (char *)p + 1;
    } while (p && byte);
    return 0;
}
int main() {
    int a = 1;
    int* p = &a;
    void* voi;
    aa(p);
    voi = p;
    printf("void val : %d \n", *(int*)voi);
    a = 0x12345678;
    read_char(&a, 4);
}
```
```text
void val : 11 
78 
56 
34 
12 
```
