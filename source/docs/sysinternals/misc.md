# Miscellaneous

## BgInfo

"It automatically displays relevant information about a Windows computer on the desktop's background, such as the computer name, IP address, service pack version, and more." (official definition) 

## RegJump

"This little command-line applet takes a registry path and makes Regedit open to that path. It accepts root keys in standard (e.g. HKEY_LOCAL_MACHINE) and abbreviated form (e.g. HKLM)." (official definition) 

## Strings

"Strings just scans the file you pass it for UNICODE (or ASCII) strings of a default length of 3 or more UNICODE (or ASCII) characters. Note that it works under Windows 95 as well." (official definition) 

### Question

Run the Strings tool on `ZoomIt.exe`. What is the full path to the `.pdb` file? 

    C:\Tools\sysint>strings .\zoomit.exe | findstr /i .pdb