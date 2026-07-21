## Lesson 1.2 — The Protocol Stack and Encapsulation (Part 1)

### Introduction

Modern computer networks consist of billions of interconnected devices manufactured by thousands of different vendors. These devices vary in hardware architecture, operating systems, programming languages, and communication technologies. <u>Despite these differences</u>, they are capable of exchanging data reliably because <u>they follow a common set of communication rules known as networking protocols</u>.

A single protocol, however, cannot solve every communication problem. Delivering data across a network involves numerous independent tasks, including identifying applications, ensuring reliable delivery, selecting network paths, transmitting data over physical media, and detecting transmission errors. Combining all of these responsibilities into one protocol would create an extremely complex and inflexible system.

To address this challenge, computer networking uses a layered architecture known as the *protocol stack*. Each layer performs a specific set of functions while <u>relying</u> on the services provided by the layer below it. This modular approach allows technologies to <mark style="background:#fff88f">evolve independently without affecting the entire networking system</mark>.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Protocol stack|Definitions]]

Understanding protocol layering is one of the most important concepts in networking. Every protocol discussed throughout this course belongs to one of these layers, and nearly every troubleshooting process follows the same layered approach.

### Learning Objectives

After completing this lesson, students should be able to :

- Explain why networking protocols are organized into layers.
- Define the concepts of separation of concerns, modularity, and interoperability.
- Describe the Internet five-layer protocol model.
- Identify the responsibility of each layer.
- Explain how multiple protocols cooperate during communication.
- Understand why different layers remain independent of one another.

### Why Networking Needs Protocols

Imagine two people attempting to communicate without speaking the same language. One speaks Arabic, the other speaks Japanese.

Even if both individuals are intelligent, communication becomes impossible without agreeing on a <u>common language</u> and communication rules. Computers face the same challenge.

Different *manufacturers* produce devices with different processors, operating systems, programming languages, and hardware architectures. A Windows computer must communicate with a Linux server, which in turn communicates with an Android phone through Cisco routers, Juniper switches, and cloud infrastructure distributed across multiple countries.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Manufacturer|Terminology]]

Without standardized communication rules, these systems could never exchange information. Networking protocols provide these rules.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What does it mean to standardize communication ?|Q&A]]

This is crucial to keep in mind your whole journey of learning networking, protocol defines :
- The *format* of transmitted data

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Data format|Definitions]]

- The order in which messages are exchanged
- Error handling procedures
- Timing requirements
- Expected responses
- Connection establishment procedures
- Connection termination procedures

Every protocol serves a <u>specific purpose</u> within the communication process.

### Why One Protocol Is Not Enough

Suppose a user downloads a webpage. Several independent problems must be solved before the webpage appears, some examples include :
- How does the browser ==request== the webpage?
- How does the server know ==which application should receive the request==?
- How is ==reliable delivery== guaranteed?
- How is the ==destination== identified?
- How is the ==packet forwarded== through multiple routers?
- How are ==bits transmitted== across a cable?
- How are transmission ==errors detected==?

Attempting to solve all of these problems with a single protocol would result in enormous complexity. Instead, networking divides communication into smaller, specialized responsibilities.

Each protocol performs one task well and cooperates with the others. This principle forms the basis of protocol layering.

### The Concept of Layering

Layering divides a complex system into multiple <u>independent components</u>. Each layer focuses on a single responsibility and communicates only with adjacent layers. A simplified representation appears below :

```text
+---------------------------+
|        Application        |
+---------------------------+
|         Transport         |
+---------------------------+
|          Network          |
+---------------------------+
|            Link           |
+---------------------------+
|         Physical          |
+---------------------------+
```

> Please note that this is a way of formalizing the concept of networking, which means when we talk about layering models that are numerous, not this one above, we're trying to give a theoretical structure to networks and how they operate so we can understand them better, separation of tasks and events and many other things into separate layers are required so you know exactly what "zone" are you talking about in networking, and this way of looking at this topic is beneficial among students and professionals, structured communication is always better.

The application layer (its protocols and responsibilities) does not concern itself with electrical signals (the responsibility of the physical layer). The physical layer does not understand webpages.

Each layer performs only the tasks assigned to it.

### Separation of Concerns

One of the primary reasons for layering is **separation of concerns**.

Separation of concerns means that each layer focuses exclusively on a well-defined responsibility without needing to understand the internal operation of other layers.

Consider a postal delivery service. Writing a letter involves several independent activities :
- Writing the message
- Placing it inside an envelope
- Printing the address
- Sorting the mail
- Transporting it
- Delivering it

The delivery driver does not need to understand the contents of the letter. Likewise, the person writing the letter does not need to understand how trucks are routed across the country.

Each participant performs a specialized role. Computer networking follows the same design philosophy.

### Separation of Responsibilities

The responsibilities of the networking layers can be summarized as follows.

| Layer       | Primary Responsibility                                |
| ----------- | ----------------------------------------------------- |
| Application | The applications data entry point to a network        |
| Transport   | Reliable communication of that data                   |
| Network     | Logical addressing and routing of that data           |
| Link        | Local network communication                           |
| Physical    | Transmission of electrical, optical, or radio signals |

Notice that each responsibility is distinct. No layer attempts to perform another layer's work.

### Advantages of Separation of Concerns

Separating responsibilities provides several important benefits :

#### Simplicity

Each protocol becomes significantly easier to design, implement, and troubleshoot.

Instead of solving every networking problem simultaneously, developers concentrate on one specific responsibility.

#### Easier Maintenance

Suppose a new wireless technology replaces Wi-Fi.
The browser does not need modification.
TCP does not require redesign.
DNS continues functioning exactly as before.
Only the physical and link layers require updates.

This independence greatly simplifies technological evolution.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What the difference between troubleshooting and maintenance ?|Q&A]]

#### Easier Troubleshooting

Professional network engineers isolate problems by layer. Examples include :

Application problem :
- Incorrect HTTP response.

Transport problem :
- TCP connection failure.

Network problem :
- Missing route.

Link problem :
- VLAN mismatch.

Physical problem :
- Damaged cable.

Rather than investigating every protocol simultaneously, engineers narrow the problem to a specific layer. This methodology significantly reduces troubleshooting time.

#### *Modularity*

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Modularity|Terminology]]

A modular system consists of independent components that can be modified or replaced without redesigning the entire system. Networking protocols behave like interchangeable building blocks.

For example :

```
HTTP
↓
HTTPS (we added encryption on top of http, and http is still independent)
```

The application protocol changes.
TCP continues operating normally.
IP continues routing packets.
Ethernet continues forwarding frames.
Nothing else requires modification. Similarly :

```
Ethernet
↓
Wi-Fi
```

The physical transmission medium changes.
Applications continue functioning without modification.
Users continue browsing websites exactly as before.
The modular architecture isolates changes to the appropriate layer.

##### Real-World Example of Modularity

Consider three users :

User A : Desktop computer connected using Ethernet.
User B : Laptop connected using Wi-Fi.
User C : Smartphone connected using 5G.

All three users access :

```
https://www.openai.com
```

Although the lower layers (responsible for how bits are transmitted) differ significantly :
- Ethernet
- Wi-Fi
- Cellular

![[Cybersecurity journey/1. Networking/Q&A#❔ - What's the difference between Ethernet, Wi-Fi and cellular ?|Q&A]]

the application layer remains identical.
The browser issues the same HTTPS request.
The server receives the same HTTP request.
The webpage appears identically on all devices.

This interoperability is possible because the protocol stack separates communication into independent layers.

#### *Interoperability*

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Interoperability|Terminology]]

Perhaps the greatest achievement of protocol layering is interoperability. Interoperability refers to the ability of systems developed by different vendors to communicate correctly.

Consider the following network :

```text
Windows PC
      │
Cisco Switch
      │
Juniper Router
      │
ISP Network
      │
Linux Web Server
```

Each device originates from a different manufacturer.
Each runs different software.
Each uses different hardware.

Despite these differences, communication succeeds because every device follows the same protocol standards.

![[Cybersecurity journey/1. Networking/Q&A#❔ - Who defines the protocol standards that every manufacturer follows ?|Q&A]]

The router does not need to know whether the packet originated from Windows, Linux, macOS, or Android. It simply examines the IP header and forwards the packet according to standardized networking rules.

### Standards Organizations

Protocol interoperability depends on internationally recognized standards. Several organizations define and maintain these standards :

| Organization | Responsibility                                     |
| ------------ | -------------------------------------------------- |
| IETF         | Internet protocols such as IP, TCP, UDP, DNS, HTTP |
| IEEE         | Ethernet, Wi-Fi, and other networking standards    |
| ISO          | OSI Reference Model                                |
| ICANN        | Domain name system coordination                    |
| W3C          | Web technologies including HTML and CSS            |

Manufacturers implement these standards within their products. As long as devices conform to the standards, they can communicate regardless of vendor.

![[Cybersecurity journey/1. Networking/Q&A#❔ - Who are the manufacturers of networking devices ?|Q&A]]

### The Need for a Protocol Stack

Combining every networking responsibility into one protocol would create software that is :
- Extremely difficult to develop.
- Nearly impossible to maintain.
- Challenging to troubleshoot.
- Difficult to extend.
- Vendor dependent.

Instead, the networking industry adopted a layered architecture where :

- Each layer solves one category of problems.
- Layers cooperate through clearly defined interfaces.
- Individual protocols can evolve independently.
- Hardware and software from different vendors remain compatible.

This layered architecture is known as the **protocol stack**.

The remainder of this lesson examines the structure of the Internet protocol stack and the responsibilities assigned to each layer.

Here is a diagram to best visualize what we learned this part of the chapter : 

![[Pasted image 20260720184038.png]]

### Key Takeaways

- Networking protocols define the rules governing communication between devices.
- A single protocol cannot efficiently solve every networking problem.
- Protocol layering divides communication into specialized responsibilities.
- Separation of concerns allows each layer to focus on a single task.
- Modularity enables technologies within one layer to evolve without affecting others.
- Interoperability allows devices from different manufacturers to communicate using common standards.
- Layered architectures simplify implementation, maintenance, troubleshooting, and future development.
- Modern Internet communication depends on standardized protocol stacks rather than proprietary communication systems.

> We will go through the protocol stack in the labs as well, if you still have some hardship understanding it fully, it'll go away.

### Preview

Part 2 introduces the **Internet Five-Layer Model**, the protocol architecture used throughout this course. Each layer will be examined individually, including its responsibilities, common protocols, devices, and relationship with adjacent layers. The lesson then introduces the **OSI Seven-Layer Model**, explaining why it remains an essential reference model in networking despite the Internet protocol stack being used in practice.

## Lesson 1.2 — The Protocol Stack and Encapsulation (Part 2)

### The Internet Protocol Stack

Throughout this course, networking concepts will be organized using the Internet protocol stack, also known as the [[Transmission Control Protocol-Internet Protocol (TCP-IP) model]] or the Internet five-layer model.

This model divides network communication into five <u>logical</u> layers. Each layer performs a specific function while providing services to the layer above it and relying on the layer below it.

```text
+-----------------------------+
|       Application           |
+-----------------------------+
|        Transport            |
+-----------------------------+
|         Network             |
+-----------------------------+
|           Link              |
+-----------------------------+
|         Physical            |
+-----------------------------+
```

When an application sends data across a network, the information moves downward through these layers on the sender's device. Each layer adds information required to perform its own task.

When the data reaches the destination, the process is reversed. Each layer removes the information added by its counterpart until the original application data reaches the receiving application.

This process is known as **encapsulation** and **decapsulation**, which will be examined later in this lesson.

### Layer 5 — Application

The Application layer is the closest layer to the user. It provides the interface between user applications and the underlying network.

Contrary to what the name suggests, this layer does not contain every application installed on a computer. Instead, it contains the networking services used by applications. it's named application layer because it provides service to application to get into the network.

Examples include:
- HTTP
- HTTPS
- DNS
- SMTP
- IMAP
- POP3
- FTP
- SSH
- DHCP
- NTP

When a browser requests a webpage, it is communicating through HTTP or HTTPS.

When a computer resolves a domain name, it uses DNS.
When an email client sends a message, it typically uses SMTP.

Each application protocol solves a different communication problem while remaining completely independent of the lower networking layers.

The browser does not need to understand Ethernet.
The DNS client does not need to understand electrical signals.
Applications simply generate data and pass it to the Transport layer.

In simple terms, even if the layer above doesn't do its work properly, the layer below still can do its job.
### Responsibilities of the Application Layer

The Application layer is responsible for :
- Providing network services to user applications.
- Defining application-specific message formats.
- Managing application-level communication.
- Supporting authentication and user interaction.
- Exchanging application data.

It is important to understand that this layer does **not** guarantee reliable delivery.
It does **not** determine network paths. 
It does **not** perform routing.

Those responsibilities belong to lower layers.

#### Real Example

Suppose a browser requests :

```
https://example.com/index.html
```

The browser creates an HTTP request.

```http
GET /index.html HTTP/1.1
Host: example.com
```

At this stage, the browser knows nothing about :
- MAC addresses
- IP addresses
- Routers
- Switches
- Ethernet frames

Its only responsibility is producing the HTTP request. The request is then handed to the Transport layer as irrelevant data to its job; which is to prepare that data to be transferred.

### Layer 4 — Transport

The Transport layer provides communication between applications running on different devices.

Instead of thinking about computers communicating, it is more accurate to think of **applications** communicating.

A single computer may simultaneously run :
- A web browser
- Spotify
- Discord
- Steam
- An SSH client
- Microsoft Teams

All of these applications share the same network interface; all the data coming in to each of those applications come in from the same entrance.

The Transport layer ensures that incoming data reaches <mark style="background:#fff88f">the correct application.</mark> and that's done with port numbers as hinted before.

The two primary Transport protocols are :
- [[Transmission Control Protocol (TCP)]]
- *[[User Datagram Protocol (UDP)]]*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - User Datagram Protocol (UDP)|Definitions]]

### TCP

Transmission Control Protocol (TCP) provides reliable communication, if we want to summarize it.

Its major features include :
- Connection establishment
- Reliable delivery
- Packet sequencing
- Flow control
- Error recovery
- Congestion control

Applications that require accuracy generally use TCP.

Examples include:
- HTTP
- HTTPS
- SSH
- FTP
- SMTP

Now, you have an idea of what is it like to have an application protocol needing the services of a transport protocol services.

### UDP

User Datagram Protocol (UDP) provides lightweight, non-reliable communication.

Unlike TCP, UDP :
- Does not establish a connection.
- Does not retransmit lost packets.
- Does not guarantee delivery.
- Does not guarantee ordering.

Its primary advantage is <u>speed</u>. Applications using UDP include :
- DNS
- VoIP
- Video streaming
- Online gaming
- DHCP

Later modules compare TCP and UDP in detail.

### Port Numbers

The Transport layer introduces another important concept : **ports**.
An IP address identifies a host.
A port identifies an application running on that host.

Examples :

| Service | Port |
| ------- | ---: |
| HTTP    |   80 |
| HTTPS   |  443 |
| SSH     |   22 |
| DNS     |   53 |
| SMTP    |   25 |

When a browser connects to a web server, it usually communicates with destination port 443.

The Transport layer combines:

- Source IP
- Destination IP
- Source Port
- Destination Port

This uniquely identifies each communication session.

### Layer 3 — Network

The Network layer is responsible for moving packets between different networks.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What's the difference between when transport layer provide moving data service and when network layer provides it ?|Q&A]]

Unlike the Link layer, which only communicates across a single local network, the Network layer enables global communication across the Internet; Meaning, the data link layer services are <u>programmed and built</u> to handle only local network communication, and the network layer services are <u>programmed and built</u> to handle more than that.

The primary protocol operating at this layer is [[Internet protocol (IP)]].
Its responsibilities include :
- Logical addressing
- Routing
- Packet forwarding
- Network identification
- *Fragmentation* (IPv4)

![[Cybersecurity journey/1. Networking/Definitions#🧠 - IP Fragmentation|Definitions]]

![[Cybersecurity journey/1. Networking/Q&A#❔ - What's the difference between segmentation and fragmentation in networking ?|Q&A]]

Every packet contains :
- Source IP address
- Destination IP address

Unlike MAC addresses, IP addresses remain largely unchanged from the sender to the receiver.

Routers make forwarding decisions based entirely on these addresses. For example :

```
Source
192.168.1.10
```

```
Destination
142.250.190.78
```

Every router examines the destination IP address and decides where the packet should travel next.

The Network layer is therefore responsible for delivering packets across multiple interconnected networks.

### Layer Relationships

At this point, three layers are involved in communication :

```text
Application
      │
Creates HTTP Request
      │
Transport
      │
Adds TCP Information
      │
Network
      │
Adds IP Information
      │
Lower Layers
```

Notice that each layer performs exactly one category of work :
The browser still has no knowledge of IP routing.
The IP protocol has no understanding of webpages.
The Transport layer has no knowledge of HTML.

Each protocol remains focused on its own responsibility, illustrating the principle of separation of concerns introduced earlier.

### Key Takeaways

- The Internet protocol stack consists of five layers (in one common model not all models) 
- The Application layer provides services for user applications.
- The Transport layer enables communication between applications using TCP or UDP.
- Port numbers identify specific services on a host.
- The Network layer provides logical addressing and routing using IP.
- Each layer provides services to the layer above while relying on services from the layer below.
- No layer performs the responsibilities of another, allowing the protocol stack to remain modular and extensible.

### Preview

The next part introduces the remaining two layers of the Internet protocol stack—the Link layer and the Physical layer—before comparing the Internet model with the OSI seven-layer reference model used throughout the networking industry.

## Lesson 1.2 — The Protocol Stack and Encapsulation (Part 3)

### Layer 2 — Link

The Link layer, sometimes called the **Network Access Layer** or **Data Link Layer**, is responsible for communication between devices connected to the same physical network.

Unlike the Network layer, which delivers packets across multiple interconnected networks, the Link layer operates only within a single local network.

For example, when a computer sends data to its default gateway, the communication occurs entirely at the Link layer.

Common technologies operating at this layer include:

- Ethernet (IEEE 802.3)
- Wi-Fi (IEEE 802.11)
- PPP
- VLANs (IEEE 802.1Q)

The Link layer is responsible for delivering data from one network interface to another within the same broadcast domain.

### Responsibilities of the Link Layer

The Link layer performs several important functions.

These include:

- Physical addressing using MAC addresses.
- Frame construction.
- Frame forwarding.
- Error detection.
- Access to the transmission medium.
- Local delivery.

Unlike IP addresses, which identify devices across the Internet, MAC addresses identify interfaces within a local network.

Every Ethernet frame contains two MAC addresses:

- Source MAC Address
- Destination MAC Address

These addresses change every time a packet passes through a router.

### MAC Addresses

A MAC (Media Access Control) address is a unique hardware identifier assigned to a network interface.

An example MAC address is:

```
A8:5E:45:3B:91:7C
```

Unlike IP addresses, MAC addresses are not used for routing across the Internet.

Their purpose is local delivery.

Consider the following network.

```text
                 Switch

        ┌────────┼────────┐
        │        │        │
       PC      Laptop   Printer
```

When the PC sends a frame to the printer, the switch examines only the destination MAC address.

It does not examine:

- URLs
- HTTP requests
- TCP ports
- IP routing information

Its responsibility is limited to forwarding frames within the local network.

### Ethernet Frames

The Link layer packages Network layer packets into **frames**.

A simplified Ethernet frame appears below.

```text
+--------------------------------------+
| Destination MAC Address              |
+--------------------------------------+
| Source MAC Address                   |
+--------------------------------------+
| EtherType                            |
+--------------------------------------+
| Payload (IP Packet)                  |
+--------------------------------------+
| Frame Check Sequence (FCS)           |
+--------------------------------------+
```

Each field has a specific purpose.

The **Destination MAC Address** tells the switch where the frame should be delivered.

The **Source MAC Address** identifies the sender.

The **EtherType** identifies the protocol contained within the payload.

Examples include:

| EtherType | Protocol |
|-----------|----------|
| 0x0800 | IPv4 |
| 0x86DD | IPv6 |
| 0x0806 | ARP |

Finally, the **Frame Check Sequence (FCS)** allows the receiving device to detect transmission errors.

### Error Detection

Noise, interference, damaged cables, and hardware faults may corrupt transmitted data.

Ethernet uses a **Cyclic Redundancy Check (CRC)** to detect these errors.

The sender calculates a mathematical value over the transmitted frame and stores it in the Frame Check Sequence field.

The receiver performs the same calculation.

If the calculated value differs from the received value, the frame has been corrupted.

The corrupted frame is discarded.

Ethernet detects transmission errors but does not recover from them.

Reliable recovery is performed later by TCP.

### Layer 1 — Physical

The Physical layer is the lowest layer of the protocol stack.

It has one primary responsibility:

Transmit bits across a physical medium.

Unlike every other layer, the Physical layer does not understand:

- Packets
- Frames
- Addresses
- Ports
- Protocols

It only understands binary values represented as physical signals.

Depending on the transmission medium, bits may be represented as:

- Electrical voltages
- Light pulses
- Radio waves

The Physical layer converts digital information into signals suitable for transmission.

### Physical Media

Different networks use different transmission media.

Common examples include:

| Medium | Signal Type |
|---------|-------------|
| Twisted Pair Copper | Electrical |
| Fiber Optic | Light |
| Wi-Fi | Radio |
| Cellular | Radio |

Regardless of the medium, the higher layers remain unchanged.

A browser sending an HTTP request behaves identically whether the underlying medium is copper, fiber, or wireless.

This is another example of protocol layering.

### Devices Operating at Each Layer

Different networking devices primarily operate at different layers of the protocol stack.

| Device | Primary Layer |
|----------|---------------|
| Hub | Physical |
| Repeater | Physical |
| Network Interface Card (NIC) | Link |
| Switch | Link |
| Wireless Access Point | Link |
| Router | Network |
| Firewall | Network / Transport / Application |
| Load Balancer | Transport / Application |

Although modern devices often operate across multiple layers, this classification provides a useful conceptual model.

### Complete Internet Five-Layer Model

The complete protocol stack now appears as follows.

```text
+------------------------------------------------+
| Application                                    |
| HTTP, HTTPS, DNS, SMTP, SSH                    |
+------------------------------------------------+
| Transport                                      |
| TCP, UDP                                       |
+------------------------------------------------+
| Network                                        |
| IPv4, IPv6, ICMP                               |
+------------------------------------------------+
| Link                                           |
| Ethernet, Wi-Fi, ARP                           |
+------------------------------------------------+
| Physical                                       |
| Copper, Fiber, Radio                           |
+------------------------------------------------+
```

Every protocol encountered throughout this course belongs somewhere within this model.

Understanding where a protocol operates greatly simplifies learning its responsibilities.

### Why Another Model Exists

Students often encounter two different networking models:

- Internet Five-Layer Model
- OSI Seven-Layer Model

This frequently causes confusion.

The Internet model describes the protocol stack actually used by modern computer networks.

The OSI model is a conceptual reference model created to standardize discussions about networking systems.

The Internet itself does not use the OSI protocol suite.

Instead, it uses the TCP/IP protocol suite.

Nevertheless, networking professionals frequently refer to OSI layer numbers during troubleshooting.

Examples include:

> "This is a Layer 2 issue."

> "The firewall is blocking traffic at Layer 4."

> "That device operates at Layer 3."

Understanding these references requires familiarity with the OSI model.

### The OSI Seven-Layer Model

The Open Systems Interconnection (OSI) model divides communication into seven layers instead of five.

```text
+---------------------------+
| 7. Application            |
+---------------------------+
| 6. Presentation           |
+---------------------------+
| 5. Session                |
+---------------------------+
| 4. Transport              |
+---------------------------+
| 3. Network                |
+---------------------------+
| 2. Data Link              |
+---------------------------+
| 1. Physical               |
+---------------------------+
```

Although very few real-world protocols map perfectly to these layers, the model provides a common vocabulary used throughout the networking industry.

### Comparing the Models

The Internet model combines several OSI layers.

The relationship is shown below.

| Internet Model | OSI Model |
|----------------|-----------|
| Application | Application, Presentation, Session |
| Transport | Transport |
| Network | Network |
| Link | Data Link |
| Physical | Physical |

The largest difference is the Application layer.

The Internet model combines the top three OSI layers into a single Application layer.

This simplification more accurately reflects how Internet protocols are actually implemented.

### Why Engineers Still Use the OSI Model

Although networking equipment implements the TCP/IP model, the OSI model remains extremely useful.

Reasons include:

- Standardized terminology.
- Vendor-neutral discussions.
- Structured troubleshooting.
- Educational simplicity.
- Certification objectives.

For example, saying:

> "The problem exists at Layer 3."

immediately tells another engineer that the issue likely involves:

- IP addressing.
- Routing.
- Subnetting.
- Gateways.

Without needing additional explanation.

### Key Takeaways

- The Link layer provides communication within a local network using MAC addresses.
- Ethernet frames encapsulate IP packets.
- The Physical layer transmits binary data as electrical, optical, or radio signals.
- Switches primarily operate at the Link layer, while routers operate at the Network layer.
- The Internet five-layer model reflects modern networking implementations.
- The OSI seven-layer model remains a conceptual framework widely used for education, documentation, and troubleshooting.
- Networking professionals frequently reference OSI layer numbers even though Internet communication uses the TCP/IP protocol suite.

### Preview

The next part introduces one of the most fundamental concepts in networking: **encapsulation**. You will follow a single HTTP request as it moves down the protocol stack, becoming a TCP segment, an IP packet, an Ethernet frame, and finally a stream of bits before reversing the process at the receiving host through **decapsulation**.

## Lesson 1.2 — The Protocol Stack and Encapsulation (Part 4)

### Encapsulation

The layered architecture of modern networks would not function unless each layer could exchange information in a standardized way. This is accomplished through a process known as **encapsulation**.

Encapsulation is the process of wrapping data with protocol-specific information as it moves down the protocol stack. Every layer receives data from the layer above, attaches its own header (and sometimes a trailer), and passes the resulting data unit to the next layer.

Each header contains information required for that specific layer to perform its responsibilities.

For example:

- The Transport layer needs port numbers.
- The Network layer needs IP addresses.
- The Link layer needs MAC addresses.
- The Physical layer converts everything into signals.

Each layer ignores information that is not relevant to its operation.

### An Everyday Analogy

Imagine sending a physical letter.

First, the message is written.

```text
Hello, how are you?
```

The message is placed inside an envelope.

The envelope receives the destination address.

The postal service then places the envelope inside containers for transportation.

Finally, trucks, airplanes, and delivery vehicles transport the package to its destination.

Each stage adds information required for the next stage without modifying the original message.

Computer networks work in the same way.

The original application data remains intact while each networking layer adds the information it requires.

### Protocol Data Units (PDUs)

As data moves through the protocol stack, it is referred to by different names.

These names are called **Protocol Data Units (PDUs)**.

| Layer | PDU |
|--------|-----|
| Application | Data |
| Transport | Segment (TCP) / Datagram (UDP) |
| Network | Packet (IP Datagram) |
| Link | Frame |
| Physical | Bits |

Understanding these terms is important because network engineers use them constantly during troubleshooting and documentation.

For example:

> "The router forwarded the packet."

or

> "The switch dropped the frame."

Each statement refers to a different protocol layer.

### Following a Web Request

Consider the browser requesting:

```http
GET / HTTP/1.1
Host: example.com
```

At the Application layer, this request is simply data.

```text
Application Data

GET / HTTP/1.1
Host: example.com
```

The browser passes this data to TCP.

### Transport Layer Encapsulation

TCP receives the application data.

It adds a TCP header containing information such as:

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- Window Size
- Flags

Conceptually:

```text
+------------------------+
| TCP Header             |
+------------------------+
| Application Data       |
+------------------------+
```

The resulting PDU is called a **TCP Segment**.

Notice that the HTTP request itself has not changed.

TCP simply added information in front of it.

### Network Layer Encapsulation

The TCP segment is passed to the Network layer.

IP adds another header.

The IP header contains information including:

- Source IP Address
- Destination IP Address
- Time To Live (TTL)
- Protocol Number
- Header Checksum (IPv4)

The packet now appears conceptually as:

```text
+------------------------+
| IP Header              |
+------------------------+
| TCP Header             |
+------------------------+
| HTTP Data              |
+------------------------+
```

The resulting PDU is called an **IP Packet**.

### Link Layer Encapsulation

The IP packet is passed to the Link layer.

Ethernet adds another header and a trailer.

```text
+------------------------+
| Ethernet Header        |
+------------------------+
| IP Header              |
+------------------------+
| TCP Header             |
+------------------------+
| HTTP Data              |
+------------------------+
| Ethernet Trailer (FCS) |
+------------------------+
```

The Ethernet header contains:

- Source MAC Address
- Destination MAC Address
- EtherType

The trailer contains the Frame Check Sequence (FCS).

The resulting PDU is called an **Ethernet Frame**.

### Physical Layer Transmission

The frame is passed to the Physical layer.

Unlike the upper layers, the Physical layer does not add protocol headers.

Instead, it converts the frame into a sequence of binary digits.

```
011010100101010011...
```

These bits are represented as:

- Electrical voltages.
- Light pulses.
- Radio waves.

The signals travel across the transmission medium to the next device.

### Complete Encapsulation Process

The complete process can be visualized as follows.

```text
Application Layer

Data

↓

Transport Layer

+--------------------+
| TCP Header         |
+--------------------+
| Data               |
+--------------------+

↓

Network Layer

+--------------------+
| IP Header          |
+--------------------+
| TCP Header         |
+--------------------+
| Data               |
+--------------------+

↓

Link Layer

+--------------------+
| Ethernet Header    |
+--------------------+
| IP Header          |
+--------------------+
| TCP Header         |
+--------------------+
| Data               |
+--------------------+
| FCS                |
+--------------------+

↓

Physical Layer

011001010101001...
```

Each layer wraps the previous layer's output without modifying the original application data.

### Decapsulation

When the frame reaches the destination computer, the process occurs in reverse.

This reverse process is known as **decapsulation**.

Each layer removes only its own information before passing the remaining data upward.

The process appears as:

```text
Bits

↓

Ethernet Frame

↓

IP Packet

↓

TCP Segment

↓

Application Data
```

The destination browser eventually receives exactly the same HTTP request that the sender originally created.

Every header added during transmission has been removed.

### Layer Independence

An important property of encapsulation is that each layer understands only its own header.

For example:

The Ethernet layer processes:

- Source MAC
- Destination MAC
- EtherType

It ignores:

- TCP ports
- HTTP headers
- DNS messages

Likewise, the IP layer processes:

- Source IP
- Destination IP
- TTL

It does not examine:

- HTML
- CSS
- Images
- JavaScript

This strict separation keeps protocols simple and allows each layer to evolve independently.

### What Happens at a Router?

A common misconception is that routers forward Ethernet frames across the Internet.

This is incorrect.

Suppose a packet travels through three routers.

```text
PC

↓

Router A

↓

Router B

↓

Router C

↓

Server
```

Each router performs the following operations:

1. Receives an Ethernet frame.
2. Removes the Ethernet header and trailer.
3. Reads the IP packet.
4. Determines the next hop.
5. Builds a completely new Ethernet frame.
6. Sends the packet toward the next router.

The IP packet continues its journey.

The Ethernet frame exists only on a single network segment.

This distinction becomes essential when studying routing.

### Encapsulation in Wireshark

Wireshark displays protocol headers exactly as they appear during encapsulation.

Expand any captured packet.

A typical packet appears similar to:

```text
Frame

└── Ethernet II

    └── Internet Protocol Version 4

        └── Transmission Control Protocol

            └── Hypertext Transfer Protocol
```

Notice that the packet structure mirrors the protocol stack.

Each protocol is nested inside the protocol below it.

Wireshark simply displays the encapsulated headers in the order they appear within the frame.

### Practical Example

Suppose a browser requests:

```
https://example.com
```

The browser generates an HTTP request.

TCP adds:

- Source Port: 52314
- Destination Port: 443

IP adds:

- Source IP: 192.168.1.10
- Destination IP: 93.184.216.34

Ethernet adds:

- Source MAC: A8:5E:45:3B:91:7C
- Destination MAC: 20:47:47:11:82:5A

Finally, the Physical layer transmits the resulting frame as electrical signals.

At the receiving host, each layer removes its corresponding header until the web server receives the original HTTP request.

### Laboratory Exercise — Identifying the Protocol Stack

#### Objective

Observe encapsulation within a real network packet using Wireshark.

#### Requirements

- Wireshark
- Internet connection
- Modern web browser

#### Procedure

1. Open Wireshark.
2. Select the active network interface.
3. Start packet capture.
4. Visit `https://example.com`.
5. Stop the capture.
6. Locate a TCP packet carrying HTTPS traffic.
7. Expand the protocol tree.

#### Expected Packet Structure

The packet should resemble:

```text
Frame

Ethernet II

Internet Protocol Version 4

Transmission Control Protocol

Transport Layer Security
```

If HTTP traffic is captured instead of HTTPS, the final protocol becomes:

```text
Hypertext Transfer Protocol
```

#### Student Tasks

Identify:

- The Link layer protocol.
- The Network layer protocol.
- The Transport layer protocol.
- The Application layer protocol.
- The corresponding Physical medium used during transmission.

Record:

- Source MAC Address
- Destination MAC Address
- Source IP Address
- Destination IP Address
- Source Port
- Destination Port

Determine which protocol belongs to each layer of the Internet protocol stack.

### Common Misconceptions

**Misconception 1**

> Routers forward Ethernet frames across the Internet.

Incorrect.

Routers forward IP packets. Ethernet frames are rebuilt at every hop.

**Misconception 2**

> IP addresses and MAC addresses serve the same purpose.

Incorrect.

MAC addresses provide local delivery.

IP addresses provide end-to-end logical addressing.

**Misconception 3**

> Applications communicate directly using IP.

Incorrect.

Application protocols rely on the Transport layer, which relies on the Network layer, which relies on the Link and Physical layers.

Every layer participates in the communication process.

### Summary

Protocol layering divides network communication into manageable components, each responsible for a specific aspect of data transmission. Encapsulation allows these layers to cooperate by progressively adding protocol information as data moves downward through the stack.

Application data becomes a TCP segment, which becomes an IP packet, which becomes an Ethernet frame before finally being transmitted as bits across a physical medium. At the destination, decapsulation removes these headers one by one until the original application data reaches its intended process.

This layered architecture enables modularity, interoperability, and efficient troubleshooting while allowing technologies at one layer to evolve without affecting the others. Understanding encapsulation is fundamental to analyzing packets, diagnosing network problems, and understanding how modern communication systems operate.

### Key Terms

| Term | Definition |
|------|------------|
| Protocol Stack | Layered architecture used for network communication |
| Encapsulation | Adding protocol headers as data moves down the stack |
| Decapsulation | Removing protocol headers as data moves up the stack |
| PDU | Protocol Data Unit used by a specific layer |
| Segment | Transport layer PDU for TCP |
| Datagram | Transport layer PDU for UDP or Network layer (context-dependent) |
| Packet | Network layer PDU |
| Frame | Link layer PDU |
| Bits | Physical layer representation of data |
| Header | Control information added by a protocol |
| Trailer | Information appended after the payload, such as the Ethernet FCS |
| Payload | Data carried by a protocol from the layer above |
