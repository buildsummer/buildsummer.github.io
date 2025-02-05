---
title: 구조체
date: 2025-02-05 132000
---
---
#4. 구조체
* 배열은 정해진 타입만 입력이 가능하다.
* 구조체를 이용하면 여러 타입 입력 가능
  * 구조체 생성시 } 뒤에 ; 필수
  * 구조체의 원소는 member라고 칭함
  * 구조체의 정의에서는 변수 초기화가 불가능함

* <font color = 'ff8000'>**구조체 또한 하나의 형**</font>이다.
  * 구조체 자체도 배열로 만들 수 있다.
  * 구조체 a = 구조체 b; 형태로 복사가 가능(구조체배열은 안된다)
  * 구조체도 포인터를 갖는다.
  * 선언 시 초기화도 가능
    * {}로 감싸서 데이터 입력

* 구조체 정의 시 끝에 이름을 적어두는 것 = main 함수에서 구조체를 해당 이름으로 할당하는 것과 같음

```c
#include <stdio.h>
struct Human {
    int age;
    int height;
    int weight;
} psi2 = {1,2,3}, psi3 = {4,5,6};

int main() {
    struct Human psi[3];

    psi[0].age = 19;
    psi[0].height = 190;
    psi[0].weight = 100;

    psi2 = psi[0];

    printf("psi2 %5d %5d \n", psi2.height, psi2.age);
    printf("psi3 %5d %5d \n", psi3.height, psi3.age);
    return 0;
}
```

```text
psi2   190    19 
psi3     5     4 
```
---
###4.1 구조체 포인터
* 배열의 이름과 달리 구조체의 이름은 포인터로 변환되지 않음
  * 포인터를 &구조체로 표기

* *ptr.a = '.' 연산이 우선하기 때문에 오류 발생
  * (*ptr).a로 작성해야 함
  * <font color = 'ff8000'>**ptr->a**</font>로 작성 가능

```c
#include <stdio.h>

struct test {
    int a, b;
    int *pointer;
};
int main() {
    struct test st;
    struct test *ptr;
    int i;

    ptr = &st;

    (*ptr).a = 1;
    ptr->pointer = &i;
    *ptr->pointer = 3;
    ptr->b = 2;

    printf("st a : %d \n", st.a);
    printf("st b : %d \n", st.b);
    printf("i    : %d \n", i);

    return 0;
}
```
```text
st a : 1 
st b : 2 
i    : 3 
```

---
###4.2 구조체 함수
* 구조체의 포인터를 인자로 전달
* 구조체를 return 할 수도 있다.

```c
#include <stdio.h>

struct TEST {
    int age;
    int gender;
};
int set_human(struct TEST *a, int age, int gender);
int main() {
    struct TEST human;
    set_human(&human, 10, 1);
    printf("AGE : %d // Gender : %d ", human.age, human.gender);

    return 0;
}
int set_human(struct TEST *a, int age, int gender) {
    a->age = age;
    a->gender = gender;
    return 0;
}
```
```text
AGE : 10 // Gender : 1
```

---
###5.4 별도 변수 선언 typedef
* typedef [형] [명칭]
* 구조체의 경우 변수 선언 시 매번 struct aaa b; 이런 식으로 하기는 귀찮음
  * typedef를 하면 쉽게 가능
  * 구조체 선언을 하면서 동시에 typedef를 같이 할 수도 있음
* 중괄호에 따른 영역에만 유효함
* 기존 변수도 선언이 가능함
  * cpu에 따라 float, double 간 치환이 필요한 경우 전체를 치환할 필요 없이 사전 정의만 바꿔주면 쉬움
 
```c
#include <stdio.h>
typedef struct TEST {
    int a;
} test; //typedef 없으면 그냥 main 함수에 변수로 포함됨에 유의할 것

typedef int tint;
int main() {
    tint a = 123;
    test b;
    b.a = a;
    printf("%d \n", a);
}
```
```text
123
```
