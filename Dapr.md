# Dapr

## Install / Upgrade

```sh
dapr uninstall --all
dapr init --runtime-version=1.4.0
dapr --version
```

## Run apps on local Kubenetes on Docker Desktop for Windows

```sh
dapr init -k --enable-mtls=false
```