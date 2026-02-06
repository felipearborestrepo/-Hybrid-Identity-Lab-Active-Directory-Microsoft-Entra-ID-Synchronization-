# 🧩 Hybrid Identity Lab: Active Directory → Microsoft Entra ID Synchronization  
**Focus:** Identity lifecycle foundation (Joiner)  
**Automation:** None (intentional)  
**Source of Authority:** On-prem Active Directory  

---

## 📘 Project Overview

In this project, I designed and implemented a **hybrid identity environment from the ground up**, starting with a clean Windows Server virtual machine and ending with **users and security groups synchronized into Microsoft Entra ID**.

The objective was to demonstrate how **Active Directory functions as the authoritative identity source** and how **Microsoft Entra Connect Sync** propagates identity and access objects into the cloud — without automation, scripts, or Conditional Access.

This mirrors how many organizations establish identity governance foundations before introducing lifecycle automation.

---

## 🎯 Objectives

- Build a Windows Server domain controller from scratch  
- Deploy Active Directory Domain Services and create a forest  
- Install Microsoft Entra Connect Sync  
- Design OU and security group structure  
- Provision users in Active Directory  
- Assign access using security groups  
- Prove successful synchronization into Entra ID  

---

## 🏗️ Environment

- Azure-hosted Windows Server VM  
- Windows Server (Domain Controller)  
- Active Directory Domain Services (`lab.local`)  
- Microsoft Entra ID tenant  
- Microsoft Entra Connect Sync (Express configuration)  

---

## ☁️ Phase 0 — Virtual Machine & Domain Foundation

I began by creating a **Windows Server virtual machine in Azure**, which served as both the **domain controller** and the **Entra Connect Sync server**.

Once provisioned, I:
- Renamed the server to align with domain controller naming standards  
- Configured DNS to point to the local server  
- Prepared the system for directory services  

I then installed **Active Directory Domain Services (AD DS)** and promoted the server to a domain controller by creating a new forest:

lab.local

After promotion and reboot, I verified:
- Domain authentication was active  
- Active Directory Users and Computers loaded correctly  
- The server was operating as the primary domain controller  

📸 **Evidence**

![Image 2-5-26 at 22 07](https://github.com/user-attachments/assets/c2f16beb-e974-426c-b26d-aeb55be2e8be)

![Image 2-5-26 at 22 14](https://github.com/user-attachments/assets/5e96572f-c919-4bc7-be91-ecd60021df50)

![Image 2-5-26 at 22 15](https://github.com/user-attachments/assets/db51174b-ba6d-49f9-9af6-c8ec1f935f65)

![Image 2-5-26 at 22 17](https://github.com/user-attachments/assets/878693b3-7d32-40fc-b659-5c8f710dc872)

---

## 🔗 Phase 1 — Hybrid Sync Foundation (Entra Connect)

With the domain in place, I deployed **Microsoft Entra Connect Sync** to establish the identity bridge between on-prem Active Directory and Microsoft Entra ID.

I installed Entra Connect using **Express Settings**, enabling:
- Password Hash Synchronization  
- Automatic configuration of the sync engine  
- Active Directory as the source of authority  

During setup, I:
- Authenticated to Entra ID using a Global Administrator account  
- Connected Entra Connect to on-prem AD using a domain administrator account  

After installation, I validated the sync engine using **Synchronization Service Manager**, confirming that both the Active Directory and Azure AD connectors were active.

📸 **Evidence**

![Image 2-5-26 at 22 25](https://github.com/user-attachments/assets/3e7f9a8b-88bf-4ffc-814c-783939822ccd)

![Image 2-5-26 at 22 27](https://github.com/user-attachments/assets/18458492-0a56-4c21-af3c-eec4942c5f2d)

![Image 2-5-26 at 22 34](https://github.com/user-attachments/assets/655f6321-c037-4618-8460-5fd2f4c7de42)

----


## 🗂️ Phase 2 — Organizational Unit Design

I designed a simple but realistic **OU structure** to logically separate identities and prepare the environment for lifecycle governance.

Created OUs:
- `OU_HR`  
- `OU_Finance`  
- `OU_IT`  
- `OU_Groups`  

Each OU was protected from accidental deletion to reflect enterprise best practices.

📸 **Evidence**

![Image 2-5-26 at 23 27](https://github.com/user-attachments/assets/3117eb51-74f0-407b-b8b4-34a29d2e776c)

---

## 🔐 Phase 3 — Security Group Design

To represent access control independently from organizational structure, I created **global security groups** inside a dedicated groups OU.

Groups created:
- `AD-HR-Users`  
- `AD-Finance-Users`  
- `AD-IT-Admins`  

These groups model role-based access control commonly used for application assignment and policy targeting.

📸 **Evidence**

![Image 2-5-26 at 23 30](https://github.com/user-attachments/assets/a30438fd-449c-47f0-aea6-7dd85ae3b5c3)

![Image 2-5-26 at 23 31](https://github.com/user-attachments/assets/f0047087-d4ea-4134-9cf2-5ef070bad723)

![Image 2-5-26 at 23 31](https://github.com/user-attachments/assets/0901002f-1d79-4b29-bcd8-12b781026dc4)

---

## 👤 Phase 4 — User Provisioning (Joiner)

I provisioned test users directly in their respective OUs to simulate onboarding events.

Users created:
- **Felipe-HR1** → `OU_HR`  
- **Felipe-FIN1** → `OU_Finance`  
- **Felipe-IT1** → `OU_IT`  

Passwords were set for lab validation, and forced password changes at first logon were disabled to simplify testing.

📸 **Evidence**

![Image 2-5-26 at 23 35](https://github.com/user-attachments/assets/07c3d393-cb17-499c-89bb-af9639eeb564)

![Image 2-5-26 at 23 36](https://github.com/user-attachments/assets/80884020-8c8b-4ea1-979a-9305ba9db894)

![Image 2-5-26 at 23 37](https://github.com/user-attachments/assets/dcc94bf0-9200-4ec5-8c4d-27ccfb808f13)

---

## 🔑 Phase 5 — Manual Access Assignment

To establish baseline identity governance without automation, I manually assigned users to their corresponding security groups:

- `Felipe-HR1` → `AD-HR-Users`  
- `Felipe-FIN1` → `AD-Finance-Users`  
- `Felipe-IT1` → `AD-IT-Admins`  

This models how access is commonly granted during early-stage IAM implementations.

📸 **Evidence**

![Image 2-5-26 at 23 38](https://github.com/user-attachments/assets/57b427c8-014e-4db7-9e48-2b5422a6b6e2)

![Image 2-5-26 at 23 38](https://github.com/user-attachments/assets/31541f76-e30a-460c-a262-3c83ee281fda)

![Image 2-5-26 at 23 38](https://github.com/user-attachments/assets/36f25640-0c31-4590-adf0-93d5b0165fc3)

---

## 🔄 Phase 6 — Synchronization to Microsoft Entra ID

Using **Synchronization Service Manager**, I manually triggered synchronization cycles to propagate identity objects into Entra ID.

Sync cycles executed:
- Delta Import (Active Directory connector)  
- Delta Synchronization  
- Export (Azure AD connector)  

All operations completed successfully.

📸 **Evidence**

![Image 2-5-26 at 23 40](https://github.com/user-attachments/assets/5588670f-d0e9-4d16-916c-3355ad468604)

---

## ☁️ Phase 7 — Entra ID Validation (Final Proof)

I validated that all objects synchronized correctly into Microsoft Entra ID.

### Users
- `felipe.hr1`  
- `felipe.fin1`  
- `felipe.it1`  

Each user appeared in Entra ID with **Windows Server AD** listed as the source.

### Groups
- `AD-HR-Users`  
- `AD-Finance-Users`  
- `AD-IT-Admins`  

Each group synchronized with correct membership.

📸 **Evidence**

![Image 2-5-26 at 23 41](https://github.com/user-attachments/assets/9dc27a80-4d8b-44de-a658-66d2e12362a7)

![Image 2-5-26 at 23 42](https://github.com/user-attachments/assets/0db24057-abfa-4134-b6eb-a960d3d6f33f)

---

## 🧠 What This Demonstrates

This project demonstrates core **Identity & Access Management fundamentals**:

- Building Active Directory from scratch  
- Establishing AD as the identity source of truth  
- Designing OU and security group structures  
- Deploying and validating Entra Connect Sync  
- Synchronizing users and access groups into Entra ID  
- Foundational Joiner lifecycle handling without automation  

This environment provides the groundwork for **Mover/Leaver workflows, Conditional Access, and identity automation**.
