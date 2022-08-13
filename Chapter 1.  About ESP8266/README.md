문서정보 : 2022.08.13 작성 (원본 2022.01.03. 작성), 작성자 @SAgiKPJH

# 목차
1. Wifi
2. Wifi 모듈 ESP8266 ESP-01, TWM-02

<br>

# 1. Wifi

### ◆ 와이파이

<img src="https://user-images.githubusercontent.com/66783849/184466126-e92f5ab9-cefa-45c4-9fb0-5cb6f50533b7.png" width="20%">

 - 와이파이(Wi-Fi, WiFi)는 전자기기들이 무선랜(WLAN)에 연결할 수 있게 하는 기술로서, 무선 통신 표준 기술 중 하나인 IEEE 802.11에 기반한 서로 다른 장치들간의 데이터 전송 규약이다.
 - 주로 2.4GHz UHF 및 5Ghz SHF ISM 무선 대역을 사용한다. 
 - 대역 내에 위치한 어느 장치라도 무선랜 네트워크의 자원에 접근할 수 있도록 개방도 가능하다.
 - 현재 Wifi의 버전은 Wi-Fi 6까지 나온 상태이다.
 - Wifi 접속 QR 코드 생성기 : https://qifi.org/
 
<br>

 # 2. Wifi 모듈 ESP8266 ESP-01, TWM-02
 
 ### ◆ ESP8266
 
 <img src="https://user-images.githubusercontent.com/66783849/184466206-b077deee-6234-414f-a7db-6a15efdb9c3f.png" width="20%"> Wi-Fi 모듈 ESP8266 ESP-01 <img src="https://user-images.githubusercontent.com/66783849/184466280-dc27d90a-a39b-4cb1-b08f-d62a34bda826.png" width="20%"> 모듈의 뒷 모습  

 
 - ESP8266은 Wi-Fi 통신을 위한 칩이다.
 - 이 칩엔 Wi-Fi를 지원하는 프로그램이 되어 있고, 외부에서 직접 코드를 구성하거나 AT명령어를 UART를 통해서 통신할 수 있다.

### ◆ ESP8266 ESP-01, TWM-02

<img src="https://user-images.githubusercontent.com/66783849/184466406-32d6cbfb-36c2-4323-94b6-e1c2d1dd61ad.png" width="70%" > 부품 사양  

<img src="https://user-images.githubusercontent.com/66783849/184466439-1a3a6fd6-0fde-400e-96dd-75ad87f7ddd5.png" width="29%"> Wi-Fi 모듈 ESP8266 TWM-02




 - ESP8266을 활용한 보드중 하나이다.
 - 입력전원 : 3.3V 300mA
 - 1MB 플래시 메모리, 802.11b/g/n
 - SDIO 2.0, SPI, UART 통신으로 Wi-Fi 통신을 진행한다. 
   - SDIO : SD카드 통신방식
   - SPI : I2C통신과 같은 통신방법의 한 종류
   - UART : 범용 비동기화 송수신기
 - 가격 : 2,400원 (ESP-01), 6,000원 (TWM-02)
