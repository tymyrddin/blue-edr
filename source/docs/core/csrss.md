# csrss.exe

`csrss.exe` (Client Server Runtime Process) is the user-mode side of the Windows subsystem. This process is always 
running and is critical to system operation. If this process is terminated by chance, it will result in system failure. 
This process is responsible for the `Win32` console window and process thread creation and deletion. For each instance, 
`csrsrv.dll`, `basesrv.dll`, and `winsrv.dll` are loaded (along with others). 

This process is also responsible for making the Windows API available to other processes, mapping drive letters, and 
handling the Windows shutdown process. 

## Normal

* Image Path: `%SystemRoot%\System32\csrss.exe`
* Parent Process:  Created by an instance of `smss.exe`
* Number of Instances:  Two or more
* User Account:  `Local System`
* Start Time:  Within seconds of boot time for the first two instances (for Session 0 and 1). Start times for 
additional instances occur as new sessions are created, although only Sessions 0 and 1 are often created.

## Unusual

* An actual parent process. (`smss.exe` calls this process and self-terminates)
* Image file path other than `C:\Windows\System32`
* Subtle misspellings to hide rogue processes masquerading as `csrss.exe` in plain sight
* The user is not the `SYSTEM` user.

## Resources

* [Client/Server Runtime Subsystem](https://en.wikipedia.org/wiki/Client/Server_Runtime_Subsystem)
