# azure-network-protocols

<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we analyze various network traffic flows to and from Azure Virtual Machines using Wireshark and explore the functionality of Network Security Groups.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Windows and Linux Command Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10
- Ubuntu Server 22.04

<h2>Actions and Observations</h2>

<p>
<img src="https://i.imgur.com/EwX8Y7s.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/JGhlxJJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Create two virtual machines in the Azure portal: one running Windows 10 Pro and the other running a Linux distribution, each configured with 2 vCPUs. Make sure both VMs are connected to the same virtual network during deployment. Once created, use RDP to access the Windows VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/1BYvdJZ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Download and install Wireshark from https://www.wireshark.org. This tool will be used to observe network traffic.
</p>
<br />

<p>
<img src="https://i.imgur.com/3gTUIDW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After installing Wireshark, launch the application and use the search bar to filter for ICMP traffic. ICMP packets are commonly used by network devices to send error messages and verify connectivity.
</p>
<br />

<p>
<img src="https://i.imgur.com/HgLp5AR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Open PowerShell and use the ping command to test connectivity to the Linux VM’s private IP address, since both machines are on the same virtual network. You can locate the Linux VM’s private IP in the Azure portal under its networking settings.
</p>
<br />

<p>
<img src="https://i.imgur.com/nurPwTN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
As you run the ping command, Wireshark will display each ICMP packet in real time, allowing you to observe and analyze the traffic being sent and received.
</p>
<br />

<p>
<img src="https://i.imgur.com/X4yfgee.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, run the ping -t command to continuously ping the Linux VM. Then, in the Azure portal, open the Linux VM’s Network Settings and create a new inbound port rule that blocks ICMP traffic. This will effectively prevent the VM from responding to ping requests.
</p>
<br />

<p>
<img src="https://i.imgur.com/UvdXvva.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
After blocking ICMP traffic on the Linux VM’s firewall, verify in Wireshark that the ping requests are no longer receiving replies. In PowerShell, the continuous ping will now display “Request Timed Out,” confirming that ICMP responses are being blocked.
</p>
<br />

<p>
<img src="https://i.imgur.com/QstVuqS.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, remove the inbound port rule to restore ICMP traffic. Once ICMP is allowed again, stop the continuous ping in PowerShell by pressing Ctrl + C.
</p>
<br />

<p>
<img src="https://i.imgur.com/5sVds9j.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we’ll move on to filtering SSH traffic in Wireshark. At this point, you should already be comfortable navigating the Wireshark interface. Open Command Prompt on your Windows machine and start an SSH session to the Linux VM by running:

ssh labuser2@10.0.0.5

Once the connection is established, you’ll see the SSH packets appear in Wireshark, where you can monitor and analyze them in real time. When you’re finished, type exit in the terminal to close the SSH session.
</p>
<br />

<p>
<img src="https://i.imgur.com/WlKABeY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Next, we’ll focus on filtering DNS traffic in Wireshark. Begin by applying a filter so that only DNS packets are shown. To generate DNS activity, run the following command:

nslookup www.yahoo.com

This will send a DNS request to look up Yahoo’s IP address, which you’ll be able to observe in Wireshark.
</p>
<br />

<p>
<img src="https://i.imgur.com/afL911f.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Now we’ll move on to filtering DHCP traffic in Wireshark. DHCP (Dynamic Host Configuration Protocol) is responsible for assigning IP addresses to devices on a network. To generate DHCP activity, run the following command in PowerShell:

ipconfig /renew

This forces your machine to request a new IP address, and Wireshark will capture the resulting DHCP packets for you to analyze.
</p>
<br />

<p>
<img src="https://i.imgur.com/KUhVqBe.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lastly, we’ll filter for RDP traffic in Wireshark. Because we’re actively using Remote Desktop Protocol to access our virtual machine, RDP packets will be flowing constantly, giving you plenty of live data to observe and analyze.
</p>
<br />
