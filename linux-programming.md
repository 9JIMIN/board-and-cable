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

커맨드라인 인자

아래와 같은 args.c 파일을 만든다.

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]){	
	printf("argc=%d\n", argc);
	for (int i=0;i<argc;i++){
		printf("argv[%d]=%s\n", i, argv[i]);
	}
	exit(0);
}

```

`argc`는 인자의 개수, `argv`는 인자의 내용이 들어간다. 

```bash
$ gcc -o args args.c

$ ./args
argc=1
argv[0]=./args
# argv[0]에는 실행할때 입력한 명령어 그자체가 들어간다.

$ ./args x y z
argc=4
argv[0]=./args
argv[1]=x
argv[2]=y
argv[3]=z

$ ./args "x y z"
argc=2
argv[0]=./args
argv[1]=x y z

$ ./args *.c
argc=3
argv[0]=./args
argv[1]=args.c
argv[2]=hello.c
```

