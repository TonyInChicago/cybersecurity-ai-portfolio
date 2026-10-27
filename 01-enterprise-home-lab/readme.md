
# Project 1: Enterprise Cybersecurity Home Lab

## Objective

Design, deploy, and secure an isolated enterprise-style cybersecurity environment using VMware Workstation Pro, Windows Server, and Active Directory.

The purpose of this project is to develop hands-on experience with enterprise infrastructure, identity management, network isolation, access controls, and security validation.

## Technologies

- VMware Workstation Pro
- Windows Server 2025
- Active Directory Domain Services (AD DS)
- Windows DNS Server
- Windows PowerShell
- Windows 11
- Kali Linux
- Git and GitHub

## Project Status

**In Progress — Active Directory and File Share Access Control Validated**

## Lab Architecture

The environment uses an isolated VMware host-only network to simulate an enterprise infrastructure without exposing lab services directly to the physical network.

| System | Role | IP Address |
|---|---|---|
| COREPC | Windows 11 virtualization host | Host system |
| NS-DC01 | Domain Controller / DNS | 192.168.100.10 |
| NS-FS01 | Domain-joined File Server | 192.168.100.20 |

**Active Directory Domain:** `northstar.test`

**Virtual Network:** `VMnet2 — 192.168.100.0/24`

## Completed Work

### 1. Virtualization and Network Isolation

- Configured VMware Workstation Pro.
- Created an isolated host-only virtual network.
- Deployed Windows Server virtual machines.
- Assigned static IP addresses to core servers.

### 2. Active Directory Infrastructure

- Installed Active Directory Domain Services.
- Created the `northstar.test` domain.
- Configured domain DNS services.
- Created organizational units for corporate departments, IT, workstations, and servers.
- Created test domain user accounts.
- Joined the file server to the domain.

### 3. Enterprise File Server

- Deployed `NS-FS01`.
- Configured a dedicated data volume.
- Created a departmental Finance folder and SMB share.
- Implemented group-based access using Active Directory security groups.

### 4. Access Control Implementation

Configured access using the **AGDLP model**:

**Accounts → Global Groups → Domain Local Groups → Permissions**

For the Finance department:

`Sarah Mitchell → GG_Finance_Employees → DL_Finance_Share_RW → Finance Share`

Configured both SMB share permissions and NTFS permissions to restrict access to authorized users.

### 5. Security Validation

Tested the configured permissions using two domain accounts.

| Test Account | Department | Expected Result | Actual Result |
|---|---|---|---|
| Sarah Mitchell | Finance | Access granted | Passed |
| James Wilson | HR | Access denied | Passed |

These tests demonstrate group-based authorization and the principle of least privilege.

## Security Evidence

The following evidence files document the implemented controls and validation results:

- [Finance NTFS Permissions](evidence/Finance-NTFS-Permissions.txt)
- [Finance SMB Share Permissions](evidence/Finance-SMB-Permissions.txt)
- [Authorized Access Test — Sarah Mitchell](evidence/Sarah-Access-Test.txt)
- [Unauthorized Access Test — James Wilson](evidence/James-Access-Test.txt)

All test accounts and domain names represent a fictional lab environment.

## Remaining Goals

- [x] Configure an isolated virtual network.
- [x] Deploy Windows Server.
- [x] Install and configure Active Directory.
- [x] Create an enterprise domain.
- [x] Deploy a domain-joined file server.
- [x] Implement and validate departmental access controls.
- [ ] Deploy and configure a Windows 11 client workstation.
- [ ] Join the workstation to the domain.
- [ ] Expand security monitoring and logging.
- [ ] Document the complete architecture and additional security testing.

## Skills Demonstrated

- Windows Server administration
- Active Directory Domain Services
- Identity and access management
- Group-based access control
- AGDLP permissions model
- SMB and NTFS security
- Least privilege
- Security control validation
- Virtualization and network isolation
- Technical documentation and Git version control
