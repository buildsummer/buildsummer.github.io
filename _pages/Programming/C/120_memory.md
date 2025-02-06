---
title: 메모리
date: 2025-02-05 135500
---

---
# 동적 메모리 할당
* 일반적으로 메모리 크기는 컴파일 이전에 확정되어야 함
  * 충분히 크게 메모리를 잡는다는 것은 그만큼 메모리를 낭비한다는 뜻

* malloc [stdlib.h]
  * memory allocation
  * 자신이 <font color ='ff8000'><b>할당한 메모리의 시작 주소</b></font>를 리턴
  * 리턴형이 <font color = 'skyblue'><b>void\*</b></font>이므로 형변환 필수

```c
  int *arr;
  arr = (int*)malloc(sizeof(int) * 12);
```

* free
  * 할당한 메모리 해제
  * free를 제대로 하지 않으면 <font color = 'ff8000'><b>메모리 누수</b></font> 발생

```c
  free(arr);
```

* 할당된 메모리는 Heap에서 관리
  * 나머지 메모리 영역은 컴파일 시에 정해져야 하기 때문에 건드릴 수 없음
  * 사람이 컨트롤하는 영역이므로 철저히 관리해야 함

* 다차원 배열의 동적 할당도 가능
  * <font color = 'ff8000'><b>포인터 배열</b></font> 이용
  * 각 요소가 다른 일차원 배열을 가리킴
  * 그러므로 1) 포인터 배열 생성, 2) 각 요소 배열 생성
  * 단, 이건 실제로 다차원 배열은 아니다
    * 다차원 배열은 메모리에 서로 붙어있어야 하는데 이건 떨어져있을 수 있음
  * 그렇지만 2차원 배열과 똑같이 연산 가능

* 구조체
  * 구조체도 구조체의 특성에 따라서만 하면 크게 차이 없음
  * 단, sizeof(구조체)를 필수로 해야 함, 구조체의 바이트를 실제로 계산해서 하면 안 됨(컴파일러가 실제로 어떻게 처리할지 모름, EX: 더블워드 경계)

```c
int main(int argc, char **argv) {
    int x, y, i;
    int **arr;
    x = atoi(argv[1]);
    y = atoi(argv[2]);
    printf("a[?][?] : a[%d][%d] \n", x, y);

    arr = (int **)malloc(sizeof(int*) * x);

    for (i = 0; i < x; i++) {
        arr[i] = (int *)malloc(sizeof(int) * y);
    }
    printf("생성 완료 \n");

    for (i = 0; i < x; i++) {
        free(arr[i]);
    }
    free(arr);
    printf("해제 완료 \n");
}
```
```text
a[?][?] : a[2][4] 
생성 완료 
해제 완료 
```


---
# 노드

* 메모리를 할당 완료 했는데 하나만 더 넣고 싶으면?
  * 다시 처음부터 하면 효율이 떨어짐

* 노드
  * 데이터와 다음 노드의 위치를 가리키는 포인터로 이루어진 구조체
  * 연결하면 배열처럼 가능하며 중간에 끼워넣기도 가능함

```c
 struct Node {
     int data;
     struct Node* nextNode;
 }
```
* 배열보다 월등한가?
  * 조작의 편의성은 있지만, 포인터를 위해 추가 메모리가 소요되고, 위치를 찾기 위해 시간이 소요되기 때문에 꼭 좋은 건 아니다.
  * 추가/삭제/삽입이 주요 오퍼레이션 : 노드
  * 찾기가 주요 오퍼레이션 : 배열

```c
struct Node {
    int data;
    struct Node* nextNode;
};
/* 노드 생성 */
struct Node* CreateNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));

    newNode->data = data;
    newNode->nextNode = NULL;

    return newNode;
}
/* 노드 삽입 */
struct Node* InsertNode(struct Node* current, int data) {
    struct Node* after = current->nextNode;

    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));

    newNode->data = data;
    newNode->nextNode = after;

    current->nextNode = newNode;
    return newNode;
}
/* 노드 파괴 */
void DestroyNode(struct Node* destroy, struct Node* head) {
    struct Node *next = head;

    if (destroy == head) {
        free(destroy);
        return;
    }

    while (next) {
        if(next->nextNode == destroy) {
            next->nextNode = destroy->nextNode;
        }
        next = next->nextNode;
    }
    free(destroy);
    return;
}
void PrintNodeFrom(struct Node* from) {
    while (from) {
        printf("노드의 데이터 : %d \n", from->data);
        from = from->nextNode;
    }
}

int main() {
    struct Node* Node1 = CreateNode(100);
    struct Node* Node2 = InsertNode(Node1, 200);
    struct Node* Node3 = InsertNode(Node2, 300);
    struct Node* Node4 = InsertNode(Node2, 400); //injection

    PrintNodeFrom(Node1);
}

```text
노드의 데이터 : 100 
노드의 데이터 : 200 
노드의 데이터 : 400 
노드의 데이터 : 300 
```

---
# 메모리 함수

* 초기 메모리 조작은 보통 문자열 처리에서 나타났기 때문에 함수가 string.h에 있는 경우가 많음
* memcpy : 메모리의 특정한 부분으로부터 얼마까지의 부분을 복사

```c
  memcpy(target, src, 복사할 길이);
```
* memmove : 메모리의 특정한 부분의 내용을 다른 부분으로 옮김
  * 옮긴다고 해서 이전 공간 데이터가 사라지지는 않음
  * 복사할 때 메모리 공간이 겹쳐도 문제없이 복사됨

```c
  memmove(target, src, 이동할 길이);
  // target과 src는 같은 배열에 다른 위치임
```

* memcmp : 메모리의 특정부분을 비교

```c
  memcmp(arr, arr2, 길이);
```

```c
int main() {
    char str[50] = "I Love";
    char str2[50], str3[50];

    memcpy(str2, str, strlen(str) +1);

    printf("%s \n", str);
    printf("%s \n", str2);

    memmove(str2 + 6, str2 +1, 6);
    printf("%s \n", str2);

    return 0;
}
```
```text
I Love 
I Love 
I Love Love 
```

---
# 데이터의 메모리

* 각 타입은 차지하는 메모리가 정해져있음
* 그럼 구조체는 각 타입의 합일까?
  * 아님

* 메모리는 WORD 단위로 처리되는 게 빠름
  * 그러므로 컴파일러가 모자랄 경우 강제로 WORD 단위로 늘려서 조정함

* 메모리의 주소 또한 WORD의 배수 위치에 있어야 빠름
  * 정렬되어 있어야 함

* 이를 더블워드경계라고 함
* 더블워드경계 때문에 컴파일러가 강제로 WORD 단위로 늘려서 조정하는 것

* 항상 좋을까?
  * 명령 전달 시 바이트가 원치 않는대로 구성된다면 오류가 생길 수 있음

* 메모리를 더블워드경계에 맞추지 말라는 명령도 존재한다
