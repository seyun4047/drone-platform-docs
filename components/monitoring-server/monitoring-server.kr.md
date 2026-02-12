# 드론 플랫폼 모니터링 서버

---
## 작동 방식
모니터링 서버는 별도의 (로컬) 서비스로 실행되며, 메인 [드론 플랫폼 서버](https://github.com/seyun4047/drone-platform-server)에서 이미 실행 중인 Redis 및 MySQL 인스턴스에 연결됩니다.

이 시스템은 Redis ZSET 기반의 심박 신호 모니터링 메커니즘을 사용하여 심박 신호 전송을 중단한 드론을 자동으로 감지하고 연결을 해제합니다.

---
## 모니터링 대상
이 서비스의 주요 역할은 다음과 같은 비정상적인 드론 상태를 감지하는 것입니다.
- 지정된 기간 동안 비활성 상태였던 드론
- 심박 신호 전송을 중단한 드론
- 상태 업데이트 전송에 실패한 드론

---
## 도움을 주는 방식
이러한 경우를 식별하고 처리함으로써 모니터링 서버는 다음을 돕습니다.
- 불필요한 서버 리소스 소모 방지
- 데이터베이스 부하 감소
- 전반적인 플랫폼 안정성 유지

---

## 사용법
```bash
# 환경 변수 로드
set -a
source dev.env
set +a
```
```bash
# 빌드
./gradlew build
```
```bash
# 실행
./gradlew bootRun
```

---
## 모니터링 흐름
모니터링 프로세스는 아래 흐름을 따릅니다:
|Monitoring Server|
|---|
|<img width="450" alt="Untitled diagram-2026-02-11-173920" src="https://github.com/user-attachments/assets/adbbeee5-7544-46c0-a276-0a04aae3e303" />|

---