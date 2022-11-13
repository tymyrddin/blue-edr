# svchost.exe

The Service Host (Host Process for Windows Services), or `svchost.exe`, is responsible for hosting and managing 
Windows services.

The services running in this process are implemented as DLLs. The DLL to implement is stored in the registry 
for the service under the Parameters subkey in `ServiceDLL`. The full path is 
`HKLM\SYSTEM\CurrentControlSet\Services\SERVICE NAME\Parameters`.

There is always a key identifier in the binary path, and that identifier is `-k` . This is how a legitimate 
`svchost.exe` process is called. The `-k` parameter is for grouping similar services to share the same process. 
This concept was based on the OS design and implemented to reduce resource consumption. Starting from 
Windows 10 Version 1703, services grouped into host processes changed. On machines running more than 3.5 GB of 
memory, each service will run its own process. 

Since svchost.exe will always have multiple running processes on any Windows system, this process has been a target 
for malicious use. Adversaries create malware to masquerade as this process and try to hide amongst the legitimate 
`svchost.exe` processes. They can name the malware `svchost.exe` or misspell it slightly, such as `scvhost.exe`. 
By doing so, the intention is to go under the radar. Another tactic is to install/call a malicious service (DLL).  

## Normal

* Image Path: `%SystemRoot%\System32\svchost.exe`
* Parent Process: `services.exe`
* Number of Instances: Many
* User Account: Varies (SYSTEM, Network Service, Local Service) depending on the `svchost.exe` instance. In Windows 10, 
some instances run as the logged-in user.
* Start Time: Typically within seconds of boot time. Other instances of `svchost.exe` can be started after boot.

## Unusual

* A parent process other than `services.exe`
* Image file path other than `C:\Windows\System32`
* Subtle misspellings to hide rogue processes in plain sight
* The absence of the `-k` parameter

## Resources

* [svchost.exe](https://en.wikipedia.org/wiki/Svchost.exe)
* [The typographical and homomorphic abuse of svchost.exe, and other popular file names](https://www.hexacorn.com/blog/2015/12/18/the-typographical-and-homomorphic-abuse-of-svchost-exe-and-other-popular-file-names/)
