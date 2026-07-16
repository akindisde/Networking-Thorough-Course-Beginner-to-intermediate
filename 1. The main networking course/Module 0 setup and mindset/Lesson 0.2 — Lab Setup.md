# Module 0 — Setup and Mindset

## Lesson 0.2 — Lab Setup

---

### Introduction

A networking course requires a practical environment in which concepts can be tested, configurations can be applied, and protocols can be observed. Reading documentation or watching demonstrations is not sufficient to develop networking skills. Every concept introduced throughout this course will be reinforced through hands-on laboratories.

This lesson prepares the working environment used for the remainder of the course. By the end of this lesson, the required software will be installed, network connectivity will be verified, and the workspace will be organized for future labs.

---

# Learning Objectives

Upon completing this lesson, students should be able to:

- Install Cisco Packet Tracer.
- Install GNS3.
- Install Wireshark.
- Understand the purpose of each tool.
- Verify basic network connectivity using command-line utilities.
- Organize course files and laboratory projects.
- Prepare a workstation suitable for the remainder of the course.

---

# Required Software

The following software will be used throughout the course.

| Software                      | Purpose                    | Required Now |
| ----------------------------- | -------------------------- | ------------ |
| Cisco Packet Tracer           | Network simulation         | Yes          |
| Wireshark                     | Packet analysis            | Yes          |
| GNS3                          | Advanced network emulation | Install only |
| Visual Studio Code (optional) | Notes and documentation    | Recommended  |
| Draw.io (optional)            | Network diagrams           | Recommended  |

---

# Hardware Requirements

The course does not require enterprise networking hardware. All practical exercises can be completed using a standard personal computer.

Recommended specifications:

| Component | Recommended |
|------------|------------|
| CPU | Quad-Core Processor |
| Memory | 8 GB Minimum (16 GB Recommended) |
| Storage | 20 GB Free Space |
| Operating System | Windows 10/11, Linux, or macOS |
| Internet Connection | Stable Broadband |

Students intending to build larger GNS3 laboratories in later modules will benefit from additional RAM and CPU resources.

---

# Cisco Packet Tracer

## Overview

Cisco Packet Tracer is a network simulator developed by Cisco Systems. It allows routers, switches, computers, servers, wireless devices, and IoT devices to be interconnected in a virtual environment.

Unlike a network emulator, Packet Tracer simulates device behavior rather than executing the actual Cisco IOS operating system. This approach significantly reduces hardware requirements while providing sufficient functionality for learning routing and switching fundamentals.

Packet Tracer will be the primary laboratory environment during the first half of this course.

---

## Creating a Cisco Networking Academy Account

Packet Tracer is available free of charge through Cisco Networking Academy (NetAcad).

Registration requires:

- Name
- Email address
- Password

After registration, Packet Tracer can be downloaded for the appropriate operating system.

---

## Installation

Install Packet Tracer using the default installation options.

After installation:

1. Launch Packet Tracer.
2. Sign in using the Cisco Networking Academy account.
3. Verify that the application opens successfully.
4. Create a blank project.
5. Save the project.

No configuration is required at this stage.

---

## Interface Overview

The Packet Tracer interface consists of several major components.

### Workspace

The workspace is the central area where network topologies are constructed.

---

### Device Palette

The device palette contains:

- Routers
- Switches
- End Devices
- Wireless Devices
- Security Devices
- WAN Equipment

These devices are dragged into the workspace during laboratory exercises.

---

### Connection Tools

Connection tools provide various cable types including:

- Copper Straight-Through
- Copper Cross-Over
- Fiber
- Console Cable
- Serial Connections

Cable selection becomes important in later modules.

---

### Simulation Mode

Packet Tracer supports two operating modes.

**Realtime Mode**

Packets move continuously.

Used during normal configuration.

**Simulation Mode**

Allows protocols to be observed step-by-step.

Useful for:

- ARP
- DHCP
- DNS
- Routing
- ICMP
- TCP

Simulation mode will be used extensively throughout the course.

---

# Wireshark

## Overview

Wireshark is the industry's most widely used protocol analyzer.

Unlike Packet Tracer, Wireshark captures actual packets transmitted across a network interface.

It provides visibility into every protocol layer, making it an essential troubleshooting and learning tool.

Throughout this course, every major protocol will be analyzed using Wireshark.

---

## Installation

Download the latest stable version of Wireshark.

During installation:

- Install Npcap when prompted.
- Allow USBPcap installation only if USB packet capture is desired.
- Use default installation settings.

Npcap provides packet capture capabilities required by Wireshark.

---

## First Launch

Open Wireshark.

The main window displays all available network interfaces.

Typical interfaces include:

- Ethernet
- Wi-Fi
- Virtual Machines
- VPN Interfaces
- Loopback Interface

Each interface displays live traffic statistics.

---

## Performing the First Capture

Select the interface currently connected to the Internet.

Start packet capture.

Open a web browser.

Visit any website.

Return to Wireshark.

Stop the capture.

Packets should now appear in the capture window.

No attempt should be made to understand these packets yet.

The objective is simply to verify that packet capture is functioning correctly.

---

# GNS3

## Overview

GNS3 (Graphical Network Simulator 3) is an advanced network emulator capable of running real network operating systems.

Unlike Packet Tracer, GNS3 executes actual router and firewall images.

This provides significantly greater realism at the cost of higher hardware requirements.

GNS3 will be introduced in later modules after networking fundamentals have been established.

---

## Installation

Download and install:

- GNS3 GUI
- GNS3 Server

Install all recommended components.

Virtual machine integration can be skipped initially.

No device images are required at this stage.

Configuration will be performed later in the course.

---

# Organizing the Workspace

A consistent directory structure makes future laboratories easier to manage.

The following layout is recommended.

```text
Networking-Course/

│

├── Notes/

├── Labs/

│ ├── Module-00/

│ ├── Module-01/

│ ├── Module-02/

│ └── ...

├── Packet-Captures/

├── Configurations/

├── Diagrams/

├── Projects/

└── Resources/
```

Each laboratory should be saved inside its corresponding module folder.

Configuration backups should be stored separately from Packet Tracer projects.

---

# Command-Line Verification

Before beginning future modules, verify that the operating system can communicate with external networks.

The following commands are platform-independent concepts, although syntax differs slightly between operating systems.

---

## Ping

```bash
ping 8.8.8.8
```

Purpose:

- Tests basic IP connectivity.
- Measures round-trip time.
- Verifies that packets reach the destination.

Expected result:

Replies should be received.

---

## Traceroute

Windows:

```cmd
tracert 8.8.8.8
```

Linux/macOS:

```bash
traceroute 8.8.8.8
```

Purpose:

Displays the routers between the local computer and the destination.

This command will become important during routing and troubleshooting modules.

---

## Display Network Configuration

Windows:

```cmd
ipconfig
```

Linux:

```bash
ip addr
```

Purpose:

Displays:

- IP Address
- Subnet Mask
- Default Gateway
- Interface Status

Students should identify the interface currently connected to the network.

---

## DNS Lookup

Windows:

```cmd
nslookup google.com
```

Linux:

```bash
nslookup google.com
```

or

```bash
dig google.com
```

Purpose:

Tests DNS name resolution.

A successful lookup confirms that the DNS server is functioning correctly.

---

# Verifying Internet Connectivity

Successful execution of the following commands indicates that the workstation is ready for the remainder of the course.

| Command | Expected Result |
|----------|----------------|
| ping 8.8.8.8 | Replies Received |
| traceroute 8.8.8.8 | Route Displayed |
| ipconfig / ip addr | Interface Information Displayed |
| nslookup google.com | IP Addresses Returned |
| Wireshark Capture | Packets Visible |

Any failures should be investigated before continuing.

---

# Common Installation Problems

## Packet Tracer Does Not Open

Possible causes:

- Outdated operating system
- Missing dependencies
- Corrupted installation

Recommended solution:

Reinstall the application using the latest installer.

---

## Wireshark Shows No Interfaces

Possible causes:

- Npcap was not installed.
- Packet capture driver failed.

Recommended solution:

Reinstall Wireshark and ensure Npcap is selected during installation.

---

## Ping Fails

Possible causes:

- No Internet connection.
- Incorrect network configuration.
- Firewall restrictions.

Verify Internet connectivity using a web browser before troubleshooting further.

---

## nslookup Fails

Possible causes:

- DNS server unavailable.
- Incorrect DNS configuration.
- Network outage.

Verify that the system has Internet connectivity before investigating DNS.

---

# Best Practices

The following practices should be maintained throughout the course.

- Save every laboratory exercise.
- Keep configuration backups.
- Name files consistently.
- Create notes after every lesson.
- Verify configurations before making changes.
- Record troubleshooting steps.
- Avoid modifying completed labs once submitted.

Good documentation habits are as valuable as technical skills.

---

# Lesson Summary

This lesson prepared the laboratory environment used throughout the course.

Packet Tracer provides a simulated environment for learning networking fundamentals. Wireshark captures and analyzes real network traffic. GNS3 provides advanced emulation capabilities that will be introduced later in the course.

Basic command-line utilities were used to verify Internet connectivity and confirm that the workstation is correctly configured. A structured directory layout was also established to organize laboratory exercises, packet captures, configurations, and documentation.

The successful completion of this lesson ensures that the environment is fully prepared for the practical exercises that begin in the next module.

---

# Checkpoint

Before proceeding to Module 1, verify that all of the following tasks have been completed.

- [ ] Cisco Packet Tracer installed.
- [ ] Cisco Networking Academy account created.
- [ ] Wireshark installed with Npcap.
- [ ] GNS3 installed.
- [ ] First packet capture completed successfully.
- [ ] `ping 8.8.8.8` executed successfully.
- [ ] `traceroute` or `tracert` executed successfully.
- [ ] `ipconfig` or `ip addr` executed successfully.
- [ ] `nslookup google.com` executed successfully.
- [ ] Course directory structure created.