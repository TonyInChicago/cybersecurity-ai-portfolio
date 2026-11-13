
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

## Active Directory Security Controls

### Workstation Security Baseline — Automatic Session Lock

**Objective:** Reduce the risk of unauthorized access to unattended Windows workstations.

**Environment:**
- Domain: `northstar.test`
- Domain Controller: `NS-DC01` (Windows Server 2025)
- Workstation: `NS-WS01` (Windows 11)
- Organizational Unit: `Workstations`
- Group Policy Object: `GPO-Workstation-Security-Baseline`

**Implementation:**
1. Moved `NS-WS01` into the Workstations OU.
2. Created and linked a dedicated workstation security GPO.
3. Configured `Interactive logon: Machine inactivity limit` to 600 seconds.
4. Applied the policy using `gpupdate /force`.

**Validation:**
- Confirmed GPO application using `gpresult /r /scope computer`.
- Verified registry value `InactivityTimeoutSecs` was `0x258` (600 seconds).
- Confirmed the workstation automatically locked after 10 minutes of inactivity.

**Result:** PASS — The workstation security control was successfully deployed and validated.

**Evidence:** `GPO-Inactivity-Timeout-Verification.png` (screenshot captured; repository upload pending).

**Security concepts:** Group Policy, Active Directory organizational units, centralized security management, and security control validation.

## SOC Detection Lab — Windows Failed Authentication

### Objective
Validate that the Wazuh SIEM can detect and alert on failed Windows authentication attempts against an Active Directory domain controller.

### Lab Environment
- Domain: `northstar.test`
- Domain controller: `NS-DC01` — Windows Server 2025 (`192.168.100.10`)
- SIEM: `NS-SIEM01` — Wazuh 4.14 (`192.168.100.40`)
- Test account: `NORTHSTAR\smitchell`

### Test Procedure
1. Installed and registered the Wazuh Windows agent on `NS-DC01`.
2. Confirmed the agent reported an Active status in Wazuh.
3. Generated one controlled failed authentication using `runas` and an intentionally incorrect password.
4. Verified Windows Security Event ID 4625 in PowerShell.
5. Located the corresponding event in Wazuh Threat Hunting.
6. Inspected the alert metadata and classification.

### Detection Results
- Windows Event ID: **4625** — Failed logon
- Wazuh rule ID: **60122**
- Wazuh severity: **Level 5**
- Alert description: **Logon Failure — Unknown user or bad password**
- Logon type: **2** — Interactive
- Failure substatus: **0xC000006A** — Incorrect password
- Source IP: **::1** — Local IPv6 loopback
- MITRE mapping: **T1531 — Account Access Removal** (rule metadata; not evidence that this technique occurred)

### Analyst Assessment
The alert corresponded to an authorized, single-attempt authentication test performed locally on the domain controller. The failure was expected and did not indicate an actual compromise. Windows event logging, Wazuh agent collection, and SIEM alert generation were successfully validated.

### Skills Demonstrated
- Windows Security event analysis
- Active Directory authentication monitoring
- SIEM agent deployment and verification
- Wazuh alert investigation and rule interpretation
- Controlled detection testing and evidence preservation

### Evidence
A PDF containing the Wazuh alert details is stored in the `evidence/` directory.
