# KineoPiRouter - Raspberry Pi Router Lab

## Description
This lab documents how I configured a Raspberry Pi 5 running Raspberry Pi OS to function as a home lab router. The goal of this project was to learn basic router configuration, IP forwarding, DHCP, NAT, firewall rules, SSH administration, and network troubleshooting.

This lab was completed in a controlled home lab environment for educational and cybersecurity training purposes.

---

## Lab Objectives
- Configure the Raspberry Pi as a router
- Use one interface for internet access
- Use another interface for client device access
- Enable IPv4 packet forwarding
- Configure NAT using iptables
- Install networking tools for monitoring and troubleshooting
- Enable SSH for remote administration
- Verify client devices can reach the internet through the Raspberry Pi

---

## Equipment Used
- Raspberry Pi 5
- Raspberry Pi OS
- Ethernet cable
- USB Wi-Fi adapter
- Windows 11 laptop
- PowerShell
- SSH
- Home internet connection

---

## Network Design

```text
Internet / Upstream Router
          |
      wlan1 or eth0
   Raspberry Pi Router
      wlan0 or eth0
          |
    Client Devices

## Step 1: Update Raspberry Pi OS

Run the following command to update the package list and upgrade installed packages:

```bash
sudo apt update && sudo apt upgrade -y
```

## Step 1: Update Raspberry Pi OS

Run the following command to update the package list and upgrade installed packages:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## Step 2: Install Required Networking Tools

Install the tools needed for routing, DHCP, firewall rules, SSH, and WAP configuration:

```bash
sudo apt install -y iptables-persistent netfilter-persistent dnsmasq hostapd net-tools openssh-server
```

---

## Step 3: Identify Network Interfaces

Use this command to view all network interfaces:

```bash
ip addr
```

Example interface layout:

```text
wlan1 = internet connection
wlan0 = wireless access point
eth0  = optional wired interface
```

---

## Step 4: Configure a Static IP Address for wlan0

Open the DHCP client configuration file:

```bash
sudo nano /etc/dhcpcd.conf
```

Add the following lines at the bottom of the file:

```bash
interface wlan0
static ip_address=192.168.10.1/24
nohook wpa_supplicant
```

Save the file and reboot:

```bash
sudo reboot
```

Verify the address:

```bash
ip addr show wlan0
```

Expected output:

```text
192.168.10.1/24
```

---

## Step 5: Configure DHCP with DNSMasq

Backup the default DNSMasq configuration:

```bash
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.backup
```

Create a new configuration file:

```bash
sudo nano /etc/dnsmasq.conf
```

Add the following configuration:

```bash
interface=wlan0
dhcp-range=192.168.10.50,192.168.10.150,255.255.255.0,24h
dhcp-option=3,192.168.10.1
dhcp-option=6,8.8.8.8,1.1.1.1
```

Restart DNSMasq:

```bash
sudo systemctl restart dnsmasq
sudo systemctl enable dnsmasq
```

---

## Step 6: Create the HostAPD Configuration File

Create the HostAPD configuration:

```bash
sudo nano /etc/hostapd/hostapd.conf
```

Add the following configuration:

```bash
interface=wlan0
driver=nl80211
ssid=KineoPiRouter
hw_mode=g
channel=6
wmm_enabled=0
macaddr_acl=0
auth_algs=1
ignore_broadcast_ssid=0
wpa=2
wpa_passphrase=ChangeThisPassword123
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
```

---

## Step 7: Point HostAPD to the Configuration File

Open the HostAPD default configuration:

```bash
sudo nano /etc/default/hostapd
```

Find:

```bash
#DAEMON_CONF=""
```

Change it to:

```bash
DAEMON_CONF="/etc/hostapd/hostapd.conf"
```

Save the file.

---

## Step 8: Enable HostAPD

Unmask and enable HostAPD:

```bash
sudo systemctl unmask hostapd
sudo systemctl enable hostapd
```

Start HostAPD:

```bash
sudo systemctl start hostapd
```

Verify HostAPD status:

```bash
sudo systemctl status hostapd
```

---

## Step 9: Enable IPv4 Forwarding

Open the sysctl configuration file:

```bash
sudo nano /etc/sysctl.conf
```

Find:

```bash
#net.ipv4.ip_forward=1
```

Change it to:

```bash
net.ipv4.ip_forward=1
```

Apply the change:

```bash
sudo sysctl -p
```

Verify:

```bash
cat /proc/sys/net/ipv4/ip_forward
```

Expected output:

```text
1
```

---

## Step 10: Configure NAT

Replace wlan1 with your internet-facing interface.

Create the NAT rule:

```bash
sudo iptables -t nat -A POSTROUTING -o wlan1 -j MASQUERADE
```

Allow forwarding:

```bash
sudo iptables -A FORWARD -i wlan0 -o wlan1 -j ACCEPT
```

Allow return traffic:

```bash
sudo iptables -A FORWARD -i wlan1 -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

Save the rules:

```bash
sudo netfilter-persistent save
```

---

## Step 11: Enable SSH

Enable SSH:

```bash
sudo systemctl enable ssh
```

Start SSH:

```bash
sudo systemctl start ssh
```

Verify status:

```bash
sudo systemctl status ssh
```

---

## Step 12: Connect a Client Device

Search for available wireless networks.

Locate the SSID:

```text
KineoPiRouter
```

Connect using the configured WPA2 password:

```text
ChangeThisPassword123
```

---

## Step 13: Verify DHCP Assignment

Verify connected devices receive an IP address.

Check active DHCP leases:

```bash
cat /var/lib/misc/dnsmasq.leases
```

Check neighboring devices:

```bash
ip neigh
```

Expected IP range:

```text
192.168.10.50 - 192.168.10.150
```

---

## Step 14: Test Connectivity

Test internet connectivity from the Raspberry Pi:

```bash
ping 8.8.8.8
```

Test DNS resolution:

```bash
ping google.com
```

Verify the router interface:

```bash
ping 192.168.10.1
```

View routes:

```bash
ip route
```

---

## Step 15: Final Validation

Verify HostAPD:

```bash
sudo systemctl status hostapd
```

Verify DNSMasq:

```bash
sudo systemctl status dnsmasq
```

Verify SSH:

```bash
sudo systemctl status ssh
```

Verify NAT rules:

```bash
sudo iptables -t nat -L -v
```

Verify forwarding rules:

```bash
sudo iptables -L -v
```

Reboot the Raspberry Pi:

```bash
sudo reboot
```

Confirm the wireless network is broadcasting and clients can obtain an IP address and access the internet through the Raspberry Pi Wireless Access Point.
##Congratulations!!!


## Useful Commands

| Command | Description |
|----------|-------------|
| `ip addr` | View network interfaces and IP addresses |
| `iw dev` | Display wireless interfaces |
| `iw list` | Verify AP mode support |
| `ip route` | View routing table |
| `cat /proc/sys/net/ipv4/ip_forward` | Check IPv4 forwarding status |
| `sudo sysctl -p` | Apply sysctl configuration changes |
| `sudo systemctl status hostapd` | Check WAP service status |
| `sudo systemctl restart hostapd` | Restart HostAPD |
| `sudo systemctl status dnsmasq` | Check DHCP service status |
| `sudo systemctl restart dnsmasq` | Restart DNSMasq |
| `sudo systemctl status ssh` | Check SSH service status |
| `ip neigh` | View connected devices |
| `cat /var/lib/misc/dnsmasq.leases` | View DHCP leases |
| `sudo iptables -L -v` | View firewall rules |
| `sudo iptables -t nat -L -v` | View NAT rules |
| `sudo netfilter-persistent save` | Save firewall rules |
| `ping 8.8.8.8` | Test internet connectivity |
| `ping google.com` | Test DNS resolution |
| `journalctl -u hostapd` | View HostAPD logs |
| `journalctl -u dnsmasq` | View DNSMasq logs |
| `ss -tulnp` | View listening ports and services |
| `ifconfig` | Display interface statistics |
| `sudo reboot` | Reboot the Raspberry Pi |
| `sudo shutdown now` | Shut down the Raspberry Pi |
| `ssh pi@192.168.10.1` | Connect to Raspberry Pi via SSH |
| `sudo apt update && sudo apt upgrade -y` | Update installed packages |
| `nmap -sn 192.168.10.0/24` | Discover hosts on the network |

## Issues Encountered and Troubleshooting Performed

- USB Wi-Fi adapter was not initially detected by Raspberry Pi OS.
- Had to verify compatible Linux drivers for the wireless adapter.
- Determined which wireless interface (`wlan0` vs `wlan1`) would be used for internet connectivity and which would be used for the Wireless Access Point.
- NetworkManager automatically attempted to reconnect interfaces and interfered with WAP configuration.
- HostAPD service failed to start due to configuration errors.
- HostAPD configuration file path was not properly referenced in `/etc/default/hostapd`.
- Wireless interface did not initially support AP mode and required validation using `iw list`.
- Incorrect static IP address configuration prevented client devices from connecting.
- DNSMasq service failed to start due to configuration syntax errors.
- DHCP leases were not initially being distributed to client devices.
- IPv4 forwarding was disabled by default and required modification in `/etc/sysctl.conf`.
- NAT rules were not persisting after reboot until `iptables-persistent` was configured.
- Client devices could connect to the SSID but could not access the internet until NAT and forwarding rules were properly configured.
- Firewall rules prevented traffic from passing between interfaces until forwarding rules were added.
- SSH connectivity issues required verification of service status and IP addressing.
- Raspberry Pi occasionally received different IP addresses from the upstream network, requiring interface verification.
- Difficulty determining which interface was acting as the upstream connection and which interface was broadcasting the WAP.
- Multiple reboots were required while testing DHCP, HostAPD, and interface configurations.
- Verified that DNS resolution was functioning correctly by testing external domain name lookups.
- Confirmed successful operation by validating client connectivity, DHCP lease assignment, internet access, and wireless network broadcasting.

---

## Lessons Learned

- How to configure a Raspberry Pi as a Wireless Access Point.
- How DHCP services distribute IP addresses to connected clients.
- How NAT allows multiple devices to share a single internet connection.
- How IPv4 forwarding enables packet routing between interfaces.
- How HostAPD broadcasts and manages a wireless network.
- How DNSMasq provides DHCP functionality for client devices.
- How to troubleshoot Linux networking services using system logs and service status commands.
- How to verify wireless adapter capabilities and AP mode support.
- How firewall rules affect traffic flow between network interfaces.
- How to remotely administer a Raspberry Pi using SSH.
