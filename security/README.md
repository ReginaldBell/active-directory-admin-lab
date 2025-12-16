# Security Hardening & Controls

## 🔒 Security Objectives

This lab implements enterprise-grade security controls to demonstrate defense-in-depth principles and role-based access management. Security is enforced through Active Directory Group Policy, organizational unit structure, and network isolation.

**Core Objectives:**
* Enforce least privilege access by department (HR, IT, Finance, Sales)
* Prevent unauthorized lateral movement from user workstations
* Centralize authentication and authorization through AD DS
* Simulate real-world enterprise security controls
* Maintain administrative accountability through separate admin accounts

## 🧱 Policy Enforcement

Security controls validated and operational in the bellcorp.local domain:

### User Restrictions
- ✅ Command Prompt access disabled for HR department
- ✅ Control Panel access restricted for standard users
- ✅ Registry editing tools blocked for non-IT users
- ✅ Task Manager disabled for HR workstations

### Administrative Controls
- ✅ Remote Desktop Protocol (RDP) allowed only for IT department
- ✅ Administrative tools hidden from standard users
- ✅ Separate administrative accounts enforced (no daily-use admin accounts)
- ✅ Local administrator rights removed from domain users

### Network & File Access
- ✅ SMB file sharing blocked for HR department workstations
- ✅ Network share access controlled via security group membership
- ✅ UNC path restrictions enforced by department

### Configuration Management
- ✅ Desktop wallpaper centrally deployed via GPO
- ✅ Screen lock timeout enforced (10 minutes)
- ✅ Password complexity enforced domain-wide
- ✅ Account lockout threshold active (5 failed attempts)

**Evidence Location:** `/screenshots/gpo-enforcement/`

## 🧠 Identity & Access Control

### Administrative Tier Model
```
Tier 0: Domain Admins (Domain Controllers only)
├── Reginald Bell (Admin Account)
└── Break-glass emergency account

Tier 1: Server Administrators (Future server OU)
└── Service account isolation

Tier 2: Workstation Users (Standard access)
├── HR Department users
├── IT Department users
├── Finance Department users
└── Sales Department users
```

### Access Separation Strategy
* **Domain Admins** - Isolated to Domain Controller management only
* **Department Admins** - Delegated control over respective OUs
* **Standard Users** - No administrative privileges on workstations
* **Service Accounts** - Dedicated accounts for applications (non-interactive)

### OU-Based Delegation
Administrative permissions delegated by organizational unit:
- IT Department OU: Password reset and account unlock permissions
- HR Department OU: Read-only access for IT, full control for HR manager
- Workstations OU: Computer object management for IT help desk
- Admin Accounts OU: Restricted access, domain admin only

### Group Policy Scoping
GPOs linked and filtered by department:
- **Domain-level:** Password policy, account lockout, Kerberos settings
- **Workstations OU:** Desktop configuration, security baselines
- **HR OU:** Restrictive user policies (CMD disabled, SMB blocked)
- **IT OU:** Administrative tool access, RDP permissions

## 🌐 Network & Trust Boundaries

### Network Isolation
- **Topology:** Oracle VirtualBox Internal Network (intnet)
- **Routing:** No NAT or bridged adapters configured
- **External Access:** Completely isolated from internet and host network
- **Segmentation:** Single broadcast domain for lab simplicity

### Trust Configuration
- **Forest:** Single forest (bellcorp.local)
- **Domain:** Single domain (no child domains)
- **External Trusts:** None configured
- **Cross-forest Trusts:** Not applicable (isolated environment)

### DNS Resolution Boundaries
- **Internal DNS:** Managed by domain controller (10.0.0.1)
- **External Forwarders:** Disabled by default
- **Zone Type:** Active Directory-integrated (secure dynamic updates)
- **Client Configuration:** All workstations point to DC for DNS

### Attack Surface Considerations
**If Domain Controller Compromised:**
- Entire lab domain is compromised (expected in single-DC lab)
- No external network exposure limits blast radius to lab only
- VirtualBox snapshots enable rapid recovery

**If Workstation Compromised:**
- User-level access only (no local admin rights)
- Lateral movement blocked by firewall rules and GPO restrictions
- Administrative accounts not accessible from workstation tier

**Mitigation Strategy:**
- Least privilege enforced at all levels
- Administrative access logged and monitored
- Network isolation prevents external threat actor access

## ✅ Validation Evidence

All security controls verified through operational testing and administrative validation.

### Command Prompt Restriction (HR Department)
**Test:** HR user attempts to launch `cmd.exe`  
**Result:** Access denied by Group Policy  
**Validation Method:** `gpresult /r` confirms GPO applied  
**Screenshot:** `gpo-cmd-disabled.png`

### RDP Access Control (IT Department Only)
**Test:** IT user connects via Remote Desktop to workstation  
**Result:** Connection successful  
**Test:** HR user attempts same connection  
**Result:** Access denied  
**Validation Method:** Local security policy + Event Viewer (Event ID 4625)  
**Screenshot:** `rdp-access-control.png`

### SMB File Sharing Block (HR Department)
**Test:** HR user attempts to access network share via `\\server\share`  
**Result:** Access denied  
**Validation Method:** Security group filtering on share permissions  
**Screenshot:** `smb-blocked-hr.png`

### Group Policy Application
**Test:** Workstation reboot, login as domain user  
**Result:** Desktop wallpaper applied, CMD restricted, policies enforced  
**Validation Method:**
```powershell
gpupdate /force
gpresult /h C:\GPReport.html
Get-GPResultantSetOfPolicy
```
**Screenshot:** `gpresult-validation.png`

### Authentication & Authorization
**Test:** Domain user login to workstation  
**Result:** Kerberos ticket issued, group memberships applied  
**Validation Method:**
```powershell
whoami /groups
klist
Test-ComputerSecureChannel
```
**Screenshot:** `kerberos-authentication.png`

### Account Lockout Policy
**Test:** 5 failed login attempts with incorrect password  
**Result:** Account locked for 30 minutes  
**Validation Method:** Event Viewer Security Log (Event ID 4740)  
**Screenshot:** `account-lockout.png`

## ⚠️ Threats Considered

This lab's security design addresses the following threat scenarios:

### Privilege Escalation
**Threat:** User attempts to gain administrative privileges  
**Control:** Separate admin accounts, UAC enabled, local admin rights removed  
**Detection:** Event ID 4672 (special privileges assigned to new logon)

### Unauthorized Lateral Movement
**Threat:** Compromised workstation used to access other systems  
**Control:** Firewall rules, SMB restrictions, RDP access control  
**Detection:** Network traffic monitoring (future SIEM integration)

### Mis-Scoped Group Policy Objects
**Threat:** GPO applied to wrong OU, granting unintended permissions  
**Control:** GPO testing in isolated OU before production deployment  
**Detection:** Regular GPO audit and RSOP validation

### Credential Misuse
**Threat:** Stolen credentials used for unauthorized access  
**Control:** Account lockout policy, Kerberos ticket lifetime limits  
**Detection:** Failed authentication monitoring (Event ID 4625)

### Insider Threat
**Threat:** Authorized user accessing data outside their role  
**Control:** OU-based access control, security group filtering  
**Detection:** File access auditing, administrative action logging

### Domain Controller Compromise
**Threat:** Full domain compromise via DC exploitation  
**Control:** Network isolation (no external access), strong admin passwords  
**Detection:** ADDS event log monitoring, replication monitoring

## 📈 Future Security Enhancements

Planned security improvements not yet implemented:

### Authentication & Access
- [ ] **LAPS (Local Administrator Password Solution)** - Automated local admin password rotation
- [ ] **Smart Card Authentication** - PKI-based two-factor authentication
- [ ] **Privileged Access Workstations (PAWs)** - Dedicated admin workstations
- [ ] **Azure AD Connect** - Hybrid identity for cloud integration

### Encryption & Data Protection
- [ ] **BitLocker Drive Encryption** - Full disk encryption for workstations
- [ ] **EFS (Encrypting File System)** - User-level file encryption
- [ ] **LDAPS** - LDAP over SSL/TLS for secure directory queries
- [ ] **SMB Signing & Encryption** - Prevent man-in-the-middle attacks

### Monitoring & Detection
- [ ] **SIEM Integration** - Splunk or Microsoft Sentinel for log aggregation
- [ ] **Advanced Audit Policies** - Granular security event logging
- [ ] **PowerShell Script Block Logging** - Detect malicious script execution
- [ ] **Sysmon Deployment** - Enhanced endpoint telemetry

### Certificate Services
- [ ] **AD CS (Active Directory Certificate Services)** - Internal PKI
- [ ] **Auto-enrollment** - Automated certificate deployment
- [ ] **Certificate-based authentication** - Replace password auth where possible

### Red Team / Blue Team Exercises
- [ ] **Simulated attacks** - Kerberoasting, pass-the-hash, golden ticket
- [ ] **Detection engineering** - Build detection rules for common attacks
- [ ] **Incident response playbooks** - Document response procedures

---

**Security Administrator:** Reginald Bell  
**Domain:** bellcorp.local  
**Last Security Audit:** December 2024  
**Compliance Framework:** NIST Cybersecurity Framework (Identify, Protect, Detect, Respond, Recover)
