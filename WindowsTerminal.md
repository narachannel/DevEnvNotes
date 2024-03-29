# Windows Terminal PowerShell Setup

## Official

[Github](https://github.com/microsoft/terminal)

[Microsoft Docs](https://docs.microsoft.com/windows/terminal)

## Before installation

- Git for windows [Donwload](https://gitforwindows.org/)
- Nerd Font [Download](https://www.nerdfonts.com/font-downloads)
- PowerShell Core [Github](https://github.com/PowerShell/PowerShell/releases)

## Winget

[Github](https://github.com/microsoft/winget-cli)

## Oh my posh

[Github](https://github.com/JanDeDobbeleer/oh-my-posh)

```powershell
winget install JanDeDobbeleer.OhMyPosh -s winget
```

## Posh-Git

[Github](https://github.com/dahlbyk/posh-git)

```powershell
Install-Module posh-git -Scope CurrentUser -AllowPrerelease -Force
```

Enable Posh Git

```powershell
$env:POSH_GIT_ENABLED = $true
```

## Git Aliases

```powershell
Install-Module git-aliases -Scope CurrentUser -AllowClobber
```

## Command Completion for Kubernetes

[GitHub](https://github.com/mziyabo/PSKubectlCompletion)

Install the module first

```powershell
Install-Module -Name PSKubectlCompletion
```

Write these lines to `$profile`

```powershell
Import-Module PSKubectlCompletion
Register-KubectlCompletion
```

[Github](https://github.com/tillig/ps-bash-completions)

This will also work

## Command Completion for dotnet

[Scott Hanselman's article](https://www.hanselman.com/blog/command-line-tab-completion-for-net-core-cli-in-powershell-or-bash)

[Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/register-argumentcompleter?view=powershell-7)

```powershell
Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock {
    param($commandName, $wordToComplete, $cursorPosition)
        dotnet complete --position $cursorPosition "$wordToComplete" | ForEach-Object {
           [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
        }
}
```

## Azure Powershell

```powershell
Install-Module -Name Az -AllowClobber -Scope AllUsers
```

## Remove redundant user name

```powershell
$env:POSH_SESSION_DEFAULT_USER = [System.Environment]::UserName
```

## PSReadline and configuration

See [this blog post](https://www.hanselman.com/blog/adding-predictive-intellisense-to-my-windows-terminal-powershell-prompt-with-psreadline)

```powershell
Install-Module PSReadLine -AllowPrerelease -Force
```

Add these lines to $PROFILE

```powershell
Import-Module PSReadLine
Set-PSReadLineOption -PredictionSource History
Set-PSReadLineOption -PredictionViewStyle ListView
Set-PSReadLineOption -EditMode Windows
```

## Profile Example

```powershell
using namespace System.Management.Automation
using namespace System.Management.Automation.Language

Set-Alias k kubectl.exe
Set-Alias code code-insiders.cmd
Set-Alias which gcm
Set-Alias tig  'C:\Program Files\Git\usr\bin\tig.exe'
Set-Alias grep select-string
Import-Module posh-git
Import-Module PSKubectlCompletion
Import-Module PSReadLine
Register-KubectlCompletion
# Import-Module Az # This will take a few seconds
oh-my-posh init pwsh | Invoke-Expression

Set-PSReadLineOption -PredictionSource History
Set-PSReadLineOption -PredictionViewStyle ListView
Set-PSReadLineOption -EditMode Windows

# Environment variables
$env:POSH_SESSION_DEFAULT_USER = [System.Environment]::UserName
$env:POSH_GIT_ENABLED = $true
$env:KUBE_EDITOR = 'code-insiders.cmd -w'

Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock {
    param($commandName, $wordToComplete, $cursorPosition)
        dotnet complete --position $cursorPosition "$wordToComplete" | ForEach-Object {
           [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
        }
}

## dapr completion powershell >> $PROFILE
```
