# Windows Server Homelab: File Services & Network Sharing

## 📝 Project Overview
This project focuses on the implementation and management of File Services within a Windows Server Active Directory environment. The lab demonstrates how to establish centralized network storage, manage access permissions, automate resource delivery using Group Policy, and implement storage limits and security policies using File Server Resource Manager (FSRM).

## 🛠️ Technologies & Tools Used
* **Operating Systems:** Windows Server, Windows 10/11 Client
* **Roles & Features:** Active Directory Domain Services (AD DS), File and Storage Services, File Server Resource Manager (FSRM)
* **Management Tools:** Group Policy Management Console (GPMC), File Explorer Properties (Security & Sharing tabs)

## 📋 Prerequisites
* A running instance of Windows Server with Active Directory Domain Services (AD DS) installed and configured.
* A Windows Client machine joined to the domain.
* Domain User accounts set up for testing.
* Basic understanding of Group Policy Objects (GPOs).

---

## 🚀 Lab Activities & Step-by-Step Implementation

### Activity 1: Configuring File Sharing, Shared Permissions, and NTFS Permissions
The goal of this activity was to create a centralized folder on the server and make it accessible to users on the network while establishing two layers of security: Share Permissions and NTFS Permissions.

**Steps Taken:**
1. **Create the Folder:** On the Windows Server, navigated to the local `C:` drive and created a new folder named `shared`.
2. **Set Share Permissions:** * Right-clicked the folder > **Properties** > **Sharing** tab > **Advanced Sharing**.
   * Checked **"Share this folder"**.
   * Clicked **Permissions**. Removed default settings and added **Domain Users** with **Read-only** access. *(Share permissions act as the "front door" to the folder over the network).*
3. **Set NTFS Permissions:**
   * Navigated to the **Security** tab in the folder properties.
   * Verified/Configured granular access for Domain Users. *(NTFS permissions apply whether the user is accessing the file over the network OR locally on the server).*

> **[Insert Screenshot of Activity 1 Here]**
*(Tip: Replace this line with `![Activity 1](path/to/screenshot1.png)` showcasing the folder properties, sharing tab, or NTFS security settings)*

---

### Activity 2: Manual Network Drive Mapping on Client
This activity tested if a client machine could access the newly created shared folder and map it as a local drive letter, while demonstrating the limitations of manual mapping.

**Steps Taken:**
1. **Log into the Client:** Switched to the Windows client VM and logged in as a standard Domain User.
2. **Access Map Network Drive:** Opened File Explorer, right-clicked **This PC**, and selected **Map network drive**.
3. **Configure the Drive:**
   * Chose the `S:` drive letter (for Shared).
   * Entered the UNC path to the server: `\\YourServerName\shared`.
   * Unchecked "Reconnect at sign-in" and finalized the connection. 
4. **Demonstrate Limitation:** Rebooted the client machine to demonstrate that mapping a drive manually is *not persistent*. Upon reboot, the `S:` drive disappeared, proving that manual configuration is inefficient for daily organizational use.

> **[Insert Screenshot of Activity 2 Here]**
*(Tip: Replace this line with `![Activity 2](path/to/screenshot2.png)` showcasing the mapped network drive visible under "This PC")*

---

### Activity 3: Automating Drive Mapping via Group Policy Object (GPO)
To solve the disappearing drive problem from Activity 2, a Group Policy Object (GPO) was created to automatically map the drive for users every time they log in.

**Steps Taken:**
1. **Open GPMC:** On the Windows Server, opened the **Group Policy Management Console**.
2. **Create the GPO:** Created a new GPO named "Map Drives" under the Group Policy Objects container.
3. **Edit the Policy:**
   * Edited the GPO and navigated to **User Configuration** -> **Preferences** -> **Windows Settings** -> **Drive Maps**.
   * Created a **New Mapped Drive** targeting the UNC location (`\\YourServerName\shared`), labeled it "shared", and assigned the `S:` drive letter.
4. **Link the GPO:** Dragged and dropped the "Map Drives" GPO onto the **Users OU** so it actively applies to the domain users.
5. **Test on Client:** Switched back to the client VM, ran `gpupdate /force` in the Command Prompt, and rebooted. Upon logging back in, the `S:` drive automatically appeared, proving the automation was successful.

> **[Insert Screenshot of Activity 3 Here]**
*(Tip: Replace this line with `![Activity 3](path/to/screenshot3.png)` showcasing the GPO configuration in GPMC or the automatically mapped drive surviving a client reboot)*

---

### Activity 4: Implementing Quotas and File Screening (FSRM)
Since server hard drives have limited capacity, this activity focused on restricting how much data users can save and what type of files they are allowed to store.

**Steps Taken:**
1. **Install FSRM:** Using Server Manager, installed the **File Server Resource Manager (FSRM)** feature under File and Storage Services.
2. **Configure a Quota (Size Limit):**
   * Opened FSRM and navigated to **Quota Management** > **Quotas**.
   * Created a quota targeting the `C:\shared` folder.
   * Applied a custom hard limit (e.g., 10GB maximum capacity).
   * Set a warning threshold at 80% to automatically trigger an email alert to the IT Admin team before the drive completely fills up.
3. **Configure a File Screen (File Type Limit):**
   * Navigated to **File Screening Management** > **File Screens**.
   * Created a file screen targeting `C:\shared`.
   * Configured custom properties to block **Audio and Video files**, **Executable files**, and **Image files**. This ensures the shared network drive is strictly used for text and productivity documents, preventing users from filling up expensive storage with heavy media.

> **[Insert Screenshot of Activity 4 Here]**
*(Tip: Replace this line with `![Activity 4](path/to/screenshot4.png)` showcasing the FSRM console with the active Quota or File Screen applied)*

---

## 🎓 Key Learnings
* The critical difference between **Share Permissions** (network-level access) and **NTFS Permissions** (local and network file-level access).
* How to transition from inefficient manual network mapping to persistent, automated distribution using **Group Policy Preferences**.
* The importance of proactive storage management using **FSRM** to establish quotas and block unnecessary or malicious file types from clogging up network storage.

## 🔗 References
* [File Services Homelab: Setting up Network Sharing on Windows Server (Ep. 4)](https://youtu.be/OBnuOOWdEmc?si=Jqn6tsFR6JkMV0_s)
