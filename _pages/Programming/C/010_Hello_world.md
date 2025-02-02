---
title: Hello, World!
date: 2025-02-02 00:00
---
---
# Basic
---
### Hello, World!
* stdio.h : standard input output header
* c는 호출한 프로그램(또는 OS)에 return (0 = success)
* #include
  * 컴파일 이전에 실행되는 전처리기명령
  * 지칭하는 파일의 내용을 <font color = '#ff8000'>**100%**</font> 정확히 복사해서 붙여넣음
  * <> : 컴파일러 디렉토리에서 include
  * "" : 현재 디렉토리에서 우선 찾고, 없으면 컴파일러 디렉토리에서 include

```c
#include <stdio.h>
// 주석
/* 주석 */
int main() {
  printf("Hello, World! \n");
  return 0;
}
```
```text
Hello, World!
```