## Overview
During routine log monitoring, multiple failed authentication attempts were detected targeting aim.user@king.local.  
Between 19:16:14 and 19:16:38 on 04 March 2026, five consecutive failed attempts originated from **King-Client.king.local** via **interactive logon (Logon Type 2)**.  

The account was automatically locked out after five failed attempts, confirming the domain’s account lockout policy was triggered.  

This represents a **targeted brute-force attempt**.

## Investigation Steps

**Event Identification**  
   - Filtered Security logs on DC-KING for Event ID 4625 (Failed Logon) related to aim.user@king.local.  
   - Observed five failed attempts from King-Client.king.local.  
   - Confirmed account lockout with Event ID 4740 at 19:16:37.

**Data Extraction**  
   - Target Account: `aim.user@king.local`  
   - Source Workstation: King-Client.king.local  
   - Logon Type: 2 (Interactive)  
   - Failed Attempts: 5 within ~4 minutes  
   - Evidence: `4625-Failed-Logons.png`, `4740-Lockout.png`

**Timeline Reconstruction**

| Time           | Event ID | Description                                   |
|----------------|----------|-----------------------------------------------|
| 19:16:14       | 4625     | First failed logon attempt                    |
| 19:16:20       | 4625     | Second failed logon attempt                   |
| 19:16:25       | 4625     | Third failed logon attempt                    |
| 19:16:29       | 4625     | Fourth failed logon attempt                   |
| 19:16:33       | 4625     | Fifth failed logon attempt                    |
| 19:16:38       | 4740     | Account `aim.user` locked out                 |

## Analysis

- Only **one account** targeted → focused brute-force attempt.  
- All attempts came from the local workstation (Logon Type 2).  
- Rapid failures show importance of account lockout policies.  
- No successful logons detected → account remained secure.


## MITRE ATT&CK Mapping

- **Technique:** T1110 – Brute Force  
- **Tactic:** Credential Access  

## Impact Assessment

**Risk Level:** Medium  
- No compromise occurred, but activity indicates potential reconnaissance or early-stage intrusion.  



## Containment & Recommendations

1. Reset password for **aim.user**.  
2. Inspect **King-Client.king.local** for compromise or automated login scripts.  
3. Monitor Event ID 4625 spikes across other domain accounts.  
4. Configure SIEM alerts for repeated failed logons.  
5. Educate users on strong passwords and reporting suspicious activity.



## Evidence

- `4625-Failed-Logons.png` – sequence of failed logons  
- `4740-Lockout.png` – account lockout event  
