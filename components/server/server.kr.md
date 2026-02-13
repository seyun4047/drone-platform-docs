# 드론 플랫폼 서버

---
## 개요
드론 플랫폼 서버는 제조사 독립형 드론 플랫폼의 핵심 백엔드 서비스입니다.<br>

이 서버는 플랫폼 전반에 걸쳐 드론 세션 관리, 인증, 텔레메트리 처리, 그리고 대량 요청 처리를 포함한 드론의 전체 라이프사이클을 관리합니다.<br>

이 서버는 드론, 데이터 저장 시스템 및 외부 서비스 간의 중앙 조정 계층 역할을 합니다.

---

## 핵심 역할

### 1. 드론 인증 및 세션 관리
서버는 다음을 사용하여 안전하고 확장 가능한 드론 세션을 관리합니다:
- 승인된 드론 데이터베이스 (MySQL)<br>
등록되고 승인된 드론 목록을 유지합니다.
- 드론 인증 토큰 (Redis)<br>
인증된 드론에 대한 접근 토큰을 발급하고 유효성을 검사합니다.
- 드론 심박수 추적 (Redis)<br>
시간 색인화된 심박수 데이터를 사용하여 실시간 연결 상태를 추적합니다.

### 2. 텔레메트리 및 이벤트 처리
서버는 [drone-client](https://github.com/seyun4047/drone-platform-client)에서 전송되는 텔레메트리 및 이벤트 데이터를 수신하고 처리하며, 다음을 포함합니다:
- 텔레메트리 데이터<br>
활성 드론의 실시간 운영 상태 데이터를 처리합니다.
- 이벤트 데이터<br>
인간 감지 및 기타 임무 트리거 활동과 같은 이벤트 데이터를 처리합니다.

### 3. 대시보드 인증
서버는 대시보드 접근을 위한 사용자 등록 및 승인을 처리합니다:
- 사용자 등록 및 승인 워크플로우<br>
사용자는 대시보드 접근을 위해 등록할 수 있습니다. 승인된 사용자만 로그인할 수 있습니다.
- 접근 제어 적용<br>
인증되고 승인된 사용자만 대시보드 API에 접근할 수 있어 데이터 무결성과 개인 정보 보호를 보장합니다.

### 4. 대시보드 API 접근
서버는 웹 대시보드 및 기타 프론트엔드 클라이언트를 위해 안전한 JWT 기반 접근을 제공합니다:
- JWT 토큰 생성 및 검증<br>
성공적인 로그인 시, 서버는 클라이언트가 API 요청에 포함해야 하는 JWT 토큰을 발급합니다.<br>서버는 모든 보호된 엔드포인트에 대해 토큰의 유효성을 검사합니다.
보안 대시보드 엔드포인트<br>
- Redis 심박수 데이터에서 활성 드론 가져오기.
- 드론의 최신 텔레메트리 및 이벤트 데이터 검색.

---
## 사용법
### 로컬 빌드
```bash
# Export env
export $(cat .env | xargs)

# Build the project (Gradle)
./gradlew build

# Run locally with 'local' profile
./gradlew bootRun --args='--spring.profiles.active=local'
```
### Docker 빌드
```bash
# Build Images and Start (MySQL, Redis, App)
docker compose up --build

# Stop and remove containers
docker compose down
```
---
## 드론 데이터 테스트
### 모의 드론 데이터로 흐름 테스트
```bash
# Test
./gradlew test
```
### 실제 드론 데이터로 흐름 테스트
> 실제 드론 데이터로 테스트하고 싶다면, 여기를 참조하세요: [Drone Data Tester](https://github.com/seyun4047/drone-platform-trans-tester)   

## 대시보드 인증 테스트
### 실제 사용자 데이터로 흐름 테스트
```bash
# 회원가입
curl -X POST http://<your_url>/dashboard/register \
  -H "Content-Type: application/json" \
  -d '{
        "username": "testid",
        "password": "testpw"
      }'
# 반환 {"status":상태,"message":"메시지","data":{"id":"테스트아이디"}}%
```
```bash
# 로그인
curl -X POST http://<your_url>/dashboard/login \
  -H "Content-Type: application/json" \
  -d '{
        "username": "testid",
        "password": "testpw"
      }'
# 반환 {"status":상태,"message":"메시지","data":{"token":"jwt"}}%
```
```bash
# 드론 데이터 가져오기
 curl -i --location --request GET 'http://<your_url>/api/dashboard/drone/<telemetry_or_event> /<testid>' \
-H "Auth: Bearer <token>"
# 반환 {"data":{},"updatedAt":currentTimeMillis}%
```
 
---
## DB 가이드
### MYSQL DB 사용 가이드
> MySQL 사용 가이드를 알고 싶다면, 여기를 참조하세요: [DB GUIDE](https://github.com/seyun4047/drone-platform-docs/blob/main/components/server/DB_GUIDE.md)
---

## 대시보드(프론트엔드) 통신
<img height="900" alt="AWS Upload Presigned URL-2026-02-13-144904" src="https://github.com/user-attachments/assets/4e956658-5ef2-4c1d-972d-ea669aa09b67" />