# PowerShell Tips

## Get all command history

```pwsh
cat (Get-PSReadlineOption).HistorySavePath
```