# PFSENSE-Firewall-Lab

## Objective
This project demonstrates a controlled cybersecurity lab environment by deploying a pfSense firewall in VirtualBox with distinct WAN and LAN network segments. Using Kali Linux as the external attacker and Ubuntu Desktop as the internal victim, the project showcases how DoS traffic can be generated, analyzed with Wireshark, and ultimately mitigated through targeted pfSense firewall rules and log analysis.


## Tools Used
<p>
  <img src="https://img.shields.io/badge/-pfSense-212121?style=for-the-badge&logo=pfsense&logoColor=white" />
  <img src="https://img.shields.io/badge/-Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" />
  <img src="https://img.shields.io/badge/-Kali%20Linux-268BEE?style=for-the-badge&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/-VirtualBox-183A61?style=for-the-badge&logo=virtualbox&logoColor=white" />
  <img src="https://img.shields.io/badge/-Wireshark-1679A7?style=for-the-badge&logo=wireshark&logoColor=white" />
</p>

## Resources
- <a href="https://www.youtube.com/watch?v=-yRvfbElT7M">PFSENSE Firewall (How to install and use)</a>
- <a href="https://drive.google.com/file/d/1Lr1NF7gJlQ8v2qirsi4klRLmhlQ4VShr/view">PFSENSE Firewall Lab Guide.pdf</a>
- <a href="https://www.pfsense.org/download/"> pfSense Installer</a>
- <a href="https://www.kali.org/get-kali/#kali-installer-images"> Kali Installer
- <a href="https://ubuntu.com/download/desktop"> Ubuntu Installer


## Table of Contents


## P1. Logical Diagram 
**Objective:** Utilize <a href ="https://app.diagrams.net/"> draw.io</a> to visualize our project</br>

<p align="center">
  <img src="https://github.com/user-attachments/assets/d9da3484-f3da-4d02-a7f3-59f4b8b37aa9" alt="pfSense Firewall Lab" />
</p>





| Device / Segment            | Adapter Type     | Interface | IP / Subnet                 | Role / Notes                               |
|-----------------------------|------------------|-----------|------------------------------|---------------------------------------------|
| **Home LAN (physical)**     | — (host network) | —         | 10.0.0.0/24               | Provides Internet & bridges to lab         |
| **pfSense – WAN**           | Bridged          | vtnet0    | 192.168.0.254/24 → GW 192.168.0.1 | Edge firewall toward home LAN              |
| **Kali Linux (Attacker)**   | Bridged          | eth0      | 192.168.0.200/24             | External attacker host                     |
| **Lab LAN (`LabNet`)**      | Internal Network | —         | 192.168.1.0/24               | Isolated subnet behind pfSense             |
| **pfSense LAN**           | Internal (`LabNet`) | vtnet1 | 192.168.1.1/24 (DHCP .100–.199) | Default GW & DHCP/DNS relay                |
| **Ubuntu Desktop (Victim)** | Internal (`LabNet`) | eth0   | 192.168.1.100/24             | Victim workload                            |




## P2. Installing our .iso files
**Objective:** Install .iso files of PfSense, Kali Linux and Ubuntu
### Step 1: Installing pfSense
<img width="1192" height="884" alt="image" src="https://github.com/user-attachments/assets/298660db-1325-40a3-8032-ac825acf6e88" />
Click <a href="https://www.pfsense.org/download/">here</a> to download the latest version of pfSense > *Make sure you choose "AMD64 ISO IPMI/Virtual Machines" in the drop down when prompeted*

### Step 2: Installing Kali Linux
<img width="1245" height="1251" alt="image" src="https://github.com/user-attachments/assets/b94a2871-1d74-494e-a59a-b873d47e087b" />
Click <a href="https://www.kali.org/get-kali/#kali-installer-images">here</a> to download the latest version of Kali


### Step 3: Installing Ubuntu
<img width="1258" height="1022" alt="image" src="https://github.com/user-attachments/assets/e3db16de-8fcf-4091-976b-4a84a2c5b050" />
Click <a href="https://ubuntu.com/download/desktop">here</a> to download the latest version of Ubuntu


## P3. Setting up our Oracle Virtual Box
<img width="1270" height="809" alt="image" src="https://github.com/user-attachments/assets/16278767-d841-48e2-9b24-9346aa3cee85" />
**Objective:** Import our recently installed .iso files into Oracle VirtualBox

### Step 1: Setting up our pfSense VM
- **Name:** PFSENSE Firewall
- **ISO:** netgate-installer-v1.1-amd64.iso
- **Type:** BSD
- **Subtype:** FreeBSD
- **Version:** FreeBSD(64-Bit)
- **Hardware:**
  - **Base Memory:** 2048MB
  - **Processors:** 2
- **Hard Disk:**
  - 20.00 GB
- **Network:**
  - **Adapter 1:** Bridged Adapter
  - **Adapter 2:** LabNet

### Step 2: Setting up our Ubuntu desktop
- **Name:** Ubuntu-desktop
- **ISO:** ubuntu-24.04.03-desktop-amd64.io
- **Type:** Linux
- **Subtype:** Ubuntu
- **Version:** Ubuntu(64-Bit)
- **Hardware:**
  - **Base Memeory** 2048MB (2GB)
  - **Processors:** 2
- **Hard Disk:**
  - 25.00 GB
- **Network:**
  - **Adapter 1:** Internal Network
  - **Name:** LabNet
 
### Step 3: Setting up our Kali Linux VM
- **Name:** Kali Linux
- **ISO:** kali-linux-2025.3-installer-amd64.iso
- **Type:** Linux
- **Subtype:** Ubuntu
- **Version:** Ubuntu(64-Bit)
- **Hardware:**
  - **Base Memeory** 2048MB (2GB)
  - **Processors:** 2
- **Hard Disk:**
  - 25.00 GB
- **Network:**
  - Adapter 1: Bridged

## P4. Setting up pfSense VM
### Step 1: Installing pfSense
<img width="715" height="404" alt="image" src="https://github.com/user-attachments/assets/9b22a00d-34f4-48cb-aea1-529c1ab359d9" />

- **WAN Interface Assignment and Configuration:** em0
- **WAN (em0) Network Mode Setup:** Keep Defaults
- **LAN Interface Assignment and Configuration:** em1
- **LAN (em1) Network Mode Setup:** Keep Defaults
- **Interface Assignment and Configuration:** Continue
- **Active Subscription Validation:** Install CE
- Keep Defaults or the first option
- Install > Shell > Power off
- In Virtualbox Remove the netgear installation attatchment from Stroage

<img width="715" height="390" alt="image" src="https://github.com/user-attachments/assets/8e3a2505-179c-4148-9e3a-d8007edbd47f" />

### Step 2: Disabling the Firewall
<img width="1553" height="785" alt="image" src="https://github.com/user-attachments/assets/3a18666a-86e4-49c6-bc94-86584f065e7a" />


In the pfsense VM (Left of photo) > Enter option: 8 > pfctl -d > Verify if you can access the WAN in a web browser > Username: admin > Password: pfsense

## P5. Configuring pfSetup GUI
<img width="1266" height="564" alt="image" src="https://github.com/user-attachments/assets/85f7afd9-0f79-46df-825f-fb956003c6e1" />

- Login to pfSense > System > Setup wizard > Next
- **Time Server Information:** Change Timezone Accordingly 
- **Configure WAN Interface:**
  -  **RFC1918 Networks:** Uncheck Block private Networks from entering via WAN
  -  **Block bogon networks:** Uncheck Block non-internet routed networks from entering via WAN
- **Configure LAN Interface:** Keep Defaults
- **Change admin Account Password:** *any password*
- Reload
- In the pfSense firewall Console disable the firewall: pfctl -d

## P6: Creating a Security Policy for WAN access
<img width="1270" height="526" alt="image" src="https://github.com/user-attachments/assets/0d9bb40c-ce43-4153-ac02-8a558a67ef0b" />

- Login to pfSense > Firewall > Rules > Add
- **Edit:**
  - **Edit Firewall Rule:**
    - **Action:** Pass
  - **Source:**
    - Network > <*home network*>/24
  - **Destination:**
    - Network > <*pfSense WAN*>/24
  - **Extra Options:**
    - Check Log packets that are handled by this rule
- Save > Apply Changes
- Verify > Hover over States > under the States Section you should see logs *see below
<img width="1274" height="515" alt="image" src="https://github.com/user-attachments/assets/3354bd48-0e30-4166-9455-7535391046e4" />

## P7. Setting up DHCP on pfSense LAN
Login to pfSense > Services > DHCP Server
- **General Settings:**
  - Ensure *Enable DHCP Server on LAN Interface* is checked
- **Primary Address Pool**
  - Subnet: 192.168.1.0/24
  - Subnet Range: 192.168.1.1 - 192.168.1.254
- **Server Options:**
  - DNS Servers: *<home-router-address>*
- Save > Apply Changes
- Open Ubuntu VM > Open Terminal > ifconfig > Verify if Ip is 192.168.1.100/24 > ping pfSense LAN
<img width="1068" height="203" alt="image" src="https://github.com/user-attachments/assets/1ec5b51e-c4c6-4ae7-83f6-6741ad496d8b" />

## P8. Setting up Kali Linux



## Lessons Learned


