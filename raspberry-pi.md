# 라즈베리파이

### 설치

공식 사이트로 가서, recommed software를 받았다. 
zip파일로 받으면 엄청 느리니까, 토렌트로 받는 것이 좋다.

SD를 부팅디스크로 만들고, 라즈베리파이 뒷판에 연결부에 삽입.
그리고 모니터, 마우스, 키보드, 전원을 연결한다. 

기본세팅

- 처음 지역은 기본(영국)으로 하고, US키보드를 체크한다. (한국으로 하면 와이파이가 안잡힘)
- 그리고 뒤에는 알아서 하고, 그럼 재부팅을 하라고 한다. 
- 그리고 터미널을 켜서 언어관련 설정을 해준다. 

```shell
~$ sudo su
~$ raspi-config
# local.. 무슨 설정에 들어간다. 
# 언어목록이 나오면, en_GB.UTF-8, en_US.UTF-8, ko_kr.UTF-8 을 체크(스페이스바)
~$ # 뭔가 설치됨.
~$ apt-get update
~$ apt-get upgrade

# 한글설정
~$ apt-get install ibus
~$ apt-get install ibus-hangul
~$ fonts-unfonts-core
# 재부팅하면, 한글 잘보임.

```

