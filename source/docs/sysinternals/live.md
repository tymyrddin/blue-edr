# Using Sysinternals Live

Sysinternals Live is a service that enables you to execute Sysinternals tools directly from the Web without hunting 
for and manually downloading them. Simply enter a tool's Sysinternals Live path into Windows Explorer or a command 
prompt as `live.sysinternals.com/<toolname>` or `\\live.sysinternals.com\tools\<toolname>`.

## Dependencies

And to make that work, the WebDAV client must be installed and running on the machine. On a Windows 10 client, the 
WebDAV client is installed but the client is most likely not running.

Check with:

    PS C:\WINDOWS\system32> get-service webclient

Start it with:

    PS C:\WINDOWS\system32> start-service webclient

Network Discovery needs to be enabled as well. This setting can be enabled in the Network and Sharing Center:

    PS C:\WINDOWS\system32> control.exe /name Microsoft.NetworkAndSharingCenter

Click on `Change advanced sharing settings` and select `Turn on network discovery` for your current network profile.

## Run the tool from the command line

    C:\Users\Administrator>\\live.sysinternals.com\tools\<toolname>

## Run the tool from a mapped drive

Create a network drive and run the tool from the mapped drive:

    C:\Users\Administrator>net use * \\live.sysinternals.com\tools\

The website is now browsable within the local machine.
