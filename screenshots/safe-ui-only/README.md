# Lab Evidence Documentation

## Server Configuration

### Server Manager — AD DS & DNS Installed
Windows Server 2019 domain controller with Active Directory Domain Services and DNS Server roles successfully deployed and operational. Validates core identity and name resolution infrastructure.

### Domain Controller Services
Confirms critical AD services running: ADDS, DNS, KDC, Netlogon. Demonstrates functional domain controller ready for client authentication and policy enforcement.

## Active Directory Structure

### OU Hierarchy — bellcorp.local
Organizational unit structure designed for role-based administration and targeted Group Policy application. Separates users, computers, and administrative accounts into logical containers.

**Structure:**
```
bellcorp.local
├── Admin Accounts
├── Workstations
├── Servers
└── Users
    ├── IT Department
    ├── HR Department
    ├── Finance Department
    └── Sales Department
```

### User and Group Management
Domain users and security groups created in Active Directory Users and Computers (ADUC). Demonstrates identity lifecycle management and group-based access control.

## Group Policy Administration

### GPO Enforcement — Command Prompt Disabled
Group Policy Object applied to Workstations OU restricting command prompt access. Validates policy inheritance, OU targeting, and client-side enforcement using `gpresult`.

**Policy Details:**
- **GPO Name:** Workstation Security Baseline
- **Target:** Workstations OU
- **Setting:** User Configuration → Administrative Templates → System → Prevent access to command prompt
- **Status:** Enabled

### Password Policy Enforcement
Domain-level password policy configured via Default Domain Policy. Enforces 14-character minimum, complexity requirements, and account lockout thresholds.

### GPResult Validation
PowerShell and `gpresult /r` output confirming applied GPOs on domain-joined workstation. Shows policy precedence and successful application from domain controller.

## DNS Configuration

### AD-Integrated DNS Zones
Forward and reverse lookup zones integrated with Active Directory. Confirms dynamic DNS updates and secure zone transfers configured for domain functionality.

### SRV Record Validation
Critical service locator records (_ldap, _kerberos, _gc) present in DNS zone. Validates domain controller advertisement and client discovery mechanisms.

**Key SRV Records:**
```
_ldap._tcp.dc._msdcs.bellcorp.local
_kerberos._tcp.bellcorp.local
_gc._tcp.bellcorp.local
```

## Domain Join & Authentication

### Workstation Domain Membership
Windows 10 client successfully joined to bellcorp.local domain. System properties confirm domain membership and secure channel established with domain controller.

### Kerberos Authentication
User logon to domain-joined workstation with Kerberos ticket-granting ticket (TGT) issued by domain controller. Validates authentication flow and trust relationship.

**Validation Commands:**
```powershell
# Verify domain membership
systeminfo | findstr /B /C:"Domain"

# Check secure channel
Test-ComputerSecureChannel -Verbose

# View Kerberos tickets
klist
```

### Network Share Access
Domain user accessing shared folder via UNC path with permissions enforced through Active Directory group membership. Demonstrates file server integration with AD.

## Security & Monitoring

### Event Viewer — Security Logs
Windows Security event log showing authentication events (4624, 4625) and account management actions (4720, 4726). Validates audit policy configuration and logging functionality.

**Monitored Events:**
- 4624: Successful account logon
- 4625: Failed logon attempt
- 4740: Account lockout
- 4720: User account created
- 4726: User account deleted

### Account Lockout Detection
Security event 4740 triggered after exceeding failed logon threshold. Confirms account lockout policy enforcement and security monitoring capabilities.

## Network Topology

### VirtualBox Network Configuration
Oracle VirtualBox internal network adapter configured on both domain controller and client VMs. Shows isolated network environment with no external routing.

**Network Settings:**
- Network Type: Internal Network
- Adapter Name: intnet
- Domain Controller IP: 10.0.0.1
- Client IP: DHCP assigned (10.0.0.x)

### DNS Client Configuration
Windows 10 workstation configured with domain controller as primary DNS server. Validates proper name resolution and domain service discovery.

```powershell
ipconfig /all
# DNS Servers: 10.0.0.1
```

## PowerShell Administration

### AD User Management
PowerShell commands creating and modifying Active Directory user accounts. Demonstrates scripting capability and automation mindset for identity management.

```powershell
Get-ADUser -Filter * | Select Name, SamAccountName, Enabled
New-ADUser -Name "John Doe" -SamAccountName "jdoe"
Set-ADUser -Identity "jdoe" -Department "IT"
```

### GPO Backup and Reporting
Group Policy backup operation and HTML report generation. Shows change management and documentation practices for policy administration.

```powershell
Backup-GPO -All -Path "C:\GPOBackups"
Get-GPOReport -All -ReportType HTML -Path "C:\GPOReport.html"
```

---

**Environment:** bellcorp.local domain  
**Administrator:** Reginald Bell  
**Platform:** Oracle VirtualBox | Windows Server 2019 | Windows 10
