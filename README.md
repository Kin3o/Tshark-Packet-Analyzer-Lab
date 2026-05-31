<h1>KineoPiRouter - TShark Packet Analyzer Lab</h1>

<h2>Description</h2>
This projects purpose was to be able to analyze all of the incoming traffic on my Raspberry Pi Router. I did this lab to further my knowledge on network troubleshooting, security and linux administration. I will be using Tshark instead of Wireshark because my I primarily configure my router through SSH on Windows PowerShell
<br />

<h2>Background Configurations</h2>

<ul>
  <li><b>Raspberry Pi 5 configured as a wireless access point (WAP) running Raspberry Pi OS (Debian-based).</b></li>
  <li><b>Raspberry Pi connected to the upstream modem/router via Ethernet for internet connectivity.</b></li>
  <li><b>SSH enabled to allow remote administration and management of the Raspberry Pi.</b></li>
  <li><b>Wireless clients connected to the Raspberry Pi access point for packet capture and traffic analysis.</b></li>
  <li><b>Packet captures performed on active network interfaces to observe real-time network communications.</b></li>
  <li><b>Testing conducted in a controlled home lab environment for educational and network security monitoring purposes.</b></li>

<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b> 

<h2>Environments Used </h2>

- <b>Windows 11</b> 

<h2>Lab walk-through:</h2>

<p align="center">
Launch Windows Powershell and SSH into Raspberry Pi: <br/>
<img src="https://i.imgur.com/F1qT6IJ.png" height="80%" width="80%" alt="test screen shot"/>
<br />
<br />
Install tshark:  <br/>
<img src="https://i.imgur.com/1ObY6n1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
If prompted "List all availible interfaces: <br/>
<img src="https://i.imgur.com/JqvtVmN.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Capture all data on wlan0 (The interface my router is wirelessly broadcasting):  <br/>
<img src="https://i.imgur.com/q3dStCT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Filter specific traffic data:  <br/>
<img src="https://i.imgur.com/SP2HW13.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Lets run the command to see what devices are doing on the AP. We can see what platforms they are usings such as Google, Youtube, Twitter Etc:  <br/>
<img src="https://i.imgur.com/SZx4PCJ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  <h2>How this application can be used</h2>
If we look at the packets that were being intercepted we can see what device they are using (Apple or Samsung), what domains they visit (Youtube or Google) and we can monitor large data tranfers, suspicous IP addresses and unsafe domains. TShark is practical for a SOC analyst because it lets you investigate network activity at the packet level. While many SOCs use tools like SIEMs, EDRs, IDSs, and network monitoring platforms, packet captures are often used to validate alerts and understand exactly what happened on the wire.
<br />


<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
