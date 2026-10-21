# Active Directory Weaknesses — TryHackMe Lab

**Date:** 2026-10-21  
**Target:** Attacktive Directory  
**Domain:** `spookysec.local`  
**NetBIOS domain:** `THM-AD`  
**Scope:** Authorized TryHackMe lab

## 1. AS-REP Roasting

### Evidence
- Used Impacket `GetNPUsers` to test the enumerated usernames.
- The account `svc-admin` returned an AS-REP hash.
- The hash began with `$krb5asrep$23$svc-admin@SPOOKYSEC.LOCAL`.
- Hashcat mode `18200` recovered the password using the RockYou wordlist.

### Security impact
The account's Kerberos pre-authentication configuration allowed authentication data to be obtained for offline password cracking. The password was recoverable with the tested wordlist.

### Remediation
- Enable Kerberos pre-authentication unless a documented exception is required.
- Use a long, unique, strong service-account password or a managed service account where appropriate.
- Review the account's permissions and group memberships.

## 2. Sensitive File and Credential Exposure

### Evidence
- Authenticated to SMB as `svc-admin`.
- Enumerated the `backup` share.
- Downloaded `backup_credentials.txt`.
- Decoded the file's Base64 content and identified credentials for the `backup` account.
- Successfully authenticated as `backup` when specifying the NetBIOS domain `THM-AD`.

**Note:** The recovered password is intentionally omitted from this report.

### Security impact
A file accessible through the SMB share exposed another account's credentials. Base64 is encoding, not encryption, so anyone who obtains the file can decode its contents without a secret key.

### Remediation
- Remove exposed credentials from accessible files.
- Rotate credentials that have been exposed.
- Restrict SMB share permissions using least privilege.
- Store secrets in an approved secrets-management solution.

## 3. Findings and Limitations

### Confirmed
- An AS-REP hash was obtained for `svc-admin`.
- The captured hash was successfully cracked using the tested wordlist.
- `svc-admin` could access and download the credentials file from the `backup` share.
- The recovered `backup` credentials successfully authenticated when the domain was specified.

### Not yet confirmed
- Whether either account belongs to `Domain Admins`.
- Whether either account has other elevated privileges.
- What files or resources the `backup` account can access beyond the tests documented above.

## 4. Lessons Learned

- Authentication success does not establish administrative privileges.
- A crackable password does not automatically prove excessive privileges.
- Base64 does not protect passwords or other sensitive information.
- Findings should clearly separate observed evidence, demonstrated impact, and unverified assumptions.
- Never include recovered passwords or other secrets in a public write-up.
