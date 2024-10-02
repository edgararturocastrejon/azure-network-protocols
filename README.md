<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- MacBook Pro 
- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04


<h2>Actions and Observations</h2>

<p>
  
![Screen Shot 2024-10-01 at 5 30 20 PM](https://github.com/user-attachments/assets/046a1ebe-fd52-4d39-a328-4638848f4598)

</p>
<p>
  !) Create Resource Group in Azure
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 5 29 39 PM](https://github.com/user-attachments/assets/811cec35-3de5-47b3-b984-0b68eb9177d2)

</p>
<p>
  2) Create Virtual Network (to ensure that both Virtual Machines (VM's) are on the same network)
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 5 36 02 PM](https://github.com/user-attachments/assets/9a64c4c9-116e-4716-9817-dfa59df92566)
![Screen Shot 2024-10-01 at 5 40 55 PM](https://github.com/user-attachments/assets/495d8bc8-43ab-4fda-aa3f-816c5c3c6385)

</p>
<p>
  3) Create first VM with Winows 10 image (make sure to have the size with at least 2 vcpus) <p/>
    Go to Networking and make sure it's on the Virtual Network you created
</p>
<br />

<p>

![Screen Shot 2024-10-01 at 5 46 58 PM](https://github.com/user-attachments/assets/ed7d0983-86eb-46ad-8366-0a90e314c7a2)
![Screen Shot 2024-10-01 at 5 47 50 PM](https://github.com/user-attachments/assets/672c94d7-ff5a-4e4a-8c5a-3b8564fd696d)

</p>
<p>
  4) Create Ubuntu VM (make sure the size is also 2 vcpus) <p/>
    Go to Networking before creating and make sure it's on the Virtual Network you created
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 5 57 22 PM](https://github.com/user-attachments/assets/7206e3fb-1fc8-4b6a-a00d-d90aae9509f2)
![Screen Shot 2024-10-01 at 6 07 37 PM](https://github.com/user-attachments/assets/cb208a91-22a1-441b-a45d-dff0a45929d9)

</p>
<p>
  5) Loggin to your Windows 10 VM using Windows Remote Desktop (since we are using a Mac) <p/>
  Install Wireshark https://www.wireshark.org 
  
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 6 15 06 PM](https://github.com/user-attachments/assets/bf83c01c-a8c3-4aaf-87ce-12e0dbe1d26f)
![Screen Shot 2024-10-01 at 6 17 57 PM](https://github.com/user-attachments/assets/efba924f-90b9-4692-8ef9-2c0f2ae070b4)

</p>
<p>
  6) Open Wireshark and start packet capture <p/>
    Within Wireshark, filter for ICMP traffic only  
</p>
<br />

<p>

![Screen Shot 2024-10-01 at 6 21 16 PM](https://github.com/user-attachments/assets/b445054d-bcb2-4a97-b904-2bca635c898e)
![Screen Shot 2024-10-01 at 6 23 08 PM](https://github.com/user-attachments/assets/35b5687a-03bb-41c4-9cb6-6b1cbc7b9cff)
![Screen Shot 2024-10-01 at 6 27 12 PM](https://github.com/user-attachments/assets/cef1e56c-d9c1-49c4-b2a3-018f2870fa6a)

</p>
<p>
  7) Retrieve the private IP address of the Ubuntu VM (linux-vm) and attempt to ping it from within the Windows 10 VM <p/>
  (Observe ICMP Traffic) <p/>
    ----------------------------------------------------------------------- <p/>
  (Configuring a Firewall [Network Security Group])
</p>
<br />

<p>

![Screen Shot 2024-10-01 at 6 52 42 PM](https://github.com/user-attachments/assets/d44cd41e-fbc8-4477-a6c3-9414b4417600)

</p>
<p>
 1) Now we can make some firewall configurations to deny any ICMP traffic. To do this I will enter a perpetual ping command into PowerShell 
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 7 04 46 PM](https://github.com/user-attachments/assets/995ca50b-7d37-46f9-8e1f-6a01bfe9d4d2)

</p>
<p>
 2) Next, I will go to Ubuntu VM (linux-vm) network security group and create a new inbound security rule denying all ICMP traffic making sure to put its priorty highest, so it will take precedence before all other rules
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 7 06 05 PM](https://github.com/user-attachments/assets/3f3a223a-c0f8-4bc0-8555-8d596d6d15ad)

</p>
<p>
  Going back to our Windows 10 VM we can see that the ICMP echo requests aren't getting any responses due to the security change
</p>
<br />

<p>
  
![Screen Shot 2024-10-01 at 7 08 34 PM](https://github.com/user-attachments/assets/e91e26b3-dbd3-4f6f-9070-9b1bc51d0295)
![Screen Shot 2024-10-01 at 7 08 56 PM](https://github.com/user-attachments/assets/3c89d533-7420-4e22-90cb-c7c3ab941cab)

</p>
<p>
  3) To revert the change I can either switch the inbound rule to "Allow" instead of "Deny" or delete the rule <p/>
    Back in the Windows 10 VM, observe the ICMP traffic in WireShark and the command line Ping activity (should start working) <p/>
  ------------------------------------------------------------------------------------ <p/>
  (Observe SSH Traffic)

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

