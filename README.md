# Help Desk Command Cheat Sheet

A personal quick-reference of the Windows commands to diagnose and resolve common IT support issues across networking, system health, user accounts, printing, and Active Directory.

**Platforms:** Windows Command Prompt, PowerShell, Active Directory

## Skills Demonstrated

| Area | Tools |
|---|---|
| Network troubleshooting | `ipconfig`, `ping`, `tracert`, `nslookup`, `Test-NetConnection` |
| System diagnostics and repair | `systeminfo`, `tasklist`, `sfc`, `chkdsk` |
| Account support | `whoami`, `net user`, `Get-ADUser`, `Unlock-ADAccount` |
| Group Policy | `gpupdate`, `gpresult` |
| Services and printing | `Get-Service`, `Restart-Service` |
| Command-line scripting | Command Prompt and PowerShell |

## Contents

- [Troubleshooting Playbooks](#troubleshooting-playbooks)
- [CMD Networking](#cmd-networking)
- [CMD System and Files](#cmd-system-and-files)
- [PowerShell Services](#powershell-services)
- [PowerShell Networking](#powershell-networking)
- [Active Directory](#active-directory)

---

## Troubleshooting Playbooks

Common ticket types and the order I work through them.

### No Internet

Work from the local machine outward to isolate where the failure is.

```cmd
ipconfig /all
ping 127.0.0.1
ping DEFAULT-GATEWAY-IP
ping 8.8.8.8
nslookup google.com
```

| Step | Command | What a failure points to |
|---|---|---|
| 1 | `ipconfig /all` | Missing or incorrect IP (DHCP or adapter issue) |
| 2 | `ping 127.0.0.1` | Problem with the local network stack |
| 3 | `ping` the default gateway | Problem reaching the local router or switch |
| 4 | `ping 8.8.8.8` | Problem reaching the internet |
| 5 | `nslookup google.com` | DNS problem (internet works, names do not resolve) |

### Locked-Out User

```powershell
Get-ADUser -Identity username -Properties LockedOut
Unlock-ADAccount -Identity username
```

### Printer Not Working

```powershell
Get-Service -Name Spooler
Restart-Service -Name Spooler
```

### Group Policy Not Applying

```cmd
gpupdate /force
gpresult /r
```

---

## CMD Networking

| Command | Purpose |
|---|---|
| `ipconfig` | Show IP address and basic network configuration |
| `ipconfig /all` | Show full details, including MAC address, DNS, and DHCP |
| `ipconfig /release` | Release the current DHCP-assigned IP address |
| `ipconfig /renew` | Request a new IP address from the DHCP server |
| `ipconfig /flushdns` | Clear the local DNS resolver cache |
| `ping 8.8.8.8` | Test basic connectivity to an IP address |
| `tracert google.com` | Show each network hop to a destination |
| `nslookup google.com` | Test DNS resolution |
| `netstat -an` | Show active connections and listening ports |

## CMD System and Files

| Command | Purpose |
|---|---|
| `hostname` | Show the computer name |
| `whoami` | Show the current logged-in user |
| `systeminfo` | Show OS, hardware, and system details |
| `tasklist` | List running processes |
| `taskkill /IM notepad.exe /F` | Force-close a process |
| `sfc /scannow` | Scan and repair protected Windows system files |
| `chkdsk` | Check the disk and file system for errors |
| `gpupdate /force` | Force a Group Policy refresh |
| `gpresult /r` | Show Group Policy settings applied to the user and computer |
| `net user username` | Show details for a local user account |

---

## PowerShell Services

| Command | Purpose |
|---|---|
| `Get-Process` | List running processes |
| `Get-Service` | List services and their status |
| `Restart-Service -Name Spooler` | Restart the Print Spooler service |
| `Get-Help Get-Service` | Show help for any PowerShell command |

## PowerShell Networking

| Command | Purpose |
|---|---|
| `Test-NetConnection google.com` | Test connectivity to a destination |
| `Test-NetConnection google.com -Port 443` | Test connectivity to a specific TCP port |
| `Resolve-DnsName google.com` | Perform a DNS lookup |
| `Get-NetIPConfiguration` | Show IP configuration |
| `Get-NetAdapter` | Show network adapters and their status |

## Active Directory

> Requires the Active Directory PowerShell module (RSAT).

| Command | Purpose |
|---|---|
| `Get-ADUser -Identity username` | Look up a user account |
| `Get-ADUser -Identity username -Properties LockedOut` | Check whether an account is locked |
| `Unlock-ADAccount -Identity username` | Unlock an account |
| `Set-ADAccountPassword -Identity username -Reset -NewPassword (Read-Host -AsSecureString)` | Reset a user's password |
| `Get-ADGroupMember -Identity "GroupName"` | List members of a group |
| `Get-ADComputer -Identity "ComputerName"` | Look up a computer account |

---

**Troubleshooting approach:** start with the simplest check that can confirm or rule out a cause, then document what you tested and what the result was.
