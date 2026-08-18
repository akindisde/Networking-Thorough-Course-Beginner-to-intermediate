## Module 2 Checkpoint Quiz — Media, Devices, and Layers

### Instructions

Answer all 10 questions.

Focus on the concepts covered in Module 2:

- Physical networking media
    
- Signals and transmission
    
- Hubs
    
- Switches
    
- Routers
    
- Access points
    
- Firewalls
    
- Network layers
    
- MAC addresses
    
- IP addresses
    
- Same-subnet vs different-subnet communication
    
- Home gateway devices
    

Try to answer each question before checking the answer key.

### Question 1

Which networking device operates primarily at Layer 1 and repeats an incoming physical signal out to its other ports?

A. Hub

B. Switch

C. Router

D. Firewall

### Question 2

Which physical medium uses pulses of light to carry network data?

A. Twisted-pair copper

B. Fiber optic

C. Coaxial cable

D. Wireless radio

### Question 3

Why are the individual conductors in twisted-pair Ethernet cable twisted together in pairs?

A. To reduce interference and crosstalk

B. To increase the cable's IP address range

C. To make every cable operate as fiber optic

D. To eliminate the need for Ethernet frames

### Question 4

Which device primarily uses a CAM/MAC address table to decide which local Ethernet port should receive a frame?

A. Hub

B. Switch

C. Router

D. Access point

### Question 5

A host has 192.168.10.10/24 and wants to communicate with 192.168.20.20. Which device is required to move the traffic between these different IP networks?

A. Hub

B. Switch

C. Router

D. Access point

### Question 6

Which statement best describes the difference between a packet and a frame when traffic crosses a router?

A. The frame is rebuilt for the next link while the IP packet continues toward its destination

B. The frame always remains unchanged from source to destination

C. The packet exists only inside the local Ethernet frame

D. The router forwards the original Ethernet frame unchanged across every network

### Question 7

What is the primary role of an access point in a typical LAN?

A. Connect wireless clients to the Layer 2 network

B. Replace every router in the network

C. Repeat every physical signal to every Ethernet port

D. Assign every Internet host a public IP address

### Question 8

Which statement best describes the primary function of a firewall?

A. Enforce security policy on network traffic

B. Convert electrical signals into light signals

C. Learn Ethernet MAC addresses for every switch port

D. Provide only physical-layer signal repetition

### Question 9

Which combination correctly matches the device with its primary layer or role?

A. Hub — Layer 1

B. Switch — Layer 3 routing

C. Router — Layer 1 signal repetition

D. Access point — Layer 3 routing only

### Question 10

A typical home gateway combines several networking functions in one physical box. Which set correctly represents common functions it may contain?

A. Router, Ethernet switch, access point, firewall, NAT, DHCP

B. Hub, fiber splice, DNS root server, and radio tower

C. Only an Ethernet switch and physical cable tester

D. Only a router and a passive electrical repeater

## Answer Key

### 1. A — Hub

A traditional hub operates at Layer 1 by repeating the received physical signal toward its other ports. It does not make MAC-based forwarding decisions.

### 2. B — Fiber optic

Fiber optic cable carries network data using pulses of light through optical fiber.

### 3. A — To reduce interference and crosstalk

Twisting the copper conductors helps reduce electromagnetic interference and limits crosstalk between pairs.

### 4. B — Switch

A Layer 2 switch learns MAC addresses and associates them with switch ports in its MAC/CAM table.

### 5. C — Router

192.168.10.10/24 and 192.168.20.20/24 belong to different IP networks. Communication between those networks requires Layer 3 routing.

### 6. A — The frame is rebuilt for the next link while the IP packet continues toward its destination

Layer 2 framing is local to each link. A router can remove the incoming frame and create new Layer 2 framing for the next link while forwarding the IP packet.

### 7. A — Connect wireless clients to the Layer 2 network

An access point provides Wi-Fi connectivity and bridges wireless clients into the local Layer 2 network.

### 8. A — Enforce security policy on network traffic

A firewall evaluates traffic against security policy and can allow, deny, or otherwise control traffic crossing a security boundary.

### 9. A — Hub — Layer 1

A traditional hub operates at the physical layer by repeating signals.

### 10. A — Router, Ethernet switch, access point, firewall, NAT, DHCP

A typical home gateway can combine routing, Ethernet switching, wireless access, firewalling, NAT, and DHCP into one physical appliance.

## Scoring

Use the following scale as a checkpoint, not as a final judgment of your networking ability.

### 10/10 — Excellent

You have a strong grasp of the Module 2 concepts. You should be ready to move forward while continuing to reinforce the device and layer relationships.

### 8–9/10 — Good

Your understanding is solid. Review the questions you missed before continuing, especially the distinction between Layer 1, Layer 2, and Layer 3 functions.

### 6–7/10 — Needs Review

You have the basic ideas but should revisit the relevant sections of Module 2 before moving on.

Pay particular attention to:

- Physical media
    
- Hub vs switch
    
- MAC vs IP addressing
    
- Same-subnet vs different-subnet communication
    
- Router vs firewall
    
- Access point function
    

### 0–5/10 — Review Module 2

Review the module before continuing. The device and layer relationships introduced here are foundational for the rest of the course.

## Core Concepts to Remember

```
Hub
→ Layer 1
→ Repeats physical signals
→ No MAC-based forwarding

Switch
→ Layer 2
→ Uses MAC addresses
→ Learns MAC-to-port mappings
→ Forwards Ethernet frames

Access Point
→ Primarily Layer 2
→ Provides wireless network access
→ Bridges Wi-Fi clients into the LAN

Router
→ Layer 3
→ Uses IP addresses
→ Connects different IP networks
→ Uses a routing table

Firewall
→ Security function
→ Inspects and controls traffic
→ Enforces security policy
```

The most important routing rule is:

```
Same subnet
→ Local Layer 2 delivery
→ Switch / Access Point

Different subnet
→ Layer 3 forwarding
→ Router
```

A typical home gateway can combine multiple functions:

```
Router
+
Ethernet switch
+
Wireless access point
+
Firewall
+
NAT
+
DHCP
```