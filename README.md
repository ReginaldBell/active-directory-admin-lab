# Active Directory Administration Lab

Enterprise-focused Windows Active Directory homelab demonstrating administration, security hardening, and operational management.

## 📋 Overview

This project simulates enterprise identity and access management (IAM) operations through hands-on administration of a Windows Active Directory environment. The focus emphasizes operational ownership, security best practices, and day-to-day administrative tasks rather than one-time deployment.

## 🏗️ Lab Architecture

### Virtualization Platform
- **Hypervisor:** Oracle VirtualBox
- **Network Configuration:** Internal network for VM-to-VM communication
- **Topology:** Domain controller with domain-joined client workstation

### Core Infrastructure
- **Windows Server 2019 (VM)** - Domain Controller
  - Active Directory Domain Services (AD DS)
  - Integrated DNS Services
  - DHCP Services
- **Windows 10 (VM)** - Domain-joined client workstation
- **Virtual Network** - Isolated internal network for secure lab environment

## 🔧 Administrative Operations

### Identity & Access Management
- User account provisioning and deprovisioning
- Security and distribution group management
- Organizational Unit (OU) structure maintenance
- Permission delegation and role-based access control

### Policy & Configuration
- Group Policy Object (GPO) creation and enforcement
- DNS record management and zone configuration
- Domain controller health monitoring and maintenance
- Change management documentation

## 🔒 Security Hardening

### Access Controls
- ✅ Principle of least privilege enforcement
- ✅ Role-based administration with tiered access
- ✅ Protected Users group implementation
- ✅ Service account isolation

### Policy Enforcement
| Policy | Configuration |
|--------|--------------|
| Account Lockout | 5 failed attempts, 30-minute lockout |
| Password Complexity | 14+ characters, complexity enabled |
| Kerberos Policy | Configured per Microsoft best practices |
| Audit Policy | Security event logging enabled |

### Logging & Visibility
- **Authentication Events:** 4624, 4625, 4740
- **Privileged Access:** Admin account activity monitoring
- **GPO Modifications:** Change tracking and alerts
- **Failed Logons:** Pattern analysis and response

## 📊 Operational Monitoring

### Health Checks
- ✓ Domain controller replication status
- ✓ DNS resolution validation
- ✓ SYSVOL and NETLOGON share integrity
- ✓ Time synchronization verification

### Security Events
- Real-time account lockout detection
- Unauthorized access attempts
- Group Policy changes
- Administrative group modifications

## 💾 Business Continuity

### Backup Strategy
```powershell
# System State backups (daily schedule)
- Active Directory database (NTDS.dit) protection
- SYSVOL replication verification
- GPO backup and version control
```

### Recovery Procedures
- Authoritative and non-authoritative restore processes
- DNS zone recovery from AD-integrated zones
- Domain controller rebuild documentation
- Disaster recovery runbook

## 🎯 Skills Demonstrated

- Windows Server administration
- Active Directory architecture and operations
- Group Policy design and troubleshooting
- PowerShell scripting for automation
- Security event log analysis
- Incident response procedures
- Technical documentation

## 📖 Documentation Standards

> **Note:** Infrastructure-specific details (IP addresses, domain names, credentials) are excluded for security. This repository functions as a procedural guide and administrative reference for AD operations.

## 🚀 Future Enhancements

- [ ] Certificate Services (AD CS)
- [ ] LDAPS implementation
- [ ] Red/Blue team exercises
- [ ] SIEM integration

---

**Built with:** Windows Server 2019 | Active Directory | PowerShell | Group Policy

**License:** MIT | **Contact:** [https://github.com/ReginaldBell]
