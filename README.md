# **Enterprise IT Infrastructure Lab | SOC/IT Support Readiness Project**

## **Project Objective & Relevance**
This project simulates a complete, security-focused enterprise IT environment. Built from scratch using Windows Server 2022, it encompasses core infrastructure services including Active Directory, Group Policy, file services, and security hardening. The goal was to develop practical, hands-on skills in system administration and security, moving beyond theoretical knowledge.

Core Technologies: Windows Server 2022, Active Directory, Group Policy, VMware, NTFS/Share Permissions, File Server Resource Manager (FSRM).

## **Lab Architecture & Implementation**
1. Active Directory & Identity Management
The foundation is a structured Active Directory Domain Services forest (`mydomain.local`), designed for security and manageability.

*   **Organizational Unit (OU) Structure:** Implemented a logical OU hierarchy to separate users, computers, and security groups for precise management and policy targeting, as shown in the screenshot below.
    ![Active Directory OU Structure](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/ad-ou-structure.png)

*   **Role-Based Access Control (RBAC) Implementation:** This structure enabled a role-based access model. Users placed in department OUs (e.g., `john.doe` in Finance) were added to specific security groups (e.g., `FileServer_Finance_RW`) to grant permissions to resources like file shares. The image below shows the group membership for a sample user.
    ![User Group Membership for RBAC](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/user-group-membership.png)
    

2. Group Policy & Security Configuration
Group Policy Objects (GPOs) were deployed to enforce security baselines, configure user environments, and automate settings across the domain.

*   **Layered GPO Strategy:**
    *   **`Corp_Workstation_Baseline`:** Applied to all computers to establish fundamental security by enabling the Windows Firewall (Domain profile) and disabling the built-in Windows Guest account.
    *   **`Finance_Secure_Workstations`:** Targeted the `Finance-WS` OU to enforce stricter controls, including disabling USB removable storage drives and enabling detailed audit logging for file access.
    *   **`User_Password_Policy`:** Enforced company-wide password rules, including a minimum length of 8 characters, a history of 4 remembered passwords, and a maximum password age of 60 days.
    *   **`IT_Admin_Restrictions`:** Applied to IT user accounts to enhance operational security by denying "logon as a batch job" rights and preventing the installation of kernel-mode printer drivers.
    *   **`Corp_Drive_Maps`:** A centralized GPO linked to the parent `Users` OU. It uses **Item-Level Targeting** to dynamically map network drives (e.g., `F:` to `\\SERVER\Company\Finance`) only for members of specific security groups.

![Group Policy Management](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/gpo-management.png)

![GPO Item-Level Targeting](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/gpo-targeting.png)

3. Enterprise File Services
A dedicated file server role was configured to provide secure, managed storage for departmental and company-wide data.

Departmental Share Structure: Created a shared folder (\\WIN-EGP61EA308B\Company) with subfolders for Finance, Marketing, IT, and Public data.

NTFS & Share Permissions: Implemented the principle of least privilege by assigning Modify NTFS permissions to department-specific security groups (e.g., FileServer_Finance_RW). Configured restrictive Share permissions.

![NTFS Permissions](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/ntfs-permissions.png)

File Server Resource Manager (FSRM):

Quota Management: Enforced a 10 GB hard quota on the Finance share with email and event log notifications at 85% usage to prevent storage exhaustion.

File Screening: Implemented an active file screen on the Finance share to block executable, compressed, and audio/video files, reducing malware risk and ensuring business use.

![FSRM Quota](https://github.com/Shahd34/Enterprise-IT-Infrastructure-Lab/raw/main/images/fsrm-quota.png)

4. Security & Operational Practices
The lab was designed with a security-first mindset, simulating real-world operational controls.

Security Principle Application: Practiced core security concepts throughout the lab, including implementing Role-Based Access Control (RBAC) via security groups for file shares and enforcing the principle of least privilege through NTFS permissions and restrictive GPOs for IT accounts.

Auditing & Monitoring: Enabled audit policies to log file access on sensitive shares.
