# **Enterprise IT Infrastructure Lab | SOC/IT Support Readiness Project**

## **Project Objective & Relevance**
This project simulates a complete, security-focused enterprise IT environment built from scratch using Windows Server 2022, the lab encompasses core infrastructure services including Active Directory, Group Policy, file services, and security hardening. The goal was to move beyond theoretical knowledge and develop practical, hands-on skills in system administration, network security, and automated policy management.

Core Technologies: Windows Server 2022, Active Directory, Group Policy, VMware/VirtualBox, NTFS/Share Permissions, File Server Resource Manager (FSRM).

## **Lab Architecture & Implementation**
1. Active Directory & Identity Management
The foundation of the environment is a structured Active Directory Domain Services forest (mydomain.local), designed with scalability and security best practices in mind.

*   **Organizational Unit (OU) Structure:** Implemented a logical OU hierarchy to separate users, computers, and security groups for precise management.
    ```
    SecureCorp.Local
    ├── Corporate
    │   ├── Users
    │   │   ├── Finance           # john.doe, jane.smith
    │   │   ├── Marketing         # michael.chan, sara.jones
    │   │   └── IT                # alex.taylor
    │   ├── Computers
    │   │   ├── Finance-WS
    │   │   ├── Marketing-WS
    │   │   └── IT-WS
    │   └── Groups               # Centralized Security Groups
    │       ├── Finance_Users
    │       ├── Marketing_Users
    │       ├── IT_Users
    │       ├── FileServer_Finance_RW
    │       ├── FileServer_Marketing_RO
    │       ├── VPN_Users
    │       └── Helpdesk_Admins
    └── Admins                   # admin.carter
    ```

*   **Role-Based Access Control (RBAC):** Users in department OUs were added to corresponding security groups to control access. For example, `john.doe` in the Finance OU is a member of `FileServer_Finance_RW`, granting modify permissions to the Finance file share.

![Active Directory OU Structure](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/ad-ou-structure.png)

Role-Based Access Control (RBAC): Created security groups (e.g., FileServer_Finance_RW, VPN_Users, Helpdesk_Admins) to manage permissions centrally. User accounts were placed in department OUs and added to relevant groups, demonstrating efficient access management.

![User Group Membership for RBAC](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/user-group-membership.png)


2. Group Policy & Security Configuration
Group Policy Objects (GPOs) were deployed to enforce security baselines, configure user environments, and automate settings across the domain.

Layered GPO Strategy:

Corp_Workstation_Baseline: Applied to all computers to enable firewall and set a secure lock screen message.

Finance_Secure_Workstations: Targeted the Finance-WS OU to disable USB storage, and enable detailed file audit logging.

Corp_Drive_Maps: A centralized GPO linked to the parent Users OU. It uses Item-Level Targeting to dynamically map network drives (e.g., F: to \\WIN-EGP61EA308B\Company\Finance) only for members of specific security groups.

![Group Policy Management](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/gpo-management.png)

![GPO Item-Level Targeting](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/gpo-targeting.png)

3. Enterprise File Services
A dedicated file server role was configured to provide secure, managed storage for departmental and company-wide data.

Departmental Share Structure: Created a shared folder (\\WIN-EGP61EA308B\Company) with subfolders for Finance, Marketing, IT, and Public data.

NTFS & Share Permissions: Implemented the principle of least privilege by assigning Modify NTFS permissions to department-specific security groups (e.g., FileServer_Finance_RW). Configured restrictive Share permissions and enabled Access-Based Enumeration (ABE) so users only see folders they can access.

![NTFS Permissions](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/ntfs-permissions.png)

File Server Resource Manager (FSRM):

Quota Management: Enforced a 10 GB hard quota on the Finance share with email and event log notifications at 85% usage to prevent storage exhaustion.

File Screening: Implemented an active file screen on the Finance share to block executable, compressed, and audio/video files, reducing malware risk and ensuring business use.

![FSRM Quota](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/fsrm-quota.png)

4. Security & Operational Practices
The lab was designed with a security-first mindset, simulating real-world operational controls.

Privileged Access Management: Created a dedicated Helpdesk_Admins group with delegated permissions, avoiding the use of Domain Admin accounts for daily tasks. Applied restrictive GPOs to IT user accounts.

Auditing & Monitoring: Enabled audit policies to log file access on sensitive shares and configured logon scripts for basic access tracking.

Troubleshooting & Recovery: Practiced core helpdesk workflows, such as resolving "access denied" issues by verifying group membership and permission inheritance. Successfully performed domain controller recovery and password resets using Windows Recovery Environment tools.

## **Skills Demonstrated**
System Administration: Active Directory design, user/group management, Group Policy deployment.

Security Hardening: RBAC, least privilege, quota/file screening, audit policy, privileged access management.

Scripting & Automation: PowerShell for AD automation and task scripting.

Network Services: DHCP scope configuration, DNS, file sharing, and permissions management.

Problem-Solving: Methodical troubleshooting of authentication, permission, and Group Policy application issues.
