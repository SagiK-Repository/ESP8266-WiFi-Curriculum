문서정보 : 2022.08.13. 작성, 작성자 [@SAgiKPJH](https://github.com/SAgiKPJH)

# 목차
0. 구현 과정
1. Arduino IDE
2. Arduino IDE - ESP8266 Library
3. ESP8266 장비
4. ESP8266 기본 프로그래밍 - 내부 LED 깜빡이기
5. HTTP
6. Json
7. ESP8266 HTTP
8. ESP8266 - Get Weather Station

<br><br><br>

# **0. 구현 과정**

```mermaid
flowchart LR
    EL["ESP8266 Library"] -.-> AI["Arduino IDE"] -."HTTP Programming".-> E["ESP8266"] --"HTTP : Location Infomation"--> OWM[("OpenWeatherMap.org")]
    OWM --"HTTP : Weather JSon Infomation"--> E --"Json Parse"--> R["Serial Output"] --> Own["Result"]
```
<br><br><br>

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


<br><br><br>


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



<br><br><br>



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



<br><br><br>



# **4. ESP8266 기본 프로그래밍 - 내부 LED 깜빡이기**

### 1. Tools > Baord > ESP8266 > Generic ESP8266 Module  
- ESP8266 TWM02보드로, 직접 연결하기 때문에 몇가지 속성을 변경해야 한다.  
   <img src="https://user-images.githubusercontent.com/66783849/184495598-5d0e8f97-24a7-4431-9683-373b72a087c3.png" width="70%">

   <img src="https://user-images.githubusercontent.com/66783849/184495695-6e124bb0-c963-4ec6-9870-a6efdfc9ad3c.png"> 연결 확인   


<br>
 

### 2. ESP8266 Board 속성 설정
 - Tool 항목에 들어가면 자세한 속성을 설정할 수 있다.
 - ESP8266 Board의 FLASH 메모리의 용량을 확인한다.
 - ESP8266 TWM02 뒷면에 FLASH 메모리가 존재하는데, 25q32fvstg이라고 한다.
 - 25q32fvstg의 FLASH 메모리 용량은 25Q32FVSIG (32M-bit)로, 이를 8로 나누어 4M-byte로 설정한다.
 - Serial Port를 설정해준다.  

   <img src="https://user-images.githubusercontent.com/66783849/184495744-c1284f5e-31aa-420a-823c-6d1675cc5fb8.png" width="30%">
   <img src="https://user-images.githubusercontent.com/66783849/184496368-c08f5736-bea6-4f6e-ab25-e7fb5ef6207e.png" width="49%">
 
   ![image](https://user-images.githubusercontent.com/66783849/184496582-e693a98e-e50b-4a5e-8969-8a7664e4dbdf.png)

   (FS : 파일 시스템 영역, OTA : Sketch 프로그램의 영역, OTA는 최대한 적게 설정한다.)  
  
<br>


### 3. ESP8266 프로그래밍 모드 설정

- ESP8266에는 프로그래밍 모드와 실행모드 등 여러 모드가 존재한다.  
- 프로그래밍 모드로 설정을 해야 프로그래밍이 가능하다.  
- 현재 ESP8266이 어떤 모드에 있는지 확인하기 위해서는 다음과 같은 과정을 거친다.  
    1. [툴 > 시리얼 모니터] 시리얼 통신을 통한 결과확인을 위해 가상 시리얼 모니터를 연다.
    2. Board Rate는 74880bps로 설정한다.
    3. ESP8266의 RST핀에 GND를 연결한다.
    4. 시리얼 모니터에 결과를 확인한다.  
       - 이때, boot mode : (1.7)에서 괄호 내 첫 번째 숫자인 1을 보고 프로그래밍 모드임을 알 수 있다.  
       - 3일 경우에는 일반(실행)모드이다.  
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

<img src="https://user-images.githubusercontent.com/66783849/184496761-b9087533-0869-42a9-922b-3f289fea1e6b.png" width="40%"> 컴파일중
<img src="https://user-images.githubusercontent.com/66783849/184496776-fe511045-2d8e-4329-b89a-80f526af308e.png" width="40%"> 업로딩중

<br>

### 5. ESP8266 실행 모드 설정
- Nomal 모드를 바꾼다.  
  Nomal Mode : GPIO0 핀이 1(VCC)에 연결된 후, RST를 GND에 연결 후 해제
![image](https://user-images.githubusercontent.com/66783849/184498628-9a1396ce-3257-4ed8-b7e6-d595a62bbc31.png) Nomal 모드

<br>

### 6. 결과 확인
- 1초마다 LED가 깜빡거리는 것을 확인한다.
- TX핀에 On/Off를 시켜 LED가 깜빡거리는 것을 확인한다.
![image](https://user-images.githubusercontent.com/66783849/184498734-98246238-29e2-4853-87b7-68c9c87c2375.png)




<br><br><br>




# **5. HTTP**
- HTTP(HyperText Transfer Protocol)는 클라이언트와 서버간의 정보를 주고받을 수 있는 프로토콜이다.
- 프로토콜이란 `상호 간에 정의한 규칙`을 의미한다.
- 하이퍼텍스트라는 용어는 1965년 제너두 프로젝트에서 테드 넬슨이 만들었으며, 팀 버너스 리와 그의 팀은 CERN에서 HTML뿐 아니라 웹 브라우저 및 텍스트 기반 웹 브라우저 관련 기술과 더불어 오리지널 HTTP을 발명하였다.
- HTTP는 주로 HTML 문서를 주고 받는데 쓰인다.
- HTTP를 통해 전달되는 자료는 http:로 시작하는 URL(인터넷 주소)로 조회할 수 있다.

<br>

### 동작 방식
- 클라이언트와 서버 사이에 이루어지는 요청/응답(request/response) 프로토콜이다.
- 클라이언트와 서버 사이의 소통은 평문(ASCII) 메시지로 이루어진다.
- 클라이언트는 서버로 요청메시지를 전달하며 서버는 응답메시지를 보낸다.
- 요청 내용과 헤더 필드는 <CR><LF>로 끝나야 한다. (2바이트 CR LF ('\r\n'))
- 주로 TCP/IP 통신을 사용하고, HTTP/3 부터는 UDP를 사용하며, 80포트를 사용한다.

<br>

## 동작 예제
### 클라이언트 요청
```http
GET /restapi/v1.0 HTTP/1.1
Accept: application/json
Authorization: Bearer UExBMDFUMDRQV1MwMnzpdvtYYNWMSJ7CL8h0zM6q6a9ntw
```
### 서버 응답
```html
HTTP/1.1 200 OK
Date: Mon, 23 May 2005 22:38:34 GMT
Content-Type: text/html; charset=UTF-8
Content-Encoding: UTF-8
Content-Length: 138
Last-Modified: Wed, 08 Jan 2003 23:11:55 GMT
Server: Apache/1.3.3.7 (Unix) (Red-Hat/Linux)
ETag: "3f80f-1b6-3e1cb03b"
Accept-Ranges: bytes
Connection: close

<html>
<head>
  <title>An Example Page</title>
</head>
<body>
  Hello World, this is a very simple HTML document.
</body>
</html>
```

코드 | 내용
:--: | :-- 
GET | 존재하는 자원에 대한 요청
POST | 새로운 자원을 생성
PUT | 존재하는 자원에 대한 변경
DELETE | 존재하는 자원에 대한 삭제

<br>

### 응답코드
- 클라이언트가 서버에 접속하여 어떠한 요청을 하면, 서버는 세 자리 수로 된 응답 코드와 함께 응답한다.
- 대표적인 HTTP의 응답 코드는 다음과 같다.

코드 | 메시지 | 설명
:---: | :--: | :--
**1xx** | **Informational<br>(정보)** | **정보   교환.**
100 | Continue | 클라이언트로부터 일부 요청을   받았으니 나머지 요청 정보를 계속 보내주길 바람. <br>(HTTP 1.1에서 처음 등장)
101 | Switching   Protocols | 서버는   클라이언트의 요청대로 Upgrade 헤더를 따라 다른 프로토콜로 바꿀 것임.<br> (HTTP 1.1에서 처음 등장)
**2XX** | **Success<br>(성공)** | **데이터   전송이 성공적으로 이루어졌거나, 이해되었거나, 수락되었음.**
200 | OK | 오류   없이 전송 성공.
**3XX** | **Redirection<br>(방향   바꿈)** | **자료의   위치가 바뀌었음.**
**4XX** | **Client   Error<br>(클라이언트 오류)** | **클라이언트   측의 오류. 주소를 잘못 입력하였거나 요청이 잘못 되었음.**
**5XX** | **Server   Error<br>(서버 오류)** | **서버   측의 오류로 올바른 요청을 처리할 수 없음.**




<br><br><br>




# **6. Json**
-  JavaScript Object Notation(JSON)은 데이터 교환 포맷으로써, 인터넷에서 자료를 주고 받을 때 그 자료를 표현하는 방법으로 알려진 개방형 표준 포맷이다.
- 인간이 읽을 수 있는 문서이고, 코딩이 적게 필요하며, 처리속도가 빠른 경량언어이다.
- JSON은 상대적으로 쉽게 읽고 작성할 수 있고, 소프트웨어에서 파싱 및 생성하기도 쉽다.
- 자료의 종류에 큰 제한은 없으며, 프로그램의 변수값을 표현하는 데 적합하다.
- 종종 구조화된 데이터를 직렬화해 이를 네트워크에서 교환할 때(보통 서버와 웹 애플리케이션 간) 사용된다.
- 자바스크립트의 객체표기법으로부터 파생되어 독립된 언어로, 다른 언어와 호환성이 쉽다.
- JSON은 4가지 구조를 갖는다.
   1. JSON 데이터는 이름과 값의 쌍으로 이루어집니다.
   2. JSON 데이터는 쉼표(,)로 나열됩니다.
   3. 객체(object)는 중괄호({})로 둘러쌓아 표현합니다.
   4. 배열(array)은 대괄호([])로 둘러쌓아 표현합니다.

<br>

### JSON 구성
- JSON에서는 데이터의 값으로 사용할 수 있는 다양한 타입을 제공하고 있다.
- JSON에서 제공하는 기본 타입은 다음과 같습니다.
1. 숫자(number)
2. 문자열(string)
3. 불리언(boolean)
4. 객체(object)
5. 배열(array)
6. null

<br>

#### 1. 숫자(number)
```json
{
    "age": 1
}
{
    "weight": 2.14
}
{
    "size": 5.8426e+2
}
```
#### 2. 문자열
```json
{
    "name": "식빵"
}
```
이스케이프 시퀀스 | 설명
-- | --
\b | 백스페이스
\f | 폼 피드(form feed)
\n | 개행
\r | 캐리지 리턴(carriage return)
\t | 탭(tab)
\\" | 큰따옴표
\\/ | 슬래시
\\\ | 역슬래시
\uHHHH | 16진수 네 자리로 표현된 유니코드 문자
```json
{
    "comment": "안녕하세요. \"식빵\" 입니다."
}
```
#### 3. 불리언(boolean)
```json
{
    "name": "식빵",
    "lunch": true
}
```
#### 4. 객체(object)
- 객체 안의 객체도 가능하다.
```json
{
    "name": "식빵",
    "family": "웰시코기",
    "age": 1,
    "weight": 2.14
}
{
    "dog": {
        "name": "식빵",
        "family": "웰시코기",
        "age": 1,
        "weight": 2.14,
        "owner": {
            "ownerName": "홍길동",
            "phone": "01012345678"
        }
    }
}
```

#### 5. 배열(array)
- 객체를 배열의 요소로 가질 수 있다.
```json
{
    "dog": [
        "웰시코기",
        "포메라니안",
        "푸들"
    ]
}
{
    "dog": [
        "웰시코기",
        "포메라니안",
        "푸들",
        {
            "ownerName": "홍길동",
            "phone": "01012345678"
        }
    ]
}
```
#### 6. null
- 항상 소문자로 표기한다.
- 아무겂도 없는 빈 값이다.
```json
{
    "id": 1,
    "name": null
}
```

#### 기타
- JSON을 활용해 정보에 대한 검증과 더불어 값의 크기 또는 범위를 감지할 수 있도록 구성할 수 있다.
- 이는 JSON의 고급문법으로, 논리계산도 다룰 수 있다.  
- 예) 해당 데이터가 숫자이면서 3의 배수이거나, 아니면 숫자이면서 4의 배수인지를 검사하는 예제
```json
{
    "oneOf": [
        { "type": "number", "multipleOf": 3 },
        { "type": "number", "multipleOf": 4 }
    ]
}
```
- 예) 다음 예제는 해당 데이터가 문자가 아닌지를 검사하는 예제
```json
{
    "not": {
        "type": "string"
    }
}
```
- 예) 배열이 중복되는지 검사하는 예제
```json
{
    "type": "array",
    "uniqueItems": true
}
```


<br><br><br>



# **7. ESP8266 HTTP**



<br><br><br>



# **8. ESP8266 - Get Weather Station**




<br><br><br>




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
- HTTP 정보
  - https://ko.wikipedia.org/wiki/HTTP
  - https://joshua1988.github.io/web-development/http-part1/
- json 정보
  - https://ko.wikipedia.org/wiki/JSON
  - https://www.oracle.com/kr/database/what-is-json/
