# Azure Active Directory Lab

## Environment Overview
Two-VM Active Directory environment deployed in Azure to simulate a small enterprise domain. Domain Controller runs Windows Server 2025 and hosts AD DS, DNS, and DHCP. A Windows 10 client is joined to the domain and managed via Group Policy.

**VMs:**
- `DC-1` — Windows Server 2025, static private IP, DNS role, AD DS role
- `CLIENT-1` — Windows 10, DNS pointed at DC-1, domain-joined

**Domain:** `lab.local`
**Resource Group:** `Active-Directory-Lab`

---

## What Was Built

### Network
- Created Azure VNet (`AD-VNet`) with both VMs on the same subnet
- Set DC-1 NIC to static private IP so it can serve as authoritative DNS
- Pointed CLIENT-1 DNS to DC-1 static IP (not Azure default DNS)
- Verified connectivity via ping and `ipconfig /all`

### Active Directory
- Installed AD DS role via Server Manager
- Promoted DC-1 to Domain Controller, created new forest (`lab.local`)
- Configured DSRM password for recovery access
- Created OU structure:
  - `_EMPLOYEES` — standard user accounts
  - `_ADMINS` — privileged accounts

### Identity and Accounts
- Created domain admin account (`domain-admin`) and added to `Domain Admins` security group
- Bulk-provisioned 1,000 user accounts into `_EMPLOYEES` OU via PowerShell script
- See: `powershell-scripts/New-BulkADUsers.ps1`

### Client Management
- Joined CLIENT-1 to `lab.local` domain
- Verified CLIENT-1 appeared in ADUC under correct OU

### Group Policy
- Opened Group Policy Management Console (`gpmc.msc`) on DC-1
- Configured Account Lockout Policy at domain level:
  - Lockout threshold: 5 failed attempts
  - Lockout duration: 30 minutes
  - Reset counter: 30 minutes
- Forced GPO update on client remotely:
```powershell
  Invoke-GPUpdate -Computer "CLIENT-1" -Force
```

---

## Key Concepts Reinforced
- `gpedit.msc` = local machine only, not domain-aware
- `gpmc.msc` = domain-wide GPO management, requires RSAT
- Static IP on DC is required for stable DNS resolution across the domain
- Forest is the top-level security boundary; domains and OUs sit beneath it
- RSAT allows workstation-based management of server roles without logging directly into DC

---

## SOC / IAM Relevance
- Account lockout policy directly maps to detection of brute force (Event ID 4740)
- OU structure is how analysts contextualize which accounts should be doing what
- Bulk user provisioning mirrors real enterprise AD automation patterns
- Domain admin separation from standard accounts is a least-privilege enforcement baseline

---

## Prior Lab Repos (Archived)
- [deploying-active-directory](https://github.com/justintmoore/deploying-active-directory)
- [active-directory-onpremise](https://github.com/justintmoore/active-directory-onpremise)
- [active-directory-group-policy](https://github.com/justintmoore/active-directory-group-policy)
