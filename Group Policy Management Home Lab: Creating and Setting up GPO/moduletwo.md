# Windows Server Home Lab: Group Policy Management (GPOs)

## 📌 Project Overview
This repository documents my hands-on practice with **Group Policy Management Console (GPMC)** in a Windows Server Home Lab environment. The goal of this project is to understand, create, and configure Group Policy Objects (GPOs) to enforce security rules, map resources, and manage user/computer behaviors across an Active Directory domain. 

## ⚙️ Prerequisites & Environment Setup
Before configuring GPOs, the following tools and roles were set up in the virtual machine (VM):
* **OS:** Windows Server (running on a VM)
* **Server Roles installed:** * Active Directory Domain Services (AD DS)
  * Group Policy Management Console (GPMC)
* **Organizational Units (OUs):** Configured for different regions (e.g., Asia, Europe, USA) to target specific policies.

![Server Manager - Adding GPMC Feature](path/to/your/gpmc-install-screenshot.png)

## 🧠 Core GPO Concepts Learned
* **Computer Configuration:** Applies policies to the local machine, regardless of who logs in.
* **User Configuration:** Applies policies directly to the user account, no matter which machine they log into.
* **Policies vs. Preferences:**
  * **Policies:** Enforced by the admin. Users *cannot* change them (e.g., minimum password length).
  * **Preferences:** Set as a default by the admin, but users *can* alter them later (e.g., creating a default mapped network drive).

---

## 🛠️ Configured Group Policies (GPOs)

Below are the 5 main Group Policies and 1 bonus policy created and configured within this lab.

### 1. Password Policy 🔐
* **Objective:** Enforce strong passwords and enhance domain security.
* **Type:** Computer Configuration -> Policies
* **Path:** `Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Password Policy`
* **Settings Applied:**
  * Minimum password length: 8 characters.
  * Password must meet complexity requirements: Enabled.
  * Maximum password age: 31 days.
  * Minimum password age: 30 days.
  
![Password Policy Configuration](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/Group%20Policy%20Management%20Home%20Lab%3A%20Creating%20and%20Setting%20up%20GPO/Password-Policy/P1.png)

### 2. Drive Mapping 📂
* **Objective:** Automatically map a network drive for users when they log into their accounts.
* **Type:** User Configuration -> Preferences
* **Path:** `User Configuration > Preferences > Windows Settings > Drive Maps`
* **Settings Applied:**
  * Created a new Map Drive mapping to a specific network share path (`\\driveone\img`).
  * Assigned a specific drive letter (e.g., `E:`).
  
![Drive Mapping Configuration](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/Group%20Policy%20Management%20Home%20Lab%3A%20Creating%20and%20Setting%20up%20GPO/D1.png)

### 3. Desktop Wallpaper Policy 🖼️
* **Objective:** Set a standard, non-changeable corporate desktop wallpaper for all users.
* **Type:** User Configuration -> Policies
* **Path:** `User Configuration > Policies > Administrative Templates > Desktop > Desktop`
* **Settings Applied:**
  * Enabled "Desktop Wallpaper".
  * Specified the local/network path to the target image and set the style to "Fill".
  
![Wallpaper Policy Configuration](path/to/your/wallpaper-screenshot.png)

### 4. Restrict Access to Control Panel 🚫
* **Objective:** Prevent end-users from accessing the Control Panel to prevent unauthorized system modifications.
* **Type:** User Configuration -> Policies
* **Path:** `User Configuration > Policies > Administrative Templates > Control Panel`
* **Settings Applied:**
  * Prohibit access to Control Panel and PC settings: Enabled.
  
![Restrict Control Panel Configuration](https://github.com/Gaurav-s-tech/ActiveDirectoryHomeLab/blob/main/Group%20Policy%20Management%20Home%20Lab%3A%20Creating%20and%20Setting%20up%20GPO/R1.png)

---

## 🚀 Next Steps
The next phase of this home lab involves setting up client VMs (e.g., Windows 10/11), joining them to the domain, and practically testing to ensure these enforced GPOs work properly on end-user environments.

## 📝 Acknowledgments
* Video Tutorial: [Group Policy Management Home Lab by East Charmer](https://youtu.be/6F7-tV4lYFw)
