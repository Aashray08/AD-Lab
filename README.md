## **Active Directory Home Lab with 1000+ User Creations**

* * *

![image](https://github.com/user-attachments/assets/f6e1cc6d-ab54-44b8-a17a-0ac312ea2e9c)


## **Introduction**

This project demonstrates the deployment of a fully functional **Active Directory (AD)** environment using **Windows Server 2019** and **Windows 10 Pro** within **Oracle VirtualBox**. The goal is to simulate a realistic enterprise setup to understand identity and access management (IAM), domain services, DHCP configuration, and user provisioning with PowerShell automation.

By isolating the lab environment using internal and NAT networks, we ensure safe experimentation while maintaining internet access. This walkthrough provides reproducible steps that reflect industry practices in configuring secure domain infrastructures and reinforces core knowledge for cybersecurity professionals.

* * *

## **Technologies and Tools Used**

- **Virtualization**: Oracle VirtualBox
    
- **Operating Systems**: Windows Server 2019, Windows 10 Pro
    
- **Languages & Utilities**: PowerShell, CMD
    
- **Roles & Features**: AD DS, DHCP, NAT Routing
    

* * *

## **Step-by-Step Implementation:**

1.  First off all you need to make sure that you've downloaded Oracle [VirtualBox](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqa0FpYjBCVU11cGU0UVVBLVBCejlwb0ltakd5QXxBQ3Jtc0ttbk9sbkZQb1FObzNWdmZLa2ctamQ2WEdjMDJpVS03YmJmVzFiZG5aLUY2Z2c5eC0yVFhhOWQtVng3akpLaWpiWXBZcHVrbWYzVlQzVHBLUlNXM1VZc0M4UmVzYXkyM2thdGViaVc5UE81anZJbWl1VQ&q=https%3A%2F%2Fwww.virtualbox.org%2Fwiki%2FDownloads&v=MHsI8hJmggI).
2.  Then Create a new virtual machine in your VirtualBox.

![image](https://github.com/user-attachments/assets/f581bd17-9f90-4a33-9352-60683b1b6eee)


3\. Assign two network interface cards (NICs) to the VM. One for Network Address Translation (NAT) to allow internet access and one for Internal Networking, isolating domain traffic between your VMs.

This configuration simulates a real-world enterprise setup with segmented traffic for security and control.

![image](https://github.com/user-attachments/assets/0f02efbb-adb6-4d3e-ac6b-5fd3b19735a7)

![image](https://github.com/user-attachments/assets/943958b6-1154-4ba3-b62b-6ac9efad9452)

4\. Download the iso Image for [Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019) . Then install the iso image in the machine.

![image](https://github.com/user-attachments/assets/a5a10506-9a5a-4d9d-bd04-12caf8dde635)

5\. Then power on the device and set it up. Configure the network adapters within the server OS.  
Assign static IP to the internal NIC and ensure NAT NIC receives automatic configuration. This enables proper communication between internal clients and external services.

![image](https://github.com/user-attachments/assets/f773bd94-86c0-4e6e-8883-e9a0cfbe42c7)

6\. Install Active Directory Domain Services.

![image](https://github.com/user-attachments/assets/153e1c1c-b71f-49ce-866e-97ae6377abd4)

7\. Promote this server to a Domain Controller. Choose to add a new forest and enter the name of your domain. In this lab, I will be using aashraydomain.com.

This step initiates your AD forest, which acts as the security boundary and central authority for identity management.

![image](https://github.com/user-attachments/assets/a79bd64c-2742-419c-95d5-1f23c6e1748e)

8\. Open Active Directory Users and Computers and create a domain administrator account so that we are no longer using the built in administrator account. Log in with your domain admin account.

![image](https://github.com/user-attachments/assets/df58fc75-b210-489e-8751-deff65730ea5)

9\. In Server Manager, install the "Remote Access" role.  
This is required to enable Routing and Remote Access Service (RRAS) features for NAT configuration.

![image](https://github.com/user-attachments/assets/7caa7e79-ed1f-4c41-8121-0a51f06c11fe)

10\. Configure NAT using Routing and Remote Access. Go to Tools > Routing and Remote Access. Right-click the server, choose Configure and Enable Routing and Remote Access. Select NAT and assign it to the external NIC.

NAT allows internal machines to access the internet without exposing their IPs, a standard security practice.

![image](https://github.com/user-attachments/assets/00b82c01-a717-4d6a-8760-e14b048ca472)

11\. Install the DHCP Server role via Server Manager.  
This allows dynamic IP address assignment to clients in the internal network.

![image](https://github.com/user-attachments/assets/78069432-644c-4c45-b911-a67c5a069e47)

12\. **Configure a DHCP scope.** IP Range: `172.16.0.100 – 172.16.0.200`. Lease Duration: 20 days

DHCP automates IP management. In this lab, the 20-day lease is acceptable, but in high-turnover environments (e.g., cafés), shorter leases prevent IP exhaustion.

![image](https://github.com/user-attachments/assets/5c988fa7-3a99-4504-b145-9dad125363f9)

13\. **Download the PowerShell script for automated user creation** from:  
https://github.com/joshmadakor1/AD_PS

- Extract the folder to the desktop
    
- Open **PowerShell ISE as Administrator**
    
- Run `Set-ExecutionPolicy Unrestricted` to permit script execution
    
- Open the script and run it within the extracted folder
    

This script efficiently creates multiple AD users, demonstrating automation in enterprise identity provisioning.

![image](https://github.com/user-attachments/assets/b5e6c6c8-30eb-492a-bf46-d9f109a4219a)

![image](https://github.com/user-attachments/assets/d368fbeb-56cf-4a53-ba42-70e6395244ea)

If we look in Active Directory, we see all the users that the script created for us.

![image](https://github.com/user-attachments/assets/90fc5f1e-f8b2-4a15-a249-82f4b208d337)

14\. **Create a new [Windows 10](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXhZUHZnNTlxazVvdWFRb21VRHpaUjM5SmdDZ3xBQ3Jtc0tudTNWWjNuWlRKRGZFZWlNcDZQS29pNnplcklndS1fSXFvaEdtUUQ5SzdaYW5pNEhvNVNqZ1A2RDJUNEFRM2M2aThKMVpqR0xkaEZnc3Vkc3E5ckhuMGRJalNmR05WTXNpc2hQQTlYMW50dG9BdDNfcw&q=https%3A%2F%2Fwww.microsoft.com%2Fen-us%2Fsoftware-download%2Fwindows10ISO&v=MHsI8hJmggI) Client VM.** Ensure NIC is set to the **internal network.** Install **Windows 10 Pro** (Home edition cannot join domains). 

Proper NIC settings ensure the client communicates with the domain controller.

![image](https://github.com/user-attachments/assets/c5487649-a238-4743-990f-7cccdf7dfa1b)

Check if you ping the internet or else something is not setup yet in your DHCP server.

![image](https://github.com/user-attachments/assets/e2d606d1-59c0-4224-94d9-e16c5808e6fd)

15\. Log into the Windows 10 Client with the user account you created during setup. Join it to the domain and reboot.

Domain join confirms correct DNS, DHCP, and AD configurations.

![image](https://github.com/user-attachments/assets/fe30c742-d59f-4495-963c-f674fe8bb2be)

After configuring a separate virtual machine that will simulate an employee logging into the domain. Lets kill two birds with one stone by renaming the computer CLIENT1 and clicking the box to become a member of the aashraysdomain.com domain. I am prompted to give my log in credential and I chose to use the Administrator account I set up earlier.

![image](https://github.com/user-attachments/assets/9897def2-f6c8-4a85-be35-bb82c21fbb5a)

I Successfully joined the domain as a member!

![image](https://github.com/user-attachments/assets/e473e0ab-205c-4002-8d23-22dbb239c261)

16\. I log into an admin account to test if everything is configured correctly. Instead of logging into the user account created when I made the virtual machine, I try to log into a user created account in AASHRAYSDOMAIN.

![image](https://github.com/user-attachments/assets/0576fe00-55fe-417f-af5a-45c23b2f4221)

17\. Here is another way to check how many computers or devices are currently connected to the domain. We can see that my CLIENT1 computer is being properly recognized in Active Directory. Again, if this was a real environment there would probably be thousands of devices in this folder.

![image](https://github.com/user-attachments/assets/03627ce8-2eab-4499-88a0-8d4a288c041d)

I log into a user account I created from the Powershell script to test if everything is configured correctly.

![image](https://github.com/user-attachments/assets/b647b622-1f69-451c-a913-55dd7f2e4142)

* * *

## Conclusion

This hands-on lab successfully emulates an enterprise Active Directory environment using VMware and Windows Server 2019. Key accomplishments include:

- Configuring a secure network topology using NAT and internal networking
    
- Deploying AD DS, DHCP, and RRAS services
    
- Automating user provisioning with PowerShell
    
- Verifying domain connectivity from a Windows 10 Pro client
    

This lab reinforces the foundational knowledge of domain management, user authentication, and enterprise networking—critical skills for any cybersecurity or IT professional seeking real-world readiness.
