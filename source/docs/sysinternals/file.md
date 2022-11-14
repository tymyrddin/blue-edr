# File and disk utilities

## Sigcheck

"[Sigcheck](https://learn.microsoft.com/en-us/sysinternals/downloads/sigcheck) is a command-line utility that shows 
file version number, timestamp information, and digital signature 
details, including certificate chains. It also includes an option to check a file’s status on VirusTotal, a site 
that performs automated file scanning against over 40 antivirus engines, and an option to upload a file for scanning." 
(official definition).

Example:

    sigcheck -u -e c:\windows\system32

And investigate the purpose of any files that are not signed.

## Streams

"The NTFS file system provides applications the ability to create 
[alternate data streams](https://learn.microsoft.com/en-us/sysinternals/downloads/streams) of information. By default, 
all data is stored in a file's main unnamed data stream, but by using the syntax `file:stream`, you are able to read 
and write to alternates." (official definition)

Alternate Data Streams (ADS) is a file attribute specific to Windows NTFS (New Technology File System). Every file 
has at least one data stream (`$DATA`) and ADS allows files to contain more than one stream of data. Natively Window 
Explorer doesn't display ADS to the user. There are 3rd party executables that can be used to view this data, but 
Powershell gives you the ability to view ADS for files.

Malware writers have used ADS to hide data in an endpoint, but not all its uses are malicious. When you download a 
file from the Internet unto an endpoint, there are identifiers written to ADS to identify that it was downloaded 
from the Internet.

### Question

There is a txt file on the desktop named `file.txt`. Using one of the three tools, what is the text within the ADS?

Open a `cmd`:

    C:\Users\Administrator>cd desktop
    C:\Users\Administrator>stream file.txt

There is an `ads.txt` inside.

    C:\Users\Administrator>notepad file.txt:ads.txt

Answer: I am hiding in the stream.

## SDelete

"[SDelete](https://docs.microsoft.com/en-us/sysinternals/downloads/sdelete) is a command line utility that takes a 
number of options. In any given use, it allows you to delete one or more files and/or directories, or to cleanse the 
free space on a logical disk." 

SDelete (Secure Delete) implements the 
[DOD 5220.22-M (Department of Defense clearing and sanitizing protocol)](https://www.lifewire.com/dod-5220-22-m-2625856) 
and has been used by adversaries and is associated with MITRE techniques 
[T1485 (Data Destruction)](https://attack.mitre.org/techniques/T1485/) and 
[T1070.004 (Indicator Removal on Host: File Deletion)](https://attack.mitre.org/techniques/T1070/004/) in 
[MITRE ID S0195](https://attack.mitre.org/software/S0195/).




