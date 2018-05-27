---
layout: post
title: CTCI C problem (string)
categories: [Snippet, C]
---

---------------
널 문자로 끝나는 문자열 뒤집는 reverse(char* str) 함수를 C나 C++로 구현하라.

<pre><code>

#include <stdio.h>
#include <string.h>
#include <stdlib.h>
char* reverse(char* str) {
  int l = strlen(str);
  char* rev_str = (char*) malloc (sizeof(str));;
  for (int i=0; i < strlen(str); i++) {
    rev_str[i] = str[l - i - 1];
  }
  rev_str[l] = '\0';

  return rev_str;
}
int main(void) {
  printf("Hello World\n");
  char* rev_str = reverse("hello world\0");
  printf(rev_str);
  return 1;
}

</code></pre>
