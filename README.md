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

<h2>Configuration Steps</h2>

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

<h2>Deployment Steps</h2>

### 4. Install Active Directory
<p>
<img src="https://github.com/user-attachments/assets/ee8dc31c-0585-49c7-8c35-abc84f4e53c8" height="60%" width="60%" alt="Install Programs"/>
</p>
<p>
Go to dc-1 and enter Server Manager ---> Add Rolls and Features ---> Click Next to Server Roles and Enable Active Directory Domain Services. ---> Install.

Go to Server Manager and Promote Server as Domain Controller ---> Add a new forest "mydomain.com" ---> Configure Password. ---> Install.

Restart the Domain Controller "dc-1" and login again using mydomain.com\labuser.
</p>
<br />

### 5. Create a Domain Admin User Within the Domain
<p>
<img src="https://github.com/user-attachments/assets/44bf5b74-f26a-4baa-9c56-c92f061d0faa" height="60%" width="60%" alt="Organizational Units"/>
</p>
<p>
Login to the domain controller and navigate to Active Directory Users and Computers. 

Create an organizational unit called "_EMPLOYEES" and "_ADMINS".

Navigate to mydomain.com ---> Right Click Add Users ---> Create the units. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/ab8d0246-aae9-4120-9ceb-208df7542689" height="60%" width="60%" alt="Create Domain User"/>
</p>
<p>
Create a new user "Jane Doe" with the username "jane_admin".

Right Click on the _ADMINS folder in the Active Directroy Users and Computers. 

Create the user and configure a password for the user.

Click on the user and add property where they are apart of Domain Admins.

Log out and close dc-1 connection and login with mydomain.com\jane_admin. 
</p>
<br />

### 6. Join Client-1 Into the Domain Controller
<p>
<img src="https://github.com/user-attachments/assets/a3639515-7f99-4ced-bb63-d1d841332cf8" height="60%" width="60%" alt="Register PHP Version on IIS Manager"/>
</p>
<p>
Login to Client-1 as the original local admin (labuser) and join it to the domain controller.

Go to Start Menu and Settings ---> Rename this PC ---> Change Domian to Member of mydomain.com ---> Login with jane_admin. 

Restart Client-1. 
</p>
<br />

<p>
<img src="https://github.com/user-attachments/assets/c3c9f9d5-27bb-4ca5-9c6c-36b1527dcb25" height="60%" width="60%" alt="Domain Client Check"/>
</p>
<p>
Go back to the domain controller (dc-1) and navigate to the start menu and then AD Users and Computers. 

Verify that client-1 is now under mydomain.com in computers folder. 

Create a new organizational unit and call it _CLIENTS. Then drag client-1 in computers folder to _CLIENTS folder. 
</p>
<br />

<h2>Creating Users using Powershell</h2>

### 7. Enable Remote Access to Domain Controller from Client-1
<p>
<img src="https://github.com/user-attachments/assets/629197ca-429a-4a70-b3ea-e9312c938186" height="60%" width="60%" alt="Installation Steps"/>
</p>
<p>
Login to Client-1 as mydomain.com\jane_admin

Open System Properties and Click Remote Desktop ---> Select user that can remotely access this PC ---> From this location: mydomain.com, Field: Domain Users.
</p>
<br />

### 8. Create a Bunch of Additional Users and Attempt to Log into Client-1 with one of the Users
<p>
<img src="https://github.com/user-attachments/assets/a729dc27-6d29-41cb-9f43-3e68efb38c90" height="60%" width="60%" alt=""/>
</p>
<p>
Log into the Domain Controller (dc-1) as jane_admin.

Open Powershell_ise as an administrator. 

Create a new file and paste the contents of the [script](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1) into it. 

Run the script and observe the accounts being created. 
</p>
<br />

<h2>Group Policy and Managing Accounts</h2>

### 9. Dealing with Account Lockouts
<p>
<img src="" height="60%" width="60%" alt=""/>
</p>
<p>
</p>
<br />

<p>
<img src="" height="60%" width="60%" alt=""/>
</p>
<p>

</p>
<br />

### 10. Enabling and Disabling Accounts

<p>
<img src="" height="60%" width="60%" alt=""/>
</p>
<p>

</p>
<br />

<p>
<img src="" height="60%" width="60%" alt=""/>
</p>
<p>

</p>
<br />

### Verification of Success

<p>
<img src="https://github.com/user-attachments/assets/d22b31d5-9cd1-4544-b79f-08655d0d60d8" height="80%" width="80%" alt="Congratulations Page"/>
</p>
