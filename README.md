# Configuring-On-premises-Active-Directory-with-Virtual-Box
Tutorial: Configure an on‑premises Active Directory domain in VirtualBox (Win Server 2022 &amp; Win 10)
## 🛠️ Environments & Technologies Used

- **Oracle VirtualBox** (Virtual Machines)  
- **Windows Server 2022** (Domain Controller)  
- **Windows 10** (Client Workstation)  
- **Active Directory Domain Services (AD DS)**  
- **Remote Desktop Protocol (RDP)**  
- **PowerShell & MMC Snap‑ins** (ADUC, Local Users & Groups)

- ### 1️⃣ Create Organizational Units

1. On your **Domain Controller VM**, open **Server Manager** → **Tools** → **Active Directory Users and Computers (ADUC)**.  
2. Right‑click your domain (e.g. `contoso.local`) → **New** → **Organizational Unit**.  
3. Create these OUs: **Administration**, **Research**, **Sales**.  
<img width="1917" height="1076" alt="SS1 - New Organizational Units in savn local" src="https://github.com/user-attachments/assets/ac473107-7a6d-49c8-8adf-3afaca247615" />

### 2️⃣ Add a Local User Account

1. On any Windows Server (before AD DS is installed)—or on the DC itself—press **Win + R**, type `lusrmgr.msc`, press **Enter**.  
2. In **Local Users and Groups**, right‑click **Users** → **New User…**.  
3. Set **User name:** `localuser01`, **Password:** _choose a secure password_, leave **User must change password at next logon** checked.  
4. Click **Create** → **Close**.
   <img width="1916" height="1077" alt="SS2 - Local user account created" src="https://github.com/user-attachments/assets/b48198fc-9a36-4ef7-858a-6a2ce513ea77" />

### 3️⃣ Add a Domain User Account

1. Back in **ADUC** on the **DC**, expand your domain → click **Users**.  
2. Right‑click **Users** → **New** → **User**.  
3. Fill in **User logon name:** `domainUser01` → **Next**.  
4. Assign a password, check **Password never expires** → **Next** → **Finish**.
<img width="1918" height="1077" alt="SS3 - New domain user account created" src="https://github.com/user-attachments/assets/888f9e96-bf11-4d89-bde4-c24d3a5b200d" />

### 4️⃣ Move the Domain User to an OU

1. In **ADUC**, find **domainUser01** under **Users**.  
2. Right‑click **domainUser01** → **Move** → select **Administration** OU → **OK**.
   <img width="1918" height="1070" alt="SS4 - domainUser01 account located in Administration OU" src="https://github.com/user-attachments/assets/dae7bac5-c10d-4773-ab85-389542e5c888" />

### 5️⃣ Log In as the Local User

1. On your **Client VM** (or on the DC before domain join), sign out → click **Other User**.  
2. Log in with `.\localuser01` and your password.  
3. Open **Task Manager** (Ctrl + Shift + Esc) → **Users** tab → confirm **localuser01** is active. 
<img width="1918" height="1072" alt="SS5 - LocalUser01 is logged in to MA-SUM25-s22-2 as a local (non-domain) user" src="https://github.com/user-attachments/assets/0b808d8a-bb60-42d9-ba8f-52ae6c24b374" />

### 6️⃣ Log In as the Domain User

1. **Join** your Windows 10 VM to the domain:  
   - **Settings** → **System** → **About** → **Rename this PC (Advanced)** → **Change…** → select **Domain**, enter `<your-domain>.local` → **OK** → reboot.  
2. At the lock screen, click **Other User**, enter `domainUser01@<your-domain>.local` or `YOURDOMAIN\\domainUser01` and your password.  
3. In **Task Manager** → **Users** tab → confirm **domainUser01** is active.  
<img width="1918" height="1075" alt="SS6 - Domain user logged into a system in the domain" src="https://github.com/user-attachments/assets/a7546290-8bfc-473c-bbf1-c897dc1cb7a4" />

### 7️⃣ Test Domain Login When DC Is Offline

1. **Power off** your DC VM (simulate a DC/DNS outage).  
2. On the client VM, attempt to log in as **domainUser01** again.  
3. You should see an error: **“Your domain isn’t available”**.  
<img width="1918" height="1078" alt="SS7 - Attempted domain login when domain server is unavailable" src="https://github.com/user-attachments/assets/3c7be7d6-abad-4685-aab4-87e83b6e4b25" />


