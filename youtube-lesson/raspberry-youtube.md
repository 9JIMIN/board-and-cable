# 라즈베리파이 - 리눅스

- 위키 문서 [명령어 테이블](https://ko.wikipedia.org/wiki/%ED%8B%80:%EC%9C%A0%EB%8B%89%EC%8A%A4_%EB%AA%85%EB%A0%B9%EC%96%B4)
- 위키 문서 [미디어 저장소](https://commons.wikimedia.org/wiki/Category:Unix_reference_cards?uselang=ko)

## 1. 소개, 접속방법

### 소개

아두이노는 메모리가 부족해서 이용이 제한된다. 
게다가 운영체제도 없어서 삽입한 프로그램만 실행을 한다.

라즈베리파이는 메모리도 넉넉하고,
무엇보다, 운영체제가 있다.
그냥 컴퓨터처럼 뭐든 할 수 있는 것이다

SD카드는 항상 라즈베리파이에 박혀있어야 한다. 

자세한 설치방법은 이 옆에 md파일에 있다. 그걸 봐.

### 원격접속

ssh를 이용하면 된다. 
그 전에 일단, 라즈베리파이에서 ssh를 쓸 수 있게 설정을 해야한다. 

```
$ sudo raspi-config
interfacing options에서 ssh를 enable
```

그리고 이 라즈베리파이의 ip주소를 알아야한다. 

```shell
$ ifconfig
# ...
# wlan0:
#	inet 192.168.0.10 
#	이게 ip주소다.
```

그럼 이제 원격으로 조종할 컴퓨터에서 아래 명령을 실행한다. 

```shell
jimin@fossa:~ $ ssh pi@192.168.0.10
# .. yes
# .. 비번입력
pi@raspberrypi: ~ $
# 연결완료!
```

## 2. 파일 시스템 명령어

리눅스 명령어는

- What: 뭐를
- Where: 어디서

이 두가지를 잘 줘야한다. 



### 네비게이션

내가 어디있고, 여기엔 뭐가 있는지를 알아보는 명령어들.

```shell
pi@raspberrypi:~ $ pwd 
/home/pi
# 내가 어디있는가.

pi@raspberrypi:~ $ ls
Bookshelf  Documents  Music     Public          Templates
Desktop    Downloads  Pictures  rpi_kernel_src  Videos

# pwd: print working directory
# cd: change directory
# ls: list directory contents

pi@raspberrypi:~ $ cd rpi_kernel_src

pi@raspberrypi:~/rpi_kernel_src $ ls
build_rpi_kernel.sh  linux
# 현재 디렉토리 안에 뭐가 있는지 나열한다.

pi@raspberrypi:~/rpi_kernel_src $ ls /bin
....bin 안에 있는 파일들... 
# ls <경로> 
# ls 뒤로 경로를 주면, 현재 위치한 디렉토리에서 /bin 경로의 내용을 나열한다. 
# cd <경로> 는 직접 이동하는 거고, ls <경로> 는 내용을 나열하는 거임.

pi@raspberrypi:~/rpi_kernel_src $ cd ../Desktop
pi@raspberrypi:~/Desktop $ 
# .. 은 현재 디렉토리에서 한칸 위로 간다.
```

- 여기서 아주 중요한 거

```shell
$ ls /home 
# 여기서 앞에 /는 맨 위(root)를 뜻한다. 
# 따라서, 현재 디렉토리안에 있는 내용은 /로 검색할 수 없다. 
```

```shell
$ ls home
# / 없이 그냥 넣으면 현제 디렉토리안에서 home을 검색한다. 
```

터미널 명령은 `어디서` `무엇을` 이 두 가지를 줘야 한다. 

이번에는 어디서. 에 대해서 알아보았음. 
연습이 중요하니, 한 동안은 라즈베리파이를 터미널로 쓰는 연습을 해야겠다. 
그리고! 1년 무료인 aws서버도 접속해서 쓰는 연습을 합시다.

### 생성과 편집

```shell
$ clear
# 터미널 치우기
```

디렉토리 생성 => `mkdir` <위치>
텅빈 디렉토리 삭제 => `rmdir` <위치>

> rmdir 명령은 디렉토리가 비어있을 때만 가능하다. 

비어있지 않은 디렉토리 삭제 => `rm -r` <위치> 
그냥 `rm` 은 파일을 삭제한다. `-r` 플래그가 있어야 디렉토리를 삭제함. 

어떤 폴더를 싹 - 비우고 싶다면, `rm <폴더경로>/*`
`/*` 는 해당경로 아래 모든 애들을 뜻함.

```
-r, 디렉터리를 삭제한다, 하위의 내용을 먼저 삭제한다. (하위에 존재하는 파일이 남아있으면 안 되기 때문에) ("recursive", 재귀적으로)
-i, 삭제를 할 때에 매번 삭제 여부를 사용자에게 묻는다. ("interactive", 대화식으로)
-f, 존재하지 않는 파일을 무시하고 어떠한 확인 메시지도 보여주지 않는다. ("force", 강제로)
-v, 삭제를 하는 동안 삭제되는 내용을 보여준다 ("verbose", 장황하게)
```

[출처는 위키](https://ko.wikipedia.org/wiki/Rm_(%EC%9C%A0%EB%8B%89%EC%8A%A4))

``` shell
pi@raspberrypi:~ $ pwd
/home/pi
pi@raspberrypi:~ $ mkdir test_files
pi@raspberrypi:~ $ ls
Bookshelf  Documents  Music     Public          Templates   Videos
Desktop    Downloads  Pictures  rpi_kernel_src  test_files
pi@raspberrypi:~ $ rmdir test_files
pi@raspberrypi:~ $ ls
Bookshelf  Documents  Music     Public          Templates
Desktop    Downloads  Pictures  rpi_kernel_src  Videos
pi@raspberrypi:~ $ mkdir /home/pi/test
pi@raspberrypi:~ $ ls /home/pi
Bookshelf  Documents  Music     Public          Templates  Videos
Desktop    Downloads  Pictures  rpi_kernel_src  test
pi@raspberrypi:~ $ rmdir /home/pi/test
pi@raspberrypi:~ $ ls /home/pi
Bookshelf  Documents  Music     Public          Templates
Desktop    Downloads  Pictures  rpi_kernel_src  Videos
```

**파일 생성과 읽기**

- `nano <파일명>`
- `cat <파일명>`

파일생성 => ` nano <파일명>` 파일명은 나중에 줘도 됨.
파일명을 줄때는 파일형식을 정해줘야한다.

그럼 편집기가 열린다. 
내용을 넣고, `F2`또는 `ctrl+x` 를 누르고 `y`를 주면 저장이 된다. 

그리고 파일 내용을 읽는 것은 `cat <파일명>`

> concatenate : "여러개를 연결하다" 에서 온것이 **cat**

```shell
jimin@fossa:~/Desktop$ nano test.txt
# 편집기에서 내용을 넣는다. 
jimin@fossa:~/Desktop$ cat test.txt
test content.
linux learning.
apple
123
```

### 옮기기, 복사, 삭제

> 파일이름에 공백을 넣지말자.
> _ 언더 스코어를 넣어서 이름을 짓자.
>
> 왜냐하면, 공백으로 해도 에러는 없지만, 
> 명령어로 파일을 검색하기 힘듦.

옮기기 => `mv <현재경로> <바꿀경로>`
이름바꾸기 => `mv <현재이름> <바꿀이름>`

`mv`로 둘다 하는게 이상할 수도 있는데, 생각해보면 이름바꾸는 거나, 위치 바꾸는거나 똑같음.
경로를 바꾸는 거랑, 이름을 바꾸는 것이 같다는 것!!
컴퓨터 입장에서 생각해봅시다.

복사하기 => `cp <복사할애 경로> <복사결과 경로>`



## 3. 명령어 심화

### wildcard

\*

만약에 특정 디렉토리에 `jimin1.txt`, `jimin2.txt`, `jimin3.txt` .. 이런 파일들이 가득하다고 하자. 
그리고 이 모든 파일을 다른 디렉토리로 옮기고 싶을때.

```shell
$ mv jimin1.txt ../test2
$ mv jimin2.txt ../test2
...
```

이렇게 하는 건 너무 귀찮다. 

이때 쓰는게 와일드 카드이다. 

```shell
$ mv jimin*.txt ../test2
```

이렇게 하면, `jimin<뭐든>.txt` 파일 전부 한방에 명령이 적용된다. 

특정 디렉토리에 있는 파일을 전부 옮기고 싶다면?

```shell
$ mv ../test2/*.txt .
# test2에 있는 txt파일 전부를 현재 디렉토리로 옮긴다. '.'은 현재 디렉토리를 뜻함.

$ cp ../test2/*.txt .
# test2에 있는 txt파일 전부를 현재 디렉토리에 복사한다.

$ cp ../test2/*.* .
$ cp ../test2/* .
# 파일형식에 상관없이 모든 파일을 복사
```

```shell
$ nano test.py
# 코드 작성...
$ python3 test.py
# => 코드 실행됨.
```

### command output to files

\>, >>

```shell
$ touch test/jimin.txt
# 해당 파일을 만든다.  
# nano는 편집을 해서 만드는데, 이건 그냥 텅빈 파일을 바로 만들어준다.
```

```shell
jimin@fossa:~/Desktop$ mkdir command
jimin@fossa:~/Desktop$ ls
command  repo  test  메모
jimin@fossa:~/Desktop$ ls test
jimin.txt  jimin1.txt  jimin2.txt  jimin3.txt
jimin@fossa:~/Desktop$ ls test>command/dir.txt
jimin@fossa:~/Desktop$ ls command
dir.txt
jimin@fossa:~/Desktop$ cat command/dir.txt
jimin.txt
jimin1.txt
jimin2.txt
jimin3.txt
# ls test>command/dir.txt
# ls test 명령의 결과가 command/dir.txt에 담긴다. 
```

```shell
$ [명령]>[결과를 저장할 파일]
```

이렇게 해서, 명령의 결과를 파일에 저장할 수 있다. 

근데, 그 파일에 더 쓰고 싶다면?
똑같이 >를 써서 같은 파일에 넣으면, 덮어쓰게 된다. 

그때는 >>를 쓰면 내용추가가 된다. 

```shell
jimin@fossa:~/Desktop/test$ ls
jimin1.txt  jimin2.txt
jimin@fossa:~/Desktop/test$ ls> ../command/dir.txt
jimin@fossa:~/Desktop/test$ cat ../command/dir.txt
jimin1.txt
jimin2.txt
# > 덮어쓰기
jimin@fossa:~/Desktop/test$ ls>> ../command/dir.txt
jimin@fossa:~/Desktop/test$ cat ../command/dir.txt
jimin1.txt
jimin2.txt
jimin1.txt
jimin2.txt
# >> 추가하기

```

> 여러가지 명령의 결과를 파일에 저장하는 방법을 배웠다.
>
> \> 는 override
> \>>는 append

### sort

```shell
$ sort [파일]
# 파일내용 줄단위로 알파벳순으로 정렬한 내용을 출력

$ sort -r [파일]
# 역순으로 정렬
```

```shell
jimin@fossa:~/Desktop/test$ nano my.txt
jimin@fossa:~/Desktop/test$ sort my.txt
asdf
bsdf
csdf
xsdf
zsdf
jimin@fossa:~/Desktop/test$ cat my.txt
zsdf
xsdf
asdf
csdf
bsdf # 원본 내용은 그대로..
# 만약 정렬한 내용을 저장하고 싶으면 아까배운 >를 쓰면 됨.
```

```shell
$ sort jimin.txt>./sortedfile.txt
# 정렬한 내용을 새로운 파일에 저장.
```

```shell
$ sort -n test.txt
# 오름차순으로 정렬
$ sort -r -n test.txt
# 내림차순으로 정렬
$ sort -M test.txt
# 날짜 january, february.. 순으로 정렬, 근데 이런거 쓸 일 아예 없을 듯..
```

### 경로 이름 심화

```
/ 는 루트 폴더
~ 는 home/사용자 폴더
. 는 현재 폴더
.. 윗 폴더
```

 ### pipe, tee

|, |tee

리눅스 커맨드는 인풋을 받고 아웃풋을 내준다. 
파이프는 첫 커맨드의 아웃풋을 다음 커맨드의 인풋으로 넣어준다. 

```shell
$ cat jimin.txt
c
b
a
$ cat jimin.txt|sort
a
b
c
# 이건 그냥 sort jimin.txt해도 같지만, 
# 암튼, [아웃풋이 있는 명령]|[인풋을 받는 명령] 
# 이렇게 작성을 해서, 말그래도 파이프처럼 결과를 이어서 명령을 줄 수 있다. 
```

파이프가 한 군데에 아웃풋을 보낸다면, tee는 여러곳으로 보낸다.

[여기 자세한 설명](https://www.lesstif.com/lpt/linux-tee-89556049.html)

```shell
$ echo "hello" | tee hello.txt
# hello.txt파일에 echo의 아웃풋은 hello가 기록된다.

$ ls / > file1.txt
# > 는 아웃풋을 파일에 저장시켜준다.

$ ls ~ |tee file1.txt file2.txt file3.txt
Android
Arduino
Desktop
...
# tee는 인풋을 읽어서, 아웃풋을 화면에 출력함과 동시에 파일에 쓰는 역할을 한다.
# 화면과 파일에 동시 출력하고 싶을때 쓰는 명령어이다.

$ ls / |tee file1.txt
bin
boot
..
# 내용이 있는 파일에 tee를 하면 override가 된다. 
# append를 하기 위해서는 -a 플래그를 추가한다. 
$ ls / |tee file1.txt -a
```

### find, grep, sed

```shell
$ find [어디서부터찾을 것인지] -name "[찾을파일 또는 디렉토리]"
$ find ~ -name "test.txt"
/home/jimin/Desktop/test.txt
# 사용자 디렉토리부터 탐색해서 test.txt를 찾고, 경로를 출력한다. 

$ find ~/Desktop/repo -name "*.py"
# repo 경로안 모든 py파일을 찾음.

$ find . -name "*.py">./test/allpy.txt
# 현재 디렉토리부터 모든 파이썬 파일을 찾아서 출력된 경로를 텍스트로 저장

$ sudo find / -name "ruby*"
# 루트부터 찾을때는 sudo를 해줘야 "허가거부"가 안남.
# 모든 ruby~~ 를 찾는다. 
```

만약에 파일내용 중에 찾고 싶은게 있다면?

그때는 grep

```shell
$ grep [-r를 하면 재귀적으로] [찾을내용] [어디서부터]
$ grep jimin ./test.txt
# test.txt파일에서 jimin이라는 내용을 찾아서 출력
# 이런 거는 별 의미없음.
# 만약에 /home폴더의 모든 파일에 대해서 jimin이라는 내용 찾고 싶다면?
$ grep -r jimin /home
./Desktop/repo/test.txt: jimin ...
..
# -r 플래그를 추가하면 재귀적으로 모든 디렉토리 파일에 대해서 찾을 수 있음
```

파일 내용을 수정할때, sed
자세한 설명은 ... [여기](https://mug896.github.io/sed-stream-editor/basics.html)

만약 test.md파일내에 있는 모든 hello란 단어를 yes로 바꾸고 싶다면?

```shell
$ sed 's/hello/yes/' test.md
```



## 4. 유저, 권한

### 새로운 유저 추가하기

home 디렉토리 안에 유저가 있다. 
원래는 pi 하나밖에 없다. 내 우분투에는 jimin하나 밖에 없다. 

여기가 내가 파일을 만들고, 작업을 하는 공간이다. 

```shell
pi@raspberrypi:/home $ sudo useradd earth -m -s /bin/bash -g users
# -m: 디렉토리를 만들어라. -s: 기본 쉘 지정 -g: 유저그룹지정
pi@raspberrypi:/home $ sudo passwd earth
# 비번 지정

pi@raspberrypi:~ $ sudo login
# 그리고 계정-비번 입력
earth@raspberrypi:~ $
# 그럼 이렇게 pi에서 earth로 바뀜.
# 이 유저는 sudo를 쓸 수 없음. 
# 다른 유저의 파일을 열람할 수는 있지만, 편집할 수는 없음. cat은 되지만, nano는 안됨.
earth@raspberrypi:~ $ logout
pi@raspberrypi: ~$
# logout하면 다시 pi로 돌아옴.
pi@raspberrypi:/home $ sudo userdel earth
# 이렇게 지우면 ls해도 아직 폴더가 남아있음.
# 그래서 깔끔하게 지울려면 sudo userdel -r earth로 -r를 붙여줘야함.
pi@raspberrypi:/home $ sudo userdel -r earth
```

### sudo권한 유저 추가하기

pi같은 sudo를 쓸 수 있는 슈퍼유저를 만드는 방법

```shell
$ sudo adduser jimin
# 비번입력, 그 뒤로도 뭐나오는데 무시해도됨.
# 유저를 만들고 다음으로 권한을 부여해야함
$ sudo usermod -a -G adm,dialout,cdrom,sudo,audio, .. 줄 권한을 선택
# 그리고 sudo jimin으로 만든계정으로 sudo를 하면 됨. 
```

### 파일, 폴더 권한

권한 보기

```shell
$ ls -l
합계 8
drwxrwxr-x 8 jimin jimin 4096  9월  9 19:20 repo
-rw-rw-r-- 1 jimin jimin    0  9월  9 23:06 test.txt
drwxrwxr-x 5 jimin jimin 4096  9월  6 01:09 메모
# -l 옵션은 폴더 및 파일의 자세한 정보를 보여준다. 
# 정보에는 permission권한 관련 내용도 있다. 

# 처음문자는 디렉토리인지 파일인지 나타낸다. 
# 그리고 이어서 3묶음씩 3개의 정보가 온다. 
# 1 = d: 디렉토리, -: 파일
# 2,3,4 = read, write, execute: r, w, x 파일을 소유한 유저(pi)의 권한(-는 권한이 없다는 뜻)
# 5,6,7 = read, write, execute: r, w, x 파일을 소유한 그룹의 권한
# 8,9,10 =read, write, execute: r, w, x 외부자의 권한
```

```shell
-rw-rw-r--
# 그냥 만든 파일의 권한: 그룹 사용자가 수정가능
-rw-r--r--
# sudo .. 로 만든 파일: 그룹 사용자 수정불가
# 수정을 위해서는 sudo nano를 해야함.
```

권한 수정

```shell
$ chmod [내용] [타겟파일]

# d rwx rwx rwx = 이게 권한 내용이다. 
# d 421 421 421 = 이렇게 숫자가 부여된다.  
$ chmod 000 test.txt
$ ls -l
---------- : 이렇게 모두든 권한이 없는 파일이 만들어진다. 
# r--r--r--: 이렇게 권한설정을 하고 싶다면?
$ chmod 444 test.txt
# rwxrw-r--: 이렇게는.. 
$ chmod 764 test.txt
# r-xr--rwx: 
$ chmod 547 test.txt

# 디렉토리의 권한
# 똑같은데, 그냥 -r 만 더하면 됨. rm이랑 같음. 재귀적으로.. 라는 뜻.
$ chmod 700 -r folder
# -r 플래그는 재귀적으로 아래모두에 적용시킨다는 뜻임.
```



# 기타

### 파이 안전하게 끄기

그냥 전원을 뽑아서 끄는 방법은 별로다. 
디스크가 망가질 수 있으니, 꼭 안전하게 끄도록 하자. 

```shell
$ sudo halt 
# 끈다. 

$ sudo reboot 
# 다시시작.

$ sudo shutdown -h now 
# 끈다.(shutdown) -하던거멈추고(-halt) 지금당장(now)

$ sudo shutdown -h now -r
# 끈다, 다멈추고, 지금당장, 그리고 다시시작
```

끌때는 그냥 `sudo halt` 하면 됨.

그리고 sd카드를 제거할 때는 컴퓨터를 끄고, 전원선을 뽑은 다음에 빼야한다.
