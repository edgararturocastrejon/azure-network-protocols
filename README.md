<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic


<h2>Actions and Observations</h2>

<p>
  <h3> A-Record Exercise </h3>
  
![Screen Shot 2024-09-30 at 6 12 37 PM](https://github.com/user-attachments/assets/7508ab08-2e3b-4745-891c-f8b0e9db2844)

</p>
<p>
1) Connect/log into DC-1 as your domain admin account (mydomain.com\edgar_admin) <p/>
Connect/log into Client-1 as an admin (mydomain\edgar_admin)
</p>
<br />

<p>

![Screen Shot 2024-09-30 at 6 20 43 PM](https://github.com/user-attachments/assets/77d7e6d1-bdc9-467c-9a31-d3ce5e7cb039)
![Screen Shot 2024-09-30 at 6 35 52 PM](https://github.com/user-attachments/assets/243b9c1a-3e8a-45ea-839c-ad7b8403a580)

</p>
<p>
2) In Client-1 go to Powershell and try to ping “mainframe” notice that it fails
</p>
<br />

<p>

![Screen Shot 2024-09-30 at 6 40 21 PM](https://github.com/user-attachments/assets/7d3ec79b-8939-4a0d-a795-52941a842aaf)

</p>
<p> 3) Nslookup “mainframe” notice that it fails (no DNS record)
</p>
<br />

<p>

![Screen Shot 2024-09-30 at 6 43 17 PM](https://github.com/user-attachments/assets/e3ade6f3-86b7-471a-8d1e-e98b4ec48e64)
![Screen Shot 2024-09-30 at 6 48 48 PM](https://github.com/user-attachments/assets/5d374b6f-9de4-4623-bc56-ddaae612945c)
![Screen Shot 2024-09-30 at 6 51 56 PM](https://github.com/user-attachments/assets/df375eb7-be1c-45de-97bd-c8264a9ca8e2)
![Screen Shot 2024-09-30 at 6 52 55 PM](https://github.com/user-attachments/assets/e0825866-c134-4435-83dd-eba26be6f82c)


</p>
<p> 4) Create a DNS A-record on DC-1 for “mainframe” and have it point to DC-1’s Private IP address <p/> 
  dc-1 --> Forward Lookup Zones --> mydomain.com (right click) --> New Host (A or AAAA)... 
</p>
<br />

<p>

![Screen Shot 2024-09-30 at 7 01 31 PM](https://github.com/user-attachments/assets/5ab3bd5e-0fa1-4833-b4aa-54d015b5702a)
  
</p>
<p>
Go back to Client-1 and try to ping it. Observe that it works <p/>
------------------------------------------------------------------
</p>
<br />

<p>
    <h3> Local DNS Cache Exercise </h3>
  
![Screen Shot 2024-09-30 at 7 08 53 PM](https://github.com/user-attachments/assets/01b6aaae-6a6c-46c5-b8ce-a0e3ef9c5b12)

</p>
<p>Go back to DC-1 and change mainframe’s record address to 8.8.8.8
</p>
<br />

<p>
  
![Screen Shot 2024-09-30 at 7 12 44 PM](https://github.com/user-attachments/assets/b74b5734-ea03-480f-9817-0a8e3fc8bf06)

</p>
<p>
Go back to Client-1 and ping “mainframe” again. Observe that it still pings the old address
</p>
<br />

<p>
  
![Screen Shot 2024-09-30 at 7 19 31 PM](https://github.com/user-attachments/assets/c2d4a77b-4e00-4003-981c-fbc9e8ad3e4a)

</p>
<p>
  Observe the local dns cache (ipconfig /displaydns)
</p>
<br />

<p>
  
![Screen Shot 2024-09-30 at 7 25 35 PM](https://github.com/user-attachments/assets/a4bf5821-8378-40a8-9c80-f310d738960c)
![Screen Shot 2024-09-30 at 7 26 07 PM](https://github.com/user-attachments/assets/1bfa4608-1301-448b-8f6a-269ed300dc0a)


</p>
<p>
Flush the DNS cache (ipconfig /flushdns) <p/>
  Open Powershell as an Admin to flush dns
</p>
<br />

<p>
  
![Screen Shot 2024-09-30 at 7 31 27 PM](https://github.com/user-attachments/assets/b31d6b21-1444-49e2-b252-3d9ddbda5e25)

</p>
<p>
  Attempt to ping “mainframe” again. Observe the address of the new record is showing up
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
