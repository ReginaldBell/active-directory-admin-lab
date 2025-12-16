# Active Directory Lab Architecture

## 🎯 Purpose

This document describes the high-level architecture of the Active Directory lab environment, focusing on identity services, DNS infrastructure, trust boundaries, and network topology.

## 🖥️ Lab Components

### Domain Controller
**Platform:** Oracle VirtualBox VM  
**Operating System:** Windows Server 2019

**Services:**
- Active Directory Domain Services (AD DS)
- DNS Server (Primary)
- DHCP Server (optional)

### Domain-Joined Workstations
**Platform:** Oracle VirtualBox VM  
**Operating System:** Windows 10

**Configuration:**
- Joined to lab domain
- DNS configured to domain controller
- Used for administrative testing and validation

## 🌳 Domain Structure

| Component | Configuration |
|-----------|--------------|
| **Forest** | Single forest |
| **Domain** | Single domain (lab scope) |
| **Functional Level** | Windows Server 2016 or higher |
| **Trust Relationships** | None (isolated lab environment) |

## 🌐 Name Resolution

### DNS Configuration
- **Primary DNS:** Hosted on Domain Controller
- **Client DNS:** Points to DC IP address
- **Zone Type:** Active Directory-integrated
- **Forwarders:** Configurable to external DNS (8.8.8.8, 1.1.1.1)

### DNS Hierarchy
```
lab.local (example)
├── _msdcs (Microsoft Domain Services)
├── _sites (AD Sites)
├── _tcp (Service records)
└── _udp (Service records)
```

## 🔐 Identity & Authentication

### Centralized Identity Management
- **Directory Service:** Active Directory Domain Services
- **Authentication Protocol:** Kerberos v5
- **Authorization:** Group-based access control
- **Password Policy:** Domain-level GPO enforcement

### Administrative Separation
```
Domain Admins
├── Tier 0: Domain Controllers
├── Tier 1: Servers and applications
└── Tier 2: Workstations and users
```

## 📊 Logical Network Topology

```
┌─────────────────────────────────────────────┐
│         Oracle VirtualBox Host              │
│                                             │
│  ┌───────────────────────────────────┐     │
│  │   Internal Network (10.0.0.0/24)  │     │
│  │                                   │     │
│  │  ┌─────────────────────────┐     │     │
│  │  │  Domain Controller VM   │     │     │
│  │  │  • AD DS                │     │     │
│  │  │  • DNS (10.0.0.1)       │     │     │
│  │  │  • DHCP                 │     │     │
│  │  └──────────┬──────────────┘     │     │
│  │             │                     │     │
│  │             │ Authentication      │     │
│  │             │ & DNS Resolution    │     │
│  │             │                     │     │
│  │  ┌──────────▼──────────────┐     │     │
│  │  │  Windows 10 Client VM   │     │     │
│  │  │  • Domain-joined        │     │     │
│  │  │  • DNS: 10.0.0.1        │     │     │
│  │  └─────────────────────────┘     │     │
│  │                                   │     │
│  └───────────────────────────────────┘     │
│                                             │
└─────────────────────────────────────────────┘
```

## 🛡️ Security Considerations

### Network Isolation
- ✅ Internal VirtualBox network (no external routing)
- ✅ DNS resolves only internal lab names by default
- ✅ No inbound connections from external networks
- ✅ No cross-forest trusts or external domain relationships

### Authentication Security
- ✅ Kerberos ticket-based authentication
- ✅ NTLM fallback disabled where possible
- ✅ Administrative accounts use separate credentials
- ✅ Password policies enforced via Group Policy

### Trust Boundaries
```
┌──────────────────────────────────┐
│   Lab Domain (Trusted Zone)      │
│                                  │
│   • Domain Controller            │
│   • Domain-joined clients        │
│   • Authenticated users          │
└──────────────────────────────────┘
         ║ (No trust)
         ║
┌────────▼─────────────────────────┐
│   External Networks (Untrusted)  │
└──────────────────────────────────┘
```

## ⚙️ Administration Boundaries

### Centralized Management
- **AD Administrative Center:** User and group lifecycle management
- **Group Policy Management Console:** GPO creation and enforcement
- **DNS Manager:** Zone and record administration
- **Active Directory Users and Computers (ADUC):** Object management

### PowerShell Administration
```powershell
# Common administrative tasks
Get-ADUser -Filter * -Properties *
New-ADUser -Name "John Doe" -SamAccountName "jdoe"
New-GPO -Name "Security Baseline" | New-GPLink -Target "DC=lab,DC=local"
```

### Remote Management Tools
- Remote Server Administration Tools (RSAT)
- PowerShell remoting (WinRM)
- Remote Desktop Protocol (RDP)

## 📝 Architecture Decisions

### Why Single Domain?
- Simplified lab environment
- Focus on core AD administration
- Reduced resource requirements
- Clear trust and authentication boundaries

### Why Integrated DNS?
- Required for AD functionality
- Simplified service record (SRV) management
- Automatic zone replication
- Seamless client authentication

## 🔄 Scalability Considerations

### Future Expansion Options
- Additional domain controllers (redundancy)
- Child domains (multi-tier structure)
- AD sites for geographical simulation
- Read-Only Domain Controllers (RODC)
- Federation services (AD FS)

---

**Last Updated:** December 2024  
**Maintained By:** [Your Name]  
**Lab Environment:** Oracle VirtualBox 7.0+ | Windows Server 2019 | Window

