---
layout: post
title: CTCI C problem (string)
categories: [Snippet, C]
---

---------------
널 문자로 끝나는 문자열 뒤집는 reverse(char* str) 함수를 C나 C++로 구현하라.


```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
char* reverse(char* str) {
  int l = strlen(str);
  char* reversed_str = (char*) malloc (sizeof(str));;
  for (int i=0; i < strlen(str); i++) {
    reversed_str[i] = str[l - i - 1];
  }
  reversed_str[l] = '\0';

  return reversed_str;
}
int main(void) {
  printf("Hello World\n");
  char* reversed_str = reverse("hello world");
  printf(reversed_str);
  return 1;
}
```

### 설명

함수 reverse는 char* str를 argument로 받아, 문자열길이와 전체 메모리 크기를 받아와 할당한다. 할당 후 역순으로 argument를 순회하여 집어넣고 끝에 escape 문자를 추가하여 리턴한다. 

### 결과

![뒤집혀진 string](/static/post_image/2018-05-27/reversed.jpg)

![결과이미지]({{ site.url }}/static/post_image/2018-05-27/reversed.jpg)
