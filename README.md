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

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller in Azure
- Setup Client-1 in Azure
- Install Active Directory
- Create a Domain Admin user within the domain
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

<p>
Create a Resource Group
</p>
<br />

![Screen Shot 2025-03-10 at 10 42 28 AM](https://github.com/user-attachments/assets/19ef657e-5729-4777-a5d2-516c71506828)


<p>
Create a Virtual Network and Subnet
</p>
<br />

![Screen Shot 2025-03-10 at 10 45 19 AM](https://github.com/user-attachments/assets/e8bfb662-2043-4073-a8b6-4924281d909f)

<p>
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
</p>
<br />

![Screen Shot 2025-03-10 at 10 49 40 AM](https://github.com/user-attachments/assets/7733a732-4a22-44f4-9d47-ed9b1e0ea837)

<p>
Create the Client VM (Windows 10) named “Client-1”
</p>
<br />

![Screen Shot 2025-03-10 at 11 11 09 AM](https://github.com/user-attachments/assets/bf5288e4-919f-4a82-96cb-b86269ec944e)

<p>
After VM is created, set Domain Controller’s NIC Private IP address to be static
</p>
<br />

![Screen Shot 2025-03-10 at 11 33 22 AM](https://github.com/user-attachments/assets/1b2c9237-32ea-4c5e-bc0b-bb04e6241cf3)


<p>
Log into the VM and disable the Windows Firewall (for testing connectivity)
</p>
<br />

![Screen Shot 2025-03-10 at 11 37 32 AM](https://github.com/user-attachments/assets/c3502fc0-39ea-4149-bbf2-c066bde1d19e)

<p>
After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address
</p>
<br />

![Screen Shot 2025-03-10 at 11 41 19 AM](https://github.com/user-attachments/assets/ebba5da2-cfcd-46a6-bdd5-b9a31c4449a8)

<p>
From the Azure Portal, restart Client-1
</p>
<br />

<p>
Login to Client-1
</p>
<br />

<p>
Attempt to ping DC-1’s private IP address
</p>
<br />

![Screen Shot 2025-03-10 at 11 58 49 AM](https://github.com/user-attachments/assets/398472cd-bfb2-4206-9283-c631add9aaf2)

<p>
From Client-1, open PowerShell and run ipconfig /all
The output for the DNS settings should show DC-1’s private IP Address
</p>
<br />

![Screen Shot 2025-03-10 at 12 02 48 PM](https://github.com/user-attachments/assets/a4e1bb03-a9f7-4292-a6b3-45254af73bef)

<p>
Login to DC-1 and install Active Directory Domain Services
</p>
<br />

![Screen Shot 2025-03-10 at 12 46 46 PM](https://github.com/user-attachments/assets/d5b44259-134b-4d58-93b3-87ae373273c3)

<p>
Promote as a DC: Setup a new forest as mydomain.com
</p>
<br />

![Screen Shot 2025-03-10 at 12 50 25 PM](https://github.com/user-attachments/assets/0dc5c178-0481-40fe-91e0-965489a5930c)

<p>
Restart and then log back into DC-1 as user: mydomain.com\labuser
</p>
<br />

<p>
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
</p>
<p>
Create a new OU named “_ADMINS”
</p>
<br />

![Screen Shot 2025-03-10 at 1 41 27 PM](https://github.com/user-attachments/assets/3a98eed6-7c1a-4bed-b125-0abc3742680d)

<p>
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!
</p>
<br />

![Screen Shot 2025-03-10 at 1 43 47 PM](https://github.com/user-attachments/assets/4c24448a-248f-4c0c-9a9b-6891eff91886)

<p>
Add jane_admin to the “Domain Admins” Security Group
</p>
<br />

![Screen Shot 2025-03-10 at 1 55 01 PM](https://github.com/user-attachments/assets/ae0cf498-f19c-49b3-9f3d-5bc251a3fa50)

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
<br />

![Screen Shot 2025-03-10 at 2 00 19 PM](https://github.com/user-attachments/assets/e321d0d7-80cd-4737-afce-e1579cf667d9)

<p>
Login to the Domain Controller and verify Client-1 shows up in ADUC
</p>
<br />

![Screen Shot 2025-03-10 at 2 03 22 PM](https://github.com/user-attachments/assets/f0f13af8-e865-4810-aa2b-897944a0bc17)

<p>
Create a new OU named “_CLIENTS” and drag Client-1 into there
</p>
<br />

![Screen Shot 2025-03-10 at 2 05 18 PM](https://github.com/user-attachments/assets/bfbbb8fc-eb15-49a3-a3a1-37732f7518fd)

<p>
Log into Client-1 as mydomain.com\jane_admin,
Open system properties,
Click “Remote Desktop”,
Allow “domain users” access to remote desktop
</p>
<br />

![Screen Shot 2025-03-10 at 2 21 18 PM](https://github.com/user-attachments/assets/2128ea0c-9c0e-4461-a661-aeb128e5fe3a)

<p>
Login to DC-1 as jane_admin,
Open PowerShell_ise as an administrator,
Create a new File and paste the contents of the script into it
</p>
<br />

![Screen Shot 2025-03-10 at 2 28 39 PM](https://github.com/user-attachments/assets/f07c3806-7022-48d3-8e5a-cdbf7ba9bedc)

<p>
Run the script and observe the accounts being created
</p>
<br />

![Screen Shot 2025-03-10 at 2 45 56 PM](https://github.com/user-attachments/assets/6cce058d-6a28-4d8f-9d2a-3dded7629f52)

<p>
When finished, open ADUC and observe the accounts in the appropriate OU　(_EMPLOYEES)
</p>
<br />

![Screen Shot 2025-03-10 at 2 47 25 PM](https://github.com/user-attachments/assets/235c4b06-6bf6-4278-b3d6-34460c7045fb)

<p>
Logged into Client-1 with one of the accounts</p>
<br />

![Screen Shot 2025-03-10 at 2 49 17 PM](https://github.com/user-attachments/assets/e32562ad-8486-4861-94fc-96adf9918c93)
