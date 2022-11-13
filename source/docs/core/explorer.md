# explorer.exe

Windows Explorer, `explorer.exe`, gives the user access to their folders and files. It also provides functionality 
for other features, such as the Start Menu and Taskbar.

The Winlogon process runs `userinit.exe`, which launches the value in 
`HKLM\Software\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell`. 

`userinit.exe` exits after spawning `explorer.exe`. Because of this, the parent process is non-existent. 

There will be many child processes for `explorer.exe`.

## Normal

* Image Path:  `%SystemRoot%\explorer.exe`
* Parent Process:  Created by `userinit.exe` and exits
* Number of Instances:  One or more per interactively logged-in user
* User Account:  Logged-in user(s)
* Start Time:  First instance when the first interactive user logon session begins

## Unusual

* An actual parent process. (`userinit.exe` calls this process and exits)
* Image file path other than `C:\Windows`
* Running as an unknown user
* Subtle misspellings to hide rogue processes in plain sight
* Outbound TCP/IP connections
