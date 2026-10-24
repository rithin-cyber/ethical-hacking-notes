Lab environment: MARVEL.local, HYDRA-DC (192.168.57.105), JACKSPARROW (192.168.57.110), and Kali (192.168.57.102).



Enumeration: Domain users, privileged groups, password policy, and SMB shares.



Discovery: The ~test.lnk shortcut pointed to \\192.168.57.102\@test.png.



Attack path: Responder received NetNTLMv2 responses after LLMNR/NBT-NS poisoning.



Evidence and limitations: Record the commands and findings. The responses were captured, but no plaintext passwords were recovered or validated through this test.



Remediation: Disable LLMNR where appropriate, disable NBT-NS where feasible, enforce SMB signing where appropriate, and apply least privilege to privileged accounts.
