# PowerShell

---

* [Output installed Visual Studio Code extensions in human-readable form to a file](#af77792b-5035-4953-b82a-192b4df6f2ef)

---




<div id="af77792b-5035-4953-b82a-192b4df6f2ef">

## Output installed Visual Studio Code extensions in human-readable form to a file

</div>

Get-ChildItem "$env:USERPROFILE\.vscode\extensions" | ForEach-Object {
  $jsonPath = Join-Path $_.FullName "package.json"
  if (Test-Path $jsonPath) {
    $json = Get-Content $jsonPath -Raw | ConvertFrom-Json
    [PSCustomObject]@{
      "Human Readable Name" = $json.displayName
      "Internal Name / ID"  = "$($json.publisher).$($json.name)"
    }
  }
} | Format-Table -AutoSize > extensions.txt
