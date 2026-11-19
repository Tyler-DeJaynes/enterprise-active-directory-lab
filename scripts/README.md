# PowerShell automation (planned)

> No PowerShell automation has been implemented or tested yet.

This folder is reserved for scripts we will write and execute on **fictional lab accounts only**:

- `New-LabUsers.ps1` — Read a CSV of fictional employee records; validate entries; create lab user accounts and assign department groups.
- `Disable-LabUser.ps1` — Safely disable an identified fictional user and record the lab action.
- `Get-LabAudit.ps1` — Report selected AD users, groups and membership information.

Each script will include:

1. Explanation of the purpose and prerequisites
2. Parameters or CSV schema and **fake** example inputs
3. Validation and error handling
4. A safe testing approach (prefer `-WhatIf` for supported modifying commands)
5. Sanitized output proving a successful test

**Do not** store passwords, credentials, tokens, secrets, or any real company's employee data in this repository. We will write the scripts after AD DS is running.
