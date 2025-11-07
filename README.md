# MSA Playground - Helm Charts

이 저장소는 Spring Boot와 Kafka를 K8s-Native 방식으로 배포하는 MSA(Microservice Architecture) Playground의 Helm Chart를 관리합니다.

### 포함된 차트
- api-gateway-chart: Spring Cloud Gateway (K8s Service Discovery 연동)
- order-service-chart: Order 서비스 (Kafka SAGA Consumer/Producer)
- user-service-chart: User 서비스 (Kafka SAGA Consumer/Producer)

[중요] 이 차트들은 "환경 변수 주입" 방식을 사용하도록 리팩토링된 Docker 이미지를 기반으로 합니다. (로컬 application.yml의 kubernetes.config.enabled: false 설정)

> 로컬 개발 및 실습용으로, H2 DB와 클러스터 내 Kafka를 사용합니다.

---

배포 가이드 (순서 중요)
로컬 K8s 클러스터(OrbStack, Docker Desktop K8s)에 전체 MSA 시스템을 배포합니다.

#### 1단계: (최초 1회) 공용 인프라 배포 (kubectl)
애플리케이션 차트가 의존하는 **공용 인프라(RBAC, Kafka)**를 kubectl로 먼저 배포합니다.

```baseh
# 1. K8s Service Discovery를 위한 공용 권한(RBAC) 생성
kubectl apply -f user-service-rbac.yml

# 2. Kafka 인프라 생성 (ConfigMap, Deployment, Service)
kubectl apply -f kafka-configmap.yml
kubectl apply -f kafka-deployment.yml
kubectl apply -f kafka-service.yml
```


#### 2단계: 애플리케이션 배포 (Helm)
helm install 명령어를 사용하여 각 마이크로서비스를 배포합니다.

```bash
# 1. User Service 배포
helm install user-service ./user-service-chart

# 2. Order Service 배포
helm install order-service ./order-service-chart

# 3. API Gateway 배포 (외부 접속 관문)
helm install api-gateway ./api-gateway-chart
```


#### 3단계: (선택) 모니터링 인프라 배포 (Helm)
K8s 클러스터의 상태와 각 서비스의 메트릭을 수집하기 위해 Prometheus와 Grafana를 설치합니다.

```bash
# 1. Prometheus Helm 저장소(북마크) 추가
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# 2. kube-prometheus-stack 설치
helm install monitoring prometheus-community/kube-prometheus-stack
```

