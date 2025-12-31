# Active Directory Home Lab Setup Guide

This documentation provides a step-by-step guide to setting up a virtualized Active Directory environment using VMware Workstation Pro and Windows Server 2022.

## Prerequisites

Before starting, ensure your host environment meets the following requirements:

* **Hardware:**
    * RAM: At least 8–16 GB recommended.
    * CPU: Modern processor with virtualization support enabled in BIOS.
* **Connectivity:** Internet access for downloading software images.
* **Accounts:** * [Broadcom Account](https://knowledge.broadcom.com/external/article?articleNumber=368667) (for VMware).
    * [Microsoft Account](https://signup.live.com/) (for ISO download).

---

## Step-by-Step Documentation

### 1. Install VMware Workstation Pro
1.  Navigate to the **Broadcom support page**: [Download VMware Workstation Pro](https://knowledge.broadcom.com/external/article?articleNumber=368667).
2.  Register or log in to download the installer (Note: It is free for personal use).
3.  Run the installer and follow the on-screen instructions to complete the installation.

### 2. Download Windows Server 2022 ISO
1.  Visit the **Microsoft Evaluation Center**: [Download Windows Server 2022](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2022).
2.  Register or sign in with your Microsoft account.
3.  Select **Windows Server 2022 (Standard or Datacenter Evaluation)**.
4.  Download the **ISO** file to your computer.

### 3. Create a New Virtual Machine in VMware
1.  Open **VMware Workstation Pro**.
2.  Click **File > New Virtual Machine** (or launch the wizard from the home screen).
3.  Select **Typical** configuration.
4.  Choose **Installer disc image file (iso)** and browse to select the Windows Server 2022 ISO you downloaded in Step 2.
5.  Select Guest OS:
    * **Microsoft Windows** > **Windows Server 2022**.
6.  Name the VM (e.g., `AD-HomeLab`) and choose a storage location on your drive.
7.  **Allocate Resources:**
    * **CPU:** 2–4 cores.
    * **Memory:** 4–8 GB minimum (allocate more if host resources allow, e.g., 20 GB).
    * **Hard Disk:** 60–100 GB (default settings are usually sufficient).
8.  Review your settings and click **Finish**.

### 4. Install Windows Server 2022 on the VM
1.  **Power on** the Virtual Machine.
2.  **Important:** Immediately click inside the VM window to capture your mouse/keyboard, then press **any key** when prompted to boot from the CD/DVD (ISO).
3.  **Windows Setup Wizard:**
    * Select your Language and Timezone, then click **Next**.
    * Click **Install now**.
4.  **Select Operating System:**
    * Choose **Windows Server 2022 Standard Evaluation (Desktop Experience)**.
    * *Note: Ensuring you select "Desktop Experience" provides the full GUI.*
5.  Accept the license terms.
6.  Installation Type: Select **Custom: Install Windows only (advanced)**.
7.  Create/Format partitions if necessary (or select the unallocated space) and click **Next**.
8.  Wait for the installation to complete. The VM will restart multiple times.
9.  **Post-Installation:**
    * Set a secure **Administrator password** when prompted.
    * Log in using the Administrator account to access the desktop.


 ### 5. Install Active Directory Domain Services (AD DS)
1.  Open **Server Manager** (it usually launches automatically upon login, or search for it in the Start menu).
2.  Click **Manage** in the top-right corner, then select **Add Roles and Features**.
3.  **In the Wizard:**
    * Select **Role-based or feature-based installation** and click Next.
    * Ensure your local server is selected in the server pool.
    * Under **Server Roles**, check the box for **Active Directory Domain Services**.
    * When prompted, click **Add Features** to include required tools (e.g., Group Policy Management).
    * Click **Next** through the remaining screens and click **Install**.
    * *Note: No reboot is required immediately after this step.*

![image alt](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/014e62d64444955d8355239e6706b7c6d5b5f266/Installing%20Active%20Directory%20on%20Windows%20Server%20on%20Virtual%20Machine%20(Home%20Lab)/images/One.png)
      

      



### 6. Promote the Server to a Domain Controller
1.  In **Server Manager**, look for the **notification flag** (yellow triangle) in the top menu.
2.  Click it and select **Promote this server to a domain controller**.
3.  **Deployment Configuration:**
    * Select **Add a new forest**.
    * Enter a **Root domain name** (e.g., `techcharmer.local`).
    * *Tip: Using `.local` is standard practice for internal home labs to avoid conflicts with real websites.*
4.  **Domain Controller Options:**
    * Set the **Forest** and **Domain functional levels** (e.g., Windows Server 2016).
    * Create and confirm a **Directory Services Restore Mode (DSRM)** password (keep this safe).
5.  **Review & Install:**
    * Click **Next** through the DNS Options and Additional Options (you can ignore the "delegation for this DNS server" warning).
    * Review your selections and click **Install**.
6.  The VM will automatically **reboot** upon completion.
7.  **Log In:**
    * After the reboot, log in using your new domain credentials.
    * Format: `DOMAIN\Administrator` (e.g., `TECHCHARMER\Administrator`).

    ![image alt](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/8b40d1ea69274d9df02af8d9c02e8b0ee2c754c7/Installing%20Active%20Directory%20on%20Windows%20Server%20on%20Virtual%20Machine%20(Home%20Lab)/images/Two2.png)



### 7. Basic Active Directory Configuration Using ADUC
1.  Open **Active Directory Users and Computers (ADUC)** via the Start menu or under *Windows Administrative Tools*.
2.  Expand your domain tree (e.g., `techcharmer.local`) in the left pane.

#### Create Organizational Units (OUs)
* **Create Top-Level OUs:**
    1.  Right-click the domain name > **New** > **Organizational Unit**.
    2.  Name it (e.g., `USA`, `Europe`, or `Asia`).
    3.  Ensure "Protect container from accidental deletion" is checked.
* **Create Sub-OUs:**
    1.  Right-click a top-level OU (e.g., `USA`) > **New** > **Organizational Unit**.
    2.  Name it (e.g., `Users` or `Computers`).

![image alt](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/8c9bbbf92d2cc4825ce5cf094923663cba253b5e/Installing%20Active%20Directory%20on%20Windows%20Server%20on%20Virtual%20Machine%20(Home%20Lab)/images/Three3.png)

#### Create Groups
1.  Right-click the target OU (e.g., `Users`) > **New** > **Group**.
2.  Enter a **Group name** (e.g., `IT Admins`).
3.  **Group Scope/Type:**
    * **Scope:** Commonly set to **Global**.
    * **Type:** Select **Security** (for permissions) or **Distribution** (for email lists only).
4.  Click **OK**.

#### Create Users
1.  Right-click the target OU > **New** > **User**.
2.  Enter the **Full name** and **User logon name** (e.g., `jdoe`).
3.  Click **Next** and set a password.
    * *Optional:* specific settings like "User must change password at next logon" or "Password never expires" (useful for service accounts in labs).
4.  Click **Finish**.
5.  **Add User to Group:**
    * Double-click the new user to open properties.
    * Go to the **Member Of** tab > **Add**.
    * Type the group name (e.g., `IT Admins`) and click **Check Names**, then **OK**.     
