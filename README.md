<h1>Tshark Packet Analyzer Lab</h1>

<h2>Description</h2>

This lab documents the installation, configuration, and use of Tshark on a Raspberry Pi 5 running Raspberry Pi OS. The objective of this project was to gain hands-on experience with packet capture, protocol analysis, DNS monitoring, and command-line network troubleshooting techniques commonly used by SOC analysts and network administrators.

<br />

<h2>Languages and Utilities Used</h2>

- <b>PowerShell</b>
- <b>Bash Shell Scripting</b>
- <b>Tshark</b>
- <b>SSH</b>

<h2>Environments Used</h2>

- <b>Windows 11</b>
- <b>Raspberry Pi OS (Debian)</b>

<h2>Lab Walkthrough</h2>

<h3>Step 1: Update Raspberry Pi OS</h3>

Update installed packages and repositories.

```bash
sudo apt update && sudo apt upgrade -y
```

<h3>Step 2: Install Tshark</h3>

Install Tshark using the Raspberry Pi package repository.

```bash
sudo apt install tshark -y
```

Verify installation:

```bash
tshark --version
```

<h3>Step 3: Identify Available Interfaces</h3>

View available network interfaces.

```bash
ip addr
```

List interfaces recognized by Tshark.

```bash
tshark -D
```

Example:

```text
1. eth0
2. wlan0
3. any
```

<h3>Step 4: Start a Basic Packet Capture</h3>

Begin capturing traffic on the wireless interface.

```bash
sudo tshark -i wlan0
```

Observe packets in real time.

<h3>Step 5: Capture a Limited Number of Packets</h3>

Stop automatically after capturing 25 packets.

```bash
sudo tshark -i wlan0 -c 25
```

<h3>Step 6: Monitor DNS Traffic</h3>

Capture DNS queries and responses.

```bash
sudo tshark -i wlan0 port 53
```

Generate traffic by browsing websites.

<h3>Step 7: Capture ICMP Traffic</h3>

Generate ping traffic.

```bash
ping google.com
```

Capture ICMP packets.

```bash
sudo tshark -i wlan0 icmp
```

<h3>Step 8: Capture HTTP Traffic</h3>

Capture web traffic.

```bash
sudo tshark -i wlan0 port 80
```

<h3>Step 9: View Detailed Packet Information</h3>

Display verbose packet information.

```bash
sudo tshark -i wlan0 -V
```

<h3>Step 10: Save a Packet Capture</h3>

Write packets to a capture file.

```bash
sudo tshark -i wlan0 -w tshark-lab.pcap
```

Stop capture:

```text
CTRL + C
```

<h3>Step 11: Read a Saved Capture File</h3>

Open a saved capture.

```bash
tshark -r tshark-lab.pcap
```

<h3>Step 12: Filter Packets by Protocol</h3>

Display only DNS traffic.

```bash
tshark -r tshark-lab.pcap -Y dns
```

Display only ICMP traffic.

```bash
tshark -r tshark-lab.pcap -Y icmp
```

<h3>Step 13: Generate Test Traffic</h3>

Create traffic by:

- Browsing websites
- Running ping commands
- Connecting additional devices

Observe protocol changes in the capture output.

<h3>Step 14: Analyze Packet Data</h3>

Review:

- Source IP addresses
- Destination IP addresses
- DNS requests
- Protocol types
- Packet timestamps
- Network conversations

<h3>Step 15: Validate Tshark Functionality</h3>

Verify packet capture operations.

```bash
tshark --version
```

```bash
tshark -D
```

```bash
sudo tshark -i wlan0 -c 10
```

<h2>Issues Encountered and Troubleshooting Performed</h2>

- Tshark was not installed by default on Raspberry Pi OS.
- Required elevated privileges to capture packets.
- Needed to determine the correct interface for packet capture.
- Generated insufficient traffic during initial testing.
- DNS packets were not visible until proper filters were applied.
- Packet output became overwhelming without protocol filtering.
- Interface naming required verification before captures could begin.

<h2>Useful Commands</h2>

| Command | Description |
|----------|-------------|
| `tshark --version` | Verify Tshark installation |
| `tshark -D` | List capture interfaces |
| `ip addr` | View network interfaces |
| `sudo tshark -i wlan0` | Start live capture |
| `sudo tshark -i wlan0 -c 25` | Capture specific number of packets |
| `sudo tshark -i wlan0 port 53` | Capture DNS traffic |
| `sudo tshark -i wlan0 icmp` | Capture ICMP traffic |
| `sudo tshark -i wlan0 -V` | Display packet details |
| `sudo tshark -i wlan0 -w capture.pcap` | Save capture file |
| `tshark -r capture.pcap` | Read saved capture |
| `ping google.com` | Generate traffic |

<h2>Skills Practiced</h2>

- Linux Administration
- Packet Analysis
- Network Monitoring
- Protocol Identification
- DNS Analysis
- Traffic Filtering
- Network Troubleshooting
- SOC Analyst Workflows

<h2>Final Result</h2>

Successfully installed and configured Tshark on a Raspberry Pi 5. Captured, filtered, and analyzed live network traffic, identified DNS and ICMP communications, and gained practical experience with packet analysis techniques used in SOC environments.
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
