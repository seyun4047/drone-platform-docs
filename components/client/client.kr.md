# 드론 데이터 전송 및 다중 ROI 감지 클라이언트

---
## 저장소 개요
- 이 클라이언트 애플리케이션은 드론에서 원격 측정 데이터를 수집하여 중앙 서버로 실시간 전송합니다.
- 이 시스템은 드론의 비디오 피드 및 화면 상의 원격 측정 데이터를 지속적으로 모니터링할 수 있는 환경에서 작동하도록 설계되었습니다.
- 일반적으로 원격 측정 정보는 속도 및 기타 비행 데이터가 표시되는 드론 컨트롤러 디스플레이에서 직접 캡처됩니다.

---

## 작동 방식

### 설정
| 1. 서버 연결 | 2. ROI 선택 | 3. 분석 |
|:---:|:---:|:---:|
|<img width="200" alt="connect_to_the_server" src="https://github.com/user-attachments/assets/d15b80d3-28c8-4fd1-aefe-d77681471950" /> |<img width="200" alt="select_roi" src="https://github.com/user-attachments/assets/3c84c375-0ae8-4964-84a5-62a603ef8cde" /> | <img width="200" alt="analysis" src="https://github.com/user-attachments/assets/b39e6ae3-44a2-4e24-93ab-0ca1fdbc4222" /> |
|승인된 드론 시리얼과 장치 이름을 입력한 후 연결합니다.|원하는 정보를 모니터링할 화면 영역을 선택합니다.|데이터가 감지되면 처리되어 메인 서버로 전송됩니다.|

### 데이터 전송
| 사람 감지 | 이벤트 데이터 전송 |
|:---:|:---:|
|<img alt="detect_human_ex" src="https://github.com/user-attachments/assets/2c66ff16-f5ff-43eb-b082-97bfa7bc7d7c" width="300">|<img width="500" height="83" alt="event_data" src="https://github.com/user-attachments/assets/c5db2f05-a378-4eae-acad-22f84dea28e1" /> |
|자동으로 사람을 감지합니다.|이벤트 데이터를 서버로 전송합니다.<br>(이 이미지는 클라이언트로부터 서버가 수신한 이벤트 데이터를 나타냅니다.)|

| 원격 측정 데이터 전송 |
|:---:|
|<img  width="500" height="66" alt="telemetry_data" src="https://github.com/user-attachments/assets/bed9bc00-f311-4313-9bef-54cb4e3d6934"/> |
|아무것도 감지되지 않으면 원격 측정 데이터를 서버로 전송합니다.<br>(이 이미지는 클라이언트로부터 서버가 수신한 원격 측정 데이터를 나타냅니다.)|


---
## 설치
필요한 종속성을 설치합니다:
```bash
pip install -r requirements.txt
```
---
## 사용법
애플리케이션을 실행합니다:
```bash
python3 main.py
```
---
## 스택
| 범주 | 기술 | 버전 | 목적 |
|------------------|---------------------------------|-------------|-------------------------------------|
| 언어 | Python | 3.x | 핵심 애플리케이션 로직 |
| 숫자 OCR | Tesseract (pytesseract) | 0.3.13 | 속도 및 원격 측정 숫자 추출 |
| 사람 감지 | YOLOv8 (Ultralytics) | 8.4.12 | 실시간 사람 감지 |
| 컴퓨터 비전 | OpenCV | 4.12.0.88 | 이미지 처리 |
| GUI | PyQt5 | 5.15.11 | 데스크톱 인터페이스 |
| 딥러닝 | PyTorch (torch, torchvision) | 2.10.0 / 0.25.0 | 모델 추론 엔진 |

---
## 이벤트 처리
비디오 피드에서 사람이 감지되면 이벤트가 생성됩니다.
이벤트 데이터는 서버로 전송됩니다.
서버와 클라이언트 모두 이러한 이벤트를 실시간으로 모니터링할 수 있습니다.

---
## 흐름
| 전체 흐름 | AWS S3 업로드 흐름 |
|:---:|:---:|
|<img height="1000" alt="Overall flow" src="https://github.com/user-attachments/assets/8e1de094-ba4e-4340-bc16-c5660f0cc688" />|<img height="1000" alt="aws s3 upload flow" src="https://github.com/user-attachments/assets/0597c7c9-e2e1-4be7-ae5c-de554b0f88ff" />|
---
## 주의사항
이 시스템은 드론의 비디오 정보를 실시간으로 모니터링할 수 있는 환경에서 작동해야 합니다.
드론 원격 측정 데이터는 일반적으로 카메라 드론의 원격 조종기 화면에서 얻습니다.
화면에는 속도 및 기타 비행 정보가 표시되어야 합니다.
좌표 감지를 위해서는 간단한 GPS 모듈을 부착해야 하며, 해당 데이터는 실시간으로 감지할 수 있도록 화면에 표시되어야 합니다.