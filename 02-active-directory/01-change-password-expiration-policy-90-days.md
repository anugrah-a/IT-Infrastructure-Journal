# Change Password Expiration Policy to 90 Days

## Task Summary
Modified the domain password policy to set maximum password age to 90 days and verified user account settings related to password expiration.

---

## Environment
- Environment Type: Production
- Directory Service: Active Directory
- Server OS: Windows Server 2019

---

## Purpose
Increase domain password expiration period to **90 days** as per organizational security requirement.

---

## Pre-Requisites

- Domain administrative privileges
- Access to Domain Controller
- Group Policy Management Console (GPMC)

---

## Steps Performed

### 1. Modify Default Domain Password Policy

1. Opened **Group Policy Management (GPMC)**
2. Navigated to: Forest → Domains → domain.local
3. Edited **Default Domain Policy**
4. Navigated to:Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy
5. Modified:
- **Maximum Password Age** → Set to `90 days`
6. Applied and saved changes

---

### 2. Verify "Password Never Expires" Attribute

Checked if any users had password expiration disabled using PowerShell:

```powershell
Get-ADUser -Filter {PasswordNeverExpires -eq $true} `
-Properties PasswordNeverExpires |
Select-Object Name, SamAccountName
```
#### Review and Compliance Action

- Reviewed PowerShell output to identify accounts with **Password Never Expires** enabled.
- Ensured all applicable users comply with the new 90-day password expiration policy.
- Disabled **Password Never Expires** attribute for accounts where it was not required.

---

## Verification

Performed the following validation steps after policy modification:

- Confirmed **Maximum Password Age** updated in *Default Domain Policy*.
- Verified policy replication across Domain Controllers (in multi-DC environments).
- Confirmed test user reflects updated password expiration timeline.
- Reviewed user account attributes for compliance.
- Ensured no conflicting Fine-Grained Password Policies (FGPP) were applied.

---

## Rollback Procedure

If policy needs to be reverted:

1. Edit **Default Domain Policy**
2. Change **Maximum Password Age** to previous value
3. Run:

   ```powershell
   gpupdate /force
   ```

   ### Replication Verification

After modifying the password policy, ensured changes were replicated across all Domain Controllers.

- Verified Active Directory replication status
- Confirmed policy consistency across multiple DCs
- Ensured no replication failures were present

---

## Common Mistakes / Observations

- Modifying the wrong GPO instead of **Default Domain Policy**
- Forgetting replication delay between Domain Controllers
- Not checking for existing **Fine-Grained Password Policies (FGPP)**
- Overlooking accounts with **Password Never Expires** enabled
- Applying policy changes without proper authorization

---

## Key Learning

- Default Domain Policy password settings apply to all domain users.
- The **Password Never Expires** attribute overrides domain-level password policy.
- Fine-Grained Password Policies (FGPP) take precedence over Default Domain Policy when configured.
- Password policy changes require replication awareness in multi-DC environments.

---

## Related Concepts

- Default Domain Policy
- Fine-Grained Password Policy (FGPP)
- Active Directory Administrative Center (ADAC)
- Account Lockout Policy
- Group Policy Replication
