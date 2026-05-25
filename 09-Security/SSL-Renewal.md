# Exchange Server 2019 SSL Certificate Renewal Documentation

## Overview

This document describes the complete process for renewing and configuring a SAN (Subject Alternative Name) SSL certificate for Microsoft Exchange Server 2019 and related services.

The certificate is used for:

* Microsoft Exchange Server 2019
* Outlook Web Access (OWA)
* Autodiscover
* VPN Service
* Web Services
* Additional HTTPS-based services

---

# Environment Details

| Component              | Value                              |
| ---------------------- | ---------------------------------- |
| Exchange Version       | Exchange Server 2019               |
| Primary Domain (CN)    | mail.company.com.au           |
| Certificate Type       | SAN / Multi-Domain SSL Certificate |
| Key Size               | 2048                               |
| Exportable Private Key | Enabled                            |

---

# Certificate Domains

## Primary Common Name (CN)

```text
mail.company.com.au
```

## Subject Alternative Names (SAN)

```text
autodiscover.company.com.au
company.com.au
vpn.company.com.au
www.company.com.au
```

---

# Prerequisites

Before starting:

* Ensure Exchange Management Shell is opened as Administrator
* Verify DNS records for all domains
* Ensure certificate provider supports SAN certificates
* Ensure backup of existing certificate is available

---

# Step 1 – Verify Existing Exchange Certificate

Run the following command in Exchange Management Shell:

```powershell
Get-ExchangeCertificate | fl FriendlyName,Thumbprint,NotAfter,Services,CertificateDomains
```

Verify:

* Current certificate expiry date
* Existing SAN entries
* Services assigned to certificate

---

# Step 2 – Create CSR Folder

```powershell
mkdir C:\CSR
```

---

# Step 3 – Generate Certificate Signing Request (CSR)

Run the following command in Exchange Management Shell:

```powershell
New-ExchangeCertificate `
-GenerateRequest `
-FriendlyName "SSLCert2026" `
-SubjectName "C=AU, O=company, CN=mail.company.com.au" `
-DomainName mail.company.com.au,autodiscover.company.com.au,company.com.au,netbook.company.com.au,vpn.company.com.au,www.company.com.au `
-PrivateKeyExportable $true `
-KeySize 2048 `
-Path "C:\CSR\exchange2026.csr"
```

Output:

```text
C:\CSR\exchange2026.csr
```

---

# Step 4 – Submit CSR to Certificate Authority

Upload the generated CSR file to the SSL provider portal.

Supported providers:

* DigiCert
* GoDaddy
* SSL2BUY
* Sectigo

During submission:

* Select SAN / Multi-Domain SSL Certificate
* Verify all SAN entries are included
* Complete domain validation process

---

# Step 5 – Download Certificate Files

After successful validation:

Download:

* Primary Certificate (.cer)
* Intermediate Certificate / CA Bundle

Store files in:

```text
C:\Cert
```

---

# Step 6 – Import Certificate into Exchange

Run the following command:

```powershell
Import-ExchangeCertificate -FileData ([System.IO.File]::ReadAllBytes("C:\Cert\certificate.cer"))
```

After import, note the certificate thumbprint.

---

# Step 7 – Verify Imported Certificate

```powershell
Get-ExchangeCertificate | fl FriendlyName,Thumbprint,NotAfter,Services
```

Verify:

* Certificate status
* Expiry date
* Thumbprint

---

# Step 8 – Assign Certificate to Exchange Services

Run:

```powershell
Enable-ExchangeCertificate -Thumbprint <THUMBPRINT> -Services IIS,SMTP
```

When prompted:

```text
Overwrite existing default SMTP certificate?
```

Select:

```text
Y
```

---

# Step 9 – Restart IIS Services

```powershell
iisreset
```

---

# Step 10 – Export Certificate for VPN / Web Services

If the same certificate is required for VPN or Web Server:

```powershell
$pwd = ConvertTo-SecureString -String "Password123" -Force -AsPlainText

Export-ExchangeCertificate `
-Thumbprint <THUMBPRINT> `
-FileName "C:\Cert\UnifiedSSL.pfx" `
-Password $pwd
```

Output:

```text
C:\Cert\UnifiedSSL.pfx
```

---

# Step 11 – Install Certificate on Additional Services

## VPN Appliance

* Import PFX certificate
* Assign certificate to SSL VPN service
* Restart VPN service if required

## Sophos Firewall SSL VPN Certificate Update

### Export Certificate from Exchange

If not already exported:

```powershell
$pwd = ConvertTo-SecureString -String "Password123" -Force -AsPlainText

Export-ExchangeCertificate `
-Thumbprint <THUMBPRINT> `
-FileName "C:\Cert\UnifiedSSL.pfx" `
-Password $pwd
```

---

## Import Certificate into Sophos Firewall

1. Log in to Sophos Firewall Web Admin
2. Navigate to:

```text
Certificates > Certificates
```

3. Click:

```text
Add > Import Certificate
```

4. Select:

```text
PKCS12 (PFX)
```

5. Upload:

```text
UnifiedSSL.pfx
```

6. Enter export password
7. Save the certificate

---

## Assign Certificate to SSL VPN Portal

Navigate to:

```text
VPN > SSL VPN (Remote Access)
```

Under:

```text
SSL VPN Global Settings
```

Select the imported certificate and save configuration.

---

## Verify Sophos VPN Certificate

Test:

```text
https://vpn.company.com.au
```

Verify:

* No SSL warning
* Correct certificate displayed
* VPN portal accessibility
* SSL VPN client connectivity

---

## IIS Web Server

* Import PFX certificate
* Bind certificate to HTTPS (443)

## Additional HTTPS Services

* Import certificate as required

---

# Step 12 – Validation and Testing

## Exchange Validation

Test:

```text
https://mail.company.com.au/owa
```

Verify:

* No certificate warning
* Outlook connectivity
* Mobile synchronization
* Autodiscover functionality

---

# Useful Verification Commands

## Verify Exchange URLs

```powershell
Get-OutlookAnywhere | fl ExternalHostname,InternalHostname
Get-OwaVirtualDirectory | fl ExternalUrl
Get-WebServicesVirtualDirectory | fl ExternalUrl
```

---

# Common Issues and Fixes

| Issue                           | Cause                     | Resolution                           |
| ------------------------------- | ------------------------- | ------------------------------------ |
| Certificate warning in Outlook  | Missing SAN entry         | Reissue certificate with correct SAN |
| Old certificate still displayed | IIS cache                 | Run iisreset                         |
| SMTP TLS issue                  | SMTP service not assigned | Re-enable certificate for SMTP       |
| VPN certificate mismatch        | Wrong hostname            | Verify VPN FQDN                      |
| Certificate import failed       | Wrong certificate format  | Use .cer or .pfx correctly           |

---

# Best Practices

* Renew SSL certificates at least 30 days before expiry
* Maintain backup of exported PFX file
* Use strong password during PFX export
* Document certificate thumbprints and expiry dates
* Validate all SAN entries before purchase
* Avoid unnecessary internal hostnames in public certificates

---

# Security Notes

Using a single certificate across multiple systems means:

* Same private key exists on multiple devices
* Compromise of one device may affect all services
* Protect exported PFX files securely

Recommended:

* Restrict access to certificate backups
* Use separate certificates for critical systems when possible

---

# Documentation References

## Exchange Certificate Information

```powershell
Get-ExchangeCertificate
```

## IIS Reset

```powershell
iisreset
```

## Verify DNS

```powershell
nslookup mail.company.com.au
nslookup autodiscover.company.com.au
```

---

# Summary

This procedure covers:

1. CSR generation from Exchange Server 2019
2. SAN certificate request and validation
3. Certificate import and Exchange assignment
4. IIS and SMTP configuration
5. PFX export for VPN and web services
6. Validation and troubleshooting

The SSL certificate is configured to support:

* Exchange Server 2019
* Outlook and OWA
* Autodiscover
* VPN services
* Website HTTPS access
* Additional SAN-enabled services
