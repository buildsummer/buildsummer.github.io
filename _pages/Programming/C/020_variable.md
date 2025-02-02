---
title: 변수
date: 2025-02-02 00:00
---
---
# 변수선언
---
* c는 변수형에 굉장히 예민, 안 맞으면 바로 에러 또는 사일런트에러 발생
  * float는 입력 시 숫자 뒤에 f를 꼭 써줘야 함
* 선언 시 선언을 읽는 순위는 오른쪽에서 왼쪽
  * 타입이 제일 마지막에 정해진다
* char는 연산에 포함되면 그냥 정수형으로 취급되기 때문에 주의할 것
  * char도 unsigned 지정이 가능함

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

* float, double, long double은 _Complex와 같이 사용하면 복소수형으로 할당 가능
* 포맷 지정자
  * int  : %{n}d
  * long int : %ld, d로 작동 안 한다
    * **<font color ='ff8000'>int -> long int</font>**로 바꾸더라도 printf를 전부 신경써야 한다
  * float, double : %{n.m}f


---
###1.14 상수
---
* 최초 선언 이후 변수를 바꿀 수 없고 바꾸면 에러 발생
  * const (변수형) (변수명) = (값);

* 정수형 상수는 10, 8, 16진법으로 작성 가능하다
  * 작성법만 다를뿐이지 똑같은 진법으로 기록됨
  * 10진법은 0으로 시작하면 안 됨
  * 8진법은 0으로 시작해야 함
  * 16진법은 0x로 시작해야 함 :영어 대소문자 구분 x
  * 섞어서 쓸 수도 있음 10 + 015 + 0x20

```c
#include <stdio.h>

int main() {
    int int_v = 123;
    int int_v2 = 12345;
    float float_v = 123.123123f; //float임을 명시하기위해 f를 붙임
    double double_v = 123.123123;
    printf("%%5d using int means pad 5 : %5d \n", int_v);
    printf("%%5d using int means pad 5 : %5d \n", int_v2);
    printf("%%5.2f using double        : %5.2f \n", double_v);
    printf("%%.25f using double        : %.25f \n", double_v);
    printf("이건 부동소수점문제 \n");
    return 0;
```
```text
%5d using int means pad 5 :   123 
%5d using int means pad 5 : 12345 
%5.2f using double        : 123.12 
%.25f using double        : 123.1231230000000067548171501 
이건 부동소수점문제 
```

---
###1.6 음수의 개념
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
#include <stdio.h>

int main() {
    int a = 2147483647;
    unsigned int b = 4294967295;
    printf("a : %d \n", a++);
    printf("a : %d \n", a);

    printf("b : %u \n", b++);
    printf("b : %u \n", b--);
    printf("b : %u \n", b);
    return 0;
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
###1.7 오버플로우
* 메모리 내에서 표현 가능한 수치를 넘어섰을 때 잘못된 수치가 표기되는 것
* <font color = 'ff8000'>**잡을 수 있는 방법이 없으므로 코딩을 잘해야 함**</font>
* 오버플로우 : 수치연산에서 최대값이나 최소값을 초과(버림 발생)
* 버퍼 오버플로우 : 할당한 메모리공간을 초과하였을 경우, 다른 메모리 영역에 데이터를 덮어씌워 시스템 크래시 등 오류를 야기
  * 보안상 취약점이며 공격자들이 의도적으로 조작 가능

---
###1.8 String
* 문자와 숫자를 매칭하여 문자를 표현
* char = 1 byte = -128 ~ 127
  * 이 체계는 ASCII임
  * ASCII로 모자라서 나중에 4byte인 Unicode 도입