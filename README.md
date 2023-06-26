# Task-Scheduler & AWS Alarm
**[1]** Notification for Disk Usage of EC2 instance
**[2]** Clearing temporary files for all users

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
* Give the path to your PowerShell script in```$scriptpath```:
```powershell
$scriptPath = "C:\Users\NikhilS\Desktop\DeleteTemporary.ps1"
```
*	Create new scheduled task action; ```$action``` variable represents an action that will execute PowerShell.exe with the necessary parameters to bypass the execution policy and run the specified PowerShell script:
```powershell
$action = New-ScheduledTaskAction -Execute "PowerShell.exe" -Argument "-ExecutionPolicy Bypass -File `"$scriptPath`""
```
* Create a trigger for task :
```powershell
$trigger = New-ScheduledTaskTrigger -Daily -AtStartup
```
-- Use ``` AtStartup``` to execute task at startup  
-- Use ```-Daily -At "09:00 AM"``` to execute task at specidied time. 

* Create a new scheduled task settings set which can be customized and passed to ```-Settings``` to configure additional  settings for the task: 
```powershell
$settings = New-ScheduledTaskSettingsSet
```
*Create a new Scheduled Task ,By providing these parameters; the New-ScheduledTask cmdlet creates a new scheduled task object (```$task```) with the specified action, trigger, and settings. This object represents the scheduled task that will be registered with the Task Scheduler:
```powershell
$task = New-ScheduledTask -Action $action -Trigger $trigger -Settings $settings
```
* Register the task with Task Scheduler:

```powershell
 Register-ScheduledTask -TaskName "TempDeleter" -TaskPath "\" -InputObject $Task -User "SYSTEM"

```

## Contributing

Pull requests are welcome. For major changes, please open an issue first
to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License

[MIT](https://choosealicense.com/licenses/mit/)
