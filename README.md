## **Active Directory Home Lab with 1000+ User Creations**

* * *

![bee106f0c7937854ca01e32d33fdc4f8.png](:/026ef54e44234f74ad84f6207077a086)

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

![6d9852694d26ee60c977c52dadd19e64.png](:/75c2db66a684478eb72d53d7028eb223)

3\. Assign two network interface cards (NICs) to the VM. One for Network Address Translation (NAT) to allow internet access and one for Internal Networking, isolating domain traffic between your VMs.

This configuration simulates a real-world enterprise setup with segmented traffic for security and control.

![d20916289ce227f87bc39226e54b2038.png](:/68ed72ee137747dfbe455bb1144e909b)

![0f9c578250f9afe4629b07ead022b388.png](:/a5fee14b1621486cbc8a0dde5eff475d)

4\. Download the iso Image for [Windows Server 2019](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019) . Then install the iso image in the machine.

![4c573c7495eda14685d3a49114a287c1.png](:/cde5103210fb44bf9ce3f6eae2315829)

5\. Then power on the device and set it up. Configure the network adapters within the server OS.  
Assign static IP to the internal NIC and ensure NAT NIC receives automatic configuration. This enables proper communication between internal clients and external services.

![a65c642a47fce1d7332448d89eea6a75.png](:/b3a6a33222c044e5804ca9d0a9919c2e)

6\. Install Active Directory Domain Services.

![a70d95a6583279a5931521168de190e7.png](:/a97ef5e494fd44e9b5603038eeb9290c)

7\. Promote this server to a Domain Controller. Choose to add a new forest and enter the name of your domain. In this lab, I will be using aashraydomain.com.

This step initiates your AD forest, which acts as the security boundary and central authority for identity management.

![2e4ccb1658b429ceff7e428fb17d18ae.png](:/0faf67ade50948e3bdf8520723b61379)

8\. Open Active Directory Users and Computers and create a domain administrator account so that we are no longer using the built in administrator account. Log in with your domain admin account.

![4a1da96d5cc7ce1425f67813f755992b.png](:/296eaaed92e845faa0d0516b127554eb)

9\. In Server Manager, install the "Remote Access" role.  
This is required to enable Routing and Remote Access Service (RRAS) features for NAT configuration.

![b7dffecbb9439d910a4a802b404d6ff1.png](:/76a2846e71844d5c9bfa48ad17bf1421)

10\. Configure NAT using Routing and Remote Access. Go to Tools > Routing and Remote Access. Right-click the server, choose Configure and Enable Routing and Remote Access. Select NAT and assign it to the external NIC.

NAT allows internal machines to access the internet without exposing their IPs, a standard security practice.

![bf33e37155d26c7a905159090929a8af.png](:/0cf3a83e6453435a82b23604b1a0d8cb)

11\. Install the DHCP Server role via Server Manager.  
This allows dynamic IP address assignment to clients in the internal network.

![0b9c4ab53b7ba0c871d58175688804fa.png](:/b231b023e184443f9f9e718d3ac9126b)

12\. **Configure a DHCP scope.** IP Range: `172.16.0.100 – 172.16.0.200`. Lease Duration: 20 days

DHCP automates IP management. In this lab, the 20-day lease is acceptable, but in high-turnover environments (e.g., cafés), shorter leases prevent IP exhaustion.

![0b608b682a4b6ac83155638dedcbe8a0.png](:/9ce7436e174d497b87901ff65b1ae835)

13\. **Download the PowerShell script for automated user creation** from:  
https://github.com/joshmadakor1/AD_PS

- Extract the folder to the desktop
    
- Open **PowerShell ISE as Administrator**
    
- Run `Set-ExecutionPolicy Unrestricted` to permit script execution
    
- Open the script and run it within the extracted folder
    

This script efficiently creates multiple AD users, demonstrating automation in enterprise identity provisioning.

![fff60309318e892b30b1ad6520484f4b.png](:/2a87aa0076004b8dbfaba27f97ad93d0)

![34d9ba67f96ce69c8f3f6ab985112b98.png](:/573a310d9270493f818300486639637a)

If we look in Active Directory, we see all the users that the script created for us.

![5c57bc0580d15ef8cb778c0894e7bfbf.png](:/ae7195dfb9a3458e9dbff1a5d3d9e5fc)

14\. **Create a new [Windows 10](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbXhZUHZnNTlxazVvdWFRb21VRHpaUjM5SmdDZ3xBQ3Jtc0tudTNWWjNuWlRKRGZFZWlNcDZQS29pNnplcklndS1fSXFvaEdtUUQ5SzdaYW5pNEhvNVNqZ1A2RDJUNEFRM2M2aThKMVpqR0xkaEZnc3Vkc3E5ckhuMGRJalNmR05WTXNpc2hQQTlYMW50dG9BdDNfcw&q=https%3A%2F%2Fwww.microsoft.com%2Fen-us%2Fsoftware-download%2Fwindows10ISO&v=MHsI8hJmggI) Client VM.** Ensure NIC is set to the **internal network.** Install **Windows 10 Pro** (Home edition cannot join domains). 

Proper NIC settings ensure the client communicates with the domain controller.

![4c1791856d33be4c1e161372f9603d4f.png](:/2d5a01f045be4c6cbb7876da9762b1db)

Check if you ping the internet or else something is not setup yet in your DHCP server.

![8dd62823fb6e189e89fc2920dc3754e5.png](:/98817c472a00422e92702b478022b168)

15\. Log into the Windows 10 Client with the user account you created during setup. Join it to the domain and reboot.

Domain join confirms correct DNS, DHCP, and AD configurations.

![f77d8cb0e91996256454185490b59fea.png](:/1c569eb9094449d8a99c6bff1bdfb01f)

After configuring a separate virtual machine that will simulate an employee logging into the domain. Lets kill two birds with one stone by renaming the computer CLIENT1 and clicking the box to become a member of the aashraysdomain.com domain. I am prompted to give my log in credential and I chose to use the Administrator account I set up earlier.

![7dec726ff9ff195ff496a15998707941.png](:/a550fd0960d04fcfa52143482dfed646)

I Successfully joined the domain as a member!

![71c023279a011722741d8c7d4407c659.png](:/0f50eb7568ed4e858ea8ed60f09cc81e)

16\. I log into an admin account to test if everything is configured correctly. Instead of logging into the user account created when I made the virtual machine, I try to log into a user created account in AASHRAYSDOMAIN.

![de1543d52b814a5e3590e233ce377e10.png](:/6bc704b57cd9473281e447281e69305b)

17\. Here is another way to check how many computers or devices are currently connected to the domain. We can see that my CLIENT1 computer is being properly recognized in Active Directory. Again, if this was a real environment there would probably be thousands of devices in this folder.

![661eef914ff24836fce89484be3a078f.png](:/edb37d540ea24ccc86c929db9a2a285c)

I log into a user account I created from the Powershell script to test if everything is configured correctly.

![352b0be04b8d8bb231a76c4719c36f95.png](:/9bad3a177dc64c139dada8e3d992bb84)

* * *

## Conclusion

This hands-on lab successfully emulates an enterprise Active Directory environment using VMware and Windows Server 2019. Key accomplishments include:

- Configuring a secure network topology using NAT and internal networking
    
- Deploying AD DS, DHCP, and RRAS services
    
- Automating user provisioning with PowerShell
    
- Verifying domain connectivity from a Windows 10 Pro client
    

This lab reinforces the foundational knowledge of domain management, user authentication, and enterprise networking—critical skills for any cybersecurity or IT professional seeking real-world readiness.