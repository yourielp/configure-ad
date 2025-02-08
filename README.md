# On-premises Active Directory Deployment in Azure Virtual Machines

![Microsoft Active Directory Logo](https://i.imgur.com/pU5A58S.png)

This tutorial provides a guide on how to deploy on-premises Active Directory within Azure Virtual Machines. The aim is to simplify the process and make it as easy to use as possible. We will focus on setting up Active Directory Domain Services (AD DS) on a Windows Server 2022 machine and joining a Windows 10 client to the domain. Additionally, we will cover creating user accounts, enabling remote desktop access, and other related tasks.  The video will guide you visually through the steps involved and then further on there are some notes and screenshots provided for highlights of some or all of the material.

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

## Operating Systems Used

- Windows Server 2022
- Windows 10 (21H2)

<h2>Video Demonstration</h2>

- ### [How to Deploy on-premises Active Directory within Azure Virtual Machines](https://drive.google.com/file/d/11CHLY2jd5xKqcplNmbpM1vO99dYDDjzH/view?usp=drive_link)

## High-Level Deployment and Configuration Steps

1. **Setup Resources in Azure**

   - Create a Resource Group and Virtual Network (VNet) in Azure.
   - Create a Windows Server 2022 Virtual Machine (VM) named "DC-1" in the Resource Group and VNet.
   - Set the NIC Private IP address of the DC-1 VM to static.
   - Create a Windows 10 VM named "Client-1" in the same Resource Group and VNet.

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/144ab003-2e2f-4fa7-8ebf-9b666b48df62)


2. **Ensure Connectivity between the Client and Domain Controller**

   - Connect to Client-1 using Remote Desktop and ping DC-1's private IP address.
   - If the ping fails, log in to DC-1 and enable ICMPv4 in the local Windows Firewall.
     ![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/fee0c53a-2509-40fe-a550-e9e269dec8e3)
![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/d80f9262-0885-4213-b73e-031e6711dbe6)

   - Verify that the ping from Client-1 to DC-1 succeeds.
![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/d809e457-52fb-4f89-8488-5d358cdd7456)

3. **Install Active Directory**

   - Log in to DC-1 and install Active Directory Domain Services.
   - Promote DC-1 as a domain controller and set up a new forest with a domain name (e.g., mydomain.com).

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/2be95767-715b-47f3-940b-06bb6d1f76f4)

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/db964fe5-a062-467b-b5fe-e45ba371f2f0)

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/1c2424f1-6368-42ae-a6db-de3e15387d8f)


4. **Create an Admin and Normal User Account in AD**

   - Open Active Directory Users and Computers (ADUC) on DC-1.
   - Create an Organizational Unit (OU) called "_EMPLOYEES" and "_ADMINS" within ADUC.
   - Create a new employee account named "Jane Doe" with the username "jane_admin" and add her to the "Domain Admins" Security Group.
   - Log out and log back in to DC-1 as "mydomain.com\jane_admin".

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/2e4f6f3f-f53f-4712-8fa3-6e2a5cd5adbc)


5. **Join Client-1 to the Domain**

   - Set Client-1's DNS settings to the private IP address of DC-1.

  ![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/83c475c3-ee9d-4597-bfbe-9b8e3e7d988f)

   - Restart Client-1 and log in as the local admin user.
   - Join Client-1 to the domain and wait for the machine to restart.

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/a915e2b9-bc20-4684-af8e-43358e8c2828)

     
   - Log in to DC-1 and verify that Client-1 appears in ADUC under the "Computers" container.
   - Create a new OU named "_CLIENTS" and move Client-1 into it.

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/04f06f48-7c68-4847-be68-349180b490e6)


5. **Setup Remote Desktop for Non-Administrative Users on Client-1**

   - Log in to Client-1 as "mydomain.com\jane_admin" and open system properties.
   - Click on "Remote Desktop" and allow "domain users" access to Remote Desktop.
     
![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/886be92c-556d-4fb8-99da-f3b37e908b3a)

        - Non-administrative users can now log in to Client-1 using Remote Desktop.

5. **Create Additional User Accounts and Test Login**

   - Log in to DC-1 as "mydomain.com\jane_admin".
   - Open PowerShell ISE as an administrator and run the provided PowerShell script to create additional user accounts.

  ![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/f90cbf10-7db6-4330-b97d-7b0b1ee729b6)

     
   - Verify that the accounts are created in the appropriate OU within ADUC.

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/a0998445-1d4b-4f64-98b0-4fcc43d68384)

     
   - Attempt to log in to Client-1 using one of the newly created user accounts.

![image](https://github.com/JasonDelahoussaye/Configuring_On-premises_Active_Directory_within_Azure_VMs/assets/106440235/11e109b3-50d0-4e26-8c6e-b1b99215eb62)


By following these steps, you will successfully configure on-premises Active Directory within Azure Virtual Machines and establish a domain environment for user and resource management.

## Summary

This project involved the setup and configuration of on-premises Active Directory within Azure Virtual Machines. We created a Windows Server 2022 VM as a domain controller and a Windows 10 VM as a client. The deployment process included configuring network settings, ensuring connectivity, installing Active Directory, creating user accounts, joining the client to the domain, enabling Remote Desktop access, and testing user logins.
