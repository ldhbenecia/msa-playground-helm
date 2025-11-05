# msa-playground-helm

이 레포지토리는 Spring Boot 기반 마이크로서비스를 쿠버네티스에서 연습하기 위한 Helm 차트를 포함합니다.

### 포함된 차트

- `api-gateway-chart`: Spring Cloud Gateway
- `order-service-chart`: Order 서비스
- `user-service-chart`: User 서비스

> 로컬 개발 및 실습용으로, H2 DB와 클러스터 내 Kafka를 사용합니다.

---

## 선행 조건

- Kubernetes 클러스터
- Helm 3.x
- 로컬에 빌드된 Docker 이미지
  - `msa/api-gateway:1.0`
  - `msa/order-service:1.0`
  - `msa/user-service:1.0`
- Kafka 클러스터 내에서 실행