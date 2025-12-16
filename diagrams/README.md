# Network & Identity Diagrams

## 📐 Purpose

This directory contains visual representations of the Active Directory lab environment, documenting network topology, trust boundaries, and identity authentication flows from both administrative and security perspectives.

## 📂 Diagram Inventory

### Network Architecture
- **`network-topology.png`** - VirtualBox internal network layout
- **`vm-relationships.png`** - Domain Controller and client workstation connections
- **`ip-addressing.png`** - Static IP assignments and DHCP scope

### Identity & Authentication
- **`dns-resolution-flow.png`** - DNS query path from client to DC
- **`kerberos-auth-flow.png`** - Authentication ticket exchange process
- **`trust-boundaries.png`** - Security zones and network isolation

### Security Architecture
- **`admin-tier-model.png`** - Administrative privilege separation
- **`attack-surface.png`** - External vs internal trust boundaries
- **`gpo-hierarchy.png`** - Group Policy inheritance structure

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────┐
│                VirtualBox Host System                │
│                                                       │
│  ┌─────────────────────────────────────────────┐    │
│  │      Internal Network (10.0.0.0/24)         │    │
│  │                                             │    │
│  │   ┌──────────────┐      ┌──────────────┐   │    │
│  │   │   DC01 VM    │◄────►│  CLIENT01 VM │   │    │
│  │   │  10.0.0.1    │ Auth │   DHCP       │   │    │
│  │   │              │ DNS  │   Assigned   │   │    │
│  │   └──────────────┘      └──────────────┘   │    │
│  │                                             │    │
│  └─────────────────────────────────────────────┘    │
│                                                       │
│  ╔═══════════════════════════════════════════════╗  │
│  ║  Isolated - No External Network Access        ║  │
│  ╚═══════════════════════════════════════════════╝  │
└─────────────────────────────────────────────────────┘
```

## 🔒 Security Scope

### Trust Boundaries

| Zone | Components | Trust Level |
|------|------------|-------------|
| **Internal Domain** | DC, Domain clients, AD users | Fully Trusted |
| **VirtualBox Host** | Host OS, Management interface | Administrative |
| **External Network** | Internet, External DNS | Untrusted (Isolated) |

### Network Isolation
- ✅ Internal-only routing (no NAT/Bridged adapters)
- ✅ No cross-forest trust relationships
- ✅ Single forest, single domain architecture
- ✅ DNS forwarders disabled by default

## 🛠️ Diagram Creation Tools

### Primary Tools
- **[Draw.io](https://app.diagrams.net/)** - Network and architecture diagrams
- **[Lucidchart](https://www.lucidchart.com/)** - Alternative diagramming tool
- **Markdown/ASCII** - Inline documentation diagrams

### Diagram Standards
- **Format:** PNG (preferred) or SVG for scalability
- **Resolution:** Minimum 1920x1080 for readability
- **Style:** Professional, minimal color palette
- **Labels:** Clear, concise annotations

## 📊 Diagram Types Explained

### Logical Topology
Shows the conceptual relationship between components without physical implementation details.

**Purpose:**
- Understand data flow
- Identify trust relationships
- Document service dependencies

### Physical Topology
Represents actual VM configuration, IP addressing, and VirtualBox network settings.

**Purpose:**
- Reproduce lab environment
- Troubleshoot connectivity issues
- Validate network configuration

### Identity Flow
Illustrates authentication and authorization processes through the AD infrastructure.

**Purpose:**
- Understand Kerberos ticket lifecycle
- Document LDAP query paths
- Map DNS service record usage

## 🎯 Design Philosophy

### High-Level Focus
Diagrams intentionally abstract vendor-specific implementation details to emphasize:
- **Architectural intent** over configuration syntax
- **Security principles** over technical minutiae  
- **Operational flow** over step-by-step procedures

### Audience Consideration
Designed for:
- Security engineers evaluating architecture
- System administrators understanding topology
- Hiring managers assessing technical knowledge

## 📝 Diagram Maintenance

### Version Control
- All diagrams tracked in Git repository
- Source files (`.drawio`, `.xml`) included when possible
- PNG exports committed for easy viewing

### Update Guidelines
When modifying diagrams:
1. Update source file first
2. Export new PNG/SVG
3. Document changes in commit message
4. Reference related documentation updates

## 🔄 Future Additions

Planned diagram expansions:
- [ ] LDAP authentication sequence
- [ ] GPO processing order flowchart
- [ ] Backup and recovery topology
- [ ] Red team attack paths
- [ ] Blue team monitoring architecture

## 📖 Related Documentation

- **[`/docs/architecture.md`](../architecture.md)** - Detailed architecture specification
- **[`/docs/security-hardening.md`](../security-hardening.md)** - Security control implementation
- **[`README.md`](../../README.md)** - Project overview

---

**Diagram Library Version:** 1.0  
**Last Updated:** December 2024  
**Maintained By:** [Reginald,Bell]
