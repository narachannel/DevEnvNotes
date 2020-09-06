# WSL Setup(Ubuntu)

Install script for Ubuntu 18.04

## Environments

### SSH Keygen with ED25519

```sh
ssh-keygen -t ed25519 -P "SOMEPASSPHRASE"  -C "github"
```

## Programing Language

### Dotnet and Function Core Tools

[Microsoft Docs](https://docs.microsoft.com/en-us/dotnet/core/install/linux-package-manager-ubuntu-1804)
[Github](https://github.com/Azure/azure-functions-core-tools)

```sh
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb
sudo add-apt-repository universe
sudo apt-get update
sudo apt-get install dotnet-sdk-3.1 azure-functions-core-tools
```

### Nodejs

```sh
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Python with Pyenv

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

### Go

```sh
sudo apt-get install golang-go
```

### Rust

```sh
curl https://sh.rustup.rs -sSf | sh
export PATH=~/.cargo/bin:$PATH
```

### Ruby

rbenv[Github](https://github.com/rbenv/rbenv)

```sh
git clone https://github.com/rbenv/rbenv.git ~/.rbenv
cd ~/.rbenv && src/configure && make -C src
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
echo "$(rbenv init -)" >> ~/.bashrc
source ~/.bashrc
which rbenv
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
mkdir -p "$(rbenv root)"/plugins
git clone https://github.com/rbenv/ruby-build.git "$(rbenv root)"/plugins/ruby-build
sudo apt-get install -y libssl-dev libreadline-dev zlib1g-dev
rbenv install --list
rbenv install 2.5.7
rbenv local 2.5.7
```

## Container Tools

### Kubectl

```sh
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
echo 'source <(kubectl completion bash)' >>~/.bashrc
source ~/.bashrc
```

### Docker

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

### Helm

[Official](https://helm.sh/docs/intro/install/)

```sh
curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
sudo apt-get install apt-transport-https --yes
echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
sudo apt-get update
sudo apt-get install helm
```

### Istio

[Official](https://istio.io/docs/setup/getting-started/)

```sh
curl -L https://istio.io/downloadIstio | sh -
sudo mv istio-v-xx/bin/* /usr/local/bin
source ~/istioctl.bash
```

### Lazy Docker

```sh
curl https://raw.githubusercontent.com/jesseduffield/lazydocker/master/scripts/install_update_linux.sh | bash
```

### Jupyter Notebook on Docker

```sh
docker run --rm -p 8888:8888 -e JUPYTER_ENABLE_LAB=yes -v "$PWD":/home/jovyan/work jupyter/datascience-notebook:latest
```

### Skaffold

[Page](https://skaffold.dev/docs/install/)

```sh
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64
chmod +x skaffold
sudo mv skaffold /usr/local/bin
```

### Dapr

[Github](https://github.com/dapr/cli)

```sh
wget -q https://raw.githubusercontent.com/dapr/cli/master/install/install.sh -O - | /bin/bash
dapr init # stand alone
docker network create dapr-network && dapr init --network dapr-network # docker
dapr init --kubernetes # k8s
```

### Terraform

```sh
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt-get update && sudo apt-get install terraform
terraform -install-autocomplete
```

## Cloud Tools

### Az

```sh
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

### gcloud

[Official Document](https://cloud.google.com/sdk/docs/quickstart-debian-ubuntu)

```sh
echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] http://packages.cloud.google.com/apt cloud-sdk main" | sudo tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -
sudo apt-get update && sudo apt-get install google-cloud-sdk
gcloud init
```

### aws-cli

[Github](https://github.com/aws/aws-cli)

```sh
sudo pip install awscli
aws configure
```

## ZSH

### Windows Setup

Customize Terminal.exe first
[See](https://www.hanselman.com/blog/HowToMakeAPrettyPromptInWindowsTerminalWithPowerlineNerdFontsCascadiaCodeWSLAndOhmyposh.aspx)

### ZSH Setup in WSL

```sh
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
# change theme to something
wget https://raw.githubusercontent.com/caiogondim/bullet-train-oh-my-zsh-theme/master/bullet-train.zsh-theme -P $ZSH_CUSTOM/themes/
```

Modify config file `~/.zshrc`

```text
ZSH_THEME="bullet-train"
```

### Auto completion with Oh my zsh plugins

```text
plugins=(git docker docker-compose dotnet helm kubectl terraform)
```

### Migrate Ruby env and Pyenv configurations

```text
# Pyenv
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi

# Ruby Env
export PATH="$HOME/.rbenv/bin:$PATH"
if command -v rbenv 1>/dev/null 2>&1; then
  eval "$(rbenv init -)"
fi
```
