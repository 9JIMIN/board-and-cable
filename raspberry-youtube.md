# 라즈베리 파이 - 리눅스 강의

## lesson 01 - 소개

아두이노는 메모리가 부족해서 이용이 제한된다. 
게다가 운영체제도 없어서 코드를 넣고 그것만 실행을 한다.

라즈베리파이는 메모리도 넉넉하고,
운영체제도 있다.



## lesson 02 - OS설치, 원격접속

SD카드는 항상 라즈베리파이에 박혀있어야 한다. 
설치는 뭐, 패스하자.

**원격접속 방법**

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



## lesson 03 - 파일 시스템

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



## lesson 10 - 파이 안전하게 끄기

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
