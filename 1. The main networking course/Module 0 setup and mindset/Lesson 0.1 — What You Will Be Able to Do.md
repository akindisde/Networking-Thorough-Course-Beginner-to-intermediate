# Module 0 — Setup and Mindset

## Lesson 0.1 — What You Will Be Able to Do
### Introduction

Computer networking is the foundation of modern digital communication. Every website, cloud service, mobile application, video conference, online game, and enterprise application depends on networks that transport data reliably between devices. Despite this importance, networking is often introduced by immediately discussing cables, IP addresses, or configuration commands without first explaining the larger picture.

This course takes a different approach. Before studying individual technologies, <u>it is important to understand the overall objective and the practical skills that will be developed</u>. Networking is not the process of memorizing commands or protocol names. <mark style="background:#d4b106">It is the discipline of designing, operating, securing, and troubleshooting systems that allow devices to communicate efficiently and reliably.</mark>

The purpose of this lesson is to establish the *scope* of the course, introduce the tools that will be used throughout the learning process, explain the teaching methodology, and define the expectations required to successfully complete the course.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Scope|Terminology]]

### Course Objectives

Upon completing this course, students should be able to:

- Explain how communication occurs between devices on local and wide-area networks.
- Understand the purpose and operation of networking protocols across all layers of the [[Transmission Control Protocol-Internet Protocol (TCP-IP) model]] model.
- Design and configure small to medium-sized *enterprise networks*. (of course learning by doing is the best way of learning)

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Enterprise network|Definitions]]

- Configure Cisco routers and switches using the command-line interface (CLI).
- Analyze network traffic using [[Wireshark]].
- *Troubleshoot* connectivity, routing, switching, and application-layer issues using structured methodologies.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Troubleshoot|Terminology]]

- Apply fundamental security practices to enterprise networks.
- Design *network topologies* based on technical and business requirements.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Network topology|Definitions]]

- Build a strong foundation for certifications such as CCNA and Fortinet NSE4.
    
The *emphasis* of the course is not on passing certification exams alone, but on developing practical engineering skills that can be applied in production environments.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Emphasis]]

### The Final Outcome

The topics covered throughout this course gradually build toward the ability to design and manage a complete enterprise network.

A typical enterprise network includes:

- Internet connectivity
- *[[Firewall]]s*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Firewall|Definitions]]

- Core and access *[[Switch]]es*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Switch|Definitions]]

- *[[Router]]s*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Router|Definitions]]

- Wireless infrastructure
- *Servers*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Server]]

- VLAN segmentation
- Dynamic routing
- Security policies
- Remote *VPN* connectivity

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Virtual private network (VPN)|Definitions]]

- Network monitoring

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Monitor|Terminology]]

A simplified enterprise topology appears as follows:

```text
                    Internet
                        │
                  ISP Router
                        │
                 Enterprise Firewall
                        │
                  Core Switch Stack
          ┌─────────────┼──────────┐
          │                        │                   │
     Access Switch  Access Switch        Server Switch
          │                        │                   │
      User PCs        IP Phones              Servers
          │
     Wireless Access Point
          │
      Mobile Devices
```

By the end of the course, every component of this topology will be understood individually before being integrated into a complete network design.

### The Role of a *Network Engineer*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Network Engineer|Definitions]]

A network engineer is responsible for designing, *deploying*, maintaining, securing, and troubleshooting communication networks.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Deploy|Terminology]]

The role extends far beyond connecting cables or installing hardware. In modern organizations, network engineers ensure that business services remain available, secure, and *performant*.

<u>Typical</u> responsibilities include:

- Deploying routers and switches.
- Configuring VLANs and IP addressing.
- Managing routing protocols.
- Maintaining firewall policies.
- Troubleshooting connectivity issues.
- Monitoring network performance.
- Supporting wireless infrastructure.
- Maintaining VPN connectivity.
- Implementing *redundancy* and high availability.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Redundancy]]

- Documenting network changes.

![[Cybersecurity journey/1. Networking/Q&A#❔ - why would a network engineer document the network changes ?|Q&A]]

Most daily work involves diagnosing problems, interpreting logs, analyzing packet captures, and making configuration changes rather than installing physical equipment.

### Skills Developed Throughout the Course

The course progresses from basic concepts to practical enterprise networking.
Students will learn how to :

- Build complete network topologies.
- Configure Cisco IOS devices.
- Read and interpret routing tables.
- Design subnetting schemes.
- Configure VLANs and trunk links.
- Deploy static and dynamic routing.
- Configure NAT and DHCP.
- Analyze TCP and UDP communications.
- Understand common application protocols.
- Secure Layer 2 and Layer 3 networks.
- Troubleshoot complex network failures.
- Produce professional network documentation.

Each module builds upon previous knowledge, gradually increasing in complexity.

### Teaching Methodology

Every lesson follows a consistent instructional structure designed to <u>connect theory with practical implementation</u>.

#### 1. The Problem

Every networking technology exists because it solves a specific problem, as probably similar to a lot of tech out there. Examples include:

| Problem                               | Technology    |
| ------------------------------------- | ------------- |
| Devices need unique identification    | IP Addressing |
| Multiple computers share one network  | Ethernet      |
| Different networks need communication | Routing       |
| Automatic address assignment          | DHCP          |
| Human-readable names                  | DNS           |
| Secure remote administration          | SSH           |

Understanding the problem provides context for the solution and reduces unnecessary memorization.

#### 2. The Concept

Once the problem has been established, the lesson explains the technology itself. This includes:

- Definitions
- *Protocol* operation

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Protocol|Definitions]]

- *Packet flow*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Packet flow|Definitions]]

- Design principles
- Advantages
- Limitations
- Common misconceptions

Concepts are introduced in plain language before progressing to protocol-level details.

#### 3. Practical Demonstration

Every theoretical concept is reinforced through practical implementation. Depending on the lesson, demonstrations may include:

- Cisco IOS configuration    
- *[[Cisco Packet Tracer]]* simulations

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Cisco Packet tracer|Definitions]]

- Linux networking commands
- Routing table analysis
- Switch configuration
- Firewall configuration

*Configuration* is treated as verification of understanding rather than memorization of commands.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Configuration|Terminology]]

#### 4. Packet Analysis

Networking cannot be fully understood without observing real traffic. Every major protocol introduced during the course will also be analyzed using Wireshark.

Packet captures provide visibility into:

- Ethernet frames
- ARP requests
- DHCP exchanges
- DNS lookups
- TCP handshakes
- HTTP requests
- Routing protocol messages

Studying packet captures bridges the gap between theoretical protocol descriptions and their actual implementation.

#### 5. Security Perspective

<u>Every networking technology introduces potential security risks</u>. Security is therefore integrated throughout the course rather than isolated into a single module. Examples include:

| Technology | Security Consideration  |
| ---------- | ----------------------- |
| ARP        | [[ARP Spoofing]]        |
| DHCP       | [[Rogue DHCP Servers]]  |
| DNS        | [[DNS Cache Poisoning]] |
| Wi-Fi      | [[Evil Twin Attacks]]   |
| TCP        | [[SYN Floods]]          |
| VLANs      | [[VLAN Hopping]]        |

Understanding both normal operation and potential abuse leads to better network design and troubleshooting.

#### 6. Practical Laboratory

Each lesson concludes with practical exercises. Laboratories are designed to reinforce concepts through direct interaction rather than observation alone. Exercises may involve:

- Building topologies
- Configuring devices
- Capturing packets
- Identifying protocol behavior
- Troubleshooting broken configurations

Hands-on experience is an essential component of networking education.

### Software Used Throughout the Course

<u>Three primary</u> tools are used consistently throughout the course.
#### Cisco Packet Tracer

Cisco Packet Tracer provides a *simulated* networking environment suitable for learning routing, switching, and basic enterprise networking concepts.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Simulator|Definitions]]

![[Cybersecurity journey/1. Networking/Q&A#❔ - what's the difference between a simulator and a emulator ?|Q&A]]

It allows students to construct complete network topologies without requiring physical networking equipment. Topics practiced in Cisco Packet Tracer include:

- Ethernet switching
- VLANs
- Routing
- DHCP
- NAT
- Wireless networking
- Access Control Lists
- Basic security

#### Wireshark

Wireshark is an industry-standard protocol analyzer used to inspect network traffic at the packet level.

Unlike network simulators, Wireshark captures real packets transmitted across a network interface.

It is used to:

- Observe protocol exchanges.
- Analyze packet headers.
- Verify protocol behavior.
- Troubleshoot communication failures.
- Understand encapsulation.

Packet analysis is a recurring activity throughout the course.

#### Command-Line Interface (CLI)

Professional network devices are primarily managed through command-line interfaces. <mark style="background:#d3f8b6">Although graphical management platforms exist, CLI access remains the standard for deployment, troubleshooting, automation, and advanced configuration.</mark>

Students will become familiar with:

- Cisco IOS
- Linux networking commands
- Basic FortiGate CLI
- Diagnostic utilities

Developing confidence with CLI operation is a fundamental objective of the course.

### Practical Expectations

<u>Networking is a practical discipline</u>, that'll be clearer the more you learn more and more. Reading documentation alone is insufficient for developing professional competence. So, students are expected to:

- Complete every laboratory exercise.
- Reproduce demonstrations independently.
- Take structured notes.
- Troubleshoot failed configurations.
- Verify protocol behavior using packet captures.

Repeated practice is significantly more valuable than passive observation.

### Daily Practice

Beginning with the *subnetting* module, students are expected to dedicate approximately thirty minutes each day to subnetting exercises.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Subnetting|Definitions]]

Subnetting is a foundational skill that affects routing, VLAN design, firewall configuration, VPN deployment, and enterprise network planning. We don't need to stress this matter any more, you <u>MUST</u> master this concept, when we learn it.

Consistent practice develops speed and accuracy that cannot be achieved through occasional study sessions, so be consistent even if you don't have long studying sessions.
### Course Progression

The course follows a structured progression in which each module builds upon the previous one.

```text
Setup and Mindset
        │
Internet Fundamentals
        │
Physical Networking
        │
Ethernet and Switching
        │
IP Addressing
        │
Routing
        │
TCP and UDP
        │
Application Protocols
        │
Network Security
        │
Wireless Networking
        │
Wide Area Networks
        │
Network Design
        │
Troubleshooting
        │
Capstone Projects
```

Each stage introduces new concepts while reinforcing previously learned material through practical exercises.

### Professional Mindset

Successful network engineers approach problems systematically rather than relying on memorized commands. Several principles guide professional practice:

- Verify assumptions before making changes.
- Document configurations and modifications.
- Test every change before deployment.
- Understand protocol behavior before troubleshooting.
- Prefer structured analysis over trial-and-error.

Technical proficiency develops through understanding, observation, and repetition rather than memorization.

### Lesson Summary

This lesson established the scope, objectives, and methodology of the course.

The remaining modules progressively develop the knowledge and practical skills required to understand modern computer networks, configure enterprise networking devices, analyze network traffic, secure communication systems, and troubleshoot real-world connectivity issues.

The next lesson focuses on preparing the laboratory environment by installing the software used throughout the course, verifying network connectivity, and organizing the workspace for subsequent practical exercises.

