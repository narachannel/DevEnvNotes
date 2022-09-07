# Mac Setup Guide(Apple Silicon)

## Install via GUI

- Chrome
- Jetbrains Toolbox
- Visual Studio Code
- Install [Nerd Font](https://www.nerdfonts.com/font-downloads)

## Install via Mac App Store

- XCode

## Via Command line

```shell
git config --global user.name "Koji Narazaki"
git config --global user.email "koji@narachannel.com"
ssh-keygen -t ed25519
cat ~/.ssh/id_ed25519 | pbcopy
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
softwareupdate --install-rosetta
```

## Via Homebrew

- dotnet
- Terraform
- JDK
- Helm
- Skaffold
- Dapr
- Go
- Node.js
- AWS Tools
- Github CLI

```shell
brew install zsh-completions
# Modify zshrc
source ~/.zshrc
rm -f ~/.zcompdump; compinit
brew install --cask google-japanese-ime
brew install zsh-autosuggestions
brew install --cask docker
brew install --cask devtoys
brew install --cask corretto
brew install helm
brew install skaffold
brew install --cask dotnet-sdk
brew install mono-libgdiplus # https://docs.microsoft.com/en-us/dotnet/core/install/macos#libgdiplus
brew install terraform
brew install golang
brew install node
brew install gh
brew install awscli
brew tap aws/tap
brew install aws-sam-cli
arch -arm64 brew install dapr/tap/dapr-cli
```

## Oh-My-Zsh configuration

```zsh
zstyle ":completion:*:commands" rehash 1

typeset -U path PATH
path=(
  /opt/homebrew/bin(N-/)
  /opt/homebrew/sbin(N-/)
  /usr/bin
  /usr/sbin
  /bin
  /sbin
  /usr/local/bin(N-/)
  /usr/local/sbin(N-/)
  /Library/Apple/usr/bin
  '/Users/narachannel/Library/Application Support/JetBrains/Toolbox/scripts'
)

export GOPATH=$HOME/go
export GOROOT="$(brew --prefix golang)/libexec"
export PATH="$PATH:${GOPATH}/bin:${GOROOT}/bin"
plugins=(git brew kubectl history-substring-search)

alias code="code-insiders"
alias k="kubectl"
alias python="python3"
alias pip="pip3"

ZSH_THEME="powerlevel10k/powerlevel10k"

#ZSH Autocomplete
if type brew &>/dev/null; then
  FPATH=$(brew --prefix)/share/zsh-completions:$FPATH
  source $(brew --prefix)/share/zsh-autosuggestions/zsh-autosuggestions.zsh
  source <(kubectl completion zsh)
  autoload -Uz compinit
  compinit
fi
```

## PowerShell Installation

```shell
brew install --cask powershell
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```

## Tips

- Register Visual Studio Code as `code` [VS Code](https://code.visualstudio.com/docs/setup/mac)
- Path for Homebrew [See](https://zenn.dev/sprout2000/articles/bd1fac2f3f83bc)