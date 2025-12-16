# Administrative Operations & Ownership

## 🎯 Purpose

This document details day-to-day administrative tasks performed within the Active Directory lab environment, demonstrating operational ownership, change management, and enterprise-level system administration rather than one-time deployment.

## 👤 Administrator

**Name:** Reginald Bell  
**Role:** Systems Administrator  
**Scope:** Full administrative control over lab domain infrastructure

## 🔧 Core Administrative Responsibilities

### 1. Identity & Access Management

**User Lifecycle Operations:**
- ✅ User account provisioning and deprovisioning via ADUC
- ✅ Security group membership management
- ✅ Distribution group creation and maintenance
- ✅ Account attribute modification (department, title, contact info)

**Organizational Structure:**
- ✅ Designed and implemented OU hierarchy
- ✅ Delegated administrative permissions per OU
- ✅ Organized users and computers into logical containers
- ✅ Applied inheritance blocking where appropriate

**Access Control:**
- ✅ Role-based access control (RBAC) implementation
- ✅ Group-based permission assignment
- ✅ Least privilege enforcement
- ✅ Service account isolation and management

**Authentication Validation:**
```powershell
# Verified Kerberos ticket generation
klist

# Validated user authentication
Get-ADUser -Identity "jdoe" -Properties LastLogonDate

# Checked account lockout status
Search-ADAccount -LockedOut
```

### 2. Group Policy Administration

**Policy Creation & Management:**
- ✅ Created domain-level and OU-specific GPOs
- ✅ Configured security baseline policies
- ✅ Implemented software restriction policies
- ✅ Deployed desktop configuration standards

**Security Policy Enforcement:**
| Policy Type | Configuration | Scope |
|-------------|--------------|-------|
| Password Policy | 14 char minimum, complexity enabled | Domain |
| Account Lockout | 5 attempts, 30-minute duration | Domain |
| Audit Policy | Logon events, account management | All OUs |
| User Rights | Restricted admin login locations | Servers OU |

**GPO Validation & Troubleshooting:**
```powershell
# Force policy update on client
gpupdate /force

# Generate policy report
gpresult /h C:\GPReport.html /f

# Verify applied GPOs
Get-GPResultantSetOfPolicy -ReportType Html -Path C:\RSoP.html
```

**OU-Based Policy Targeting:**
- Scoped policies to specific OUs (Workstations, Servers, Users)
- Applied WMI filters for hardware-specific configurations
- Used security group filtering for targeted deployment
- Documented GPO precedence and inheritance

### 3. DNS Administration

**Zone Management:**
- ✅ Managed Active Directory-integrated forward lookup zones
- ✅ Created and maintained reverse lookup zones
- ✅ Configured zone transfer settings (secure only)
- ✅ Enabled DNS aging and scavenging

**Service Record Validation:**
```powershell
# Verified critical AD SRV records
nslookup -type=SRV _ldap._tcp.dc._msdcs.lab.local

# Validated Kerberos authentication records
nslookup -type=SRV _kerberos._tcp.lab.local

# Checked domain controller locator records
nslookup -type=SRV _ldap._tcp.lab.local
```

**Client Configuration:**
- ✅ Configured primary DNS on domain-joined workstations
- ✅ Set DNS suffix search order
- ✅ Validated DNS client cache behavior
- ✅ Tested external DNS forwarders (optional)

**Troubleshooting & Validation:**
```cmd
# DNS resolution testing
nslookup dc01.lab.local
ping dc01.lab.local

# DNS cache management
ipconfig /displaydns
ipconfig /flushdns

# DNS registration verification
ipconfig /registerdns
```

### 4. Domain Controller Health Monitoring

**Replication Status:**
```powershell
# Check AD replication status
repadmin /replsummary

# Verify replication partners
repadmin /showrepl

# Force replication sync
repadmin /syncall /AdeP
```

**Service Validation:**
- ✅ Active Directory Domain Services (ADDS) - Running
- ✅ DNS Server - Running
- ✅ Kerberos Key Distribution Center (KDC) - Running
- ✅ Netlogon - Running
- ✅ Windows Time (W32Time) - Synchronized

**SYSVOL & NETLOGON Share Health:**
```powershell
# Verify SYSVOL share
Get-SmbShare | Where-Object {$_.Name -eq "SYSVOL"}

# Check NETLOGON share
Get-SmbShare | Where-Object {$_.Name -eq "NETLOGON"}

# Validate share permissions
Get-SmbShareAccess -Name "SYSVOL"
```

### 5. System Validation & Testing

**Domain Join Verification:**
```powershell
# Verify computer domain membership
Get-WmiObject -Class Win32_ComputerSystem | Select-Object Domain

# Test secure channel
Test-ComputerSecureChannel -Verbose

# List domain controllers
nltest /dclist:lab.local
```

**Authentication Testing:**
- ✅ Verified user logon to domain-joined workstations
- ✅ Tested group policy application on client machines
- ✅ Validated Kerberos ticket generation
- ✅ Confirmed access to network shares via DFS/SMB

**Network Isolation:**
- ✅ Verified VirtualBox internal network configuration
- ✅ Confirmed no external routing (isolated environment)
- ✅ Tested DNS forwarder behavior (internal-only by default)
- ✅ Validated firewall rules on domain controller

## 🛠️ Administrative Toolset

### GUI Management Consoles
- **Active Directory Users and Computers (ADUC)** - User/group/OU management
- **Active Directory Administrative Center (ADAC)** - Advanced AD operations
- **Group Policy Management Console (GPMC)** - GPO creation and linking
- **DNS Manager** - Zone and record administration
- **Active Directory Sites and Services** - Replication topology
- **Event Viewer** - Security and system log analysis

### PowerShell Administration
```powershell
# User management
Get-ADUser -Filter * -Properties *
New-ADUser -Name "John Doe" -SamAccountName "jdoe" -Path "OU=Users,DC=lab,DC=local"
Set-ADUser -Identity "jdoe" -Department "IT" -Title "Systems Administrator"
Remove-ADUser -Identity "jdoe" -Confirm:$false

# Group management
New-ADGroup -Name "IT-Admins" -GroupScope Global -GroupCategory Security
Add-ADGroupMember -Identity "IT-Admins" -Members "jdoe"
Get-ADGroupMember -Identity "Domain Admins"

# GPO management
New-GPO -Name "Security Baseline" | New-GPLink -Target "DC=lab,DC=local"
Get-GPO -All | Select-Object DisplayName, GpoStatus
Backup-GPO -All -Path "C:\GPOBackups"

# DNS operations
Add-DnsServerResourceRecordA -Name "fileserver" -ZoneName "lab.local" -IPv4Address "10.0.0.50"
Get-DnsServerZone
Clear-DnsServerCache
```

### Remote Server Administration Tools (RSAT)
- Installed on Windows 10 workstation for remote management
- Enables centralized administration without RDP to DC
- Provides full AD/DNS/GPO management capabilities

### Monitoring & Logging
- **Windows Event Viewer** - Security events (4624, 4625, 4740)
- **Performance Monitor** - DC resource utilization
- **AD Replication Status Tool** - Health checks
- **PowerShell logging** - Administrative action audit trail

## 📋 Operational Procedures

### Change Management Workflow

```
1. Plan Change
   ↓
2. Document Expected Outcome
   ↓
3. Take Snapshot (VirtualBox)
   ↓
4. Implement Change
   ↓
5. Validate Functionality
   ↓
6. Document Results
   ↓
7. Monitor for Issues
```

### Daily Administrative Tasks
- ✅ Review security event logs for anomalies
- ✅ Check domain controller service status
- ✅ Validate replication health (if multi-DC)
- ✅ Monitor failed authentication attempts
- ✅ Verify backup completion (system state)

### Weekly Maintenance
- ✅ Review and prune inactive user accounts
- ✅ Audit group membership changes
- ✅ Validate GPO application across OUs
- ✅ Test disaster recovery procedures
- ✅ Update documentation with configuration changes

### Monthly Tasks
- ✅ Full system state backup of domain controller
- ✅ GPO backup and version control
- ✅ Security audit of administrative accounts
- ✅ DNS zone cleanup (aging/scavenging)
- ✅ Review and update security policies

## 🎯 Operational Mindset

### Treated as Production Environment
This lab is administered as a **managed production environment**, not a disposable test bed:

- ✅ **Intentional Changes** - All modifications planned and documented
- ✅ **Validation First** - Changes tested before broader deployment
- ✅ **Rollback Planning** - VirtualBox snapshots before major changes
- ✅ **Documentation** - Configuration changes tracked in Git repository
- ✅ **Security Focus** - Least privilege and defense-in-depth principles

### Real-World Administrative Workflows
Simulates enterprise IT operations:
- Ticketing system mindset (problem → solution → validation)
- Change control procedures (plan → implement → verify)
- Incident response readiness (monitoring → detection → remediation)
- Continuous improvement (review → optimize → document)

## 📊 Skills Demonstrated

| Skill Category | Specific Competencies |
|----------------|----------------------|
| **Identity Management** | User provisioning, RBAC, account lifecycle |
| **Group Policy** | GPO creation, security baselines, troubleshooting |
| **DNS Administration** | Zone management, SRV records, client configuration |
| **PowerShell** | Automation, bulk operations, reporting |
| **Security** | Audit policies, least privilege, authentication |
| **Troubleshooting** | Event logs, replication, connectivity issues |
| **Documentation** | Runbooks, architecture diagrams, change logs |

## 🔄 Continuous Learning

### Areas of Ongoing Development
- Advanced PowerShell scripting for automation
- Disaster recovery and business continuity testing
- Security hardening beyond baseline configurations
- Integration with monitoring solutions (SIEM)
- Certificate Services (AD CS) implementation

---

**Administrator:** Reginald Bell  
**Lab Environment:** Oracle VirtualBox | Windows Server 2019 | Windows 10  
**Last Updated:** December 2024  
**Documentation Status:** Active Maintenance
