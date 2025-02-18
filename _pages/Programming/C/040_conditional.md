---
title: 조건문
date: 2025-02-02 184400
---

---
# 조건문

* 관계연산자 : ==, 참이면 1 거짓이면 0
  * <font color = 'ff8000'><b>중요 : 0이 아닌 값은 전부 참으로 판단</b></font>
* int의 1 / 0은 <font color = 'ff8000'><b>정의</b></font>되어 있지 않아 프로그램이 에러를 뱉으며 종료됨
  * double은 inf로 표기
* 논리연산자 : && (and), \|\| (or), ! (not)
  * &는 숫자 연산에서 사용
    * 실제로는 비트연산인데 1 & 1인 경우만 1이 뜨므로 제한적 true 가능
* if 문은 명령이 한줄일 경우 {} 생략 가능하다.
* 약식으로 <font color = 'ff8000'><b>삼항연산자</b></font> 사용 가능
  * [조건] ? [trueval] : [falseval]
  * 단, 값을 반환할 뿐 호출 등의 작업은 할 수 없다

```c
#include <stdio.h>

int main() {
    int i = 3;
    if (i == 7) printf("7은 두번 출력 \n");

    if (i == 7)  // 괄호 생략
        printf("당신은 행운의 숫자 7을 입력했습니다 \n");
     else if (i == 3) {
        printf("당신은 숫자 3을 입력했습니다 \n");
    } else {
        printf("당신은 일반 숫자를 입력했습니다 \n");
    }
    printf("삼항연산자 %s \n", i == 3 ? "aa" : "bbb");
    return 0;
}
```
```text
당신은 숫자 3을 입력했습니다 
삼항연산자 aa
```
