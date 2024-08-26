# Tri lecture note

## Undestanding Opentelemetry 

1. Core Demo Services
  - Ad Services
  - Cart Services
  - Checkout Services
  - Currency Services
  - Email Services
  - Fraud detection
  - Frontend 
  - Shipping 
  - Recommendation
  - Quote
  - Payment
  - product catalog
  - frontend proxy
  - image provider
  - Load Generator

2. Dependent Services
  - Flagd, Feature flagging service
  - Kafka - checkout accounting, **fraud detection**
  - valkey used by **cart Services

3. Telemetry Components - [otelcol-config.yml](src/otelcollector/otelcol-config.yml)
  - Jaeger
  - Grafana
  - Opentelemetry Collector
  - Prometheus
  - Open Search 
