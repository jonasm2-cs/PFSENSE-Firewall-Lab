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
| **Home LAN (physical)**     | — (host network) | —         | 192.168.0.0/24               | Provides Internet & bridges to lab         |
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

## Lessons Learned


