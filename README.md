<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022, 2vCPUs, 8GM RAM
- Windows 10 Pro (22H2), 2vCPUs, 8GB RAM

<h2>High-Level Deployment and Configuration Steps</h2>

- Install Active Directory
- Create a Domain Admin user within the domain
- Join Client-1 to your domain (mydomain.com)

<h2>Deployment and Configuration Steps</h2>

<p>
Before deploying Active Directory, we'll need to install 2 VMs on Azure, one will be a Windows Server for the domain controller and one will be Windows Pro machine as a client. We can name the server machine DC-1 and the client machine Client-1. We also need to make sure that both of these machines are on the same virtual network.
</p>

<p>
Login to DC-1 and install Active Directory Domain Services
</p>
<p>
<img src="https://github.com/user-attachments/assets/d5b44259-134b-4d58-93b3-87ae373273c3" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Promote as a DC: Set up a new forest as mydomain.com
</p>
<p>
<img src="https://github.com/user-attachments/assets/0dc5c178-0481-40fe-91e0-965489a5930c" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>

<br>
<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES” and create a new OU named “_ADMINS”
</p>
<p>
<img src="https://github.com/user-attachments/assets/3a98eed6-7c1a-4bed-b125-0abc3742680d" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!
</p>
<p>
<img src="https://github.com/user-attachments/assets/4c24448a-248f-4c0c-9a9b-6891eff91886" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Add jane_admin to the “Domain Admins” Security Group
</p>
<p>
<img src="https://github.com/user-attachments/assets/ae0cf498-f19c-49b3-9f3d-5bc251a3fa50" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”
</p>

<br />
<p>
Use jane_admin as your admin account from now on
</p>

<br />
<p>
Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)
</p>
<p>
<img src="https://github.com/user-attachments/assets/e321d0d7-80cd-4737-afce-e1579cf667d9" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Login to the Domain Controller and verify Client-1 shows up in ADUC
</p>
<p>
<img src="https://github.com/user-attachments/assets/f0f13af8-e865-4810-aa2b-897944a0bc17" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<br>
<p>
Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<p>
<img src="https://github.com/user-attachments/assets/bfbbb8fc-eb15-49a3-a3a1-37732f7518fd" height="80%" width="80%" alt="server manager in DC-1"/>
</p>

<h2>Active Directory is Deployed and Ready for Use! </h2>

<b>We've successfully installed AD on the domain controller, created a domain admin, and joined the client VM to the domain. Next, we'll create users in Powershell by running a script, and then manage the accounts and group policy!  </b>
<br />
<br />
</p>
