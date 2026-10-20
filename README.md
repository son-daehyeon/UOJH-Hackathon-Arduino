# UOJH-Hackathon-Arduino

> 원격 제어 공기청정기 펌웨어, 온습도·먼지 측정과 팬 제어 (ESP8266 · PlatformIO)

해커톤에서 만든 원격 제어 공기청정기의 펌웨어다. 2023년에 만들었다. 온습도와 미세먼지를 측정해 중계 서버로 보내고, 대기질에 따라 팬과 LED를 제어한다.

## 주요 기능
- Wi-Fi 연결 후 중계 서버의 `/ws`에 WebSocket으로 접속하고 `auth:arduino`로 자신을 등록한다
- DHT11로 온도·습도, 아날로그 먼지 센서로 대기질을 1초마다 측정해 값이 바뀌면 `from_arduino:` 메시지로 보낸다
- 대시보드 요청(`request`)에 현재 측정값과 팬 상태를 응답한다
- 팬 제어: 자동 모드에서는 대기질 50 이상일 때 팬을 켜고, 수동 모드에서는 `fan_on` / `fan_off` 명령을 따른다
- 대기질에 따라 초록·노랑·빨강 LED 표시
- 16x2 I2C LCD 시작 화면

## 기술 스택
| 영역 | 기술 |
|---|---|
| 언어 | C++ (Arduino) |
| 프레임워크 | PlatformIO, espressif8266 (보드 `d1`), WebSockets, LiquidCrystal_I2C, DHT sensor library for ESPx |

## 구조
```
platformio.ini      보드·라이브러리 설정
src/main.cpp        펌웨어 (Wi-Fi, WebSocket, 센서, 팬·LED)
other/i2c_scan.cpp  I2C 주소 확인용 스케치
other/wifi_scan.cpp Wi-Fi 스캔용 스케치
```

## 실행
```bash
pio run -t upload
pio device monitor
```
`src/main.cpp` 상단의 `WIFI_SSID`, `WIFI_PW`, `WEBSOCKET_HOST`를 사용할 환경에 맞게 바꾼다.

## 관련 저장소
- [UOJH-Hackathon-Server](https://github.com/son-daehyeon/UOJH-Hackathon-Server) — WebSocket 중계 서버 (Spring Boot)
- [UOJH-Hackathon-Client](https://github.com/son-daehyeon/UOJH-Hackathon-Client) — 모니터링·제어 대시보드 (React)

---
2023 · 개인 프로젝트 · 보관됨
