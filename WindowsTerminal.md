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

```ps1
Install-Module oh-my-posh -Scope CurrentUser -AllowPrerelease
```

## Posh-Git

[Github](https://github.com/dahlbyk/posh-git)

```ps1
Install-Module posh-git -Scope CurrentUser -AllowPrerelease -Force
```

## Command Completion for Kubernetes

[GitHub](https://github.com/mziyabo/PSKubectlCompletion)

Install the module first

```ps1
Install-Module -Name PSKubectlCompletion
```

Write these lines to `$profile`

```ps1
Import-Module PSKubectlCompletion
Register-KubectlCompletion
```

[Github](https://github.com/tillig/ps-bash-completions)

This will also work

## Command Completion for dotnet

[Scott Hanselman's article](https://www.hanselman.com/blog/command-line-tab-completion-for-net-core-cli-in-powershell-or-bash)

[Microsoft Docs](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/register-argumentcompleter?view=powershell-7)

```ps1
Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock {
    param($commandName, $wordToComplete, $cursorPosition)
        dotnet complete --position $cursorPosition "$wordToComplete" | ForEach-Object {
           [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
        }
}
```

## Azure Powershell

```ps1
Install-Module -Name Az -AllowClobber -Scope AllUsers
```

## Remove redundant user name

```ps1
$env:POSH_SESSION_DEFAULT_USER = [System.Environment]::UserName
```

## Enable Posh Git

```ps1
$env:POSH_GIT_ENABLED = $true
```

## Profile Example

```ps1
Set-Alias k kubectl.exe
Set-Alias code code-insiders.cmd
Set-Alias which gcm
Set-Alias tig  'C:\Program Files\Git\usr\bin\tig.exe'
Set-Alias grep select-string
Import-Module posh-git
Import-Module oh-my-posh
Import-Module PSKubectlCompletion
Register-KubectlCompletion
# Import-Module Az # This will take a few seconds
Set-Theme Agnoster

Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock {
    param($commandName, $wordToComplete, $cursorPosition)
        dotnet complete --position $cursorPosition "$wordToComplete" | ForEach-Object {
           [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
        }
}
```
