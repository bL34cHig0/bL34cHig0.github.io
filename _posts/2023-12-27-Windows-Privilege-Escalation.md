---
title: Windows Privilege Escalation
date: 2023-12-27 
categories: [Notes, Windows, Privilege Escalation]
tags: [windows,privilege escalation,manual checks,enumeration,]
---

![Windows](https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/1df7b8d9-3471-49df-8745-44ddebbade6e/d1gdhfz-bc79238c-ed39-485e-aab4-24f9958d5243.jpg/v1/fit/w_800,h_500,q_70,strp/windows_splash_by_zaif06_d1gdhfz-414w-2x.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7ImhlaWdodCI6Ijw9NTAwIiwicGF0aCI6IlwvZlwvMWRmN2I4ZDktMzQ3MS00OWRmLTg3NDUtNDRkZGViYmFkZTZlXC9kMWdkaGZ6LWJjNzkyMzhjLWVkMzktNDg1ZS1hYWI0LTI0Zjk5NThkNTI0My5qcGciLCJ3aWR0aCI6Ijw9ODAwIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmltYWdlLm9wZXJhdGlvbnMiXX0.U_r4sVOR_i_On-TXkEzu17H5NwQwfG5YiFlm78xNhtw){: .center-image }

# Manual Checks

A list of useful commands to perform manual privilege escalation on `Windows` operating system. 

## Windows Version and Configuration

> Get system info

```terminal
systeminfo | findstr /B /C:"OS Name" /C:"OS Version"
```

> List all env variables

```terminal
set
```

```powershell
Get-ChildItem Env: | ft Key,Value
```

> List all drives

```powershell
Get-PSDrive | where {$_.Provider -like "Microsoft.PowerShell.Core\FileSystem"}| ft Name,Root
```

## User Enumeration

> Get current username

```terminal
echo %USERNAME% || whoami
```

```powershell
$env:username
```

> List user privilege

```terminal
whoami /priv
```

```terminal
whoami /groups
```

> List all users

```terminal
net user
```

```terminal
whoami /all
```

```powershell
Get-LocalUser | ft Name,Enabled,LastLogon
```

```powershell
Get-ChildItem C:\Users -Force | select Name
```

> Get details about a specific user i.e administrator

```terminal
net <user-name>
```

> List all local groups

```terminal
net localgroup
```

```powershell
Get-LocalGroup | ft Name
```

> Get details about a group i.e. administrators

```terminal
net localgroup administrators
```

```powershell
Get-LocalGroupMember Administrators | ft Name, PrincipalSource
```

> Get Domain Controller

```powershell
nltest /DCLIST:DomainName
```

```powershell
nltest /DCNAME:DomainName
```

## Network Enumeration

> List all network interfaces, IP, and DNS

```terminal
ipconfig /all
```

```powershell
Get-NetIPConfiguration | ft InterfaceAlias,InterfaceDescription,IPv4Address
```

```powershell
Get-DnsClientServerAddress -AddressFamily IPv4 | ft
```

> List current routing table

```powershell
route print
```

```powershell
Get-NetRoute -AddressFamily IPv4 | ft DestinationPrefix,NextHop,RouteMetric,ifIndex
```

> List ARP table

```powershell
arp -A
```

```powershell
Get-NetNeighbor -AddressFamily IPv4 | ft ifIndex,IPAddress,LinkLayerAddress,State
```

> List all current connections

```powershell
netstat -ano
```

> List all network shares

```terminal
net share
```

## IIS Web config

```powershell
Get-Childitem –Path C:\inetpub\ -Include web.config -File -Recurse -ErrorAction SilentlyContinue
```

## WiFi Passwords

> Find Access Point (AP) SSID

```terminal
netsh wlan show profile
```

> Get Cleartext Pass

```terminal
netsh wlan show profile <SSID> key=clear
```
