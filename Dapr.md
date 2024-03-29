# Dapr

## Install / Upgrade

Download CLI from [github](https://github.com/dapr/cli/releases/) and replace with the new one

```shell
dapr uninstall --all
dapr init --runtime-version=1.7.2
dapr upgrade --runtime-version 1.7.2 -k
dapr --version
```

## Run apps on local Kubenetes on Docker Desktop for Windows

Under enabling mTLS on Kubernetes cluster on Docker for Windows,
sidecar containers are not injected.

```shell
dapr init -k --enable-mtls=false
```

## Tracing with Jaeger on self-host

```powershell
docker run -d --name jaeger `
  -e COLLECTOR_ZIPKIN_HOST_PORT=:9412 `
  -p 16686:16686 `
  -p 9412:9412 `
  jaegertracing/all-in-one:1.27
```
