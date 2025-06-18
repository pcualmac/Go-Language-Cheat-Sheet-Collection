Here are Windows equivalents for the Go test coverage commands:

### 1. As a batch file command (save as `gocover.bat`):
```batch
@echo off
go test -coverprofile=coverage.out
if %errorlevel% equ 0 (
    go tool cover -html=coverage.out
    del coverage.out
)
```

### 2. As a PowerShell function (add to your `$PROFILE`):
```powershell
function gocover {
    go test -coverprofile=coverage.out
    if ($?) {
        go tool cover -html=coverage.out
        Remove-Item coverage.out
    }
}
```

### 3. As a CMD alias (add to your command prompt):
```cmd
doskey gocover=go test -coverprofile=coverage.out $T go tool cover -html=coverage.out $T del coverage.out
```

### Key differences from Linux/Mac:
1. Uses `del` instead of `rm` (CMD) or `Remove-Item` (PowerShell)
2. Error handling is explicit (`%errorlevel%` or `$?`)
3. The HTML report will open in your default browser automatically
4. The coverage file is cleaned up after viewing

### To use:
1. For the batch file version:
   - Save as `gocover.bat` in a directory in your PATH
   - Run with just `gocover`

2. For PowerShell version:
   - Add to your profile (`notepad $PROFILE`)
   - Run `gocover` in any PowerShell session

3. For CMD version:
   - Add to your `AutoRun` registry key or run manually each session
   - Run `gocover` in CMD