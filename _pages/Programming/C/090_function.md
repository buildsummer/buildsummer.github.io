---
title: 함수
date: 2025-02-05 130005
---
---
# 함수

* 함수 이름은 20자 안팎으로 하자
* 함수 내용을 Body라고 지칭
* return 하면 함수가 종료
* return 없으면 함수가 끝까지 실행
  * return으로 종료되는 게 안전함
* 프로그램 실제 실행은 main을 호출함으로써 진행됨
  * main도 함수다
  * 호출 안 하면 코드 짜놔도 실행 안 됨
* 호출하는 주체를 Caller라고 함
* c는 로컬함수를 정의할 수 없음
  * 단 prototype은 함수 안에서 쓸 수 있다 이러면 해당 함수에서만 호출 가능

```c
int print_hello() {
    printf("hello, World \n");
    return 0;
    printf("이건실행안됨");
}

int main() {
    int ret_val;
    ret_val = print_hello();
    if (ret_val == 0) printf("Success! \n");
}
```

```text
hello, World 
Success! 
```

---
# Input Parameter
* 인자를 상수로 받게 되면, 함수 내에서 인자의 변경이 허용되지 않음

```c
int slave(const int money) {
    //return money += 20000; 허용되지 아니함 money가 const이기 때문
    return money + 10000;
}

int main() {
    int money = 10000;
    printf("돈 : %d \n", slave(money));
}
```

```text
돈 : 20000 
```

---
# Input Parameter With Pointer
* 어떤 함수가 Caller 내의 값을 바꾸려면 포인터를 사용
  * 글로벌 변수가 없는 건 아니다
* 인풋에 arr의 포인터를 넣는 경우 두 가지 표현 방식이 가능
  * 인풋에서만 가능한 표현식임
```c
int aa(int (*a)[2])
int aa(int a[][2])
```

```c
int change_val(int* pi) {
    *pi = 3;
    return 0;
}

int main() {
    int i = 1;
    printf("포인터 변경 전 %d \n", i);
    change_val(&i);
    printf("포인터 변경 후 %d \n", i);
}
```
```text
포인터 변경 전 1 
포인터 변경 후 3 
```

---
# Prototype
* 코드가 복잡해지면 코드가 항상 순차적으로 잘 짜여지지는 않음
  * 함수 정의 이전에 함수 호출이 발생할 경우 컴파일러는 암시적 선언을 하면서 대충 함수의 인자를 추론한다. 그러면 실제 함수랑 안 맞게 된다.
  * 선후순위 안지키고 코딩하면 컴파일이 되더(사실상 컴파일 오류임)라도 디버깅이 잘 안된다

* 상단에 함수의 원형(Prototype)을 넣으면 해결됨
  * 순차적으로 짜지 않더라도 위쪽에 함수의 원형을 넣게 되면 해당 함수를 파악한 상태로 컴파일이 진행되기 때문에 디버깅을 해줌
  * 매개변수의 형만 중요하며 변수 이름은 안 넣어도 되긴 한다.

* 대부분 프로그래머는 main을 맨 위에 짜는 것을 선호
  * 그러므로 반드시 넣어야 함

* 주의 : 함수의 input도 형이 안맞을 경우 형변환이 발생한다.
* 함수에 배열을 줄 때는 배열의 크기도 같이 주어야 한다.
* 가변크기배열을 매개변수로 입력하는 경우 아래와 같이 할 수도 있음
  * int get_sum_of_array(int size, int arr[size])
  * 배열 선언 시 static
    * arr[static 3] : 최소 배열 크기가 3임을 특정

```c
int pswap(int **pa, int **pb);
int main1();
int main2();
int arr_add(int (*arr_inp)[3], int row);
int main() {
    printf("main1==================================\n");
    main1();
    printf("main2==================================\n");
    main2();
}
int main1() {
    int a = 1, b = 2;
    int *pa, *pb;

    pa = &a;
    pb = &b;

    printf(" a  : %d,  b  : %d \n", a, b);
    printf("*pa : %d, *pb : %d \n", *pa, *pb);

    pswap(&pa, &pb);
    printf(" a  : %d,  b  : %d \n", a, b);
    printf("*pa : %d, *pb : %d \n", *pa, *pb);
    return 0;
}

int pswap(int **ppa, int **ppb) { //포인터를 조작하므로 이중포인터여야 함
    int *temp = *ppa;
    *ppa = *ppb;
    *ppb = temp;
    return 0;
}
int main2() {
    int i;
    int arr[2][3] = {1,2,3,4,5,6};
    arr_add(arr, 1);
    for (i = 0; i < 3; i++) {
        printf(" %d 번째 원소 값 : %d \n", i + 1, arr[1][i]);
    }
    return 0;
}
int arr_add(int arr_inp[][3], int row) {
    int i;
    for (i = 0;i < 3; i++) {
        arr_inp[row][i] += 1;
    }
    return 0;
}
```
```text
main1==================================
 a  : 1,  b  : 2 
*pa : 1, *pb : 2 
 a  : 1,  b  : 2 
*pa : 2, *pb : 1 
main2==================================
 1 번째 원소 값 : 5 
 2 번째 원소 값 : 6 
 3 번째 원소 값 : 7 
```
---
# 함수포인터
* 메모리상에 올라간 함수의 시작점을 가리키는 포인터
  * 포인터 선언 시 인풋까지 전부 정의해주어야 함
  * int (*pfunt)(int,int)
  * 인풋 없는 경우 int (\*pfunt)()
  * 배열인 경우    int (\*pfunt)(int (\*)[3])

```c
int max(int a, int b);
int main() {
    int a =1, b = 2;
    int (*pmax)(int, int);
    pmax = max;
    printf(" max(a,b) : %d \n", max(a, b));
    printf("pmax(a,b) : %d \n", pmax(a, b));
}
int max(int a, int b) {
    if (a > b)
      return a;
    else
      return b;

}
```
```text
 max(a,b) : 2
pmax(a,b) : 2
```

---
# 재귀
```c
int recursive(int a) {
    if (a == 0 || a == 1) return 1;
    if (a > 0) return a * recursive(a-1);
    return 0;
}
int main() {
    int a;
    printf("factorial number : ");
    scanf("%d", &a);
    printf("factorial %d! : %d", a, recursive(a));
}
```
```text
factorial number : 5
factorial 5! : 120
```

---
# 메인함수
* main()은 사실 main(void)에서 void가 생략된 것
* 메인함수의 인자
  * 첫 번째 : 인자 개수
  * 두 번째 : 각 인자를 가리키는 <font color = 'ff8000'><b>포인터의 배열</b></font>

```c
 char **argv
 char* argv[]
```
* 인자의 첫 번째는 프로그램의 이름이다.
* <font color = 'ff8000'><b>각 인자는 문자열 리터럴임</b></font>에 유의
  * 그자체로 배열

* exit()
  * 어느 함수에서든 프로그램을 종료시킴

```c
int main(int argc, char **argv) {
    int i;
    printf("받은 인자 개수 : %d \n", argc);
    for (i = 0; i < argc; i ++) {
        printf("받은 인자 %d : %s \n", i, argv[i]);
    }
}
```
```text
받은 인자 개수 : 5 
받은 인자 0 : ./A_out 
받은 인자 1 : 1 
받은 인자 2 : 2 
받은 인자 3 : awdf 
받은 인자 4 : ss 
```
