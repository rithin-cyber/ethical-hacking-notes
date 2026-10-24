Lab environment: MARVEL.local, HYDRA-DC (192.168.57.105), JACKSPARROW (192.168.57.110), and Kali (192.168.57.102).



Enumeration: Domain users, privileged groups, password policy, and SMB shares.



Discovery: The ~test.lnk shortcut pointed to \\192.168.57.102\@test.png.



Attack path: Responder received NetNTLMv2 responses after LLMNR/NBT-NS poisoning.



Evidence and limitations: Record the commands and findings. The responses were captured, but no plaintext passwords were recovered or validated through this test.



Remediation: Disable LLMNR where appropriate, disable NBT-NS where feasible, enforce SMB signing where appropriate, and apply least privilege to privileged accounts.


Day 18 — Full Active Directory Lab
Date: 2026-10-24
Status: Practical lab completed; documentation prepared
Environment: Authorized, isolated home lab
1. Objective
Enumerate the lab Active Directory environment, identify potential security weaknesses, investigate an SMB shortcut, and observe whether name-resolution poisoning can cause a Windows client to send an NTLM challenge-response to a lab listener.
2. Lab environment
System	Hostname / Domain	IP address	Role
Domain controller	HYDRA-DC.MARVEL.local	192.168.57.105	Active Directory Domain Services and DNS
Windows client	JACKSPARROW	192.168.57.110	Domain-joined client
Kali Linux	Kali	192.168.57.102	Authorized test machine running Responder
Active Directory domain	MARVEL.local (NetBIOS: MARVEL)	—	Lab domain
Test account	MARVEL\labuser	—	Ordinary domain-user account


Kali's eth0 interface was up with 192.168.57.102/24.
3. Enumeration performed
Domain discovery and client membership
Commands used:
nltest /dsgetdc:MARVEL.local
nltest /domain_trusts
Findings:
- The client discovered HYDRA-DC.MARVEL.local at 192.168.57.105.
- nltest /domain_trusts listed MARVEL.local as the forest-root and primary domain. No additional domain trust was listed.
- The client was joined to MARVEL.local (PartOfDomain = True).
- %LOGONSERVER% returned \\HYDRA-DC for the lab-user session.
Users and groups
Commands used:
net user /domain
net group /domain
net group "Domain Admins" /domain
net group "Enterprise Admins" /domain
net group "Schema Admins" /domain
net group "Group Policy Creator Owners" /domain
whoami /all
Findings:
- Domain users included Administrator, fcastle, Guest, hawkeye, krbtgt, labuser, pparker, SQLService, and tstark.
- Domain Admins listed Administrator, hawkeye, SQLService, and tstark.
- Enterprise Admins, Schema Admins, and Group Policy Creator Owners each listed Administrator, SQLService, and tstark.
- labuser had ordinary user-level group membership in the observed whoami /all output and was not shown as a member of these privileged groups.
- The SQLService account comment contained a password-like string. The string is intentionally not copied into this report. Treat the exposure as a credential-handling weakness and rotate any affected lab credential.
- The same account was listed in several highly privileged groups, indicating excessive privilege in this lab configuration.
Password policy
Command used:
net accounts /domain
Observed policy:
- Minimum password length: 7 characters.
- Maximum password age: 42 days.
- Password history: 24 passwords.
- Account lockout threshold: Never.
- Lockout duration and observation window: 10 minutes each.
The lack of an account-lockout threshold is a potential weakness and should be reviewed against the lab's intended policy.
SMB share enumeration
Commands used:
net view /domain
net view \\HYDRA-DC
net view \\JACKSPARROW
dir \\HYDRA-DC\hackme
Findings:
- net view \\HYDRA-DC listed hackme, NETLOGON, and SYSVOL.
- The hackme share was readable by the test account and contained ~test.lnk.
- The shortcut properties showed:
  - Target: \\192.168.57.102\@test.png
  - Description: Test
- net view \\JACKSPARROW returned no shared resources.
- net view /domain returned system error 6118. This browsing failure did not prevent other domain queries from succeeding.
DNS checks
Commands used:
nslookup MARVEL.local
nslookup HYDRA-DC.MARVEL.local
Both names resolved to 192.168.57.105, although the queries displayed a timeout before returning the answer. This suggests DNS response timing or configuration warrants further checking; it does not alone prove DNS is nonfunctional.
4. Responder test and evidence
Kali's Responder was run on the isolated lab interface eth0. During troubleshooting, the configuration showed SMB = Off and HTTP = Off. After SMB was enabled and Responder restarted, a second terminal confirmed listeners on ports 139 and 445 bound to eth0:
sudo ss -lntp | grep -E ':(139|445)\b'
Responder then logged NBT-NS and LLMNR poisoned-answer events for traffic from 192.168.57.110. After the Windows client attempted to access the shortcut target, Responder logged NetNTLMv2 challenge-response events associated with:
- MARVEL\labuser
- MARVEL\king
Sensitive response material is intentionally excluded from this report. It was displayed during the lab session and should not be copied into notes, commits, screenshots, or a public portfolio.
What the evidence demonstrates
- Responder received LLMNR/NBT-NS name-resolution traffic from the Windows client and returned poisoned answers.
- The client subsequently sent NetNTLMv2 challenge-response material to the lab listener.
- This demonstrates a credential-exposure risk when name-resolution protocols and SMB authentication are susceptible to this type of redirection.
What the evidence does not demonstrate
- The captured response is not the plaintext password.
- No plaintext password was recovered by this test.
- The password-like text in the SQLService account comment was not validated by the failed runas attempts; Windows returned error 1326 (username or password incorrect).
- Capturing a challenge-response does not, by itself, prove account compromise or successful authentication as that account.
5. Issues and limitations encountered
- net view /domain failed with system error 6118, while other domain enumeration commands worked.
- nslookup showed timeouts despite returning the expected domain-controller IP.
- The initial shortcut access failed because the target was not providing SMB at that time. Responder's SMB service was then enabled, and the SMB listener was verified before the later capture.
- Responder emitted OSError: [Errno 98] Address already in use during the session, indicating a port or listener conflict may have existed. The later output nevertheless confirmed that ports 139 and 445 had listeners and challenge-response events were recorded.
6. Remediation recommendations
1. Disable LLMNR where it is not needed, using the organization's supported policy settings.
2. Disable NBT-NS where feasible and after checking for legacy dependencies.
3. Enforce SMB signing where appropriate and review SMB security settings across clients and servers.
4. Remove password-like secrets from account descriptions/comments immediately; rotate any credentials that may have been exposed.
5. Apply least privilege. Review why SQLService is listed in Domain Admins, Enterprise Admins, Schema Admins, and Group Policy Creator Owners; service accounts should not receive these rights without a documented necessity.
6. Configure a suitable account-lockout policy, balancing risk reduction with operational needs.
7. Review DNS and network-browsing configuration to understand the observed timeouts and error 6118.
8. Keep captured challenge-response material private, remove it from terminal transcripts/screenshots where possible, and do not publish it in a portfolio or repository.
7. Cleanup checklist
- [ ] Stop Responder with Ctrl+C.
- [ ] Confirm Responder is no longer listening on lab ports 139 and 445 if the service is not needed.
- [ ] Keep all tests limited to the isolated lab and authorized test accounts.
- [ ] Rotate lab credentials that may have been exposed and replace the example password-like string in account comments with a non-secret description.
- [ ] Do not include captured NetNTLMv2 response strings in this report or any shared evidence.
8. Conclusion
The lab successfully covered Active Directory enumeration, SMB share inspection, identification of an exposed password-like account comment and excessive service-account privileges, and a controlled demonstration in which Responder received NetNTLMv2 challenge-response material following LLMNR/NBT-NS poisoning. The challenge-response was not a plaintext password, and the test did not validate the displayed SQLService credential. The findings and limits are documented above along with remediation steps.
