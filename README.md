<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />




<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
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

<h3>Setup Domain Controller in Azure</h3>

<p>Create a Resource Group</p>

![{B1A2A4DB-B400-42EA-9182-66CDEC24D440}](https://github.com/user-attachments/assets/48271635-749e-40fb-865f-5d4cfe3f5016)

<p>Create a Virtual Network and Subnet</p>

![{4221942C-945A-45BD-B9F9-3484902F7270}](https://github.com/user-attachments/assets/a213cea2-4f4b-4d30-9b02-22661f0ee4be)

<p>Create the Domain Controller VM (Windows Server 2022) named “DC-1”</p>

![{699EFC15-F5EA-4322-A47C-82199813657F}](https://github.com/user-attachments/assets/453420bb-139d-40bc-aedf-5a552fb5c8e4)

![{E64D1F11-4E89-408C-8E1F-A6A14A7B9EA8}](https://github.com/user-attachments/assets/a68b40bf-4d22-493f-96dd-396df55a7388)

<p>Username: labuser</p>
<p>Password: Cyberlab123!</p>

![{C665C350-9990-456D-BADA-B74ABF7A7D23}](https://github.com/user-attachments/assets/0b6337e0-c8a1-4a98-bfd2-53603fc7015a)

<p>After VM is created, set Domain Controller’s NIC Private IP address to be static</p>

![{0AE58743-52E2-41B7-9C69-3A831F4B4CB4}](https://github.com/user-attachments/assets/75f93db1-7c58-4510-8ee4-65968e9f058f)

<p>Log into the VM and disable the Windows Firewall (for testing connectivity)</p>

![{A7FBCBD9-3C7D-46CF-95B4-14DFB5C61B19}](https://github.com/user-attachments/assets/c263bb0f-ebcf-4f36-991c-602215634153)

![{8141CEB9-5E7D-48E9-8D30-C4A7CB0DAB60}](https://github.com/user-attachments/assets/11805232-3412-4d60-83c3-5eed90834d0b)

<h3>Setup Client-1 in Azure</h3>

<p>Create the Client VM (Windows 10) named “Client-1”</p>

![{B8134FDF-2222-400C-BECD-A8BCBEEEA6CD}](https://github.com/user-attachments/assets/c5e48a2b-d121-4ac3-af47-29450f3352af)

<p>Username: labuser</p>
<p>Password: Cyberlab123!</p>
<p>Attach it to the same region and Virtual Network as DC-1</p>

![{96BC2A67-F9E1-4B99-A293-035F63AE5DB6}](https://github.com/user-attachments/assets/18f4ef24-878e-41f4-90ba-79d15944a23c)

<p>After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address</p>

![{714D309B-E282-4087-B9CB-8813D9E8C2A6}](https://github.com/user-attachments/assets/ca1b6c98-85be-4fd2-9b76-23568b6daf4d)

<p>From the Azure Portal, restart Client-1</p>

![{226BFE93-50D6-4655-9DD9-78F4DBB5BFBB}](https://github.com/user-attachments/assets/d527f70b-1c28-493a-b1ea-a85a1c9bc68f)

<p>Login to Client-1</p>
<p>Attempt to ping DC-1’s private IP address</p>
<p>Ensure the ping succeeded</p>

![{6972B467-7B13-4F45-A097-5C891BAB4BC9}](https://github.com/user-attachments/assets/6c2b7178-e33a-4a8d-829c-5230834b97f2)

<p>From Client-1, open PowerShell and run ipconfig /all</p>
<p>The output for the DNS settings should show DC-1’s private IP Address</p>

![{1099C741-92BB-4284-AD5B-D7136DA0FC42}](https://github.com/user-attachments/assets/baa105bb-3be3-41a7-a8a0-70e4be1cdab3)

<h3>Install Active Directory</h3>

<p>Login to DC-1 and install Active Directory Domain Services</p>

![{8FC6EED0-1D62-43C3-9500-3858BD7ACF68}](https://github.com/user-attachments/assets/6a9936bf-d155-482a-a675-492464a3277a)

<p>Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)</p>

![{DA4A18CD-240E-49BD-843A-DC218B3B5F98}](https://github.com/user-attachments/assets/4fb4d3d7-6798-4465-96cb-180ef952d651)

<p>Restart and then log back into DC-1 as user: mydomain.com\labuser</p>

<h3>Create a Domain Admin user within the domain</h3>

<p>In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”</p>

![{8BD16D08-837F-4BBB-BE28-FFD180DCB548}](https://github.com/user-attachments/assets/88e7762b-4ebc-4316-a6e9-8953d6f6a592)

<p>Create a new OU named “_ADMINS”</p>

![{29B3A62E-182D-4197-B2C9-3AEEDEACB704}](https://github.com/user-attachments/assets/41bdb35d-7a1a-431f-9b7f-1cb845d1fdf5)

<p>Create a new employee named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!</p>

![{C00B4AFA-86D5-4315-9536-31D85109FB6F}](https://github.com/user-attachments/assets/34b75cc9-df00-44c1-b653-4b4fe9d31b6f)

<p>Add jane_admin to the “Domain Admins” Security Group</p>

![{ADC15DE6-16BA-4F0B-B9F2-18BDB28E7F50}](https://github.com/user-attachments/assets/b370b2ed-f264-4d90-9d39-ad7c4e503067)

<p>Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”</p>

<p>User jane_admin as your admin account from now on</p>

<h3>Join Client-1 to your domain (mydomain.com)</h3>

<p>Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart)</p>

![{CBAE7F78-611B-4EB3-8A26-A22140B25023}](https://github.com/user-attachments/assets/c095048e-70b0-4dc6-a1b7-a79d9270fb0b)

<p>Login to the Domain Controller and verify Client-1 shows up in ADUC</p>

![{F0197D14-FFC3-48EC-B9C9-9EDF1FD90042}](https://github.com/user-attachments/assets/3d8af87a-7064-43ca-9229-367704cd8620)

<p></p>Create a new OU named “_CLIENTS” and drag Client-1 into there</p>

![{FACBBEA0-DF9F-4F34-8B9D-A4AAA6159B74}](https://github.com/user-attachments/assets/5f349693-cc20-499e-9d4c-a2e2aa1b378e)

