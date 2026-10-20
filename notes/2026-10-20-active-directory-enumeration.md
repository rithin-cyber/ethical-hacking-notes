# Day 14 — Active Directory Enumeration

## Objective
Identify Active Directory-related services and enumerate available domain information in the authorised TryHackMe Attacktive Directory lab.

## 1. Nmap Enumeration

**Command:**
```bash
nmap -sV -p 53,88,135,139,389,445,464,636,3268,3269 10.49.157.5
```

**Key findings:**
- Target IP: `10.49.157.5` (temporary lab IP)
- Hostname: `ATTACKTIVEDIREC`
- DNS domain: `spookysec.local`
- DNS: Port 53
- Kerberos: Port 88
- LDAP: Port 389
- SMB: Port 445
- Global Catalog LDAP: Port 3268
- Other Windows-related services were identified on ports 135, 139, 464, 636 and 3269.

**Conclusion:** The combination of services strongly indicates an Active Directory Domain Controller.

## 2. SMB Enumeration

**Command:**
```bash
enum4linux -a 10.49.157.5
```

**Findings:**
- Anonymous SMB session accepted.
- Domain reported: `THM-AD`
- Domain SID reported by the tool.
- User enumeration attempts returned `NT_STATUS_ACCESS_DENIED`.
- The tool did not successfully list shares.

**Share-listing command:**
```bash
smbclient -L //10.49.157.5 -N
```

**Result:** Anonymous login was successful, but no share names were returned.

**Conclusion:** An anonymous SMB session was accepted, but access to files or shares was not established by these results.

## 3. Kerberos Username Enumeration

**Tool:** Kerbrute v1.0.3

**Command:**
```bash
kerbrute userenum --dc 10.49.157.5 -d spookysec.local userlist.txt
```

**Results:**
- Candidate usernames tested: 73,317
- Valid entries reported: 16
- Distinct usernames after ignoring case variations: 8
- Domain used: `spookysec.local`

**Distinct usernames identified:**
- `james`
- `svc-admin`
- `robin`
- `darkstar`
- `administrator`
- `backup`
- `paradox`
- `ori`

**Conclusion:** The results identify recognised usernames; they do not establish passwords or privileges.

## 4. Security Relevance

- Exposed domain services provide information about the environment.
- Anonymous SMB sessions may warrant further authorised permission checks.
- Username enumeration may help an attacker identify valid accounts.
- Excessive privileges and improperly configured shares can increase security risk.

## 5. Remediation Considerations

- Restrict unnecessary network exposure.
- Review anonymous SMB access and share permissions.
- Apply least privilege to user and service accounts.
- Reduce unnecessary username disclosure.
- Monitor and investigate suspicious authentication attempts.

## 6. Limitations

- User enumeration did not reveal passwords.
- SMB share access was not demonstrated.
- No account compromise was established by these scans.
- The target IP may change when the lab is restarted.

## 7. Practice Record

- Scanned common Active Directory ports with Nmap.
- Enumerated SMB using enum4linux and smbclient.
- Enumerated usernames using Kerbrute.
- Recorded findings, limitations and remediation considerations.
