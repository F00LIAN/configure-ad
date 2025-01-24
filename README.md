<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines<br />

<h1>Familiar Use Case</h1>
You go to a university and you can log into any computer on campus with the same username and password. This is because all of those computers are joined to the Active Directory "Domain".<br />

<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1
- Step 2
- Step 3
- Step 4

<h2>Installation Steps</h2>

### 1. Create Resource Group and Virtual Network
<p>
<img src="https://github.com/user-attachments/assets/e561a859-d224-4ab0-aa21-a8ad9e9655fb" height="60%" width="60%" alt="Azure VM image"/>
</p>
<p>
Create a Windows 10 virtual machine in Microsoft Azure.
  
Use a dedicated resource group "active-directory-lab" for connection purposes.

Create a virtual network under the new resource group and call it "active-directory-vnet" under East US.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/ff64c3f6-01c4-4cb2-b457-e5ddfef53836" height="60%" width="60%" alt="Azure VM image"/>
</p>
<p>
Create a Windows 10 virtual machine in Microsoft Azure, call it "DC-1" under East US.
  
Make sure to use the dedicated resource group "active-directory-lab".

For image use "Windows Server 2022 Datacenter: Azure Edition". Make sure to enable licensing and RDP/SSH.

Under Networking make sure to select the Active-Directory-Vnet we created. Then Create. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d1c31d13-9136-410e-90f1-4b9ccb6b089a" height="60%" width="60%" alt="Azure VM image"/>
</p>
<p>
Create a Windows 10 virtual machine in Microsoft Azure, call it "client-1" under East US.
  
Make sure to use the dedicated resource group "active-directory-lab".

For image use "Windows 10 Pro". Make sure to enable RDP/SSH and licensing.

Under Networking make sure to select the Active-Directory-Vnet we created. 
</p>
<br />

### 2. Set Domain Controller IP Addressing to Static and Disable Windows Firewall (For Lab Purposes)
<p>
<img src="https://github.com/user-attachments/assets/f56d858f-5145-4693-9f45-e0265931976a" height="60%" width="60%" alt="DNS IP Addressing"/>
</p>
<p>
Set Virtual NIC to become static. 

On Azure go to VM dc-1 ---> Network Settings ---> Click Virtual NIC ---> Click ipconfig1 and set Private IP address settings to static.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/71becd86-5d4a-4a01-837b-3d13998dab19" height="60%" width="60%" alt="DNS IP Addressing"/>
</p>
<p>
Login to the domain controller (dc-1).

Go to "run" and open windows firewall by entering "wf.msc".

Click "Properties" and turn it off. Make sure to apply changes. 
</p>
<br />

### 3. Set Client-1 VM DNS Settings and Point it to DC-1 IP Address
<p>
<img src="https://github.com/user-attachments/assets/6c7cb46a-da29-4583-ba88-c79a0ceac911" height="60%" width="60%" alt="Set Client 1 DNS"/>
</p>
<p>
Go to Client-1 VM NIC interface. 

On Azure go to VM client-1 ---> Network Settings ---> Click Virtual NIC ---> Click DNS Servers ---> Change from "Inherent from Virtual Network" to Custom and Paste dc-1 private IP-address.

Restart client-1 VM.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/aa8ad062-7c0e-457a-9e1c-67117e122ebd" height="60%" width="60%" alt="Set Client 1 DNS"/>
</p>
<p>
Login to client-1 VM via remote desktop. 

Open terminal and ping dc-1 private address. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/aa4bbfca-8719-4f3d-a6d8-0a539f12fbf8" height="60%" width="60%" alt="Set Client 1 DNS"/>
</p>
<p>
Enter the command "ipconfig /all""

Verify the DNS Servers is the correct IP Address for DNS Servers "dc-1".
</p>
<br />

### 4. Install Prerequisite Programs
<p>
<img src="https://github.com/user-attachments/assets/c5f6d44b-c41b-433b-9b5c-a3ef4554c41d" height="90%" width="90%" alt="Install Programs"/>
</p>
<p>
Install PHP Manager for IIS.
  
Install Rewrite Module.

Extract and move the "php-7.3.8-nts-Win32-VC15-x86.zip" file to C:\PHP.

Install VC_redist.x86.exe.

Install MySQL 5.5.62.
</p>
<br />

### 5. Configure MySQL Server
<p>
<img src="https://github.com/user-attachments/assets/b4e62953-c03c-42f9-93a8-9cf45806a780" height="80%" width="80%" alt="Configuring MySQL"/>
</p>
<p>
Run the MySQL installer and follow the default setup process.
  
Launch the Configuration Wizard and select Standard Configuration.

Create a root username and password (both set to "root" for this setup).
</p>
<br />

### 6. Configure PHP in IIS
<p>
<img src="https://github.com/user-attachments/assets/5287aa0c-bc96-44be-8ba1-2dd576f20266" height="80%" width="80%" alt="Register PHP Version on IIS Manager"/>
</p>
<p>
Open IIS Manager → PHP Manager.
  
Click Register new PHP version and navigate to C:\PHP\php-cgi.exe.

Restart IIS using the left-side menu in IIS Manager.
</p>
<br />

### 7. Install osTicket
<p>
<img src="https://github.com/user-attachments/assets/5401ec68-6d70-4b68-b9ab-276bf25cb98b" height="80%" width="80%" alt="Installation Steps"/>
</p>
<p>
Extract osTicket-v1.15.8.zip and copy the upload folder to C:\inetpub\wwwroot.
  
Rename the upload folder to osTicket.

Restart IIS.
</p>
<br />

### 8. Configure IIS for osTicket
<p>
<img src="https://github.com/user-attachments/assets/723822ba-1131-45b9-a821-794467a77d7c" height="80%" width="80%" alt="Installation Steps"/>
</p>
<p>
In IIS Manager, navigate to Sites → Default Web Site → osTicket.
  
Click *Browse :80 (http) to verify the installation.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/0bfba6da-35f7-47d0-9ea4-a5cf1b416740" height="80%" width="80%" alt="os-ticket-extensions"/>
</p>
<p>
In IIS Manager, navigate to Sites → Default Web Site → osTicket.
  
Click on PHP Manager ---> PHP Extensions (Enable and Disable Extensions). 

Enable PHP extensions:

- php_imap.dll
  
- php_intl.dll
  
- php_opcache.dll
</p>
<br />

### 9. Rename and Assign Permissions
<p>
<img src="https://github.com/user-attachments/assets/ed86df0b-a033-40c4-8b8c-bac4a6dd50fa" height="80%" width="80%" alt="os-ticket-extensions"/>
</p>
<p>
Go to C:\inetpub\wwwroot\osTicket\include.
  
Rename "ost-sampleconfig.php" to "ost-config.php".
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/7b161c16-e311-4897-b85e-95996a0b1327" height="80%" width="80%" alt="os-ticket-extensions"/>
</p>
<p>
Right-click ost-config.php → Properties → Security → Advanced.
  
Disable inheritance and grant Full Control to Everyone.
</p>
<br />

### 10. Set Up the Database

<p>
<img src="https://github.com/user-attachments/assets/315aaaac-0ded-4a3d-854c-815dd3bdfa18" height="80%" width="80%" alt="Setup HeidiSQL"/>
</p>
<p>
Install HeidiSQL from the "osTicket-Installation-Files" folder. 
  
Create a new session using the MySQL root username and password. 

Connect to the IIS session and create a database named osTicket.
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/d01f7721-6701-4507-9cb8-7f386229a6b3" height="80%" width="80%" alt="Configure MySQL"/>
</p>
<p>
Return to the osTicket installer in your browser.
  
Enter the required details (Helpdesk Name, Default Email, Admin Username, Password, and osTicket database settings).

Click Install Now to complete the setup.
</p>
<br />

### Verification of Success

<p>
<img src="https://github.com/user-attachments/assets/d22b31d5-9cd1-4544-b79f-08655d0d60d8" height="80%" width="80%" alt="Congratulations Page"/>
</p>
