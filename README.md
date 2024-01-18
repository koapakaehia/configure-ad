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
- Mac OS (User)

<h2>High-Level Deployment and Configuration Steps</h2>

- Establish a Virtual Machine as a Domain Controller by deploying Windows Server 2022 on the designated server.
- Make sure there is a smooth connection between the client and the Domain Controller.
- Install Active Directory (AD) on the designated server.
- Generate both an administrative and a regular user account within the Active Directory.
- Connect the client machine to your domain.
- Configure Remote Desktop settings on the client computer, specifically tailored for non-administrative users.
- Generate additional user accounts within the Active Directory and ensure that all created users can successfully log in as intended.

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/2w6LHCg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/mQGv4Zj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Step 1: Create Domain Controller VM (DC-1.)</b>

- Open the Azure Portal (portal.azure.com).
- Click on "Virtual Machines" and then click "Add."
- Choose the "Windows Server 2022" image.
- Fill in the necessary details for VM name ("DC-1"), username, and password.
- Select the appropriate Resource Group or create a new one.
- Configure networking settings, ensuring that you note the Resource Group and Virtual Network created.
<p>
<img src="https://i.imgur.com/RQXXC7P.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 2: Set DC-1 NIC Private IP Address
- Navigate to the "Networking" tab of DC-1 in the Azure Portal.
- Under "Settings," select the network interface.
- Set the NIC's private IP address to be static.
<p>
<img src="https://i.imgur.com/iDIBSIa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 3: Create Client VM (Client-1)
- Click on "Virtual Machines" and then click "Add" again.
- Choose the "Windows 10" image.
- Configure VM details, setting the VM name as "Client-1," and ensuring 2 vCPUs.
- Use the same Resource Group and Virtual Network as DC-1.
- Verify both VMs are in the same Virtual Network.
<p>
<img src="https://i.imgur.com/EGCNlHC.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/NGs6bs7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 4: Ensure Connectivity

- Remote Desktop into Client-1 and open a command prompt.
- Ping DC-1's private IP address (ping -t <ip address>) to test connectivity.
<p>
<img src="https://i.imgur.com/kN3k8aT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/U9FM0Ws.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Remote Desktop into DC-1
- On DC-1, open Windows Firewall settings and enable ICMPv4.
- Confirm a successful ping from Client-1.
<p>
<img src="https://i.imgur.com/rzeSb0f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 5: Install Active Directory on DC-1
- Log in to DC-1 using the provided credentials.
    - On VM you can open command line and type “hostname” to see which VM you are on. 
- Install Active Directory:
    - Add roles and features.
    - Check Active Directory Domain Services.
    - Add features and install.
<p>
<img src="https://i.imgur.com/MJ0hp9j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/wElx0Tp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Promote DC-1 to a domain controller using the Active Directory Domain Services Configuration Wizard:
    - Set up a new forest as "mydomain.com" (customize as needed).
    - Restart and log back into DC-1 as user: (domain)\labuser.
<p>
<img src="https://i.imgur.com/RwgTCUM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 6: Create Admin and Normal User in AD
- Open "Active Directory Users and Computers" on DC-1.
- Create an Organizational Unit (OU) named "_EMPLOYEES."
- Create another OU named "_ADMINS."
<p>
<img src="https://i.imgur.com/oC5qwAQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Inside "_ADMINS," create a new user "Jane Doe" with the username "jane_admin."
<p>
<img src="https://i.imgur.com/sZv9nWU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Right-click the user(jane doe,) go to properties, Member of, and add "jane_admin" to the "Domain Admins" Security Group.
<p>
<img src="https://i.imgur.com/s62yvFv.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 7: Switch to Admin Account
- Log out or close the Remote Desktop connection to DC-1.
- Log back in as "mydomain.com\jane_admin."
<p>
<img src="https://i.imgur.com/NTktUy0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 8: Join Client-1 to Domain
- From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address.
- Restart Client-1 VM from the Azure Portal.
<p>
<img src="https://i.imgur.com/bKXt4y3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/RZ5Jc2e.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Log in to Client-1 (Remote Desktop) as the original local admin (labuser).
- Join it to the domain:
    - Right-click the start menu.
    - System.
    - Rename This PC.
    - It will ask for permission to join the domain.
    - Log in using (domain)jane_admin.
    - The computer will restart.
    - Log in to Domain Controller on Client-1 using Client-1 and the domain associated with the Domain Controller.
- Verify Client-1 shows up in Active Directory Users and Computers.
<p>
<img src="https://i.imgur.com/yXrQKbe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/mXWnR4d.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 9: Setup Remote Desktop
- Log into Client-1 as "mydomain.com\jane_admin."
- Right-click on "This PC," select "Systems," and click "Remote desktop."
- In the "Remote Desktop" tab, click "Select users that can remotely access this computer."
- Click "Select Users," add "DOMAIN\domain users," and grant remote access.
<p>
<img src="https://i.imgur.com/csDJDH3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 10: Create Additional Users
- Log into DC-1 as jane_admin.
- Open PowerShell ISE as an administrator.
- Copy the script from [GitHub](https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).
- Run the script to create additional user accounts.
<p>
<img src="https://i.imgur.com/DF3do1L.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 11: Verify Accounts
- Open ADUC on DC-1 and observe the newly created accounts in the appropriate OU.
- Attempt to log into Client-1 with one of the accounts (take note of the password in the script).
<p>
<img src="https://i.imgur.com/FNHK1BQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<p>
<img src="https://i.imgur.com/LUSwHLB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Step 12: Finish
- Verify the functionality and access for the created users on Client-1.
- Ensure successful logins for both administrative and non-administrative users.
- Confirm that organizational units in ADUC reflect the desired structure.
<p>
<img src="https://i.imgur.com/pU5A58S.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<b>Congratulations on successfully setting up your Azure environment and Active Directory integration!</b>
</p>
<br />
