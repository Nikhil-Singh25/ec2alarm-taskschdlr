# Task-Scheduler & AWS Alarm

Foobar is a Python library for dealing with word pluralization.

## Deleting Temporary files:

```powershell
$profilesPath = "C:\Users"
$users = Get-ChildItem -Path $profilesPath -Directory

foreach ($user in $users) {
    $tempPath = Join-Path -Path $user.FullName -ChildPath "AppData\Local\Temp"
    
    $files = Get-ChildItem -Path $tempPath -File -Recurse | Where-Object { -not $_.PSIsContainer }
    
    foreach ($file in $files) {
        try {
            $file | Remove-Item -Force -ErrorAction Stop
        } catch {
            Write-Host "Error deleting file: $($file.FullName)"
        }
    }
}

```
* The above scripts deletes temporary files present in **C:\Users\Users\AppData\Local\Temp**

* **NOTE:** Running the script and registering the task with the Register-ScheduledTask cmdlet requires administrative privileges to manage scheduled tasks on the system.

## Creating task  
* Give the path to your PowerShell script in``` $scriptpath```:

```powershell
$scriptPath = "C:\Users\NikhilS\Desktop\DeleteTemporary.ps1"

```
```powershell
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File `"$scriptPath`""
```
```powershell
$trigger = New-ScheduledTaskTrigger -Daily -AtStartup
```
```powershell
$settings = New-ScheduledTaskSettingsSet
```
```powershell
$task = New-ScheduledTask -Action $action -Trigger $trigger -Settings $settings
```
```powershell
 Register-ScheduledTask -TaskName "TempDeleter" -TaskPath "\" -InputObject $Task -User "SYSTEM"

```
```powershell

```

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)
