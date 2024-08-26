# Preprequisite

install helm

> sudo snap install helm --classic

or 

```bash
$ curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
$ chmod 700 get_helm.sh
$ ./get_helm.sh # may need sudo here
```
start minikube

> minikube start

install telemetry by helm

> helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts

Services are installed
- Webstore             http://localhost:8080/
- Grafana              http://localhost:8080/grafana/
- Load Generator UI    http://localhost:8080/loadgen/
- Jaeger UI            http://localhost:8080/jaeger/ui/

Use the demo

> kubectl port-forward svc/my-otel-demo-frontendproxy 8080:8080



