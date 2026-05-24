# SharePoint Online External Sharing & Guest Access Management

## Project Summary

Configured SharePoint Online external collaboration access for external users requiring document editing permissions and long-term access without repeated guest expiration interruptions.

The implementation included:

- External user access configuration
- Edit permission assignment
- Guest access policy modification
- External user group creation for permission management
- SharePoint sharing policy review

---

# Environment

- Platform: SharePoint Online
- Microsoft 365 Environment
- Role: Infrastructure Engineer / IT Support Intern

---

# Purpose

The client required external users to:

- Access shared SharePoint resources
- Edit and collaborate on documents
- Maintain uninterrupted access without frequent reapproval requests

Existing guest expiration policies were causing external users to lose access periodically.

---

# Pre-Requisites

- SharePoint Administrator access
- Microsoft 365 Admin access
- External user email addresses
- SharePoint site ownership permissions

---

# Initial Issue

External users experienced:

- Repeated guest access expiration
- Reapproval requests every few months
- Collaboration interruptions
- Limited document editing access

This affected operational workflow and external collaboration efficiency.

---

# Configuration Steps

---

# Step 1 – Verify SharePoint External Sharing Settings

1. Logged into:
   ```text
   Microsoft 365 Admin Center
   ```

2. Opened:
   ```text
   SharePoint Admin Center
   ```

3. Navigated to:
   ```text
   Policies → Sharing
   ```

4. Verified external sharing is enabled.

Configured sharing level as required:

- Existing Guests
- New and Existing Guests

---

# Step 2 – Configure Site-Level Sharing

1. Opened:
   ```text
   Active Sites
   ```

2. Selected target SharePoint site.
3. Opened:
   ```text
   Policies → Sharing
   ```

4. Enabled external sharing for the specific SharePoint site.

---

# Step 3 – Create External User Group

Created a dedicated security/permission group for external users.

Example:

```text
External_SharePoint_Users
```

Purpose:

- Simplify permission management
- Avoid assigning permissions directly to individual users
- Improve scalability and administration

Added all approved guest users to this group.

---

# Step 4 – Assign Edit Permissions

1. Opened target SharePoint library/folder.
2. Selected:
   ```text
   Manage Access
   ```

3. Assigned:
   ```text
   Edit Permission
   ```

4. Granted access to:
   ```text
   External_SharePoint_Users
   ```

5. Sent sharing invitations to guest users.

---

# Step 5 – Fix Guest Access Expiration

Navigated to:

```text
Policies → Sharing
```

Reviewed:

```text
Guest Expiration
```

Issue identified:

- Guest users required repeated reapproval approximately every 2 months.

Configuration change performed:

- Disabled Guest Expiration

This prevented repeated guest access interruptions.

---

# Validation

Performed validation checks:

- External users received invitation successfully
- Guest users accessed SharePoint resources
- Verified edit permissions functioning correctly
- Confirmed users can:
  - Upload files
  - Edit documents
  - Save changes
- Verified guest accounts remain active after policy modification

---

# Root Cause

Guest expiration policy and insufficient external permission structure caused recurring access interruptions for external collaborators.

---

# Resolution

- Enabled proper external sharing
- Assigned edit permissions using group-based access
- Modified guest expiration policy
- Validated long-term external collaboration functionality

---

# Security Considerations

Reviewed the following before implementation:

- Least privilege access
- External sharing scope
- Sensitive document exposure
- Permission inheritance
- Guest access governance

---

# Lessons Learned

- Group-based permission management simplifies administration.
- Guest expiration policies can disrupt external collaboration workflows.
- SharePoint external sharing requires both tenant-level and site-level configuration.
- External edit access should be carefully controlled and monitored.

---

# Common Mistakes to Avoid

- Assigning permissions directly to users
- Enabling anonymous sharing unnecessarily
- Ignoring tenant-level sharing restrictions
- Disabling guest expiration without business justification
- Not validating guest user experience after configuration

---

# Related Concepts

- SharePoint Online External Sharing
- Microsoft Entra ID Guest Users
- RBAC (Role-Based Access Control)
- SharePoint Permission Inheritance
- Microsoft 365 Collaboration Security
- B2B Guest Access

---

# Project Outcome

Successfully improved external collaboration by:

- Providing stable long-term guest access
- Reducing repeated approval requests
- Simplifying permission management using groups
- Enabling secure document collaboration for external users
