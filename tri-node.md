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

Opentelemetry generates traces, metrics, and logs.

Configuration defines pipelines which each type of telemetry data is handled basically receivers, processors and exporters

> /opentelemetry-demo$ docker compose up --force-recreate --remove-orphans --detach

> docker compose down

# Otel Collector
The OpenTelemetry Collector receives traces, metrics, and logs, processes the telemetry, and exports it to a wide variety of observability backends using its components. For a conceptual overview of the Collector,
## Prerequisites

- docker
- Go
- GOBIN env 

> export GOBIN=${GOBIN:-$(go env GOPATH)/bin}

## Environment

"""bash
# pull Opentelemetry collector docker image
docker pull otel/opentelemetry-collector:0.107.0

# install telemetrygen utility
go install github.com/open-telemetry/opentelemetry-collector-contrib/cmd/telemetrygen@latest
"""

## Generate and collect telemetry

""" bash
docker run \
  -p 127.0.0.1:4317:4317 \
  -p 127.0.0.1:55679:55679 \
  otel/opentelemetry-collector:0.107.0 \
  2>&1 | tee collector-output.txt # optionally tee output for easier search later
"""

#
