문서정보 : 2022.08.13 작성 (원본 2022.01.03. 작성), 작성자 [@SAgiKPJH](https://github.com/SAgiKPJH)

# 목차
1. UART 통신
2. ESP8266 직접 통신
3. AVR
4. AVR 활용한 Wi-Fi

<br>

## 배경지식

[ESP8266에 대한 정보](https://github.com/SagiK-Repository/ESP8266-WiFi-Curriculum/tree/main/Chapter%201.%20%20About%20ESP8266)를 통해 ESP8266에 대한 정보를 얻자.

 <br>

## 1. UART 통신

### ◆ UART

<img src="https://user-images.githubusercontent.com/66783849/184471962-418d88fe-956b-4bb0-aa7e-881fe555b9ed.png"> UART 회로
<img src="https://user-images.githubusercontent.com/66783849/184471973-c54010dc-49b9-4ba5-88ea-e131f45b7f7f.png"> 연결방법
<img src="https://user-images.githubusercontent.com/66783849/184471985-7778ecee-75fb-46d1-8d24-21deb37793e9.png"> UART 통신

- UART(universal asynchronous receiver/transmitter : 범용 비동기화 송수신기)는 시리얼 통신을 담당하는 회로를 말한다.
- UART는 일반적으로 EIA RS-232, RS-422, RS-485와 같은 통신 표준과 함께 사용한다.
- 병렬 데이터를 직렬화하여 통신하는 개별 집적 회로이다.
- 시리얼 통신 연결은 TX-RX를 엇갈리게 해야한다.
- 통신 데이터는 메모리 또는 레지스터에 들어 있어 이것을 차례대로 읽어 직렬화 하여 통신한다.
- 최대 8비트가 기본 단위이다.
- <img src="https://user-images.githubusercontent.com/66783849/184471908-34c63a6f-b9aa-4057-8869-c8bf1b1a08af.png" width="80%"> 데이터 송 수신 형태
  - 시작 비트 : 통신의 시작을 의미. 한 비트 시간 길이 만큼 유지.
  - 데이터 비트 : 5~8비트의 데이터를 전송. 몇 비트를 사용할 것인지는 해당 레지스터 설정에 따라 결정.
  - 패리티 비트 : 오류 검증을 하기 위한 패리티 값을 생성하여 송신하고 수신쪽에 오류 판단한다.
  - 끝 비트 : 통신 종료를 알린다. 세가지의 정해진 비트 만큼 유지해야 한다. 1, 1.5, 2비트로 해당 레지스터 설정에 따라 결정된다.


<br>

## 2. ESP8266 직접 통신

### ◆ ESP8266 직접통신

<img src="https://user-images.githubusercontent.com/66783849/184472035-9e1a3330-6b0d-4289-9741-f66a8d8aa738.png"> CP2102 
<img src="https://user-images.githubusercontent.com/66783849/184472050-75862989-c2c3-460a-bb62-791c4c01a6c8.png"> CP2102 및 ESP8266 연결 모식도

- 컴퓨터와 ESP8266간의 UART통신을 직접 할 수 있다.
- RS-232를 활용해 UART통신을 한다.
- 이때 사용한 보드는 CP2102이다.
- 이 보드는 3.3V를 지원하기에 ESP8266에 VCC, GND를 직접 연결한다.
- VCC, GND, TX-RX, RX-TX를 연결한다.
- 이때 UART를 통해서 AT 명령어를 전달하여 제어한다.

<br>

### ◆ ESP8266 Serial 통신 준비
- 설치 소프트웨어 : HTerm (터미널 소프트웨어) https://www.der-hammer.info/pages/terminal.html
- CP2102와 ESP8266가 연결된 후, US와 컴퓨터를 연결한다. 

<br>

### ◆ ESP8266 Serial 통신 확인

<img src="https://user-images.githubusercontent.com/66783849/184472222-0b189226-cecb-474b-8005-14d534242093.png"> HTerm 소프트웨어
<img src="https://user-images.githubusercontent.com/66783849/184472247-194a7b31-7285-4686-b850-ee4c1201ad6a.png"> Newline 설정
<img src="https://user-images.githubusercontent.com/66783849/184472259-2232aea5-2038-4047-b6df-53c4cd03a1a5.png"> Send on Enter

- 컴퓨터에 연결 후 COM선택창 옆 R버튼을 눌러 UART 보드가 인식이 되는지 확인한다.
- 시각적 편의성을 위해 몇가지 설정을 한다.
  1) Newline at > LF로 변경한다. 이는 들여쓰기를 하기 위한 설정이다.
  2) Send on enter > CR-LF로 변경한다. 이는 엔터를 활용한 명령어 전송을 위한 설정이다.
  3) Connect 버튼을 눌러 연결한다.

<br>

### ◆ ESP8266 Serial 통신
- AT 명령어를 전송하여 제어한다. https://blog.daum.net/swimming_7291/35
- AT명령어로 정상작동 확인, 재시작, AP접속을 진행한다.  
주의) 반드시 띄어쓰기는 제거해야 한다.

#### AT 주요 명령어 표
Command | Funtion | Response
-- | -- | --
정상작동 확인 | AT | OK
재시작 | AT+RST | OK
접속 가능한AP 리스트 출력 | AT+CWLAP | 리스트 OK
AP접속 | AT+CWJAP=\<ssid\>,\<password\> | 연결확인 OK
TCP/UDP 통신시작 | AT+CIPSTART=\<type\>,\<remote IP\>, \<remote port\> | id = 0-4, type = TCP/UDP, addr = IP address, port= port
데이터 전송 | AT+CIPSEND=\<length\> | OK
현재 연결 종료 | AT+CIPCLOSE | CLOSED OK

<br>

### ◆ ESP8266 Serial 통신으로 구글 기본페이지 받기
<img src="https://user-images.githubusercontent.com/66783849/184472687-a65e8f1a-4fc1-41b9-b06f-beefcaf1d124.png"> 구글의 기본 페이지

- 구글 기본페이지 받기 AT 명령어 과정
  - AT
  - AT+RST
  - AT+CWJAP=“wifi 이름”,“비밀번호”
  - AT+CIPSTART="TCP","www.google.com",80 (이때 80은 HTTP의 포트주소이다)
  - AT+CIPSEND=18 (엔터 하나는 2개의 길이)
  - GET / HTTP/1.1(엔터 2번)
  - AT+CIPCLOSE  
  ![image](https://user-images.githubusercontent.com/66783849/184472714-c420310d-e68c-4752-8e63-a71e169ea84a.png) Wi-Fi Connect 성공
  ![image](https://user-images.githubusercontent.com/66783849/184472719-501a4e5d-bcca-4638-a50d-4a4e3dcabccf.png) TCP/UDP 성공  
  ![image](https://user-images.githubusercontent.com/66783849/184472723-24a02de3-3082-4e8a-a9bd-6f5778184dec.png) www.google.com 기본 페이지 받기

<br>

### ◆ GET 상세 설명
- GET /내용 HTTP/1.1 서버인식엔터 페이지수신엔터
   - 예) www.google.com/search?q=hell... 접속  
     GET /search?q=hell... HTTP/1.1 (엔터)(엔터)

<br>

### ◆ ESP8266 Serial 통신으로 수원시 고등로 날씨정보 받기 (2022년 기준)
- 기상청 사이트에 접속 후 RSS위치로 이동한다.
- 위치를 입력 후 사이트 정보를 획득한다.  
  예)수원시 팔달구 고등동 [날씨정보 사이트](http://www.kma.go.kr/wid/queryDFSRSS.jsp?zone=4111567000)  
![image](https://user-images.githubusercontent.com/66783849/184472857-fe819b3a-e31d-4b8d-a0ef-99a96d24cc34.png) 기상청 사이트 RSS
![image](https://user-images.githubusercontent.com/66783849/184472858-c68f09dc-ff2e-4f20-acd6-fa7a0f59c6d5.png) 위치에 대한 정보 받는 사이트 획득 화면

- 기상청 정보 받기 AT 명령어 과정
  - AT
  - AT+RST
  - AT+CWJAP=“wifi 이름”,“비밀번호”
  - AT+CIPSTART="TCP","www.kma.go.kr",80
  - AT+CIPSEND=53
  - GET /wid/queryDFSRSS.jsp?zone=4111567000 HTTP/1.1(엔터)(엔터) (이때 엔터를 빠르게 눌러준다)
  - AT+CIPCLOSE  
  ![image](https://user-images.githubusercontent.com/66783849/184472907-b8234ef0-0095-4516-8965-07c1fa5f1d44.png) 받아온 기상청 정보

  


<br>


## 3. AVR

![image](https://user-images.githubusercontent.com/66783849/184473047-ce7d183f-4631-4610-bf3d-78c454bd44d6.png) AVR Atmega8

### ◆ Atmel AVR
- 아트멜 AVR(Atmel AVR)은 1996년 아트멜 사에서 개발된 하버드 구조로 수정한 8비트 RISC 단일칩 마이크로컨트롤러이다.
- 다음과 같은 시리즈가 존재한다.
  - tinyAVR
  - megaAVR
  - XMEGA
  - Application-specific AVR
  - FPSLIC (AVR에 FPGA 추가)
  - 32-bit AVRs

![image](https://user-images.githubusercontent.com/66783849/184473085-06d17383-b95a-4f3a-be0c-6710aa0843bc.png) ISP 연결 커넥터  


### ◆ ISP
- ISP(in-system programming)는 가장 일반적인 방법이다.
- 가장 일반적인 AVR 프로그램 인터페이스 방법으로, 프로그램 전송 방식은 기능적으로 SPI 방법에 Reset 선을 추가한 것이다.  




<br>


## 4. AVR 활용한 Wi-Fi

### ◆ 준비물
- AVR Atmega128A
- AVR USBISP (프로그래밍 인터페이스)
- Microchip Studio (프로그래밍 프로그램)
- ![image](https://user-images.githubusercontent.com/66783849/184473148-b9f37acf-a130-420f-9b44-383f5ea00929.png) ISP 스위치
  - 1번 on : USB 전원사용
  - 2번 on : 3.3V, off : 5V 사용
  - > 1번 on, 2번 on 상태로 설정
-  Wi-Fi 모듈의 입력전원은 3.3V이기 때문에 별도의 전원공급장치를 준비한다.

![image](https://user-images.githubusercontent.com/66783849/184473554-36e93b15-da7a-40ed-89a0-c670327f7415.png) AVR USB ISP
![image](https://user-images.githubusercontent.com/66783849/184473558-cb6a6da6-a792-42f1-a5ac-ed9eaa87d333.png) AVR Atmega128  

![image](https://user-images.githubusercontent.com/66783849/184473565-c70f214f-8be1-4230-a10d-a97499354dfd.png) 
![image](https://user-images.githubusercontent.com/66783849/184473568-18f3eebb-65cf-4fee-9dbf-04219b27acf8.png) Microchip Studio  

<img src="https://user-images.githubusercontent.com/66783849/184473600-5791c7e7-3462-48ea-a427-edc1014bdc55.png" width="60%"> AVR Atmega128 - WiFi 모듈 연결도
<img src="https://user-images.githubusercontent.com/66783849/184473613-d4932136-9542-4fa8-8bd2-ae6450cb2b80.png" width="80%"> 연결모습

![image](https://user-images.githubusercontent.com/66783849/184473624-3688a347-506f-4823-b0fe-c138ee13d69f.png) 사용자 외부전원 보드

<br>

### ◆ MicroChip Studio 프로젝트 설정
1) 새 프로젝트 생성 (폴더에 한글이 없도록 한다)  
  ![image](https://user-images.githubusercontent.com/66783849/184473532-929037d0-882f-4b63-960b-49275ba62df1.png) 새 프로젝트
2) Device 선택 (Atmega128A)  
  ![image](https://user-images.githubusercontent.com/66783849/184473544-c4d8a3c7-dfea-4394-8068-697480427339.png) Device Selection

<br>

### ◆ MicroChip Studio 프로그래밍 준비
 1) AVR과 AVR-ISP를 연결한 후 컴퓨터에 연결한다.
 2) 장치관리자를 통해 AVR ISP의 COM를 알아낸다.
 3) Microchip Studio에 AVR ISP 연결여부를 확인한다.
    - Tools > Add target에서 Select Tool : STK500을 입력하고 COM번호를 등록한다.  
      ![image](https://user-images.githubusercontent.com/66783849/184473757-ac24daf0-538f-4471-82e0-37611914365f.png) Tools > Add Target  
    - View > Available Microchip Tools를 통해 연결장비를 관리한다.  
      ![image](https://user-images.githubusercontent.com/66783849/184473846-7d6a9cc0-6b07-4bcc-aecb-e091ab23c586.png) View > Available Microchip Tools  
    -  Tools > Device Pack Manager를 통해 ATmega_DFP를 Install한다.  
      ![image](https://user-images.githubusercontent.com/66783849/184473767-85436ca0-0257-4d1f-a96e-3f25affb7357.png) Tools > Device Pack Manager  
    -  Tools > Device Programming으로 프로그래밍 수단을 선택한다.
    이후 AVR ISP TOOL 및 COM번호를 입력한다.  
    ![image](https://user-images.githubusercontent.com/66783849/184473781-1507bbf7-6f5b-4c01-acaf-9e7e88c1a0ca.png) Tools > Device Programming을 통해 정상연결을 확인하는 모습  

![image](https://user-images.githubusercontent.com/66783849/184473789-7e41ba05-6d6d-4410-bbd0-1ef305722048.png) Tool Setting 완료 화면

- 이때 Device Programming 창의 Interface의 선택목록이 존재하고, Device signature의 read를 클릭했을 때 AVR의 고유번호가 나타나면 정상작동됨을 알 수 있다.  

  ![image](https://user-images.githubusercontent.com/66783849/184473792-9cbff1d0-eca4-4cf0-8ee6-45b0cc3abdde.png) 생성 오류 창

<br>

### ◆ MicroChip Studio 정상작동 확인 프로그램 Test
- AVR이 정상작동하는지 확인하기 위해 간단한 LED깜빡임 프로그래밍을 진행한다.

#### AVR Atmega128 LED TEST (main.c)
```cpp
#define F_CPU 16000000UL
#include <xc.h>
#include <avr/io.h>
#include <avr/delay.h>

int main(void) {
 DDRD = 0xFF;
 int i = 0;
 
 while(1) {
  i+=10;
  PORTD=0xFF;
  for(int j=0;j<i;j++) _delay_ms(1);
  PORTD=0x00;
  for(int j=0;j<i;j++) _delay_ms(1);
  if(i > 1000)i = 0;
 }
 
}
```
![image](https://user-images.githubusercontent.com/66783849/184474106-d390e014-4ab5-4ce4-95ae-04f697e5fc8d.png) 결과화면  


<br>

### ◆ MicroChip Studio UART Testing
- HTerm을 활용해 Serial 화면을 확인한다.
- AVR에 Serial통신을 위해 CP2102를 통해 연결한다.
- bps를 9600으로 설정 후 결과를 확인한다.  
![image](https://user-images.githubusercontent.com/66783849/184473954-0f340ea0-671a-4bff-a06b-97b866de5496.png) Serial 통신 결과
- 이때 USART0_init 함수 파라미터를 결정하는 bps는 다음과 같이 설정한다.
  - UART_Buffer = Clock_Speed/16/bps-1
  - 9600bps와 같은 경우는 16000000/16/9600-1 = 103.16 > 103 이 된다.
  - 115200bps는 16000000/16/115200-1 = 7.6 > 8 이 된다.


<br>


### ◆ AVR – WiFi UART 통신
- AVR의 시리얼통신 포트를 WiFi 모듈과 연결시킨다.
- 그 후 시간에 맞춰서 AT명령어를 전송한다.
- 이때 Serial 통신 bps는 115200으로 설정한다.

#### AVR Atmega128 UART 통신 송수신 코드 (main.c)
```cpp
#define F_CPU 16000000L
#include <avr/io.h>
#include <util/delay.h>
#include <avr/interrupt.h>
#include <string.h>

void USART0_init(unsigned int UBRR0){
  UBRR0H=(unsigned char)(UBRR0>>8);
  UBRR0L=(unsigned char)(UBRR0);
  UCSR0B=(1<<TXEN0)|(1<<RXEN0)|(1<<RXCIE0); //RX TX
}

char str[100], rx_buf[100], rx_ch, rx_cnt, rx_flag = 0;

ISR(USART0_RX_vect){
  rx_ch=UDR0;
  UDR0=rx_ch; //하이퍼터미널에 입력한 내용을 에코동작으로 확인
  if(rx_ch==0x0D){ //CR이 입력되면
    rx_flag = 1; rx_cnt = 0;
    memcpy(str,rx_buf,rx_cnt);
  }else{ rx_buf[rx_cnt++]=rx_ch; } //수신데이터 저장
}

void TX0_ch(unsigned char data){ //문자송신
  while(!(UCSR0A&(1<<UDRE0))); UDR0 = data;
}

void TX0_STR(unsigned char *str){ while(*str) TX0_ch(*str++); }

int main(void) {
  USART0_init(103); // UART0 초기화 9600bps
  unsigned char str[] = "Hellow \n";
  sei();
  
  while(1) {
    if(rx_flag){
      rx_flag = 0;
      PORTD=0xFF; _delay_ms(1000);
    }
    TX0_STR(str); //문자열 송신
    PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(500);
  }
  
  return 0;
}
```

#### AVR Atmega128 ESP8266 WiFi 통신 코드 (main.c)
```cpp
#include <xc.h>
#define F_CPU 16000000L
#define FOSC 16000000 // Clock Speed
#define BAUD 115200
#define U2X_S 2
#define MYUBRR FOSC/16/BAUD-1
#include <avr/io.h>
#include <util/delay.h>
#include <avr/interrupt.h>
#include <string.h>

#include <stdio.h>

void USART0_init(unsigned int UBRR0){
	UBRR0H=(unsigned char)(UBRR0>>8);
	UBRR0L=(unsigned char)(UBRR0);
	UCSR0B=(1<<TXEN0)|(1<<RXEN0)|(1<<RXCIE0); //RX TX
}

char str[100], rx_buf[100], rx_ch, rx_cnt, rx_flag = 0;

ISR(USART0_RX_vect){
	rx_ch=UDR0;
	UDR0=rx_ch; //하이퍼터미널에 입력한 내용을 에코동작으로 확인
	if(rx_ch==0x0D){ //CR이 입력되면
		rx_flag = 1; rx_cnt = 0;
		memcpy(str,rx_buf,rx_cnt);
		}else{ rx_buf[rx_cnt++]=rx_ch; } //수신데이터 저장
}
void TX0_ch(unsigned char data){ //문자송신
	while(!(UCSR0A&(1<<UDRE0))); UDR0 = data;
}
void TX0_STR(unsigned char *str){ while(*str) TX0_ch(*str++); }


int usartTxChar(char ch, FILE *fp) {  
	TX0_ch(ch);
	return 0;
}

int main(void) {
	USART0_init(8); // UART0 초기화 103 : 9600bps, 8 : 115200bps
	unsigned char str[] = "AT\n";
	sei();
	DDRD = 0xFF;
	
	PORTD=0xFF; _delay_ms(1000);
	PORTD=0x00; _delay_ms(1000);
  
	{
		unsigned char str[] = "AT\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(5500);
	}
	{
		unsigned char str[] = "AT+RST\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(5500);
	}
	
	{
		unsigned char str[] = "AT+CWLAP\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(5500);
	}
	
	{
		unsigned char str[] = "AT+CWJAP=\"2017225030\",\"juya1021\"\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(10500);
	}
	
	{
		unsigned char str[] = "AT+CIPSTART=\"TCP\",\"www.kma.go.kr\",80\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(500);
	}
	
	{
		unsigned char str[] = "AT+CIPSEND=54\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(500);
	}
	
	{
		unsigned char str[] = "GET /wid/queryDFSRSS.jsp?zone=4111567000 HTTP/1.1\r\n\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(5000);
	}
	
	{
		unsigned char str[] = "AT+CIPCLOSE\r\n";
		TX0_STR(str); PORTD=0xFF; _delay_ms(500); PORTD=0x00; _delay_ms(5000);
	}
	
	return 0;
}
```

![image](https://user-images.githubusercontent.com/66783849/184474218-152fe937-80c9-41a2-a943-af85b87e560c.png) AVR Test 결과

## 참고문헌
- 와이파이
  - https://ko.wikipedia.org/wiki/와이파이_얼라이언스
  - https://namu.wiki/w/Wi-Fi
- ESP8266 ESP-01
  - https://www.devicemart.co.kr/goods/view?no=1279338
  - https://www.ktron.in/product/esp8266-esp-01-wifi-module/
- ESP8266EX
  - https://www.mouser.kr/ProductDetail/Espressif-Systems/ESP8266EX?qs=chTDxNqvsynorwcu2ADV3g%3D%3D&gclid=Cj0KCQjwvqeUBhCBARIsAOdt45b_pJNNAVglO2yBeQZ3HHjaZBDXBcdgLuJSRou_Jvh5DeUnTN71d9waAsgQEALw_wcB
- PN25F08
  - https://datasheetspdf.com/datasheet/PN25F08.html
- ESP8266 Pin-Out
  - http://john-home.iptime.org:8085/xe/index.php?mid=board_HzuF34&document_srl=416
- 시리얼 통신 기초 – UART
  - https://www.hardcopyworld.com/?p=2080
  - https://ko.wikipedia.org/wiki/UART
- Microchip Studio
  - https://docs.whiteat.com/wat128_07_02-atmega128a%ec%97%90%ec%84%9c-%ec%97%90%ec%bd%94-%ec%a0%84%ec%86%a1-2-2/
- 아두이노
  - https://ko.wikipedia.org/wiki/아두이노
- AT명령어
  - https://blog.daum.net/swimming_7291/35


