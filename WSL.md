# WSL Setup(Ubuntu)

Install script for Ubuntu 18.04

## SSH Keygen with ED25519

```sh
ssh-keygen -t ed25519 -P "SOMEPASSPHRASE"  -C "github"
```

## Az

```sh
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

## Dotnet and Function Core Tools

[Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/install/linux-package-manager-ubuntu-1804)
[Github](https://github.com/Azure/azure-functions-core-tools)

```sh
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install dotnet-sdk-3.1 azure-functions-core-tools
```

## Nodejs

```sh
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## Python with Pyenv

```sh
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.bashrc
source ~/.bashrc
pyenv install --list
pyenv install anaconda3-2019.03
pyenv local anaconda3-2019.03
```

## Kubectl

```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
echo 'source <(kubectl completion bash)' >>~/.bashrc
source ~/.bashrc
```

## Docker

See [this](https://nickjanetakis.com/blog/setting-up-docker-for-windows-and-wsl-to-work-flawlessly)

```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update && sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo usermod -aG docker $USER
echo "export DOCKER_HOST=tcp://localhost:2375" >> ~/.bashrc && source ~/.bashrc
pip install --user docker-compose
```

If you've already been trying Windows Insider build version, WSL 2 is available.
[Docker](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)

## Helm

[Official](https://helm.sh/docs/intro/install/)

```sh
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```

## Go

```sh
sudo apt-get install golang-go
```

## Rust

```sh
curl https://sh.rustup.rs -sSf | sh
export PATH=~/.cargo/bin:$PATH
```

## Lazy Docker

```sh
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
```

## Jupyter Notebook on Docker

```sh
docker run --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v "$PWD":/home/jovyan/work jupyter/datascience-notebook:latest
```

## gcloud

[Official Document](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)

```sh
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
gcloud init
```
