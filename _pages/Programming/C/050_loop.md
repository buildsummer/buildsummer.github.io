---
title: 반복문
date: 2025-02-02 184500
---
---
# 반복문
* for (초기식;조건식;증감식) {}
* while (조건식) {}
  * do {} while (조건식) : 로직 후에 조건 검사 = 한 번은 무조건 실행됨
* 중단 : break;
* 패스 : continue;
* switch : 동일한 변수에 대한 반복문
  * switch (변수){ case "b": ... case "a": ... default: }
  * char, short, int, long 중에 하나만 가능(=정수류만 가능)
  * <font color = 'ff8000'><b>매 case 마다 break 필수</b></font>, 안하면 해당 case 밑의 모든 명령이 실행됨
  * case의 값 크기가 크지 않고, **순차 정렬**, 값끼리 차이가 크지 않으면 성능이 훨씬 좋음

```c
#include <stdio.h>

int main() {
    int i;
    for (i = 0; i < 2; i++) {
        printf("증가 %d \n", i);
    }
    for (i = 20; i > 15; i = i - 2) {
        if ( i % 9 == 0) continue;
        printf("감소 %d \n", i);
    }
    printf("숫자를 맞춰보세요 \n");
    for (;;){
        int i = 3;
        if(i == 3){
            printf("정답! \n");
            break;
        } else {
            printf("땡! \n");
        }
    }

    printf("숫자를 맞춰보세요 \n");
    while (1) {
        int i = 3;
        if(i == 3){
            printf("정답! \n");
            break;
        } else {
            printf("땡! \n");
        }
    }

    return 0;
}
```

```text
증가 0 
증가 1 
감소 20 
감소 16 
숫자를 맞춰보세요 
정답! 
숫자를 맞춰보세요 
정답! 
```
