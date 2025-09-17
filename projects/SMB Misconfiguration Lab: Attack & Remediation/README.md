# SMB Misconfiguration Lab: Attack & Remediation

## Overview
This lab demonstrates how weak SMB configurations and poor password policies can be exploited, and how proper remediation makes brute force infeasible.  
The goal was not only to exploit the misconfiguration, but also to **apply defensive measures** (password policy, lockout policy, firewall rules) to harden the system.

---

## Lab Setup
- **Platform:** Hyper-V  
- **Attacker VM:** Kali Linux  
- **Target VM:** Windows Server 2022 (Domain Controller)  
- **Services:** SMB (port 445), Active Directory  

---

## Attack Phase
- **SMB Brute Force:** Used `smb_login` module in Metasploit to discover weak credentials.  
- **NTLM Hash Dump:** Used `psexec` exploit to gain a Meterpreter shell and dump NTLM hashes.  
- **Hash Cracking:** Leveraged Hashcat to crack NTLM hashes using:
  - Dictionary (`rockyou.txt`) + mask ('?d?d?d') hybrif attack

**Key Finding:** Passwords like `Abcdef123` that are common human patterns were easily cracked in seconds.

---

## Defense Phase
- **Password Policy:** Enforced 12+ character minimum, complexity enabled.  
- **Account Lockout:** Configured lockout threshold and duration to prevent repeated brute force attempts.  
- **Firewall Hardening:** Restricted SMB access to domain-only profile, disabling public exposure.  

**Result:**  
When retesting, brute force attempts were blocked by lockout policy, and 12+ character passwords were infeasible to crack (estimated ~150,000 years at 108 GH/s).

---

## Lessons Learned
- Weak passwords, even when following Windows complexity (e.g., `Abcdef123`), are trivial with modern GPU cracking.  
- Enforcing **length + complexity** drastically raises the cost of attack.  
- Pairing password policy with **lockout + restricted SMB rules** provides layered defense.

---

## Screenshots
📷 Include:
- Successful nmap port scan (0-500) to confirm port 445 was open and available
- Successful brute force (Metasploit)
- SMBClient listing share + file upload to prove read/write access
- SMB Psexec exploit result (Metasploit
- NTLM hash dump (Meterpreter Reverse Tcp)
- Hashcat cracking short password  
- Hashcat infeasible time estimate for 12+ password  
- Windows Group Policy settings (before & after)

---

## Conclusion
This lab demonstrates how a **misconfigured SMB service** can expose an organization to credential-based attacks. With proper remediation, the same attack becomes impractical, showcasing the importance of defense-in-depth.

---

## Key Skills Demonstrated
- SMB enumeration & exploitation (Metasploit, smbclient)
- Privilege escalation & hash dumping (Meterpreter, NTLM)
- Password cracking (Hashcat, dictionary & mask attacks)
- Windows hardening (password policy, lockout policy, firewall rules)
- Security reporting & documentation
