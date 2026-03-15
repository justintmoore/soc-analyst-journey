# powershell-scripts

PowerShell scripts built during hands-on lab work and daily practice. Each script includes a header with purpose, requirements, and MITRE ATT&CK mapping where applicable.

All scripts are sanitized. No real company names, usernames, IPs, hostnames, or emails.

---

## Scripts

### `New-BulkADUsers.ps1`
Bulk-creates Active Directory user accounts in the `_EMPLOYEES` OU using randomized first/last name combinations. 
Use: Built for lab environments to simulate enterprise user provisioning and generate Event ID 4720 (account creation) events for SIEM testing and alert tuning.

- **Requirements:** AD DS, PowerShell 5.1+, Domain Admin, existing `_EMPLOYEES` OU
- **MITRE:** T1136.001 — Create Account: Local Account
- **Lab:** Azure Active Directory Lab (`homelab/homelab-ad-azure-lab.md`)




## Prior Lab Repos (Archived)
- [deploying-active-directory](https://github.com/justintmoore/deploying-active-directory)
- [active-directory-onpremise](https://github.com/justintmoore/active-directory-onpremise)
- [active-directory-group-policy](https://github.com/justintmoore/active-directory-group-policy)
