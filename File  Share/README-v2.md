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

![Insert Screenshot of Activity 1 Here](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/File%20%20Share/A1.png)

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

![Insert Screenshot of Activity 2 Here](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/File%20%20Share/A2.png)

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

![Insert Screenshot of Activity 3 Here](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/File%20%20Share/A3-1.png)
![q](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/File%20%20Share/A3-2.png)

---

## 🎓 Key Learnings
* The critical difference between **Share Permissions** (network-level access) and **NTFS Permissions** (local and network file-level access).
* How to transition from inefficient manual network mapping to persistent, automated distribution using **Group Policy Preferences**.

