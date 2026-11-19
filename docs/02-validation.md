# Validation checklist

> Record **real test results** only after running each command. All entries begin as **Not tested**.

| ID | Verification | Command / method | Expected result | Actual result | Status |
|---|---|---|---|---|---|
| NET-01 | DC01 static network configuration | `ipconfig /all` (DC01) | IP 10.10.10.10/24 and intended DNS | Not tested | ⬜ |
| DNS-01 | Domain DNS resolution | `nslookup corp.example.test 10.10.10.10` | Response from DC01 DNS | Not tested | ⬜ |
| AD-01 | Domain is present | `Get-ADDomain` (DC01) | Domain corp.example.test | Not tested | ⬜ |
| CLI-01 | Client-to-DC communication | `ping 10.10.10.10` (CLIENT01) | Replies if ICMP is allowed | Not tested | ⬜ |
| CLI-02 | Domain login | `whoami` (CLIENT01, after login) | CORP\\testuser | Not tested | ⬜ |
| GPO-01 | Policy actually applies | `gpresult /r` (CLIENT01) | Intended GPO listed | Not tested | ⬜ |
| ACL-01 | Authorized share access | Open department share as permitted fictional user | Access granted | Not tested | ⬜ |
| ACL-02 | Unauthorized share denied | Open department share as unauthorized fictional user | Access denied | Not tested | ⬜ |

**Testing date:** Not yet tested

## Evidence capture template

For each successful test document:

- **Test ID:** 
- **Date:**
- **Command / GUI action:**
- **Expected:**
- **Observed:**
- **Conclusion:**
- **Screenshot:** `../screenshots/<filename>.png` (only after uploading the actual screenshot)

A ping timeout may simply mean Windows Firewall blocks ICMP; prioritize DNS, domain membership and actual domain sign-in checks. Redact all sensitive data before committing logs.
