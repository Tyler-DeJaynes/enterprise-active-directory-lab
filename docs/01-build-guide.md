# Step-by-step lab build guide

> **Status: Planned.** Update dates, evidence, and outcome only after you personally complete and verify the work.

## Phase 1 — Prepare your Windows host

- **Date:** Not yet started
- **Host OS:** To be recorded
- **RAM:** 16 GB DDR4-3200
- **Free storage:** To be recorded
- **Virtualization:** Not yet verified

1. Open **Task Manager → Performance → CPU** and confirm **Virtualization: Enabled**. If disabled, enable Intel VT-x or AMD SVM in BIOS/UEFI.
2. Download [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads) for Windows hosts and install it.
3. Download the [Windows Server 2025 evaluation ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2025) and [Windows 11 Enterprise evaluation ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-11-enterprise).
4. Aim for at least 100 GB of free host disk space. Do not upload the ISO or VM disks to GitHub.

**Checkpoint:** VirtualBox installed, required ISOs available, virtualization enabled.

**Outcome:** Not tested  
**Evidence:** Add screenshot and description after successful installation.

## Phase 2 — Shared VirtualBox NAT Network

In VirtualBox, open **Tools → Network → NAT Networks** (menu names can vary by version) and create:

- Name: `ADLAB-NAT`
- IPv4 CIDR: `10.10.10.0/24`
- DHCP: Disabled
- Gateway: Verify the NAT gateway address; `10.10.10.1` is the planned value
- Inbound port forwarding: None

Attach **both** VM network adapters to **NAT Network** named `ADLAB-NAT` (not the individual default `NAT` adapter).

### Planned VM resources

| VM | OS | RAM | vCPUs | Disk | Network |
|---|---|---:|---:|---:|---|
| DC01 | Windows Server 2025 Standard Evaluation (Desktop Experience) | 3072 MB | 2 | 60 GB dynamic | ADLAB-NAT |
| CLIENT01 | Windows 11 Enterprise Evaluation | 4096 MB | 2 | 64 GB dynamic | ADLAB-NAT |

For Windows 11, configure virtual TPM 2.0, EFI and Secure Boot as needed to meet official requirements. Do not bypass the OS hardware requirements.

**Outcome:** Not tested

## Phase 3 — Install and configure DC01

1. Create the DC01 virtual machine with the Windows Server ISO, 3 GB RAM, 2 vCPUs, and 60 GB dynamically allocated virtual disk.
2. Install **Desktop Experience** and create a private, strong local Administrator password.
3. Rename the computer to `DC01` and restart.
4. In the network adapter IPv4 properties, assign:
   - Address: `10.10.10.10`
   - Mask: `255.255.255.0`
   - Gateway: Verify first; planned `10.10.10.1`
   - Preferred DNS: `10.10.10.10`
5. In **Server Manager → Manage → Add Roles and Features**, install **Active Directory Domain Services**.
6. Choose **Promote this server to a domain controller → Add a new forest**. Root domain name: `corp.example.test`. Include DNS.
7. Select a strong, private Directory Services Restore Mode (DSRM) password; never record it in this repository.
8. Complete the wizard and reboot. Log in as your domain administrator.

### Validation (run after deployment)

In PowerShell **on DC01**:

```powershell
ipconfig /all
Get-ADDomain
nslookup corp.example.test 10.10.10.10
```

Expected: DC01 has the configured network information; AD reports `corp.example.test`; the domain resolves through its DNS service. Add *actual* sanitized results to [validation](02-validation.md).

**Outcome:** Not tested

## Phase 4 — Install and join CLIENT01

1. Create a second VM using the Windows 11 Enterprise ISO, 4 GB RAM, 2 vCPUs, 64 GB dynamically allocated disk, and the **same NAT Network**.
2. Finish Windows setup, rename the computer `CLIENT01`, and restart.
3. Configure its network adapter:
   - Address: `10.10.10.20`
   - Mask: `255.255.255.0`
   - Gateway: Verify first; planned `10.10.10.1`
   - **Preferred DNS: `10.10.10.10` only**
4. Test `nslookup corp.example.test 10.10.10.10`; optionally `ping 10.10.10.10` (ICMP may be blocked).
5. Open `sysdm.cpl` → **Computer Name → Change → Domain** and enter `corp.example.test`.
6. Authenticate with authorized test-domain credentials and restart.
7. Confirm a successful domain sign-in and that CLIENT01 appears in Active Directory Users and Computers.

**Outcome:** Not tested

## Future phases

For every remaining milestone, add a dated section with **objective → actions → commands → evidence → verification → lessons learned**:

- AD OUs and fictional employee accounts
- Security groups and least-privilege permissions
- Group Policy creation and `gpresult /r` validation
- Shared folders with NTFS and share access rules
- PowerShell test-account onboarding/offboarding
- Troubleshooting a deliberately broken configuration and documenting its fix

Never mark a milestone complete unless you've actually performed it. Never commit real credentials, personal data, ISO images, virtual disks, or secret outputs.
