# 아두이노 노트



##   01. 시작하기

**IDE설치, 설정**

공식 사이트에서 `linux64 ~tar.xz` 압축파일을 다운받고, 압축을 푼다.
압축푼 폴더를 jimin 즉, 사용자 디렉토리에 넣는다. 

> 사용자명 폴더에는 Desktop 같은 애들이 있다. 잘 확인하고, 여기에 폴더를 두어야함.

터미널로 압축 푼 폴더에 들어간다.
그리고 터미널에 아래 명령을 준다.

```shell
$ ./install.sh
# sudo는 절대 붙이지 않는다. 
```

그럼 설치끝.

하지만, 이렇게만 하면 아두이노에 코드를 올릴 수 없다. 
권한설정을 해줘야 한다. 

```shell
jimin@fossa:~$ ls -l /dev/ttyACM*
crw-rw---- 1 root dialout 166, 0  9월  1 01:07 /dev/ttyACM0
jimin@fossa:~$ sudo usermod -a -G dialout jimin
[sudo] jimin의 암호: 
jimin@fossa:~$ 
```

그리고 컴터 다시시작.
그럼 모든 설정 완료.

**LED깜빡이기**

코드를 업로드하기 전에는 보드를 리셋해야한다. 
보드의 빨간버튼을 누르면 된다.

```c++
void setup() {
// 처음에 한번만 실행되는 코드
}

void loop() {
// 반복되는 코드
}
```

ide에서 `Tools - port`에서 연결된 아두이노를 볼 수 있다.

```c++
void setup() {
    pinMode(13, OUTPUT); 
    // 아두이노의 13번 핀을 이용한다. 그리고 그건 OUTPUT이다.
    // 13번 핀은 보드에 기본으로 내장된 LED에 연결되어있다. 
}

void loop() {
    digitalWrite(13, HIGH);
    // HIGH: LED가 켜진다. 
    // LOW: LED가 꺼진다.
}
```

깜빡이게 하기 위해서는 `HIGH`와 `LOW`중간에 딜레이가 있어야한다. 

```c++
void setup() { 
  pinMode(13, OUTPUT);
}

void loop() {
  digitalWrite(13, HIGH);
  delay(50);
  digitalWrite(13, LOW);
  delay(50);
}
```



##   02. LED 이해하기

- 전구: 와이어에 전기가 흐르면 빨간 빛을 낸다. 더 흘려보내면 하얀 빛을 낸다.
  이건 빛을 내지만, 엄밀히 열을 낸다. 에너지 낭비가 심함. (95%가 열, 5%가 빛)
- 도체: 전자가 원자가띠-전도띠(conduction band-valence band) 사이를 자유롭게 오가면서 전도성이 큰 물질.
- 부도체: 원자가띠-전도띠가 도체처럼 겹쳐지지 않고, 틈이 크기때문에 전자가 오갈 수 없다. (전도성이 작아진다.)
- 반도체: 전류를 흘려주는 방향에 따라, 도체가 되기도 하고 부도체가 되기도하는 물질.

- LED: Light Emitting Diode, 빛을 내는 반도체 소자.


> 웨이퍼(wafer):  집적회로 제작을 위해 실리콘 반도체 소재를 얇게 깎아 만든 기판.
> 집적회로(integrated Circuit): 반도체로 만든 전자회로의 집합.

전자는 에너지를 가진다. 
전자는 에너지 밴드에 속해야한다.

실리콘은 전자가 잘 묶여있다. (conduction-band는 텅비고, valence-band는 꽉 차있다.)

전자는 물이 아래로 흐르듯, 낮은 밴드(Valence Band)에 속하려 한다(에너지를 방출 하고자 한다).

전기를 흘려보내면, 마치 물이 담긴 그릇을 기울이는 것처럼 밴드의 전자를 움직이게 한다. 
하지만, 실리콘은 valence band는 꽉차고, conduction band는 텅비어서, 전자가 움직일 수 없다.(=전기가 흐를 수 없다.)

이때 온도를 높이면, 운 좋은 전자 몇개가 conduction 밴드로 점프한다.
그럼, 텅 비었던 conduction밴드에는 몇 개의 전자가 생기고, 꽉 차있던 valence밴드에 몇 개의 구멍이 생기게 된다.
이런 상태에서 전기를 흘려서 기울이게 되면, 전자가 드디어 움직인다.(=전기가 흐른다.)

이런 순수한 물질인 실리콘에 불순물 원자(donor)를 넣는다. (이 불순물 원자는 여분의 전자를 가지고 있다.)
이 원자들은 conduction밴드 근처에 위치하며 여분의 전자를 conduction밴드로 donate 한다.    
그래서 conduction밴드는 쉽게 전자를 얻게 된다. (이동할 수 있는 전자를 가지게 된다.) 
valence밴드는 여전히 꽉차서 전자가 못 움직인다. 

이러한 conductor(전도체)를 n타입이라고 한다. n은 전자를 뜻한다.
**n타입에서 전도는 전자에 의해 일어난다.**

반대로, 여분의 전자가 아닌, 부족한 전자를 가진 불순물(acceptor)을 valence밴드 근처에 위치시켜보자. 
그럼 꽉찬 valence밴드에 있는 전자가 불순물 원자로 이동하게 되면서 밴드에 구멍이 생기게 된다. 

**이것이 p타입이다. 이 물질에서 전도는 구멍(홀)을 통해 일어난다.** 

n-타입 물질과 p-타입 물질을 접합한다. 
두 물질에는 에너지 밴드차이가 있어서 낮은 준위의 n타입 conduction밴드의 여분의 전자는 높은 준위의 텅빈 p타입의 conduction밴드로 이동할 수 없고, 
p-타입의 구멍은 n-타입으로 이동할 수 없다.
쉽게 말해, 에너지 베리어에 막혀서 전자와 구멍이 n-p물질 사이를 이동할 수 없다. 
이때 전압을 걸어준다. (-를 n에, +를 p에)

> 아마도, -는 전자를 공급, +는 전자를 빼가는 역할을 하는 듯, 그래서 전자가 남는 n-타입에 전자는 더 공급하고, 구멍이 몇개 있는 p-타입에는 구멍을 더 만들고..
> 그래서 에너지 준위를 맞추는 것 같다. 

아무튼, 그렇게 에너지 밴드 차이를 낮추게 되고, 전자와 구멍이 이동할 수 있게 된다. 
n-타입의 여분의 전자가 p-타입의 conduction밴드로 이동하고, 그 전자는 다시 valence밴드로 내려오게 된다. 
이때 전자는 에너지를 뿜는데, 이 과정에서 나오는 게 photon(광자)이다.

전압을 더 높이면, 에너지 베리어가 더 낮아지면서 전자와 구멍이 더 많이 이동을 하고, 광자가 더 많이 뿜어져나오면서 더 밝아진다. 

그리고 이때 나오는 빛의 색은 재결합이 일어날때 전자가 이동한 밴드 갭의 크키가 결정한다. 
갭이 크면 더 큰 에너지를 가진 빛(자외선) 이 나오고, 갭이 작으면 에너지가 작은 빛(적외선) 이 나온다.
따라서 원하는 빛이 있다면, 그 만큼의 밴드갭을 가진 물질을 만들면 된다.  

이것이 LED가 작동하는 방식이다. 

근데 반대로 -를 p에, +를 n에 연결하게 되면, 에너지 베리어를 높이게 된다. 
그럼 전자나 구멍은 이동을 못한다.

이게 다이오드가 한 방향으로 전기를 흘리는 방법이다. 
제대로 연결해야 전기가 흐른다. 

하나더.
전압과 전류는 비례한다. 
그 이유는 전압을 높이면, 에너지 베리어가 낮아지게 되고 그래서 전자나 구멍이 더 잘 이동하게 되고, 
이걸 전류량이 많아진다. 

하지만, 다이오드에서는 전압이 양의 방향일 때(제대로 연결했을 때는) 그에 비례해서 전류도 높아지지만, 
음의 값일 때 전류는 0 이 된다. 
그리고 다이오드의 전압에 대해 전류는 제곱함수 그래프를 그린다. (전압이 높아질 수록, 흐르는 전류도 빠르게 커진다는 뜻.)
그래서 높은 전압에 다이오드를 다이렉트로 연결하면 다이오드가 탈 수 있다.  

##   03. 빵판

**빵판의 구조**

빵판의 긴부분을 옆으로 해서 두었을 때,

아래 위 두 부분으로 나뉜 것을 볼 수 있다.
각 부분의 세로로 나란한 구멍들은 연결되어있다.
하지만, 두 부분으로 나눠진 부분은 연결되지 않는다. 

옆으로 나란한 구멍들은 연결되지 않았다. 

아래위에 보면 +, - 로 표시된 긴 부분의 가로로 모든 구멍이 연결되어 있다. 

<img src='./images/빵판.png' width=500>

**회로구성 실습**

> 아두이노의 1~13 번 핀이 전압을 준다. ( + )
> GND 핀은 ( - ) 이다. 
>
> 회로를 구성할때, 저 두 핀을 사용한다. 
>
> LED의 긴 다리가 p, 짧은 다리가 n이다. p가 +, n이 -에 연결되어야 한다. 

아래와 같이 회로를 구성하고, 아래 코드를 넘겨보자.

<img src='./images/빵판-led.jpg' width='500'>

```c++
void setup() {
  pinMode(13, OUTPUT);
}
// 핀 번호는 1~13 중 아무거나 상관없다. 선을 연결한 핀을 넣어주면 된다. 
void loop() {
  digitalWrite(13, HIGH);
  delay(900);
  digitalWrite(13, LOW);
  delay(100);
}
```

그럼 LED가 깜빡인다!!

> 숙제
>
> LED 3개를 다른 주기로 깜빡이게 하기.

<img src='./images/빵판-led-3.jpg' width=500>

빵판의 최상단 길게 행으로 연결된 곳에 GND를 넣어서 받게 했다. 

```c++
void setup() {
  pinMode(13, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(11, OUTPUT);

}

void loop() {
  digitalWrite(13, HIGH);
  delay(900);
  digitalWrite(13, LOW);
  delay(100);
  digitalWrite(12, HIGH);
  delay(90);
  digitalWrite(12, LOW);
  delay(10);
  digitalWrite(11, HIGH);
  delay(500);
  digitalWrite(11, LOW);
  delay(500);
}
```

이제 빵판을 어떻게 쓰는지 알겠슴다. 

##   04. 변수

. . . ㅡ ㅡ ㅡ . . .

위의 모스부호는 SOS를 의미한다. 
이걸 LED로 나타내는 방법은 간단하다. 

루프 함수 안에 다 - 넣어주면 된다.

```c++
digitalWrite(8, HIGH);
delay(50);
digitalWrite(8, LOW);
delay(50); // 이걸 3번 반복해서 쓰고,
// ...
digitalWrite(8, HIGH);
delay(500);
digitalWrite(8, LOW);
delay(50); // 이것도 3번 반복
// ...
digitalWrite(8, HIGH);
delay(50);
digitalWrite(8, LOW);
delay(50); // 또 3번
// ...
```

넘나 비효율적이다. 

```c++
int led = 8;
int st = 100;
int lg = 500;
int tm = 150;
int looptm = 1000;

void shortSignal() {
  digitalWrite(led, HIGH);
  delay(st);
  digitalWrite(led, LOW);
  delay(tm);
}

void longSignal() {
  digitalWrite(led, HIGH);
  delay(lg);
  digitalWrite(led, LOW);
  delay(tm);
}

// setup(), loop() 함수전에 필요한 애들을 먼저 정의하고, 함수안에서는 가져다 쓰기만 한다.
void setup() {
  pinMode(led, OUTPUT);
}

void loop() {
  shortSignal();
  shortSignal();
  shortSignal();
  longSignal();
  longSignal();
  longSignal();
  shortSignal();
  shortSignal();
  shortSignal();
  
  delay(looptm);
}
```

- setup(), loop() 안에서 쓸 값, 함수는 밖에서 미리 정의를 하자.



##   05, 06. 바이너리

디지털 디바이스는 스위치의 집합, 켜지면 1, 꺼지면 0.
음악, 색깔, 텍스트 모두 숫자로 바꿀 수 있다. 
이것이 컴퓨터가 0, 1로 세상을 이해하는 방식이다. 

십진법은 0, 1, 2, ... 9 까지의 부호가 있다. 
그리고 10이라는 숫자를 표현해야할때, 자릿수를 늘린다. 

이진법은 0, 1 두개 뿐이다. 표현한는 방법은 이미 잘 알고 있으니 패스.

이진법을 1부터 쭉 나열하면 규칙이 보인다. 
첫째자리는 1, 0의 반복
둘째자리는 11, 00의 반복
셋째자리는 1111, 0000의 반복
왜냐하면 각 자리는 1, 2, 4, 8, 16 의 값을 가지니까.

십진법의 각 자리는 1, 10, 100 의 값을 가짐. 

그리고 아두이노나 컴퓨터의 실리콘안에 있는 것이 0, 1을 위한 스위치들이다. 

**LED 바이너리 카운터 만들기**

1~15까지의 십진법 숫자를 바이너리로 LED를 통해 출력되도록 하는 것이 숙제였다. 
그냥 1~15를 0001, 1111까지 배열에 저장해서 해도 되지만, 난 직접 이진법으로 변환을 하고 싶었다. 

근데 C언어가 익숙하지 않아서, 완성하지 못했다. 
나중에는 꼭 직접 인풋을 받아서, 변환한 바이너리를 출력하는 완성도 있는 애를 만들어보자!

아래는 바이너리 값을 다 저장해서 만든 프로그램.

```c++
int pin = 8;
int wait = 500;
int bin[15][4] = {
  {0, 0, 0, 1},
  {0, 0, 1, 0},
  {0, 0, 1, 1},
  {0, 1, 0, 0},
  {0, 1, 0, 1},
  {0, 1, 1, 0},
  {0, 1, 1, 1},
  {1, 0, 0, 0},
  {1, 0, 0, 1},
  {1, 0, 1, 0},
  {1, 0, 1, 1},
  {1, 1, 0, 0},
  {1, 1, 0, 1},
  {1, 1, 1, 0},
  {1, 1, 1, 1},
};

void setup() {
  for (int p = 0; p < 4; p++) {
    pinMode(pin + p, OUTPUT);
  }
}

void loop() {
  for (int i = 0; i < 15; i++) {
    for (int j = 0; j < 4; j++) {
      if (bin[i][j] == 1) {
        digitalWrite(pin + j, HIGH);
      } else {
        digitalWrite(pin + j, LOW);
      }
    }
    delay(wait);
    for (int k = 0; k < 4; k++) {
      digitalWrite(pin + k, LOW);
    }
    delay(wait);
  }
}
```

요렇게 단순하게 코딩하고, 

핀 8, 9, 10, 11 에 LED와 저항을 각각 연결해서 배열의 값이 1 이면 켜지게 했다.

- LED나 저항을 같은 열에 연결하면 안된다. 2개 다리를 각각 다른 열에 연결해야함.
- 왜냐하면 Row(열) 끼리는 연결되어 있지 않기때문에, 다리를 통해서 연결해야함.
- 그리고 저 아래위 판이 분리된 것을 활용해서 저항을 세로로 연결, 아래와 같이 깔끔하게 회로를 구성할 수 있다.

<img src='./images/바이너리.jpg' width=500>



##   07. - analogWrite

지금까지 배운 아두이노 명령은,  LED를 켜고, 끄는 것이 전부였다.
0 과 1, 즉 digitalWrite() 만 이용했다. 

켜고 끄는 사이의 값을 주는 명령은 analogWrite() 이다.  

analogWrite 명령은 ~ (물결) 표시가 있는 pin만 가능하다. (3, 5, 6, 9, 10, 11)

```c++
void setup(){
    pinMode(9, OUTPUT);
    // 3, 5, 6, 9, 10, 11핀만 analogWrite가 가능하다. 
}
void loop(){
    analogWrite(핀, 숫자);
    // 숫자는 0 ~ 255 의 범위를 가진다. 
}
```

- 0 은 0 볼트
- 255 는 255볼트가 아니라, 5볼트이다. 

숫자의 범위는 0~255 까지, 8bit 즉 1바이트의 값이 가능하다.  
이것은 0~5볼트에 대응한다.



##   08. PWM

Pulse Width Modulation(펄스- 폭 - 변조)

강의에서는 오실로스코프를 회로에 연결해서 흐르는 전압을 측정할 수 있게 하였다. 

오실로스코프를 통해서 analogWrite의 값에 따라서 전압이 변하는 것을 볼 수 있다. 
여기서 재밌는것은, 0과 255는 일직선으로 나오는 것에 반해서, 
그 사이 값들은 0과 255 값들의 반복으로 나온다. 

그 이유는 예를 들어, 127을 넣으면 아두이노가 딱 저만큼의 전압을 흘려주는 것이 아니라, 
0과 255를 반반씩 반복하면서 그 사이값인 127을 평균적으로 만들어내는 것이다. 

그래서 10을 넣으면, 0을 더 오래 반복하고, 200을 넣으면 255인, 5볼트 전압을 더 오래 유지하게 된다.
반복되는 주기는 언제나 같지만, 
주어진 전압에 따라서 한주기 안에서 0과 5v가 차지하는 비율이 변하는 것이다.

그런 식으로 아날로그 값을 준다. 
결국 아날로그도 디지털의 빠른 반복으로 만들어내는 것이다.

그리고, 이걸 평균값으로 만들어주는 소자가 있다. 
그것이 바로, capacitor, condensor, 축전기로 불리는 장치이다. 

이걸 회로에 연결하고, 오실로스코프를 보면, 0과 255를 넘나드는 그래프가 
그 사이 값을 가진 일직선으로 부드럽게 바뀐다.

> 아두이노에 analogWrite는 키고 끄고를 반복해서 사이값을 만들어내는 것이다. 
> 콘덴서, 케퍼시터는 이런 반복을 부드럽게 바꿔준다.



##   09. 옴의 법칙

물의 흐름에 비유를 하자면, 
전압은 수압, 물의 압력이고.
전류는 흐르는 물이라고 볼 수 있다. 

그래서 회로도에서 v 부분은 펌프정도로 생각하면 된다. 
압력이 높으면(볼트가 높으면), 물이(전기가) 많이 흐른다. 

그리고 저항은 물이 흐르는 파이프의 관을 좁히는 장애물로 볼 수 있다. 
당연히 파이프가 좁아지니 물은 덜 흐르게 되고, 압력은 높아질 수 밖에 없다. 

이것이 옴의 법칙이다.
물의 흐름으로 생각해보면, 아주 당연한 **3가지 값의 관계**이다.  

```
전압 = 전류 * 저항
// V = IR
```

그래서 회로에서 두 가지값을 알면 나머지 한 값을 알 수 있다.

> 전류는 + 에서 나온다. 
> 근데 전자는 - 에서 나온다. 

직렬로 저항이 연결되어 있으면, 
그냥 두 저항을 더하면 된다.
그럼 전체 회로에 흐르는 전류가 나온다. 

그리고 각각의 저항에 걸리는 전압을 그 저항*전류 로 구할 수 있다. 
그리고 그 두 전압을 더하면, 총 전압이 나온다.   

이걸 전압계로 측정할 수 있다. 
빵판에 직접 저항 두개로 간단한 회로를 만든다. 

> 아두이노에는 항상 5v 를 공급하는 핀이 있으니 거기에 연결하면 됨.

그리고 전압계를 회로 사이 적절하게 꽃아주면 계산한 전압값을 실제로 볼 수 있다.

> 한 회로 안에서 전류는 일정한데, 전압이 다르다는 것은 뭔가 이상하게 들린다. 
> 다시, 물의 흐름에 비유하자면, 흐르는 물의 양은 일정하지만, 관이 좁아지고 넓어지고에 따라서 구간마다 수압이 달라지는 것으로 비유할 수 있다. 

##   10. analogRead

digitalWrite, analogWrite 모두 아두이노에게 신호를 보내기만 했다. 
이번에는 반대로 아두이노로부터 신호를 읽어들이는 것에 대해서 알아본다.

값을 읽을 때는 A1~5 핀을 써야한다.
그리고 출력값은 실제전압은 아니다.

실제 전압이 0 이면 0,
5면 1023이 나온다. 

일단, 아래와 같이 코딩을 해서 출력값을 보자.

```c++
int readPin = A3;
int V2 = 0;
int delayTime = 500;

void setup() {
  pinMode(readPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  V2 = analogRead(readPin);
  Serial.println(V2);
  delay(delayTime);
}
```

모니터를 열면, 전압의 정확한 값이 나오는 게 아니라.
0 - 5 사이를 1023로 나눠져 있고, 측정값을 그 사이에 놓았을 때 대응하는 수가 출력되는 것이다. 

> 아두이노는 (2^10)인, 10bit로 결과를 출력할 수 있다.  0~ 1023

즉, 나온값을 1023으로 나누고, 5를 곱하면 전압을 구할 수 있다. 

```c++
int readPin = A3;
int readVal;
float V2 = 0;
int delayTime = 500;

void setup() {
  pinMode(readPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  readVal = analogRead(readPin);
  V2 = (5./1023.)*readVal;
  Serial.println(V2);
  delay(delayTime);
}
```

```c++
V2 = (5./1023.)*readVal;
```

5/1023을 하면 `int`로 계산을 해버려서 `V2`는 항상 0 이 되버린다. 
그래서 소수가 나올 수 있도록 소수끼리 나눠주는 것이다. ( 숫자 끝에 쩜을 더해주면 float로 인식을 한다.)

그리고 모니터를 보면 계산된 전압값이 나온다. 

> 아두이노로 전압을 읽는 것을 배웠다. 

##   11. Serial

핀을 쓰기 위해서는 pinMode() 를 setup() 에서 불러줘야한다. 
Serial모니터를 쓰기위해서도 setup()에서 불러줘야 한다. 

`setup()`설정은 `baud rate`라고 하는 통신속도를 정해주면 된다.
중요한건, 코드에서 정한 속도하고, 모니터의 속도가 일치해야한다.

```c++
int j = 1;
int waitT = 750;
String myString = "j = ";

void setup() {
    // Serial.begin(baud rate) => 시리얼 통신 속도이다. 
  Serial.begin(9600);
}

void loop() {
  // print() 는 줄바꿈을 안한다. println() 은 출력하고 줄을 바꾼다.
  Serial.print(myString);
  Serial.println(j); 
  j = j + 1;
  delay(waitT);

}
```

- 함수에서 쓸 값은 초기에 변수에 저장한다. 
- 보기좋게 출력을 한다.

```c++
int j = 1;
int waitT = 750;
float area;
float pi = 3.14;
float r = 2;
String m1= "A Circle With Radius ";
String m2 = " Has a Area of ";
String m3 = ".";

void setup() {
  Serial.begin(19200);
}

void loop() {
  area = pi * r * 2;
  Serial.print(m1);
  Serial.print(r);
  Serial.print(m2);
  Serial.print(area);
  Serial.println(m3);
  delay(waitT);
  r = r + .5;
}
```

```
시리얼 모니터
A Circle With Radius 96.50 Has a Area of 606.02.
A Circle With Radius 97.00 Has a Area of 609.16.
A Circle With Radius 97.50 Has a Area of 612.30.
...
```

##   12. Potentiometers

Potentiometers = 가변저항
말 그래로 값이 고정되지 않고 바꿀 수 있는 저항이다. 다리가 3개인 것이 특징.

원리는 그림을 보면 한방에 이해된다. [네이버 블로그의 설명](https://m.blog.naver.com/haneham/221235789444)

최대 저항값이 아두이노 킷에 있는 거는 10,000옴이다. 0~10K 까지 조절가능.

3개 다리를 빵판에 연결하고 LED를 넣어서 밝기 조절이 되게 만들 수 있다. 
그리고 11강에서 배운대로 하면, LED에 걸리는 전압도 모니터로 볼 수 있다. 

> 근데 아직도 전압을 측정할때, 어디에 꽃아야 되는 건지 이유를 잘 모르겠음.

##   13, 14. if

아두이노에는  ground 핀이 3개 였음.

이번 시간에는 if를 배우기 위해서 지난 시간에 배운 가변저항을 이용했다. 
가변저항을 조절하면서 전압에 따라 서로 다른 LED가 켜지게 하는 방식.

조건에 따라 다른 코드를 실행하는 if를 배우는 아주 좋은 방법같다. 

```c++
if(a>2 && a<5){
    // a가 2~5사이 숫자일때 실행됨. 
}else if(a==6 || a==7){
    // a가 6또는 7일때 실행됨.
}
// ==
// !=
// &&
// ||
```

가변저항에 선을 연결할때 변하는 저항값을 읽기 위해서는 

- 첫번째 다리를 5v전압에
- 가운데 다리는 읽을 핀에 (A~)
- 마지막 다리는 Ground 연결한다.

읽은 전압에 따라서 red, yel, gre이 켜지는 회로

> 저항 한개를 3개에 연결하는 참신한 방법!

<img src = './images/삼단계.jpg' width = 400>

```c++
int red = 8;
int yel = 9;
int gre = 10;
int readPin = A5;
float volt;
int waitTime = 500;

void setup() {
  pinMode(red, OUTPUT);
  pinMode(yel, OUTPUT);
  pinMode(gre, OUTPUT);
  pinMode(readPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  volt = (5./1023.)*analogRead(A5);
  Serial.println(volt);
  digitalWrite(gre, LOW);
  digitalWrite(yel, LOW);
  digitalWrite(red, LOW);
  if(volt<2){
    digitalWrite(gre, HIGH);
  }
  if(volt>2 && volt <4){
    digitalWrite(yel, HIGH);
  }
  if(volt>4){
    digitalWrite(red, HIGH);
  }
  delay(waitTime);
}
```

그리고, 읽은 전압에 따라서 LED 밝기를 조절하는 회로

<img src = './images/디밍.jpg' width = 400>

```python
int pin = 9;
int readPin = A5;
float val;
int waitTime = 500;

void setup() {
  pinMode(pin, OUTPUT);
  pinMode(readPin, INPUT);
  Serial.begin(9600);
}

void loop() {
  val = (255./1023.)*analogRead(A5);
  Serial.println(val);
  analogWrite(pin, val);
}
```

##   15, 16, 17. loop

빨간거 3번, 노란거 5번 깜빡이는 회로, 코드 짜기 

for 문에 대해서 배움.

```c++
int red = 8;
int yel = 9;
int redBlink = 3;
int yelBlink = 5;
int wait = 100;
void setup() {
  pinMode(8, OUTPUT);
  pinMode(9, OUTPUT);
}

void loop() {
  for (int i = 0; i < redBlink; i++) {
    digitalWrite(red, HIGH);
    delay(wait);
    digitalWrite(red, LOW);
    delay(wait);
  }

  for (int i = 0; i < yelBlink; i++) {
    digitalWrite(yel, HIGH);
    delay(wait);
    digitalWrite(yel, LOW);
    delay(wait);
  }
  delay(2000);

}
```

while문에 대해서도 배움.

1~10까지 출력하기.

- for 문은 반복의 횟수를 알때.
- while문은 반복의 횟수를 모를때.

```c++
int j;
int wait = 100;

void setup() {
  Serial.begin(9600);
}

void loop() {
  j = 1;
  while(j<=10) {
    Serial.println(j);
    delay(wait);
    j++;
  }
}
```

가변저항으로 회로를 구성하고, 
전압을 while 루프의 조건으로 한다. 

그래서 특정 전압 이상이면 led를 킨다. 

중요한건, **while루프 안에서 계속 조건을 업데이트 해야한다는 것.**

while루프가 이런거 만들때 for문에 비해서 많이 쓰이는 이유가, 
이런 고정되지 않은 조건에 대해서 반복을 하게 할 수 있기 때문에 그렇다.

```c++
int pin = 8;
int apin = A5;
int readValue;
int wait = 200;

void readfn() {
  readValue = analogRead(apin);
  Serial.println(readValue);
  delay(wait);
}
void setup() {
  pinMode(apin, INPUT);
  pinMode(pin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  readfn();

  while (readValue > 1000) {
    digitalWrite(pin, HIGH);
    readfn(); // 루프 안에서 조건이 되는 값(readValue)을 업데이트 해줘야한다.
  }
  digitalWrite(pin, LOW);
}

```

##   18, 19. Read from Serial

지금까지는 가변저항으로부터 데이터를 읽었다. 
이번 시간에는 Serial 모니터로 부터 데이터를 읽는 방법에 대해서 배운다. 
읽는 방법은 3단계로 나뉜다. 

- ask

```
Serial.println("질문");
```

- wait

```c++
while(Serial.available()==0){
    // 입력값이 없을때는 여기서 루프돌면서 대기
}
```

- read

```c++
Serial.parseInt();
Serial.parseFloat();
Serial.readString(); // parseString이 아님에 주의
// 값을 읽는다.
```

> Ask => Wait => Read 
> 시리얼 모니터로 입력값을 받는 방법 꼭 기억!

아래와 같이 숫자를 입력받아 출력하는 프로그램을 만들 수 있다.

```c++
int n;
String msg = "Please enter you number: ";
String msg2 = "your number is: ";
void setup() {
  Serial.begin(9600);
}

void loop() {
  Serial.println(msg);
  while (Serial.available() == 0) {
    // Do nothing.
  }
  n = Serial.parseInt();
  Serial.print(msg2);
  Serial.println(n);
}
```

그리고 간단한 회로를 구성해서 입력한 숫자만큼 깜빡이는 LED를 만들 수 있다. 

```c++
int pin = 8;
int n;
String msg = "How may blinks do you want?";
int wait = 100;

void setup() {
  pinMode(pin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  Serial.println(msg);
  while (Serial.available() == 0) {
    // Do nothing.
  }
  n = Serial.parseInt();
  for (int i = 0; i < n; i++) {
    digitalWrite(pin, HIGH);
    delay(wait);
    digitalWrite(pin, LOW);
    delay(wait);
  }
}
```

받은 문자열에 맞는 LED를 키는 코드, 회로

```c++
int pin1=8;
int pin2=9;
int pin3=10;
String input;

void setup() {
  pinMode(pin1, OUTPUT);
  pinMode(pin2, OUTPUT);
  pinMode(pin3, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(pin1, LOW);
  digitalWrite(pin2, LOW);
  digitalWrite(pin3, LOW);
  Serial.println("Which LED you want to ON?(red, yellow, blue)");
  while(Serial.available()==0){}
  input = Serial.readString();
  if(input=="red"){
    digitalWrite(pin1, HIGH);
  }
  if(input=="yellow"){
    digitalWrite(pin2, HIGH);
  }
  if(input="blue"){
    digitalWrite(pin3, HIGH);
  }
  delay(1000);
}
```

##   20, 21. RGB LED

두 개가 아닌, 4개의 다리가 있다. 
R, G, B, Ground 핀이다.

각 핀 연결을 하면 빨간색, 초록색, 파란색 빛을 낸다.
한 LED로 3가지 신호를 다르게 보여줄 수 있다는 장점이 있다. 

또한 analogWrite로 값을 잘 조절하면, 색을 섞을 수도 있다.

##   22, 23, 24. Buzzer

 버저는 두가지 종류가 있다. 

- active buzzer
  전원만 넣어주면 자동으로 소리가 출력된다.

- pessive buzzer

  내장된 회로가 없어서, 아두이노의 tone함수로 제어를 하던가 delay를 주기적으로 줘서 소리를 내게 해야한다.

버저에 보면, +부분이 있다. 
여기에 핀이 연결되도록하고, 반대부분은 ground에 연결하면 된다. 

analogWrite로 해서 5 정도로 설정하면 들을 만하다. 그냥 digitalWrite로 해서 255전압이 풀로 흐르면 시끄러움. 

가변저항으로 값을 읽어서, 일전 전압이 넘어가면 버저가 울리도록 하는 회로 만들기

```c++
int readPin = A5;
int buzzerPin = 9;
int value;

void setup() {
  pinMode(readPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  value = analogRead(readPin);
  Serial.println(value);
  while (value > 1000) {
    value = analogRead(readPin);
    analogWrite(buzzerPin, 5);
  }
  analogWrite(buzzerPin, 0);
  delay(50);
}
```

그리고 중간에 짧은 딜레이를 넣어서 톤을 조절할 수도 있다. 

```c++
analogWrite(buzzerPin, 5);
delay(10);
analogWrite(buzzerPin, 0);
delay(10);
```

패시브 버저는 바닥이 초록색이다. 
근데 왜 내 키트에는 없지..

암튼 패시브 버저는 우리가 수동으로 떨어줘야한다. 
위의 코드처럼 중간에 딜레이를 넣는데, digitalWrite로 해도 된다. 

딜레이 시간을 늘릴 수록 소리가 낮아진다.

```c++
delayMicroseconds(1000) == delay(1)
// 마이크로 딜레이도 있으니 이걸로 더 높은 소리를 만들 수 있다. 
```

숙제.
가변저항의 전압에 따라, 딜레이를 바꿔주는 회로 만들기

전압이 높아질수록 딜레이 시간이 커지면서 삑-- 삑-- 하고,
전압이 낮아지면, 딜레이 시간이 짧아지면서 삑삑삑삑 함.

```c++
int readPin = A5;
int buzzerPin = 9;
int value;

void setup() {
  pinMode(readPin, INPUT);
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  value = analogRead(readPin);
  Serial.println(value);
  int com = (1000. / 1023.) * value;
  analogWrite(buzzerPin, 5);
  delay(com);
  analogWrite(buzzerPin, 0);
  delay(com); 
}
```

##   25, 26. photoresistor

포토센서의 원리

LED를 먼저 이해해야한다. 
초반에 본 것 처럼 LED의 재료가 되는 실리콘은 순수하게는 전기가 흐르지 않는다. (저항이 겁나 크다.)

근데 여기에 광자가 쏴주면?
**valence밴드에 있는 전자가 conduction밴드로 뛰어오른다.**

그럼 전류를 흐르게 할 conduction밴드의 저항과 valence밴드의 구멍이 생기고, 
저항은 낮아지게 된다. 

광자를 더 쏴주면(빛을 더 주면)
저항은 더더 낮아진다.

그게 포토센서 photoregister의 원리이다.

> 어두우면 저항이 커지고, 밝으면 낮아짐. 
> 빛에 따라 저항이 변함.
>
> 원리를 잘 생각하자.

포토레지스터와 저항을 연결하고, 그 사이의 전압을 측정하는 회로를 구성한다. 

``` c++
int pin = A0;
int value;


void setup() {
  // put your setup code here, to run once:
  pinMode(pin, INPUT);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  value = analogRead(pin);
  Serial.println(value);
  delay(100);
}
```

그럼 빛의 양에 따라 변하는 전압을 볼 수 있다. 

전체 회로에 흐르면 전류는 일정하고, 저항에 따라 걸리는 전압이 달라진다. 

- 밝으면=> 저항이 작아지고=> 전압이 커진다.
- 어두우면=> 저항이 커지고 => 전압이 작아진다.

 

숙제. 
빛의 양에 따라서 다른 LED를 켜기.

위에 있는 전압을 읽는 회로에 따로, LED 회로를 구성해서 읽을 값에 따라 지정한 LED를 켜준다.

```c++
int readlight = A0;
int pin1 = 8;
int pin2 = 9;
int value;

void setup() {
  pinMode(readlight, INPUT);
  pinMode(pin1, OUTPUT);
  pinMode(pin2, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  value = analogRead(readlight);
  Serial.println(value);

  if(value>700){ // analogRead에서 읽은거니까, 0~1023의 값, 전압에 비례한다.
    digitalWrite(pin1, HIGH);
    digitalWrite(pin2, LOW);
  }
  if(value<700){
    digitalWrite(pin1, LOW);
    digitalWrite(pin2, HIGH);
  }
  delay(100);
}
```

또 하나의 숙제, 

어두워지면, 밝게 비춰주는 LED만들기.
계산이 필요하다. 

방이 밝을때 analogRead로 읽은 값이 760 정도, 어두울때 320 정도 이다.
그리고 밝을때 0, 어두울때 255의 값을 analogWrite로 주고 싶다. 

줄값을 y, 읽을값을 x라고 하면 아래와 같은 식을 만들 수 있다.

```
y = -(255/320)*x + a
```

그리고, (760, 0)을 대입해서 a를 구한다. 

```
y = -0.7*x + 538
```

그렇게 완성한 코드, 
어두우면 밝히고, 밝으면 어두워진다. 

```c++
int readlight = A0;
int pin1 = 9;
int pin2 = 10;
float raw;
float cal;


void setup() {
  // put your setup code here, to run once:
  pinMode(readlight, INPUT);
  pinMode(pin1, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  raw = analogRead(readlight);
  cal = -0.58 * raw + 441;
  Serial.println(cal);

  analogWrite(pin1, cal);
  delay(100);
}
```

##   27, 28, 29, 34. pushbutton

버튼

안누른 상태에서는 회로가 연결되지 않음.
누르면 연결됨.

0, 1이므로, 이런 상태를 읽는 것은 `analogRead`가 아닌 `digitalRead`를 쓰면 됨.

- **pull up register**

만약에 5v핀에 선-저항-버튼을 연결하고 여기에 read하는 핀을 연결.
버튼을 누르기 전이면, 전류가 흐르지 않기에 전압이 떨어지지 않는다. 
5v가 그대로 있다. 그래서 이 다리에 A핀을 연결해서 digitalRead로 읽을 수 있다. 5v가 흐르면 1, 보다 낮은 전압은 0

버튼을 누르면 전류가 흐르고, 전압이 0이 되면서 digitalRead로 0이 읽힌다.

> - 버튼을 누르면 0
> - 안누르면 1

- **pull down register**

5v핀-버튼, 아까와 반대로 읽는 핀을 전압핀과 다른 다리에 연결한다. 
그럼 버튼을 안눌러서 전류가 안흐르면 전압이 안읽혀서 0이 되고, 
버튼을 누르면, 1이 된다. 

pull up이랑, 반대임.
read하는 선을 어디에 연결하냐 차이임. 

숙제. pull up으로 연결해서 누르면 켜지는 LED 만들기.
값을 읽어서 만들어야 함.

읽는 방법은 5v-저항-버튼으로 가는 경로에..
A핀의 케이블을 넣어주면 된다. 

```c++
int pin = 8;
int readpin = A5;
int value;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(pin, OUTPUT);
  pinMode(readpin, INPUT);
}

void loop() {
  // put your main code here, to run repeatedly:
  value = digitalRead(readpin); // 누르면 5v가 흐르면서 1, 때면 0, analogRead로 했으면 1023, 0이 되었을 거임.
  Serial.println(value);
  delay(100);

  if(value==0){
    digitalWrite(pin, HIGH);
  }
  if(value==1){
    digitalWrite(pin, LOW);
  }
}
```

pull up버튼을 만들때, 5v에 연결해서 하는 방법말고, readpin으로 바로 연결할 수도 있다. 

```c++

void setup() {
  // put your setup code here, to run once:
  pinMode(8, INPUT);
  digitalWrite(8, HIGH);
  Serial.begin(9600);
}

void loop() {
  // put your main code here, to run repeatedly:
  int value = digitalRead(8);
  Serial.println(value);
}
```

> setup() 함수에 INPUT핀에 write를 하고 있다.  
> 그리고 버튼의 한 다리는 핀8번에 나머지는 그라운드에 연결하고, 
>
> 모니터를 보면 안누르면 1, 누르면 0이 된다. 
> read pin을 통해서 pull up을 만드는 간단한 방법이다. 

어려운 문제. 

그럼 스위치를 누를때마다 키고 끄고의 상태가 바뀌도록 하려면 어떻게 코딩을 해야할까..

- 현재 LED의 상태를 저장해야한다. 

그래서 버튼을 눌러서 0을 받게 되면, 현재 LED상태에 따라 켜고 끄고의 상태를 변경하면 된다. 
하지만, 이렇게만 생각하고 코딩하면 잘 안된다. 

왜냐하면, 버튼을 누를때 0이 한번만 오는 것이 아니라, 보통 111111100111 이런식으로 꾹- 눌러서, 0이 여러번 오는 경우가 많다.
그렇기에 이걸 고려안하면 깜빡깜빡 거리게 된다. 

그래서 LED상태 뿐만 아니라, 지난번에 받은 value의 값도 저장해야한다. 
그리고 현재 value가 0인데, 지난번에 받은 value가 1인 경우.

즉, 11111에서 000이 나오는 그 때를 캐치해야한다.

생각 좀 오래했다. 재밌네.

> 버튼상태와 LED상태를 (State)를 모두 저장하는 것.
> 이게 핵심!

```c++
int pin = 8;
int readpin = A5;
bool isLedOn = false;
int newState;
int oldState;

void setup() {
  Serial.begin(9600);
  pinMode(pin, OUTPUT);
  pinMode(readpin, INPUT);
}

void loop() {
  newState = digitalRead(readpin);

  if (oldState == 1 && newState == 0) {
    if (isLedOn) {
      isLedOn = false;
      digitalWrite(pin, LOW);
    }
    else {
      isLedOn = true;
      digitalWrite(pin, HIGH);
    }
  }
  oldState = newState;
}
```

다른 문제, 

2개의 버튼이 있다. 
한 버튼을 꾹 누르고 있거나, 계속 누르면 LED가 점점 밝아지고, 
다른 버튼은 반대로 어두워 진다.

그리고, 0, 255에 도달하면, 경고 LED를 켜준다.

> 몰랐던게 난 A1, A2 이것만 Read할 수 있는 줄 알았는데, 
> A계열은 analogRead만 가능하고, 
> digitalRead는 그냥 핀도 가능하더라. 
>
> A계열은 analogRead시에만 쓴다!
>
> OK?

```c++
int upBtn = 11;
int downBtn = 10;
int pin = 9;
int upValue;
int downValue;
int light = 0;

void setup() {
  // put your setup code here, to run once:
  pinMode(pin, OUTPUT);
  pinMode(upBtn, INPUT);
  pinMode(downBtn, INPUT);
  pinMode(12, OUTPUT);
  Serial.begin(9600);

}

void loop() {
  // put your main code here, to run repeatedly:
  analogWrite(pin, light);
  upValue = digitalRead(upBtn);
  downValue = digitalRead(downBtn);
  if (upValue == 0) {
    if (light < 255) {
      light++;
    } else {
      digitalWrite(12, HIGH);
      delay(10);
      digitalWrite(12, LOW);
    }
  }
  if (downValue == 0) {
    if (light > 0) {
      light--;
    } else {
      digitalWrite(12, HIGH);
      delay(10);
      digitalWrite(12, LOW);
    }
  }
  delay(10);
}
```

##   30, 31. Servos

일반 모터와 달리, 뱅뱅 도는게 아니라, 일정 각도만큼 움직이게 할 수 있는 것이 servo이다. 

5v, GND에 연결을 위한 선이 필요하고, 명령을 받기위한 선이 하나더 필요하다. 
서보에 보면, 갈색, 빨간색, 주황색 선이 있다. 

- 갈색: Ground
- 빨간색: 5V
- 주황색: 신호 pin

> 서보를 쓰기위해서는
>
> ```c++
> #include <Servo.h>
> Servo servo;
> void setup(){
>     servo.attach(9);
> }
> void loop(){
>     servo.write(90); 
> }
> // 외부 라이브러리를 받고, 
> // 서보를 만들어주고, 
> // 그걸 setup에서 명령을 줄 핀에 attach
> // 그리고 write함수로 0~180의 각도를 주면 된다.
> ```

포토센서와 같이 빛에 따라 각도가 변하는 회로를 만들어보았다. 

```c++
#include <Servo.h>
int readPin = A5;
int readValue;
int calcValue;
int servoPin = 9;
int servoPos;
Servo servo;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(A5, INPUT);
  pinMode(servoPin, OUTPUT);
  servo.attach(servoPin);
}

void loop() {
  // put your main code here, to run repeatedly:
  readValue = analogRead(A5);
  calcValue = 0.35*readValue-94.7;
  Serial.println(calcValue);
  servo.write(calcValue);
}
```

##   32, 33. joystick

조이스틱은 가변저항 2개와 스위치로 구성되어었다.

총 5개의 핀이 있는데 각각

- GND
- 5V
- VRX: x 방향 가변저항이 연결
- VRY: y 방향
- SW: 스위치 클릭

연결은 암-수 와이어를 써야한다. 
조이스틱의 움직임에 따른, VRX(0~1023), VRY(0~1023), SW(0, 1)의 값을 받아야하기에 
전원연결을 위한 5v, ground 말고도 3개의 인풋을 위한 선이 필요하다. 

x, y 두개는 아날로그 read=> A핀
sw 하나는 디지털 read 핀에 연결한다. 

```c++
int xpin = A0;
int ypin = A1;
int spin = 8;
int xval;
int yval;
int sval;
int wait = 200;
void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(xpin, INPUT);
  pinMode(ypin, INPUT);
  pinMode(spin, INPUT);
  digitalWrite(spin, HIGH);
}

void loop() {
  // put your main code here, to run repeatedly:
  xval = analogRead(xpin);
  yval = analogRead(ypin);
  sval = digitalRead(spin);
  delay(wait);
  Serial.print("x:");
  Serial.print(xval);
  Serial.print(" y:");
  Serial.print(yval);
  Serial.print(" s:");
  Serial.println(sval);
}
```

위와 같은 코드로 조이스틱의 조작에 따른 값을 출력해서 볼 수 있다. 

보통은 이걸로 서보모터를 조작한다. 
근데, 내 서보모터는 두개 다 고장이네.. 새로 사야할듯.

암튼 조이스틱은 하나의 스틱으로 3가지 신호를 줄 수 있기에 유용하다. 


## 35. stepper motor

서보 모터처럼 각도를 제어할 수 있는 모터이다. [두 모터의 차이](https://techgoogleblogger.blogspot.com/2019/01/difference-between-servo-motor-and.html)

이름처럼 한 스텝이 몇 도를 돌아가는지를 제어할 수 있는 모터이다. 

스태퍼모터를 쓰기 위해서는 전용드라이버를 연결해야한다. 

전원을 위해 2개의 선
조작을 위해 4개의 선이 필요하다. 

```c++
#include <Stepper.h>

const int stepsPerRevolution = 200; // 360를 돌기위해 몇 스텝이 필요한지
Stepper myStepper(stepsPerRevolution, 8, 9, 10, 11); // 클래스 생성자에는 스텝과 핀 4개를 전달.
void setup() {
  myStepper.setSpeed(60); // rpm, 분당 회전수가 들어간다. 60이면. 1분에 200*60 = 12000 스텝이 진행된다. 
}
void loop() {
  myStepper.step(stepsPerRevolution); // 출력할 스텝수 200.
  delay(100);

  myStepper.step(-stepsPerRevolution);
  delay(100);
}
```

## 36. tilt switch

기울임에 따라 열리고 닫히는 스위치. 볼 스위치라고도 한다.
내부에 작은 쇠구슬이 있어서, 세우면 두 다리가 연결되고, 눕히면 분리되는 단순한 구조이다. 

회로를 연결해서 읽을때는 setup에 read핀에 digitalRead HIGH를 해줘야 한다. pushbutton참고.

기울기에 따라 서로다른 pin이 켜지도록 하기

```c++
int tiltPin = 8;
int yelPin =9;
int redPin =10;
int value;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(9600);
  pinMode(tiltPin, INPUT);
  pinMode(yelPin, OUTPUT);
  pinMode(redPin, OUTPUT);
  digitalWrite(tiltPin, HIGH);
}

void loop() {
  // put your main code here, to run repeatedly:
  value = digitalRead(tiltPin);
  Serial.println(value);
  if(value==0){
    digitalWrite(yelPin, HIGH);
    digitalWrite(redPin, LOW);
  }
  if(value==1){
    
    digitalWrite(yelPin, LOW);
    digitalWrite(redPin, HIGH);
  }
  delay(100);
}
```

> 기울이면 1이, 세우면 0이 출력된다. 

## 37, 38, 39, 40. DC 모터

내 키트에는 없다. 

16개의 핀이 있는 드라이버랑 같이 모터가 있다. 
이 드라이버 칩의 각 핀에는 역할이 있다. 그걸 이해해야함. L293D.

그리고 맞는 핀에 모터를 연결해야한다. 

속도, 방향, 그라운드, 전원을 받는 핀을 연결해야한다.


## 41. Hex, 16진수

스위치가 1개면, 2개의 값을
2개면, 4개의 값을
3개면 8개의 값을.. 나타낼 수 있다.

> 강의를 더 이상 듣는 것은 의미가 없는 것 같다. 
>
> 이제 얼추 알았으니, 모르는 건 그때그때 찾아서 하면 되는 수준이다. 
> 그래도 강의 덕에 많이 배웠다.