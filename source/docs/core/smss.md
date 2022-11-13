# smss.exe

`smss.exe` (Session Manager Subsystem alias Windows Session Manager), is responsible for creating new sessions. It 
is the first user-mode process started by the kernel.

This process starts the kernel and user modes of the Windows subsystem. This subsystem includes 
`win32k.sys` (kernel mode), `winsrv.dll` (user mode), and `csrss.exe` (user mode). 

`smss.exe` starts [csrss.exe](csrss.md) (Windows subsystem) and [wininit.exe](wininit.md) in Session 0, an isolated 
Windows session for the operating system, and [csrss.exe](csrss.md) and [winlogon.exe](winlogon.md) for Session 1, 
which is the user session. The first child instance creates child instances in new sessions, done by `smss.exe` copying 
itself into the new session and self-terminating. 

Any other subsystem listed in the `Required` value of 
`HKLM\System\CurrentControlSet\Control\Session Manager\Subsystems`is also launched. 

`smss.exe` is also responsible for creating environment variables and virtual memory paging files.

## Normal

* Image Path:  `%SystemRoot%\System32\smss.exe`
* Parent Process:  `System`
* Number of Instances:  One master instance and child instance per session. The child instance exits after creating the session.
* User Account:  Local System
* Start Time:  Within seconds of boot time for the master instance

## Unusual

* A different parent process other than System (PID `4`)
* The image path is different from `C:\Windows\System32`
* More than one running process. (children self-terminate and exit after each new session)
* The running `User` is not the `SYSTEM` user
* Unexpected registry entries for `Subsystem`

## Resources

* [Architecture of Windows NT](https://en.wikipedia.org/wiki/Architecture_of_Windows_NT)
* [Session Manager Subsystem](https://en.wikipedia.org/wiki/Session_Manager_Subsystem)
