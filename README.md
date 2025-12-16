Active Directory Administration Lab
Enterprise-focused Windows Active Directory homelab demonstrating administration, security hardening, and operational management.
Overview
This project simulates enterprise identity and access management (IAM) operations through hands-on administration of a Windows Active Directory environment. The focus emphasizes operational ownership, security best practices, and day-to-day administrative tasks rather than one-time deployment.
Lab Architecture
Core Infrastructure:

Windows Server 2019 Domain Controller
Active Directory Domain Services (AD DS)
Integrated DNS Services
Domain-joined Windows 10/11 workstations
Isolated virtual network environment

Administrative Operations
Identity & Access Management:

User account provisioning and deprovisioning
Security and distribution group management
Organizational Unit (OU) structure maintenance
Permission delegation and role-based access control

Policy & Configuration:

Group Policy Object (GPO) creation and enforcement
DNS record management and zone configuration
Domain controller health monitoring and maintenance
Change management documentation

Security Hardening
Access Controls:

Principle of least privilege enforcement
Role-based administration with tiered access
Protected Users group implementation
Service account isolation

Policy Enforcement:

Account lockout thresholds (5 failed attempts, 30-minute lockout)
Password complexity requirements (14+ characters, complexity enabled)
Kerberos policy configuration
Audit policy for security events

Logging & Visibility:

Authentication event tracking (Event IDs 4624, 4625, 4740)
Privileged access monitoring
GPO modification alerts
Failed logon analysis

Operational Monitoring
Health Checks:

Domain controller replication status
DNS resolution validation
SYSVOL and NETLOGON share integrity
Time synchronization verification

Security Events:

Real-time account lockout detection
Unauthorized access attempts
Group Policy changes
Administrative group modifications

Business Continuity
Backup Strategy:

System State backups (daily schedule)
Active Directory database (NTDS.dit) protection
SYSVOL replication verification
GPO backup and version control

Recovery Procedures:

Authoritative and non-authoritative restore processes
DNS zone recovery from AD-integrated zones
Domain controller rebuild documentation
Disaster recovery runbook

Skills Demonstrated

Windows Server administration
Active Directory architecture and operations
Group Policy design and troubleshooting
PowerShell scripting for automation
Security event log analysis
Incident response procedures
Technical documentation

Documentation Standards
Infrastructure-specific details (IP addresses, domain names, credentials) are excluded for security. This repository functions as a procedural guide and administrative reference for AD operations.

Future Enhancements: Certificate Services (AD CS), LDAPS implementation, Red/Blue team exercises, SIEM integration
