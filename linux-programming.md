# linux-programming

c언어 파일을 만들고, 소스코드를 빌드, 실행하는 방법

```bash
$ nano hello.c
##--
#include<stdio.h>
int main(){
	printf("hello world!\n");
	return 0;
}
##--

$ gcc -o hello hello.c # gcc -o [실행파일이름] [c파일]
$ ls # 빌드된 파일확인
hello hello.c
$ ./hello # 실행
hello world!
```

