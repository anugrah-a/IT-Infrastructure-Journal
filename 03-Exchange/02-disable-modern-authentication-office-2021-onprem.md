# Disable Modern Authentication (ADAL) for Outlook – On-Prem Exchange (Office 2021)

## Task Summary
Disable Modern Authentication (ADAL) for Microsoft Outlook (Office 2021 perpetual license) in a fully on-premises Exchange environment to prevent repeated Microsoft account sign-in prompts.

---

## Environment
- Environment Type: Production
- Mail System: On-Premises Microsoft Exchange
- Client: Microsoft Outlook (Office 2021 – Perpetual License)
- Operating System: Windows 11

---

## Problem Description

In a fully on-premises Exchange environment:

- Outlook intermittently prompts users to sign in with a Microsoft account.
- Modern Authentication (ADAL) attempts to authenticate against cloud services.
- Users experience repeated and unnecessary login prompts.

Since the environment relies on:
- Windows Integrated Authentication (NTLM / Kerberos)

Modern Authentication causes authentication conflicts.

---

## Purpose

Ensure Outlook uses traditional authentication (NTLM/Kerberos) instead of Modern Authentication in a fully on-premises Exchange setup.

---

## Pre-Requisites

- Access to user's computer (remote)
- Administrative privileges (required for registry modification)

---

## Steps Performed

### 1. Disable Modern Authentication via Registry

Opened Registry Editor: Win + R → type regedit → Press Enter

Navigated to: HKEY_CURRENT_USER\Software\Microsoft\Office\16.0\Common\Identity

If the **Identity** key did not exist:
- Created it manually.

Added / Modified the following DWORD (32-bit) values:

| Name                         | Type       | Value |
|------------------------------|------------|--------|
| EnableADAL                   | REG_DWORD  | 0      |
| DisableADALatopWAMOverride   | REG_DWORD  | 1      |

Closed Registry Editor.

Restarted the computer.

---

### 2. Clear Stored Cloud Credentials

1. Opened **Control Panel → Credential Manager**
2. Selected **Windows Credentials**
3. Removed entries related to:
   - MicrosoftOffice
   - Outlook
   - ADAL
   - login.microsoftonline.com

Restarted Outlook.

---

## Verification

- Logged into Outlook using on-prem Exchange credentials
- Confirmed no Microsoft account sign-in prompt
- Verified mailbox synchronization successful

---

## Rollback Procedure

If Modern Authentication needs to be restored:

1. Change registry values:
   - EnableADAL = 1
   - DisableADALatopWAMOverride = 0
2. Restart system
3. Reconfigure Outlook if necessary

---

## Common Mistakes / Observations

- Entering incorrect registry path
- Creating incorrect DWORD type (must be 32-bit)
- Forgetting to restart system
- Not clearing cached cloud credentials
- Applying this in hybrid/cloud environments where Modern Auth is required

---

## Key Learning

- Office 2021 perpetual licenses in fully on-prem Exchange environments may conflict with Modern Authentication.
- Registry-based control can enforce legacy authentication behavior.
- Always evaluate environment type (On-Prem vs Hybrid vs M365) before modifying authentication settings.

---

## Related Concepts

- Modern Authentication (ADAL)
- Windows Integrated Authentication (NTLM / Kerberos)
- Outlook profile behavior
- Credential Manager
- Exchange authentication mechanisms
