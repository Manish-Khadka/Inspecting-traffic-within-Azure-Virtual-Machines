# Inspecting-traffic-within-Azure-Virtual-Machines
This repository features a lab I completed in Azure focused on analyzing network traffic.
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

<h2>Setting Up</h2>

In Azure, I set up two virtual machines (VMs) within the same virtual network to ensure seamless communication between them. One VM runs Windows 10 Pro, while the other runs Ubuntu. The Windows VM connects to the Ubuntu VM using the command line or PowerShell.
</p>
<img width="1036" alt="Creating a VM" src="https://github.com/user-attachments/assets/6fc5dc1d-cc87-4bf5-b60f-2bc4a389e4eb" />

<img width="824" alt="linux-vm" src="https://github.com/user-attachments/assets/4ed5f71a-1af2-43ae-9e6a-e39f023f0c52" />





<h2>Actions and Observations</h2>

I used Remote Desktop Connection to access the Windows VM via its public IP address. Once connected, I installed Wireshark to start inspecting network traffic.

<p>
<img width="492" alt="wireshark installed" src="https://github.com/user-attachments/assets/ab0d350d-80ab-4507-b6f4-9ed2470f6038" />

</p>
<p>
In Wireshark, I filtered for ICMP (Internet Control Message Protocol) traffic and opened PowerShell to run the ping command. Ping uses ICMP to help devices on a network identify and communicate issues with data transmission. I used ping to test communication with the Ubuntu VM using its private IP address, as well as with google.com. Following this, I executed a perpetual ping to the Ubuntu VM to observe how network security groups operate. The perpetual ping was run using the command: ping -t (IP address).
</p>

<img width="1470" alt="checkig icmp traffic" src="https://github.com/user-attachments/assets/d908d294-cb18-47f9-9588-b4b5006d0892" />
<p>


</p>
<p>
In the Azure portal, I accessed the networking settings for the Ubuntu VM and added an inbound security rule to block ICMP traffic. I ensured the rule's priority was set higher than SSH (250) so it would take precedence and apply first.
</p>
<p>

<img width="1453" alt="adding rule to block ICMP" src="https://github.com/user-attachments/assets/b1d96a74-1998-4938-b797-09f1cfe99a67" />



Back on the Windows VM, I observed that the ICMP traffic was blocked due to the inbound security rule. After modifying the rule to allow traffic again, the perpetual ping successfully resumed without timing out.
</p>

<p>
<p>

<img width="1165" alt="observing traffic after adding rule" src="https://github.com/user-attachments/assets/345958dc-8a4a-4c75-9768-bfeec96d72c7" />




Next, using the ssh command, I analyzed SSH traffic by connecting to the Ubuntu server through PowerShell.
</p>

<p>
<p>
<img width="1314" alt="observing SSH " src="https://github.com/user-attachments/assets/b3803253-3cb8-4c33-8e3b-9709489a943b" />



After analyzing SSH traffic, I logged out of the Ubuntu server to focus on DHCP traffic. To observe it in action, I attempted to obtain a new IP address for my VM. Using the command ipconfig /renew, the VM requested a new IP address, causing a brief disconnection for a few seconds. Once reconnected, the resulting DHCP traffic was visible in Wireshark.
</p>

<p>
<p>
</p>


<img width="1396" alt="observing DHCP" src="https://github.com/user-attachments/assets/78447e02-b958-4a0f-a687-6049b4200fc7" />





Next, to examine DNS traffic, I applied the filter udp.port == 53 in Wireshark and used the nslookup command. I tested the DNS resolution for two popular websites, google.com and disney.com, to observe the resulting traffic.



<img width="1376" alt="obseerving DNS" src="https://github.com/user-attachments/assets/67476ba7-5b22-4b8f-8e1e-c26b880e1644" />


<img width="1376" alt="obseeving DNS using port 53" src="https://github.com/user-attachments/assets/500b01f0-4930-4a19-aab6-dfc162d103f5" />


<img width="1470" alt="checking ip of disney" src="https://github.com/user-attachments/assets/158fcca3-dbd5-4104-94f5-1fb838cde0ab" />

To complete my lab, I decided to analyze RDP traffic. In Wireshark, I applied the filter tcp.port == 3389. Since RDP provides a continuous live stream from one computer to another (in this case, my local computer accessing the VM hosted on Azure), the traffic was constant, with data being continuously transmitted.


<img width="816" alt="observing RDP or port 3389" src="https://github.com/user-attachments/assets/112d7cfa-dd9e-4ddd-ad19-fe530e5ecb82" />
</p>
<br />

<h2>Lessons Learned </h2>

The purpose of this lab was to observe how different protocols and ports are utilized in a network between devices. While this lab doesn't directly involve troubleshooting, it provides valuable insight into network traffic. In a troubleshooting scenario, I would use tools like Wireshark and the command line to analyze how traffic flows through various ports and protocols. Developing familiarity with these tools and maintaining an inquisitive mindset are essential for success!







