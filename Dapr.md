# Dapr

## Install / Upgrade

```shell
dapr uninstall --all
dapr init --runtime-version=1.4.0
dapr --version
```

## Run apps on local Kubenetes on Docker Desktop for Windows

Under enabling mTLS on Kubernetes cluster on Docker for Windows,
sidecar containers are not injected.

```shell
dapr init -k --enable-mtls=false
```
