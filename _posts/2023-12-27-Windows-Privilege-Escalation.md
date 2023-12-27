---
title: Windows Privilege Escalation
date: 2023-12-27 
categories: [Notes, Windows, Privilege Escalation]
tags: [windows,privilege escalation,manual checks,enumeration,]
---

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
