---
title: 포인터
date: 2025-02-02 185100
---
---
###1.16 포인터
* 메모리의 각 위치는 고유의 주소를 갖고 있음
* int, char가 각 데이터를 저장하는 변수
* 포인터는 <font color='ff8000'>데이터의 (시작)주소를 보관하는 변수</font>
  * <font color = 'ff8000'>**포인터도 변수**</font>이므로 포인터에 저장된 값이 바뀔 수 있다.
* 선언
  * int\* a : (int\*)형으로 지정
  * int \*a : \*a가 int, 실제로는 (int \*)a 이거긴 하다.
  * 다중선언 int *a, *b;
    * <font color = 'ff8000'>**int *a, b;하면 b는 그냥 int임**</font>
* 입력
  * &a;
  * 이런 &를 단항 연산자라고 하며, 앞선 &와 다르게 해석됨

* 포인터는 **데이터의 형에 \*를 붙임으로써 정의**되고, &연산자로 특정한 데이터의 메모리 상 주소값을 알아올 수 있다.

* \*a = **주소값**에 저장된 데이터(그렇기에 int *a)
* &a = **데이터**의 주소

* 이중포인터 : 포인터(포인터도 변수)의 주소를 갖는 포인터

* 초기화되지 않은 포인터를 참조하는 것은 불능행동을 야기하므로 하지 말것
  * 그러므로 함수에서 인풋을 포인터를 받은 경우 널포인터인지 아닌지 체크할 필요가 있다.
    * assert(var != NULL);

```c
#include <stdio.h>

int main() {
    int a;
    int *p;
    a = 2;
    p = &a;
    printf("a의 포인터 : %p \n", &a); // 포인터 주소의 앞쪽 0은 생략된 채 출력됨
    printf("p의 값     : %p \n", p);
    printf("p의 포인터 : %p \n", &p);

    printf("a의 값     : %d \n", a);
    printf("p의 데이터 : %d \n", *p);

    *p = 3;
    printf("p의 데이터에 3 대입하고 a를 출력하면? : %d \n", a);
    return 0;
}
```
```text
a의 포인터 : 0x7ffc8b08a04c 
p의 값     : 0x7ffc8b08a04c 
p의 포인터 : 0x7ffc8b08a050 
a의 값     : 2 
p의 데이터 : 2 
p의 데이터에 3 대입하고 a를 출력하면? : 3 
```

---
###1.17 상수 포인터
* const int* p;
  * p가 <font color = 'ff8000'>**가리키는 데이터**</font>는 상수여야 함
  * *p = 0;와 같은 방식으로 조작 불가능
  * p가 가리키는 데이터가 상수이기 때문에 포인터의 원본 직접 변경은 가능함
    * p = &a일 때 a= 0; 가능
  * p = &b;로 포인터를 바꾸는 건 가능함
* int* const p;
  * p가 상수여야 함
  * p = &a; 불가능
  * *p = a; 가능

* const int* const p;
  * 아예 불변


---
###1.18 포인터의 덧셈
* p + 1 = p + 데이터형 바이트 수만큼 더한 위치를 가리킴
  * <font color = 'red'>p++, ++p</font>
* p - 1 = p - 데이터형 바이트 수만큼 뺀 위치
* pa + pb = 불가
  * 포인터끼리의 덧셈은 메모리 상 제3의 어딘가일 뿐이므로 의미가 없음
  * 애초에 주소이기 때문에 덧셈이 성립해야 할 이유가 없음
* pa - pb = 가능
  * 뺄셈은 제한적으로 허용되는데 p++연산이 가능하기 때문임
  * 같은 배열 내에 있어야 함 = pa - pb는 원소 위치 차이

```c
#include <stdio.h>

int main() {
    int a;
    int* pa;
    double c;
    double* pc;
    int d;
    int* pd;

    pa = &a;
    pc = &c;
    pd = &d;

    printf("pa     : %p \n", pa);
    printf("pa + 1 : %p \n", pa + 1); // 4 차이

    printf("pc     : %p \n", pc);
    printf("pc + 1 : %p \n", pc + 1); // 8 차이

    printf("pa - pd : %ld \n", pa - pd);

    return 0;
}
```
```text
pa     : 0x7ffd56482e30 
pa + 1 : 0x7ffd56482e34 
pc     : 0x7ffd56482e38 
pc + 1 : 0x7ffd56482e40 
pa - pd : -1 
```

---
###1.19 배열의 포인터
* 포인터의 덧셈은 데이터형만큼 더해지므로, 배열 내 데이터의 참조는 첫번째 데이터의 포인터 + i과 같은 형태로 쉽게 가능하다.

* arr의 값과 &arr[0]이 같음
  * 그럼 arr이 포인터일까?
  * ㄴㄴ, sizeof 쓰면 배열 전체 크기 뱉음 포인터면 8byte 뱉어야 함
  * sizeof와 &를 쓴 경우를 제외하고 <font color = 'ff8000'>**암묵적으로 배열의 이름이 첫번째 원소의 상수 포인터로 타입 변환**</font>

* []는 연산자다
  * arr[3]은 실제로 *(arr + 3)으로 연산됨
  * 그러므로 3[arr]도 가능함 = *(3 + arr)


* &arr : &가 붙은 경우 그냥 포인터임
  * 단, <font color = 'ff8000'>**&arr은 arr의 사이즈가 정해진 포인터**</font>
  * **arr은 arr[0]**의 포인터
  * 그러므로 int *p에 arr는 들어가지만 &arr은 안들어감
  * int (\*p)[4]에 &arr이 들어갈 수 있음 = 크기 4 배열의 포인터
    * int \*p[4]는 <font color='ff8000' >**포인터 배열**</font>을 의미하므로 이렇게 선언하면 안 됨

* 선언에서 형은 제일 마지막에 연산된다
  * int (\*p)[] : 배열의 포인터
  * int \*p[] : 포인터 배열
    * (int \*)(p[])

* 다차원 배열(2차원의 경우)
  * arr[0], arr[1]...이 포인터로 변환됨

* 컴파일 시 arr = &arr[0] = &(arr[0]) (우선순위)
  * 일차원 배열은 그냥 숫자가 되지만, 다차원 배열에서는 arr이 <font color = 'ff8000'>**크기가 정해진 배열의 포인터를 의미**</font>
  * 그러므로 할당하려면 포인터 선언 시 크기를 명확히 선언해주어야 함

* arr = &arr[0] : &가 붙은 경우 사이즈가 정해진 포인터임에 유의
* &arr = arr의 사이즈를 그대로 사용하는 포인터

* 결국 포인터 형 결정은 아래와 같음
  * 가리키는 데이터의 정보
    * int*
  * \+ 1 시 커지는 크기
    * int*[3]이면 \+1 = 12

```c
#include <stdio.h>

int main() {
    int i;
    int arr[] = {1,2,3,4};
    int* parr;

    parr = &arr[0]; // 자동 타입변환으로 parr = arr; 도 가능하긴 함

    for (i = 0; i < 4; i++){
        if (&arr[i] == parr + i) {
            printf("%d 일치   \n", i);
        } else {
            printf("%d 불일치 \n", i);
        }
    }
    for (i = 0; i < 4; i++){
        if (arr[i] == *(parr + i)) {
            printf("%d 원소 일치   \n", i);
        } else {
            printf("%d 원소 불일치 \n", i);
        }
    }

    printf("arr 출력 : %p \n", arr);
    printf("arr[0] p : %p \n", &arr[0]);

    printf("size of arr  : %ld \n", sizeof(arr));  // 배열의 실제 크기
    printf("size of parr : %ld \n", sizeof(parr)); // 포인터 크기 (64bit = 8bit)

    printf("arr[3]      : %d \n", arr[3]);
    printf("*(arr + 3)  : %d \n", *(arr+3));
    printf("3[arr]      : %d \n", 3[arr]);
    printf("parr[3]     : %d \n", parr[3]);
    printf("*(parr + 3) : %d \n", *(parr+3));
    printf("3[parr]     : %d \n", 3[parr]);



    int *p;
    int (*p4)[4];
    p = arr;
    p4 = &arr;
    printf("p  : %p \n", p);
    printf("p4 : %p \n", p4);

    int arr2[2][3] = {1,2,3,4,5,6};
    printf("총 크기    %ld \n", sizeof(arr2));
    printf("총 원소 수 %ld \n", sizeof(arr2) / sizeof(arr2[0][0]));
    printf("행 수      %ld \n", sizeof(arr2) / sizeof(arr2[0]));
    printf("열 수      %ld \n", sizeof(arr2[0]) / sizeof(arr2[0][0]));

    int (*parr2)[3];
    parr2 = arr2;
    printf("parr2[2][3] : %d \n", parr2[2][3]);
    printf("arr2[2][3]  : %d \n", arr2[2][3]);
    printf("arr[10]     : %d \n", arr[10]);

    printf("arr2 : %p, arr2 + 1 : %p \n", arr2, arr2 + 1);  // 12 차이


    return 0;
}
```
```text
0 일치   
1 일치   
2 일치   
3 일치   
0 원소 일치   
1 원소 일치   
2 원소 일치   
3 원소 일치   
arr 출력 : 0x7ffffca20830 
arr[0] p : 0x7ffffca20830 
size of arr  : 16 
size of parr : 8 
arr[3]      : 4 
*(arr + 3)  : 4 
3[arr]      : 4 
parr[3]     : 4 
*(parr + 3) : 4 
3[parr]     : 4 
p  : 0x7ffffca20830 
p4 : 0x7ffffca20830 
총 크기    24 
총 원소 수 6 
행 수      2 
열 수      3 
parr2[2][3] : 0 
arr2[2][3]  : 0 
arr[10]     : 1888771072 
arr2 : 0x7ffffca20840, arr2 + 1 : 0x7ffffca2084c 
```

---
###1.20 포인터의 포인터
* \*\*p : 포인터의 포인터

```c
#include <stdio.h>

int main() {
    int a =3;
    int *pa;
    int **ppa;

    pa = &a;
    ppa = &pa;

    printf("value   a : %d, pa : %d, ppa : %d \n", a, *pa, **ppa);
    printf("pointer a : %p, pa : %p, ppa : %p \n", &a, pa, *ppa);
    
    return 0;
}
```
```text
value   a : 3, pa : 3, ppa : 3 
pointer a : 0x7fff351189f4, pa : 0x7fff351189f4, ppa : 0x7fff351189f4 
```

---
###1.21 포인터배열
* 포인터로 이루어진 배열
  * int \*p[];  (\*p)[]가 아님에 유의
  