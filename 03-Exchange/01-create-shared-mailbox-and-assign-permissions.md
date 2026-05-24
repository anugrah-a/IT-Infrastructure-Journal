# Create Shared Mailbox and Assign User Permissions

## Task Summary
Create a new shared mailbox in Exchange and assign required user permissions (Full Access and Send As).

---

## Environment
- Environment Type: Production
- Directory Service: Active Directory
- Mail System: Microsoft Exchange
- Server OS: Windows Server 2019

---

## Purpose
Provision a shared mailbox based on customer requirements to allow multiple users to:

- Access common email communications
- Send emails on behalf of a department or function
- Maintain centralized mailbox management

---

## Pre-Requisites

- Administrative access to Exchange
- Approved mailbox name and alias
- Storage/database availability
- List of users requiring access

---

## Steps Performed

### 1. Create Shared Mailbox

1. Logged into **Exchange Control Panel (ECP)**
2. Navigated to: Recipients → Shared Mailboxes
3. Clicked **+ (Add)**
4. Entered required details:
- Display Name
- Alias
- Mailbox Database
- Storage configuration (if applicable)
5. Saved configuration

---

### 2. Assign Permissions

Configured mailbox permissions:

- **Full Access** → Allows users to open and manage mailbox
- **Send As** → Allows users to send emails as the shared mailbox

Assigned permissions to authorized users.

---

## Verification

- Confirmed shared mailbox appears in user Outlook (after sync)
- Verified users can:
- Open mailbox
- Send emails as shared mailbox
- Sent test email to confirm functionality

---

## Rollback Procedure

If mailbox creation needs reversal:

1. Remove assigned permissions
2. Disable or delete shared mailbox (as per policy)
3. Confirm mailbox removal from user Outlook

---

## Common Mistakes / Observations

- Assigning incorrect permissions (Full Access without Send As)
- Forgetting to wait for mailbox replication/synchronization
- Typographical errors in alias
- Not validating user access after configuration

---

## Key Learning

- Shared mailboxes improve collaboration and centralized communication.
- Proper permission assignment is critical for expected functionality.
- Role-based permission management is preferable to manual user configuration.

---

## Related Concepts

- Exchange mailbox provisioning
- Mailbox permissions (Full Access vs Send As vs Send on Behalf)
- Mailbox database management
- Exchange synchronization behavior
