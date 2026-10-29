# Kali-2026.3-VM-lab-setup


# Project Overview
------------------------------------------------------------------------
This project focuses on setting up a virtual cybersecurity and penetration-testing laboratory using a modern hypervisor and the latest Kali Linux release. The goal is to create a controlled, reproducible environment where network scanning, vulnerability assessment, exploitation practice, and packet analysis can be performed safely. The lab is built on a private virtual network so that additional target machines—such as intentionally vulnerable VMs—can be added later.


# Objectives
---------------------------------------------------------------
* Install and configure a current hypervisor (VirtualBox 7.2.22).

* Import/install Kali Linux 2026.3 as the attacker workstation.

* Create a private NAT Network for isolated lab traffic.

* Assign consistent static IP addresses to lab machines.

* Verify connectivity, DNS resolution, and tool availability.

* Take a clean snapshot for rollback.


# Lab Architecture
----------------------------------------------------------
[Host OS]
   │
  
   ├── VirtualBox NAT Network (10.0.0.0/24)
  
   │       ├── Kali Linux 2026.3  → 10.0.0.2 (Attacker)
   
  
   │
  
   └── Internet (via NAT)


# Lab Configuration
----------------------
# Component	                    Configuration
---------------------------------------------
 Host OS     ------>	Windows 10

Hypervisor	   ------>  VirtualBox 7.2.22 

VM OS  ------>	Kali Linux 2026.3 

Virtual Network	  ------>  NAT Network

Network Address	 ------>  10.0.0.0/24

Kali IP	  ------>  10.0.0.2/24

Target IP	  ------>  10.0.0.3/24

Gateway	  ------>  10.0.0.1

DNS	  ------>  8.8.8.8

Future VM Range	10.0.0.4 – 10.0.0.99


# Lab Setup Procedure
---------------------------
# Step 1 — Install the Hypervisor

Install VirtualBox 7.2.22

>[!Note:]
> Enable hardware virtualization (Intel VT‑x / AMD‑V) in the host BIOS/UEFI before starting any VM.

# Step 2 — Create the NAT Network In VirtualBox:

Network Name: NatNetwork

IPv4 Prefix: 10.0.0.0/24

DHCP: Enabled

IPv6: Disabled

A NAT Network allows multiple VMs to communicate with each other and reach the internet through the host, which is ideal for a multi‑machine lab.


# Step 3 — Import Kali Linux 2026.3

Download the latest Kali Linux 2026.3 installer or pre‑built VM image from the official [kali.org](https://kali.org/get-kali).

Import the VM into VirtualBox and configure the network adapter:

Adapter 1: Attached to NAT Network

Network: NatNetwork

Adapter Type: Intel PRO/1000 MT Desktop

RAM: 2048–4096 MB (recommended)

A shared folder can be added to transfer tools and scripts between the host and Kali.
<img width="2736" height="1824" alt="kali nat config" src="https://github.com/user-attachments/assets/bd2921d8-5155-411d-8929-f061e050eba4" />


# Step 4 — Configure Static IP Addressing

Assign consistent IPv4 addresses to the VM.

Kali Linux (2026.3):
<img width="2736" height="1824" alt="k8" src="https://github.com/user-attachments/assets/a332b0d9-76d4-477b-8f79-eaf087892e73" />


#Step 5 — Create a Clean Snapshot
-----------------------------------
After verifying that the VM boot, network correctly, and have the required tools, take a clean snapshot of each machine.

Snapshot name (Kali): Clean Kali 2026.3 – Network Ready

This snapshot becomes your known‑good baseline. If a future exercise corrupts the VM, you can restore it instantly.


# Lab Verification

# Test	Command	    Expected         Result

Check Kali IP	ip a	10.0.0.2/24 displayed

Check target IP	ip a (Linux target) or ipconfig (Windows target)	10.0.0.3/24 displayed

Test gateway	ping 10.0.0.1	Successful replies

Test internet	ping 8.8.8.8	Successful replies

Test DNS	nslookup kali.org	Domain resolves

Verify Nmap	nmap --version	Nmap version 7.x+ displayed

Snapshot restore	Restore snapshot, run ip a	Baseline configuration restored


# Problem 1 — Internet Connectivity After Static IP Configuration

Symptom: After manually setting a static IP on Kali, DNS resolution or internet access failed.

# Solution: I Disable IPv4 Duplicate Address Detection (DAD) for the connection:
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0

Then restart the connection or reboot.

>[!Tip:]
> Always confirm the exact connection name with nmcli connection show before running the command.


# Problem 2 — Hardware Virtualization Disabled (VT‑x / AMD‑V)

Symptom: The VM fails to start with an error about hardware virtualization.

# Solution: Reboot the host, enter BIOS/UEFI, enable Intel VT‑x (or AMD‑V), save, and restart.


# What I Learned
----------------------------------------------------------------------------

* NAT vs NAT Network: A NAT Network enables VM‑to‑VM communication while still providing outbound internet access—essential for a multi‑machine lab.

* Static IP planning: A consistent addressing scheme (10.0.0.0/24) simplifies documentation and repeatable exercises.

* Snapshot discipline: Always snapshot before installing tools or running exploits, so you can return to a clean baseline.

* Documentation: Recording commands, screenshots, and problems/solutions is as important as the technical setup itself.


# Security & Ethical Use
-----------------------------------------------------------------------------
⚠️ This laboratory is for education and authorized security testing only.
All exercises must be performed on systems you own or have explicit written permission to test. Never use these tools or techniques against unauthorized systems.


# Tools & Resources


Tool	and Link
VirtualBox 7.2.22	  -->  [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

Kali Linux 2026.3  --> 	[Kali](https://www.kali.org/get-kali/)

WINRAR  -->  [Winrar]()


# Author
Nicholas Gyemfi 

Cybersecurity Entusiast

LinkedIn:
