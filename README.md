# PowerShell-Automation-Case 

## Scenario 
XYZ company in Smithfield, New South Wales, Australia, is planning to automate repetitive tasks, time-consuming processes, error-pronte operations, and resource intensive operations. The following are what XYZ company plans to automate using PowerShell: 
1. Install Windows update
2. Find the top memory-consuming application (In this case, it is `Memory Compression`)
3. Terminate the most likely memory-consuming application (In this case, I chose software `ShareX`)
4. Generate backup data on Folder `Python - Project` to `Python - Project - Backup`

Your task is to develop the PowerShell script to reflect the requirements above. 

> You may use the Scheduled task(`taskschd.msc`) to automate a task to perform it at any time. For example, you may select "Daily" and set a time or choose another trigger. But in this analysis, we just develop the PowerShell command. 

## Solution

### 1. Install Windows Update 
> The name of the computer and some information are censored with ####.

```
PS C:\Users\####> Import-Module PSWindowsUpdate
PS C:\Users\####> Get-WindowsUpdate
```
This execution command will just show the windows updates without installing it. First, we load the module `Import-Module PSWindowsUpdate` into our PowerShell Session (Make sure we run it as an administrator). Then we check for available updates without installing them by using `Get-WindowsUpdate`. 

The output:

```
ComputerName Status     KB          Size Title
------------ ------     --          ---- -----
XXXX  -------    KB5034467   61MB 2024-01 Cumulative Update Preview for .NET Framework 3.5 and 4.8.1 for Window…
XXXX  -------    KB2267602  827MB Security Intelligence Update for Microsoft Defender Antivirus - KB2267602 (Ve…
XXXX  -------                46KB #### - System - 9/19/2017 12:00:00 AM - 11.7.0.1000
XXXX  -------                 6MB ##########
```

### 2. Find the top memory-consuming application 
> The name of directory is censored with ####.

```
# Get all running processes, sort them by descending memory usage, and select the top one
PS C:\Users\####> $topMemoryProcess = Get-Process | Sort-Object WorkingSet -Descending | Select-Object -First 1
PS C:\Users\####>
PS C:\Users\####> # Display the process name and memory usage
PS C:\Users\####> Write-Output "The process consuming the most memory is: $($topMemoryProcess.ProcessName)"
The process consuming the most memory is: Memory Compression
PS C:\Users\####> Write-Output "Memory Usage (KB): $($topMemoryProcess.WorkingSet / 1KB -as [int])"
Memory Usage (KB): 499856
```

Here, `Get-Process` retrieves all the processes running on the system. `Sort-Object WorkingSet-Descending` sorts their working set size in descending order (memory usage in physical RAM memory). `Select-Object-First 1` selects the first process from the sorted list. As of the time I published this project, the `Memory Compression` is the top memory-consuming application by using `Write-output` to display the name of the process (`$(stopMemoryProcess.ProcessName)`) and the memory usage (`$($StopMemoryProcess.WorkingSet / 1KB -as [int])`).  

Output: 

![pwsh_eiIvMbezvs](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/40c76f53-c378-4c81-a6a8-67f34a6d13b6)

![pwsh_UyKsp43OHM](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/81a3eb0f-5ed2-41a6-9e86-336f5caa6a68)

![pwsh_cVTz2betMm](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/90dc39a1-3471-43a2-8d1b-15231d8775c1)

![pwsh_JBFiP51lN4](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/b57ab241-2496-4837-aa50-35794f901b1d)


 
### 3. Terminate the most likely memory-consuming application (In this case, I chose software `ShareX` over `Memory compression` as an example only)
> The name of directory is censored with ####.

> Following the previous task, let's say, you find `ShareX` and would like to terminate it.

```
$processName = "ShareX"
Get-Process | Where-Object { $_.ProcessName -eq $processName } | Stop-Process
```
The `$processName = ShareX` variable will set the value to `ShareX` for the process name. The `Get-Process` will retrieve all processes currently running on the system. the pipe `|` will pass the output of the `Get-Process` to the next cmdlet. The `where-object` will filter the input based on specified criteria. 
This `$_.ProcessName` represents each item(process) in the input collection. The `-eq $processName` compares the property of `$_.ProcessName` to the value stored in the `$processName`. Therefore, the filter matches `ShareX`. 

Output: 

![Taskmgr_cmmmEJS0KM](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/4f484b13-d46e-47dd-be2b-c74430394be5)

![Taskmgr_viVt5aTjGn](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/068781ef-129e-4c03-a48e-2f5217e7573a)

### 4. Generate backup data on Folder `Python - Project` to `Python - Project - Backup`
> The name of directory is censored with ####.

```
PS C:\Users\####> $sourcePath = "C:\Users\wangs\Downloads\Python - Project"
PS C:\Users\####> $backupPath = "C:\Users\wangs\Desktop\Data_Backup_13-02-2024_to_14-02-2024"
PS C:\Users\####> Copy-Item -Path $sourcePath -Destination $backupPath -Recurse
```
The `$sourcePath` represents the location of the `Python - Project` folder while the `$backupPath` represents the destination folder for backup. The execution command to backup the data is `Copy-Item` with `-Recurse` to copy the entire directory. 

![pwsh_rMxK60MNyx](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/cb404958-1f3f-43c7-864f-62130d38c886)

![explorer_gE0TlTkf5X](https://github.com/Kwangsa19/PowerShell-Automation-Case/assets/135963482/0eb01812-fc37-4069-b56e-45308656669c)



