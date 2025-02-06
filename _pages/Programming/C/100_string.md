---
title: 문자열
date: 2025-02-05 131100
---

---
# 문자열

* string = 실, 문자열이 실처럼 나열되어 있기 때문에 string이라고 함

---
# Null-terminated String 널종료문자열

* 배열의 끝임을 알려주는 것. 
  * 아스키 코드가 0이며 '\0'임. <font color = 'ff8000'><b>'0' 이 아니다</b></font>
  * 없으면 컴퓨터에 항상 문자열 길이를 입력해야함 = 문자열로서 역할을 하기가 힘듦
  * 있으면 자동으로 0이 나올때까지 출력 = 메모리 범위 벗어날 수 있음
    * 또는 (char)NULL은 엄격하게는 안 된다
    * NULL 자체가 (void*)0이기 때문
  *  <font color = 'ff8000'><b>%s</b></font>는 Nullstring이 나올때까지 읽으라는 얘기임
    * 그런고로 0 뒤에 어떤 문자가 있다고 하더라도 출력 안 됨

* 배열에는 필수로 들어가기 때문에 배열의 크기는 항상 문자열 수 + 1이어야 함
  * 관용구
```c
#define STR_LEN (80)
…
char str[STR_LEN + 1];
```

* <font color="skyblue"><b>따옴표</b></font>
  * 작은따옴표 : 문자열 하나만 지정할때
  * 큰따옴표 : 여러 문자열 지정할 때 = <font color = 'ff8000'><b>널스트링 자동포함</b></font>
```c
int main() {
    char null_1 = '\0';
    char null_2 = 0;
    char not_null = '0';

    char st_1[4] = {'P', 's', 'i', 0};
    char st_3[30] = {"Long Psi"};

    printf("NULL 정수값 : %d, %d \n", null_1, null_2);
    printf("'0'  정수값 : %d         \n", not_null);

    printf("문자열 : %s \n", st_1);
    printf("문자열 : %s \n", st_3);

    st_3[0] = 'a';
    printf("문자열 : %s \n", st_3);
}
```
```text
NULL 정수값 : 0, 0 
'0'  정수값 : 48         
문자열 : Psi 
문자열 : Long Psi 
문자열 : aong Psi 
```

---
# 문자열 count
* 0이 나오기 전까지 세주면 됨

```c
int str_length(char str_arr[]);
int main() {
    char t[] = {"Psiawefw"};
    printf("ret %d \n", str_length(t));
}
int str_length(char str_arr[]) {
    int i = 0;
    while (str_arr[i]) {
        i++;
    }
    return i;
}
```
```text
ret 8 
```

---
# 입출력함수
* printf : 출력
* scanf : 입력
  *  <font color = 'ff8000'>주의</font> : scanf는 각 자료형마다 모두 포맷이 다름(printf와 다르다)
    * double는 무조건 %lf, float는 무조건 %f임

```c
int main() {
    double c;

    printf("섭씨온도를 입력하세요 : ");
    scanf("%lf", &c); // 입력받음
    printf("섭씨 %f도는 화씨 %f도 입니다 \n", c, 9 * c / 5 + 32);
}
```
```text
섭씨온도를 입력하세요 : 29
섭씨 29.000000도는 화씨 84.200000도 입니다 
```

---
# 버퍼
* cmd에 파라미터 입력 시, 입력할 때마다 상호작용되는게 아니라 실제로는 한 군데 모아두고 한번에 처리됨
  * 이를 버퍼라고 함
  * 키보드의 입력은 입력버퍼(stdin : 입력스트림)에 저장되었다가 한번에 처리됨
  * 입력 종료는 <font color = "ff8000">엔터(\n) </font>로 식별

* scanf
  *  <font color = 'ff8000'><b>' ', '\n', '\t'(공백문자)를 만날 때까지</b></font> stdin에서 데이터를 가져온 후, 가져온 건 버퍼에서 삭제
    * 버퍼에서 가져온 것만 삭제하기 때문에 개행문자등은 버퍼에 남아있음
  * 이후에 <font color='ff8000'><b>%c</b></font>가 있는 경우, 인풋을 안 받아도 %c가 냉큼 \n을 가져오는 문제가 발생할 수 있음
  * 숫자형과 %s는 구분자를 만나면 멈춤
    * 구분자가 공백문자인 경우 버퍼에서 해당 구분자 삭제, 아닌경우 냅둠
    * 숫자형은 숫자가 나오기 전까지 아무리 공백, 엔터를 쳐도 안 넘어가고, 숫자 이후 숫자가 아닌 걸 만나면 멈춤
    * %s는 공백문자를 만나면 멈춤
  * 결국, %c를 사용할 때는 버퍼를 신경써야 하고, 숫자형, %s를 쓸 때는 버퍼를 신경쓰지 않아도 된다.
    * **되도록이면 문자(%c) 대신 문자열(%s)를 쓰자!**
  * 선언된 형식에 맞는 데이터가 아니더라도 기계적으로 실행되므로 형식에 맞지 않을 경우 문제가 생길 수 있음

* stdin
  * stdin에 남은 문자열은 getchar()를 호출해서 처리할 수 있음
    * getchar() : stdin에서 한 문자열을 가져와 return

```c
int main() {
    int num;
    char c;

    printf("숫자를 입력하세요 : ");
    scanf("%d", &num);
    printf("입력한 숫자       : %d \n", num);

    getchar();

    printf("문자를 입력하세요 : ");
    scanf("%c", &c);
    printf("입력한 문자       : %c \n", c);
}
```
```text
자를 입력하세요 : 123abc
입력한 숫자       : 123 
문자를 입력하세요 : 입력한 문자       : b 
```

---
# 문자열 리터럴
* "문자열" 은 문자열이지만, 그 자체로 <font color = 'ff8000'><b>포인터</b></font> 이기도 함
   * str == "abc" : 이 연산은 str의 포인터와 "abc" 포인터를 비교하는 것

* <font color = 'ff8000'><b>Literal</b></font>
  * 소스코드 상 고정된 값을 지칭(=불변성)하며, 컴퓨터는 이러한 리터럴을 따로 모아서 보관 = 특정한 메모리 공간에 리터럴이 보관됨
    * 불변성 때문에 읽기만 가능한 곳임 (const와 같음)
    * 변경 시도 시 에러 또는 프로그램 강제종료 등 발생 가능
  * c언어는 큰 따옴표로 묶인 건 무조건 문자열리터럴이라 지칭
    * "why so serious", "%c" 등
* char *p = "ab"; = ab 리터럴의 시작위치를 가져와라
  * 컴파일러에 따라 const char로만 가능한 경우가 있음
* a[] = "ab";는 문자열리터럴을 <font color = 'ff8000'><b>복사</b></font>하는 행위이므로, a는 리터럴이 아니며 수정 가능
  * 이건 스택(Stack, 메모리수정가능영역)에 저장됨

* 여러줄로 작성하고 싶은 경우 줄을 \\로 끝내고 이어서 작성
  * 근데 이거보다 아래가 더 낫다
* 구분자 없이 <font color = 'ff8000'><b>연속된 문자열은 그냥 하나</b></font>로 합쳐짐
  * "a""b" = "ab"

```c
int main() {
    char str[] = "sentence";
    char *pstr = "sentence";

    // pstr[1] = 'a'; 이걸 주석처리를 안 하면 제대로 실행이 안 됨
    printf("str  : %s \n", str);
    printf("pstr : %s \n", pstr);
    printf("%p  %p \n", "sentence", str);
}
```
```text
str  : sentence 
pstr : sentence 
0x5569a583e004  0x7ffe49461f1f 
```

---
# 문자열 컨트롤
* 문자열 복사
  * b = a;와 같은 방식으로 코딩 불가
    * b와 a는 각각 상수포인터이기 때문(불변)
* 문자열 합치기
  * 합칠 대상 문자열의 0 등장 위치에 가서 하나씩 입력

```c
int mycopy(char src_str[], char ret_str[]);
int stdcopy(char dest[], char src[]);
int strconcat(char dest[], char src[]);
int strcomp(char str1[], char str2[]);

int main() {
    char a[] = "aasdfdss";
    char b[10];
    char c[10];
    char d[20] = "zzzzz";

    // b = "aasdfdss"; 이렇게 코드를 짤 수는 없다.

    mycopy(a, b);
    printf("b : %s \n", b);

    stdcopy(c, a);
    printf("c : %s \n", c);

    strconcat(d, a);
    printf("d : %s \n", d);

    if (strcomp(a, b)) printf("a and b are same \n");
}
int mycopy(char src_str[], char ret_str[]) {
    int i = 0;
    while (src_str[i] > 0) {
        ret_str[i] = src_str[i];
        i++;
    }
    ret_str[i] = 0;
    return 1;
}

int stdcopy(char dest[], char src[]) {
    while (*src) {
        *dest = *src;
        dest++;
        src++;
    }
    *dest = 0;
    return 1;
}

int strconcat(char dest[], char src[]) {
    while (*dest) {
        dest++;
    }
    while (*src){
        *dest = *src;
        dest++;
        src++;
    }
    *dest = 0;
    return 1;
}
int strcomp(char str1[], char str2[]) {
    while (*str1) {
        if (*str1 != *str2) return 0;
        str1++;
        str2++;
    }
    if (*str2 != 0) return 0;
    return 1;
}
```
```text
b : aasdfdss 
c : aasdfdss 
d : zzzzzaasdfdss 
a and b are same 
```
