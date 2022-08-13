문서정보 : 2022.08.13. 작성, 작성자 [@SAgiKPJH](https://github.com/SAgiKPJH)

# 목차
0. 구현 과정
1. Arduino IDE
2. Arduino IDE - ESP8266 Library
3. ESP8266 장비
4. ESP8266 기본 프로그래밍 - 내부 LED 깜빡이기
5. HTTP
6. ESP8266 HTTP
7. Json
9. ESP8266 - Get Weather Station

<br>

# **0. 구현 과정**

```mermaid
flowchart LR
    EL["ESP8266 Library"] -.-> AI["Arduino IDE"] -."HTTP Programming".-> E["ESP8266"] --"HTTP : Location Infomation"--> OWM[("OpenWeatherMap.org")]
    OWM --"HTTP : Weather JSon Infomation"--> E --"Json Parse"--> R["Serial Output"] --> Own["Result"]
```


# **1. Arduino IDE**

### ◆ Arduino

<img src="https://camo.githubusercontent.com/a9e049ade1147226016feb1ab0024b7e09cf5e6ce7921aa9e7326942f98c71dd/687474703a2f2f636f6e74656e742e61726475696e6f2e63632f6272616e642f61726475696e6f2d636f6c6f722e737667" width="30%"> 아두이노 로고
![image](https://user-images.githubusercontent.com/66783849/184478258-8d3cd20a-dd56-40bc-8d73-4cfb2d168a31.png) 아두이노 보드  

- 오픈소스를 기반으로 한 단일 보드 마이크로컨트롤러로 완성된 보드 관련 개발 도구, 환경을 말한다.
- 이탈리아에서 하드웨어를 손쉽게 제어할 수 있게 하려고 고안된 아두이노는 처음에 AVR을 기반으로 만들어졌다.
- 아두이노는 임베디드 시스템 중의 하나로 쉽게 개발할 수 있는 환경을 이용하여, 장치를 제어할 수 있다.
- 사이트 : https://www.arduino.cc/

### ◆ Arduino IDE

<img src="https://user-images.githubusercontent.com/66783849/184478387-45e0c140-d61b-4912-9b0c-6f3818d9bfda.png" width="30%"> 시작화면
<img src="https://user-images.githubusercontent.com/66783849/184478390-e28922f6-aae9-4ef8-a976-d6988be18bb0.png" width="30%"> Arduino IDE 화면

- **아두이노 통합개발환경(Arduino IDE)** 은 프로세싱을 기반으로 개발된 편집기, 컴파일러, 업로더 등이 합쳐진 소프트웨어 환경이다. 
- '아두이노 소프트웨어'라고도 불린다.
- 시리얼 통신할 수 있는 가상 시리얼모니터와, USB를 UART통신으로 바꾸는 방법 및 통신 기능, 개발에 필요한 각종 라이브러리 관리 기능을 갖고 있다.
- 다음은 아두이노 IDE의 기능이다.
  - 편집기 : UTF-8을 기반의 편집기
  - 컴파일러 : ATmega의 경우 AVR-GCC를 이용하여 컴파일 한다.
  - 업로드 : USB-UART 변환 후, MCU 부트로더가 작동하여 기계어 코드가 업로드 된다.
  - 라이브러리 관리 : 등록 된 라이브러리 목록 및 예제를 지원한다. 라이브러리 검색 및 등록이 가능하다.
  - 시리어 모니터 : 시리얼 통신할 수 있는 가상 시리얼 모니터를 제공한다.
  - 아두이노 스케치 문법 및 함수등 자료 레퍼런스
  - 프로그래밍 언어 : C++
  - 깃허브 : https://github.com/arduino/Arduino

<br>

# **2. Arduino IDE - ESP8266 Library**

### ◆ 아두이노 IDE에 ESP8266 보드 설치하기 (Installing ESP8266 Board in Arduino IDE)
- ESP8266을 프로그래밍할 수 있는 Arduino IDE용 라이브러리가 존재한다.
- Windows, Mac OS X, Linux를 사용하는지 여부에 관계없이 Arduino IDE에 ESP8266 보드를 설치할 수 있다.

1. Arduino IDE에서 파일 > 환경설정 으로 이동한다.
  <img src="https://user-images.githubusercontent.com/66783849/184493883-02475d77-dbcf-49ff-b441-84218dc1d816.png" width="30%">
  
2. "추가 보드 관리자 URL"에 URL를 입력한다.
  이미 존재하는 경우 쉼표(,)로 구분하여 기제한다.
  
```bash
https://dl.espressif.com/dl/package_esp32_index.json, 
http://arduino.esp8266.com/stable/package_esp8266com_index.json
```

  <img src="https://user-images.githubusercontent.com/66783849/184494050-7e6b6fd2-1dc0-4c0a-a546-0e87785370b2.png" width="70%">
  
3. 도구 > 보드 > 보드 관리자 로 이동한다.
<img src="https://user-images.githubusercontent.com/66783849/184494162-a9452530-1cc2-4ed0-bee1-5e8c18f5ea20.png" width="70%">

4. ESP8266을 검색하고 **"esp8266 by ESP8266 Community"** 선택 후 **Install** 버튼을 누른다.
<img src="https://user-images.githubusercontent.com/66783849/184494291-e52eacc7-18e3-4e97-b96c-66fdb5c8554c.png" width="70%">

<img src="https://user-images.githubusercontent.com/66783849/184494393-cb320c47-26d6-41f9-a6de-f6b7f4e5c122.png" width="70%"> 다운로드 진행중

5. 설치됨을 확인한다.
<img src="https://user-images.githubusercontent.com/66783849/184494454-fb12b96e-6294-4a95-9881-608811eba789.png" width="70%">

<br>

# **3. ESP8266 장비**

- ESP8266 활용을 위한 준비물을 준비한다.
  - ESP8266 TWM-02 (여기서는 이 보드를 활용하여 실습했다. ESP8266관련 보드면 충분히 문제 없다.)
  - UART to USB Board
  - [LD1117AV33](https://www.mouser.kr/ProductDetail/STMicroelectronics/LD1117AV33?qs=hUhhBvpTJN9Rfx8G8TXH1A%3D%3D) (2.75~8V Input, 3.3V 1A Output)  

![image](https://user-images.githubusercontent.com/66783849/184495010-38fb580f-5661-4855-912c-b179a81abb01.png)

![image](https://user-images.githubusercontent.com/66783849/184495322-05c70ac5-f205-4855-80af-f04af5be316a.png) 연결도

ESP8266 | UART to USB Board
-- | --
RX | TX
TX | RX
CH_PD | 3.3V
GPIO 0 | GND
VCC | 3.3V
GND | GND

# **4. ESP8266 기본 프로그래밍 - 내부 LED 깜빡이기**

### 1. Tools > Baord > ESP8266 > Generic ESP8266 Module  
  ESP8266 TWM02보드로, 직접 연결하기 때문에 몇가지 속성을 변경해야 한다.
<img src="https://user-images.githubusercontent.com/66783849/184495598-5d0e8f97-24a7-4431-9683-373b72a087c3.png" width="70%">

<img src="https://user-images.githubusercontent.com/66783849/184495695-6e124bb0-c963-4ec6-9870-a6efdfc9ad3c.png"> 연결 확인   

<br>
 

### 2. ESP8266 Board 속성 설정
 - Tool 항목에 들어가면 자세한 속성을 설정할 수 있다.
 - ESP8266 Board의 FLASH 메모리의 용량을 확인한다.
 - ESP8266 TWM02 뒷면에 FLASH 메모리가 존재하는데, 25q32fvstg이라고 한다.
 - 25q32fvstg의 FLASH 메모리 용량은 25Q32FVSIG (32M-bit)로, 이를 8로 나누어 4M-byte로 설정한다.
 - Serial Port를 설정해준다.  

 <img src="https://user-images.githubusercontent.com/66783849/184495744-c1284f5e-31aa-420a-823c-6d1675cc5fb8.png" width="50%">
 
 <img src="https://user-images.githubusercontent.com/66783849/184496368-c08f5736-bea6-4f6e-ab25-e7fb5ef6207e.png" width="50%">
 
 ![image](https://user-images.githubusercontent.com/66783849/184496582-e693a98e-e50b-4a5e-8969-8a7664e4dbdf.png)

(FS : 파일 시스템 영역, OTA : Sketch 프로그램의 영역, OTA는 최대한 적게 설정한다.)  
  
<br>    
<br>
<br>


### 3. ESP8266 프로그래밍 모드 설정

- ESP8266에는 프로그래밍 모드와 실행모드 등 여러 모드가 존재한다.  
- 프로그래밍 모드로 설정을 해야 프로그래밍이 가능하다.  
- 현재 ESP8266이 어떤 모드에 있는지 확인하기 위해서는 다음과 같은 과정을 거친다.  
    1. [툴 > 시리얼 모니터] 시리얼 통신을 통한 결과확인을 위해 가상 시리얼 모니터를 연다.
    2. Board Rate는 74880bps로 설정한다.
    3. ESP8266의 RST핀에 GND를 연결한다.
    4. 시리얼 모니터에 결과를 확인한다.  
       이때, boot mode : (1.7)에서 괄호 내 첫 번째 숫자인 1을 보고 프로그래밍 모드임을 알 수 있다.  
       3일 경우에는 일반(실행)모드이다.  

<img src="https://user-images.githubusercontent.com/66783849/184497244-4b3bfa2d-7542-4b2e-921b-b0e1ab665905.png" width="70%"> 프로그래밍 모드

- 모드를 바꾸는 방법은 다음과 같다.  
  Programming Mode : GPIO0 핀이 0(GND)에 연결된 후, RST를 GND에 연결 후 해제
  Nomal Mode : GPIO0 핀이 1(VCC)에 연결된 후, RST를 GND에 연결 후 해제



<br>


### 4. ESP8266 프로그래밍

```cpp
void setup() {
  // put your setup code here, to run once:
  pinMode(1, OUTPUT); //TX Pin On, Off
}

void loop() {
  // put your main code here, to run repeatedly:
  digitalWrite(1, HIGH);
  delay(1000);
  digitalWrite(1, LOW);
  delay(1000);
}
```

![image](https://user-images.githubusercontent.com/66783849/184496699-ac850592-8666-4a6d-af68-3e277bedeef1.png) 업로드 버튼

<img src="https://user-images.githubusercontent.com/66783849/184496761-b9087533-0869-42a9-922b-3f289fea1e6b.png" width="50%"> 컴파일중

<img src="https://user-images.githubusercontent.com/66783849/184496776-fe511045-2d8e-4329-b89a-80f526af308e.png" width="50%"> 업로딩중



### 5. ESP8266 실행 모드 설정
- Nomal 모드를 바꾼다.
  Nomal Mode : GPIO0 핀이 1(VCC)에 연결된 후, RST를 GND에 연결 후 해제
![image](https://user-images.githubusercontent.com/66783849/184498628-9a1396ce-3257-4ed8-b7e6-d595a62bbc31.png) Nomal 모드


### 6. 결과 확인
- 1초마다 LED가 깜빡거리는 것을 확인한다.
- TX핀에 On/Off를 시켜 LED가 깜빡거리는 것을 확인한다.
![image](https://user-images.githubusercontent.com/66783849/184498734-98246238-29e2-4853-87b7-68c9c87c2375.png)



<br>

# 참조
- ESP8266 라이브러리 설치(설정)
  - https://randomnerdtutorials.com/how-to-install-esp8266-board-arduino-ide/
- ESP8266 HTTP, WeatherStation Guide
  - https://randomnerdtutorials.com/esp8266-nodemcu-http-get-open-weather-map-thingspeak-arduino/
- ESP8266 Weather Station 영상
  - https://mcuoneclipse.com/2017/09/09/wifi-oled-mini-weather-station-with-esp8266/
  - https://www.youtube.com/watch?v=McwarmDpQXo
  - https://www.youtube.com/watch?v=7uV1J4KVtQs
- 아두이노 정보
  - https://ko.wikipedia.org/wiki/아두이노
  - https://ko.wikipedia.org/wiki/아두이노_IDE
- Arduino IDE ESP8266 FlashSize : FS, OTA
  - https://www.sysnet.pe.kr/2/0/12638
