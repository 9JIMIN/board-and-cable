# 라즈베리파이 - 리눅스

- 위키 문서 [명령어 테이블](https://ko.wikipedia.org/wiki/%ED%8B%80:%EC%9C%A0%EB%8B%89%EC%8A%A4_%EB%AA%85%EB%A0%B9%EC%96%B4)
- 위키 문서 [미디어 저장소](https://commons.wikimedia.org/wiki/Category:Unix_reference_cards?uselang=ko)

## lesson 01~02

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

## lesson 03~06

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
