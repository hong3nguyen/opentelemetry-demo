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

# Deployment

3 ways to deploy an Opentelemetry

1. No collector: The simplest pattern is not to use a collector at all. This pattern consists of applications instrumented with **an OpenTelemetry SDK** that export telemetry signals (traces, metrics, logs) directly into a backend
2. Agent: The agent collector deployment pattern consists of applications — instrumented with **an OpenTelemetry SDK** using **OpenTelemetry protocol (OTLP)** — or other collectors (using the OTLP exporter) that send telemetry signals to a collector instance running with the application or on the same host as the application (such as a sidecar or a daemonset).
3. Gateway: The gateway collector deployment pattern consists of applications (or other collectors) sending telemetry signals to a **single OTLP endpoint** provided by one or more collector instances running as a standalone service (for example, a deployment in Kubernetes), typically per cluster, per data center or per region.

# Otel Collector installation
The OpenTelemetry Collector receives traces, metrics, and logs, processes the telemetry, and exports it to a wide variety of observability backends using its components. For a conceptual overview of the Collector,

## Quick run
### Prerequisites

- docker
- Go
- GOBIN env 

> export GOBIN=${GOBIN:-$(go env GOPATH)/bin}

### Environment

```bash
# pull Opentelemetry collector docker image
docker pull otel/opentelemetry-collector:0.107.0

# install telemetrygen utility
go install github.com/open-telemetry/opentelemetry-collector-contrib/cmd/telemetrygen@latest
```

### Generate and collect telemetry
launch the collector
``` bash
docker run \
  -p 127.0.0.1:4317:4317 \
  -p 127.0.0.1:55679:55679 \
  otel/opentelemetry-collector:0.107.0 \
  2>&1 | tee collector-output.txt # optionally tee output for easier search later
```

Generate sample traces 
```bash
$GOBIN/telemetrygen traces --otlp-insecure --traces 3
# or 

$GOBIN/telemetrygen traces --otlp-insecure \
  --traces 3 2>&1 | grep -E 'start|traces|stop'

# show the trace results
$ grep -E '^Span|(ID|Name|Kind|time|Status \w+)\s+:' ./collector-output.txt
```
## Install collector
docker
```bash
docker pull otel/opentelemetry-collector-contrib:0.107.0

docker run otel/opentelemetry-collector-contrib:0.107.0 # or
docker run -v $(pwd)/config.yaml:/etc/otelcol-contrib/config.yaml otel/opentelemetry-collector-contrib:0.107.0



```

Docker compose with docker-compose.yaml
```yaml
otel-collector:
  image: otel/opentelemetry-collector-contrib
  volumes:
    - ./otel-collector-config.yaml:/etc/otelcol-contrib/config.yaml
  ports:
    - 1888:1888 # pprof extension
    - 8888:8888 # Prometheus metrics exposed by the Collector
    - 8889:8889 # Prometheus exporter metrics
    - 13133:13133 # health_check extension
    - 4317:4317 # OTLP gRPC receiver
    - 4318:4318 # OTLP http receiver
    - 55679:55679 # zpages extension
```

Kubernetes
> kubectl apply -f https://raw.githubusercontent.com/open-telemetry/opentelemetry-collector/v0.107.0/examples/k8s/otel-config.yaml

Linux
## Configuration

### Location
Default: /etc/<otel-directory>/config.yaml
```bash
otelcol --config=env:MY_CONFIG_IN_AN_ENVVAR --config=https://server/config.yaml
otelcol --config="yaml:exporters::debug::verbosity: normal"
```

### Configuration structure
- Receivers: Receivers collect telemetry from one or more sources. They can be pull or push based, and may support one or more data sources. The Collector requires one or more receivers.

- Processors: Processors take the data collected by receivers and modify or transform it before sending it to the exporters. Data processing happens according to rules or settings defined for each processor, which might include filtering, dropping, renaming, or recalculating telemetry, among other operations. The order of the processors in a pipeline determines the order of the processing operations that the Collector applies to the signal.

- Exporters: Exporters send data to one or more backends or destinations. Exporters can be pull or push based, and may support one or more data sources.

- Connectors: Connectors join two pipelines, acting as both exporter and receiver. A connector consumes data as an exporter at the end of one pipeline and emits data as a receiver at the beginning of another pipeline. The data consumed and emitted may be of the same type or of different data types. You can use connectors to summarize consumed data, replicate it, or route it. You can configure one or more connectors using the connectors section of the Collector configuration file. By default, no connectors are configured. Each type of connector is designed to work with one or more pairs of data types and may only be used to connect pipelines accordingly.

- Extensions: Extensions are optional components that expand the capabilities of the Collector to accomplish tasks not directly involved with processing telemetry data. For example, you can add extensions for Collector health monitoring, service discovery, or data forwarding, among others.

- Service section:The service section consists of three subsections:  Extensions, Pipelines, Telemetry

- Pipelines: The pipelines subsection is where the pipelines are configured, which can be of the following types: traces collect and processes trace data, metrics collect and processes metric data, logs collect and processes log data.

- Telemetry: The telemetry config section is where you can set up observability for the Collector itself. It consists of two subsections: logs and metrics. To learn how to configure these signals, see Activate internal telemetry in the Collector.


# Management 
## basically
- Querying the agent information and configuration. 
- Upgrading/downgrading agents and management of agent-specific packages,
- Applying new configurations to agents. Applying new configurations to agent-specific
- Health and performance monitoring of the agents, typically CPU and memory usage and also agent-specific metrics, for example, the rate of processing or backpressure-related information.
- Connection management between a control plane and the agent such as handling of TLS certificates (revocation and rotation).
## OpAMP
Observability vendors and cloud providers offer proprietary solutions for agent management. In the open source observability space, there is an emerging standard that you can use for agent management: Open Agent Management Protocol (OpAMP).
The OpAMP specification defines how to manage a fleet of telemetry data agents. These agents can be OpenTelemetry collectors, Fluent Bit or other agents in any arbitrary combination.




