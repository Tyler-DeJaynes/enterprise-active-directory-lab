# Troubleshooting journal

> Keep detailed records of real lab problems, even if they were intentionally introduced. Do not invent outcomes.

## Incident template

**Date:** Not recorded  
**Scenario:** What service or configuration stopped working?  
**Observed symptoms:** Exact error messages (redacted where needed)  
**Initial hypothesis:** What could cause the issue?  
**Tests performed:** Commands and actual observed results  
**Confirmed root cause:** Only after verifying the cause  
**Resolution:** Exact change made  
**Verification:** How did you prove the fix worked?  
**Lessons learned:** What will you remember for the next time?  
**Evidence:** Link to a genuine screenshot in `../screenshots/`

---

## Host prerequisite — Hardware virtualization disabled

**Date reported:** 2026-11-19  
**Status:** Resolved by user report; screenshot and VM boot test pending  
**Scenario:** Preparing Windows host for VirtualBox-based Active Directory lab.  
**Observed symptom:** User reported that virtualization is disabled. No firmware settings or screenshot have been verified yet.  
**Hardware identified:** AMD Ryzen 5 5600GT with Radeon Graphics (reported by user).  
**Likely cause to investigate:** CPU hardware virtualization / SVM Mode is disabled in the system firmware (BIOS/UEFI). This is a hypothesis, not a confirmed root cause.  
**Recommended action:** Enter UEFI settings via Windows advanced startup, locate AMD **SVM Mode** (exact path depends on motherboard manufacturer), enable it, and save changes without modifying unrelated firmware settings.  
**Verification reported:** User confirmed Windows virtualization now reads **Enabled** on 2026-11-19 and that VirtualBox's new VM wizard is open. Starting a 64-bit guest is still pending.  
**Evidence:** Not yet provided.  
**Resolution:** User reports virtualization was successfully enabled. Exact firmware steps and screenshot were not recorded; 64-bit VM boot not yet tested.

---

## Incident 01 — Pending

Not started. Suggested future practice: intentionally point CLIENT01 DNS to an incorrect address in the isolated lab, observe failed Active Directory name resolution, restore DC01 as the DNS server, and verify successful resolution. Do not claim this exercise is complete until you perform it.
