# MSA Playground - Helm Charts

Spring Boot + Kafka 기반 MSA Playground Helm 차트

## 포함 차트
- `api-gateway-chart` : Spring Cloud Gateway
- `order-service-chart` : Order 서비스 (Kafka SAGA)
- `user-service-chart` : User 서비스 (Kafka SAGA)
- `kafka-chart` : Kafka

> 모든 차트는 환경 변수 주입 방식 Docker 이미지 기반

## 배포
```bash
# Kafka
helm install kafka ./kafka-chart

# 애플리케이션
helm install user-service ./user-service-chart
helm install order-service ./order-service-chart
helm install api-gateway ./api-gateway-chart

# 모니터링
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack
