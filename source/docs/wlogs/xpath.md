# XPath queries

The W3C created XPath, or XML Path Language in full, to provide a standard syntax and semantics for addressing parts 
of an XML document and manipulating strings, numbers, and booleans. The Windows Event Log supports a subset of XPath 1.0.

1. Using `Get-WinEvent` and `XPath`, what is the query to find `WLMS` events with a System Time of `2020-12-15T01:09:08.940277500Z`?

Answer: `Get-WinEvent -LogName Application -FilterXPath '*/System/Provider[@Name="WLMS"] and */System/TimeCreated[@Name="SystemTime"]="2020-12-15T01:09:08.940277500Z"'`

2. Using `Get-WinEvent` and `XPath`, what is the query to find a user named `Sam` with a Logon Event ID of `4720`?

Answer: `Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="Sam" and */System/EventID=4720'`

3. How many results are returned?

Answer: `2`

4. What is the Message?

Answer: `A user account was created`

5. Still working with Sam as the user, what time was Event ID 4724 recorded? `MM/DD/YYYY H:MM:SS [AM/PM]`

```text
Get-WinEvent -LogName Security -FilterXPath '*/EventData/Data[@Name="TargetUserName"]="Sam" and */System/EventID=4724'
```

Answer: `12/17/2020 1:57:14 PM`

6. What is the Provider Name?

Answer: `Microsoft-Windows-Security-Auditing`