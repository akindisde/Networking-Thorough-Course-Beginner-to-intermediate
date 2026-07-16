## Module 1 — How the Internet Actually Works

## Lesson 1.3 — Protocols, RFCs, and Standards Bodies (Part 1)

### Introduction

Modern computer networks connect billions of devices across the world. These devices are manufactured by different companies, run different operating systems, and use different hardware architectures. Despite these differences, they communicate seamlessly because they all follow the same set of standardized communication rules.

These rules are known as **protocols**.

Every action performed on a network—loading a webpage, sending an email, streaming a video, or transferring a file—is governed by one or more protocols. Without standardized protocols, devices would not understand one another, making large-scale networking impossible.

This lesson introduces what a protocol is, why protocols are necessary, and how networking standards ensure interoperability across the global Internet.

### Learning Objectives

After completing this lesson, students should be able to:

- Define a networking protocol.
- Explain why protocols are required.
- Describe the three fundamental components of a protocol.
- Distinguish between protocols and implementations.
- Understand how multiple protocols cooperate during communication.
- Explain why standardization is essential for the Internet.

### What Is a Protocol?

A protocol is a formal set of rules that defines how devices exchange information across a network.

These rules specify:

- How data is structured.
- When data should be transmitted.
- How devices respond to received messages.
- How errors are detected and handled.
- How communication begins and ends.

Without agreed-upon rules, communication cannot occur.

A protocol acts as a common language understood by every participating device.

For example, when a browser communicates with a web server, both devices already know:

- Which messages should be sent.
- The order in which they are sent.
- The format of each message.
- How failures should be handled.

Neither device needs prior knowledge of the other's manufacturer or operating system.

As long as both correctly implement the protocol, communication succeeds.

### A Real-World Analogy

Imagine two pilots communicating with an airport control tower.

Their conversation follows strict procedures.

The pilot cannot simply speak freely.

Instead, communication follows standardized phrases and sequences.

Example:

```text
Pilot:
Requesting permission to land.

↓

Control Tower:
Cleared to land on Runway 27.

↓

Pilot:
Cleared to land, Runway 27.
```

Every message has:

- A defined structure.
- A specific meaning.
- An expected response.

If either participant ignored these rules, communication would become unreliable and potentially dangerous.

Networking protocols operate in exactly the same way.

### Why Protocols Are Necessary

Suppose every company designed its own proprietary networking language.

A Windows computer might communicate differently from a Linux server.

Cisco routers might require completely different packet formats than Juniper routers.

A web browser developed by Mozilla might be incompatible with a server running Apache.

The Internet would fragment into isolated networks unable to communicate.

Protocols prevent this fragmentation by defining universal communication rules.

Regardless of vendor, operating system, or hardware architecture, every compliant implementation behaves according to the same specification.

### The Three Components of Every Protocol

Although networking protocols vary considerably in complexity, every protocol defines three fundamental aspects of communication:

- Syntax
- Semantics
- Timing

Together, these describe **what** is transmitted, **what it means**, and **when it should occur**.

### Syntax

Syntax defines the format and structure of transmitted information.

It specifies how messages are organized.

For example, an IPv4 packet contains fields arranged in a specific order.

```text
+-----------------------------+
| Version                     |
+-----------------------------+
| Header Length               |
+-----------------------------+
| Total Length                |
+-----------------------------+
| Identification              |
+-----------------------------+
| Time To Live                |
+-----------------------------+
| Protocol                    |
+-----------------------------+
| Source Address              |
+-----------------------------+
| Destination Address         |
+-----------------------------+
```

Every IPv4 packet follows this exact structure.

If devices interpreted fields differently, communication would fail immediately.

Syntax therefore ensures that every implementation understands the physical layout of transmitted data.

### Semantics

Semantics define the meaning of each field and message.

Knowing where a field appears is not sufficient.

Devices must also understand what that field represents.

Consider the following HTTP request.

```http
GET /index.html HTTP/1.1
```

The syntax specifies where each word appears.

The semantics define what the word **GET** actually means.

In HTTP,

```
GET
```

means:

> Retrieve the requested resource.

Similarly,

```
POST
```

means:

> Submit data to the server.

Every protocol field has a precisely defined meaning.

This prevents ambiguity between communicating devices.

### Timing

Timing defines when communication occurs.

It specifies:

- Transmission order.
- Waiting periods.
- Retransmission behavior.
- Timeouts.
- Response expectations.

For example, TCP requires a three-way handshake before application data may be exchanged.

The sequence must always occur in the following order.

```text
Client

SYN

↓

Server

SYN-ACK

↓

Client

ACK
```

Changing this order would violate the protocol specification.

Timing therefore ensures that devices remain synchronized throughout communication.

### Protocol State

Many networking protocols are **stateful**, meaning that devices keep track of the current stage of communication.

Consider a simplified TCP connection.

```text
Closed

↓

SYN Sent

↓

Established

↓

Closing

↓

Closed
```

Each state determines which messages are valid.

For example, application data cannot be transmitted before the connection reaches the **Established** state.

State machines are a common feature of networking protocols and help ensure predictable communication.

### Request–Response Protocols

Many application-layer protocols follow a request–response model.

In this model, one device sends a request and the other returns a response.

Example:

```text
Browser

↓

HTTP Request

↓

Web Server

↓

HTTP Response

↓

Browser
```

Examples of request–response protocols include:

- HTTP
- DNS
- DHCP

Each request expects a corresponding response defined by the protocol.

### Client–Server Communication

Most Internet applications use the client–server model.

A **client** initiates communication.

A **server** provides a service.

Examples include:

| Client | Server | Protocol |
|----------|----------|----------|
| Browser | Web Server | HTTP |
| Email Client | Mail Server | SMTP |
| SSH Client | Linux Server | SSH |
| DNS Resolver | DNS Server | DNS |

The protocol defines how both sides communicate.

Neither side needs to know how the other is internally implemented.

### Protocol Independence

One of the most important characteristics of networking protocols is independence.

Each protocol performs one specialized task.

Consider a browser downloading a webpage.

Several protocols cooperate.

```text
HTTP

↓

TLS

↓

TCP

↓

IP

↓

Ethernet
```

HTTP does not perform routing.

TCP does not resolve domain names.

IP does not establish encrypted sessions.

Ethernet does not understand webpages.

Each protocol focuses on one responsibility while relying on lower layers to perform theirs.

This modular architecture greatly simplifies networking.

### Protocol Layering

Protocols rarely operate alone.

Instead, they form a stack.

Each protocol uses services provided by lower protocols.

For example:

```text
Application

HTTP

↓

Transport

TCP

↓

Network

IPv4

↓

Link

Ethernet

↓

Physical

Copper Cable
```

Each layer adds functionality without modifying the responsibilities of the others.

This layered architecture allows protocols to evolve independently.

For example:

```
HTTP

↓

HTTPS
```

Only the application layer changes.

TCP, IP, and Ethernet continue functioning normally.

Likewise,

```
Ethernet

↓

Wi-Fi
```

changes only the lower layers.

Applications remain completely unaware of the difference.

### Protocol vs Implementation

Students often confuse a protocol with the software that implements it.

A protocol is a specification.

An implementation is software that follows that specification.

For example:

Protocol:

```
HTTP
```

Implementations:

- Apache HTTP Server
- Nginx
- Microsoft IIS
- Caddy

Each server is written differently.

Each contains different source code.

Yet all communicate correctly because they implement the same HTTP specification.

This distinction applies throughout networking.

Protocols define behavior.

Implementations execute that behavior.

### Proprietary vs Open Protocols

Protocols can be categorized as either proprietary or open.

An **open protocol** is publicly documented and available for anyone to implement.

Examples include:

- TCP
- IPv4
- IPv6
- DNS
- HTTP
- HTTPS

Because their specifications are public, thousands of vendors can develop compatible products.

A **proprietary protocol** is controlled by a specific company.

Its specification may be restricted or unavailable.

Examples include certain vendor-specific routing protocols and management protocols.

The modern Internet relies primarily on open standards because they encourage interoperability and innovation.

### Why Standardization Matters

Without standardization:

- Devices from different vendors could not communicate.
- Software developers would need separate versions of every application.
- Internet growth would be severely limited.
- Competition would decrease.
- Costs would increase.

Standardized protocols allow any compliant device to join the Internet.

A smartphone manufactured in one country can communicate with a web server located on another continent because both implement the same protocol standards.

This universal compatibility is one of the greatest achievements of modern computer networking.

### Key Takeaways

- A protocol is a formal set of communication rules followed by networked devices.
- Every protocol defines syntax, semantics, and timing.
- Protocols ensure predictable and reliable communication between independent systems.
- Most Internet communication follows the client–server model.
- Networking protocols operate together as a layered protocol stack.
- A protocol specification is different from the software that implements it.
- Open standards enable interoperability between products developed by different vendors.
- Standardized protocols are the foundation of the global Internet.

### Preview

The next part introduces the **Internet Engineering Task Force (IETF)** and the **Request for Comments (RFC)** publication system. It explains how Internet standards are developed, why RFCs are considered the authoritative specifications for protocols such as IPv4, TCP, UDP, DNS, HTTP, and TLS, and introduces several RFC numbers that every networking professional should recognize.

## Lesson 1.3 — Protocols, RFCs, and Standards Bodies (Part 2)

### The Need for Internet Standards

The Internet is not owned or operated by a single company or government. It is a global collection of independently managed networks that agree to communicate using common protocols.

Every day, billions of devices manufactured by thousands of vendors exchange data successfully because they implement the same protocol specifications.

Someone must therefore define:

- How an IPv4 packet is structured.
- How a TCP connection is established.
- How DNS queries are formatted.
- How HTTP requests are transmitted.
- How encryption is negotiated.

Without official specifications, every vendor would create incompatible implementations, making worldwide interoperability impossible.

This responsibility is carried out by organizations that develop and publish networking standards.

The most influential of these organizations for Internet protocols is the **Internet Engineering Task Force (IETF)**.

### The Internet Engineering Task Force (IETF)

The **Internet Engineering Task Force (IETF)** is an international organization responsible for developing and maintaining the technical standards that define the Internet.

Unlike many standards organizations, the IETF is an open community rather than a traditional membership-based institution. Engineers, researchers, vendors, network operators, academics, and individuals from around the world collaborate to improve Internet technologies.

The IETF's primary goal is straightforward:

> Develop open, interoperable standards that allow the Internet to function reliably and evolve over time.

The organization does not manufacture networking equipment or sell products.

Instead, it publishes technical specifications that vendors voluntarily implement.

Today, nearly every Internet protocol depends on specifications produced by the IETF.

Examples include:

- IPv4
- IPv6
- TCP
- UDP
- ICMP
- DNS
- DHCP
- SMTP
- HTTP
- TLS
- BGP
- OSPF

### Guiding Principles of the IETF

The IETF has become known for several guiding principles that have shaped the development of the modern Internet.

One of its best-known philosophies is:

> **"Rough consensus and running code."**

This phrase emphasizes two important ideas.

First, standards should emerge through technical agreement rather than authority.

Second, protocols should be proven through working implementations instead of remaining purely theoretical.

A protocol that functions successfully in real-world networks is generally considered more valuable than one that exists only as a design document.

### Working Groups

The IETF organizes its work into specialized **Working Groups (WGs)**.

Each working group focuses on a particular area of Internet technology.

Examples include:

- Routing
- Security
- Transport protocols
- Applications
- DNS
- Network management
- IPv6
- HTTP

Each group discusses technical problems, proposes solutions, reviews implementations, and develops protocol specifications.

When consensus is reached, the resulting document may eventually become an RFC.

### What Is an RFC?

The official documents published by the IETF are called **Requests for Comments**, commonly abbreviated as **RFCs**.

Despite the name, RFCs are not simply requests for feedback.

They are the official publications that define many of the protocols used throughout the Internet.

An RFC may contain:

- A protocol specification.
- An extension to an existing protocol.
- Operational recommendations.
- Security considerations.
- Best practices.
- Informational guidance.
- Experimental technologies.

Some RFCs eventually become Internet Standards.

Others remain informational or experimental.

Regardless of category, every RFC receives a permanent identification number.

### Why the Name "Request for Comments"?

The term dates back to the early days of the ARPANET.

Researchers exchanged technical documents describing proposed networking ideas and invited comments from colleagues.

Although the Internet has changed dramatically since then, the historical name has remained.

Today, RFCs are carefully reviewed technical publications that often define globally deployed Internet standards.

### RFC Numbering

Each RFC receives a unique number when published.

For example:

- RFC 791
- RFC 793
- RFC 768
- RFC 1035
- RFC 8446

Once assigned, an RFC number is **never reused**.

If a specification is updated, the newer RFC receives a different number.

This numbering system provides a permanent historical record of Internet development.

### RFC Categories

Not every RFC represents an Internet Standard.

RFCs are published in several categories, each serving a different purpose.

#### Standards Track

Standards Track RFCs define protocols intended for widespread deployment.

Many of the protocols studied throughout this course belong to this category.

These documents undergo extensive technical review before publication.

#### Best Current Practice (BCP)

Best Current Practice documents describe recommended operational procedures.

Rather than defining new protocols, they explain how existing technologies should be deployed and managed.

#### Informational

Informational RFCs provide technical information without establishing standards.

They may describe architectures, implementation techniques, or historical background.

#### Experimental

Experimental RFCs describe new ideas that require evaluation before potential standardization.

These protocols may never become Internet Standards.

### Anatomy of an RFC

Although RFCs vary in length and complexity, most follow a similar structure.

A typical RFC includes:

- Title
- Authors
- Publication date
- Abstract
- Introduction
- Protocol specification
- Packet formats
- State machines
- Error handling
- Security considerations
- References

Modern RFCs often exceed one hundred pages.

Some protocol specifications span several related RFCs.

### Reading an RFC

RFCs are written as engineering specifications rather than textbooks.

They define exactly how a protocol must behave.

Consider the following simplified example:

```text
The sender MUST acknowledge every received segment.

The receiver SHOULD advertise its receive window.

The client MAY terminate the connection.
```

Notice the capitalization of the words **MUST**, **SHOULD**, and **MAY**.

These words have precise meanings.

### Requirement Levels

Many RFCs use standardized requirement terminology defined by RFC 2119.

The most common keywords include:

| Keyword | Meaning |
|----------|---------|
| MUST | Mandatory requirement |
| MUST NOT | Absolutely prohibited |
| SHOULD | Strong recommendation |
| SHOULD NOT | Generally discouraged |
| MAY | Optional behavior |

These words eliminate ambiguity.

For example:

```
The client MUST verify the server certificate.
```

This is not a suggestion.

Every compliant implementation is required to perform certificate verification.

### Why RFCs Matter

Networking professionals frequently consult RFCs when:

- Troubleshooting protocol behavior.
- Developing networking software.
- Implementing new protocols.
- Verifying standards compliance.
- Understanding protocol details.

Although introductory networking courses rarely require reading RFCs directly, experienced engineers often refer to them when resolving complex technical issues.

RFCs represent the authoritative source for Internet protocol specifications.

### RFC Updates and Obsolescence

Protocols evolve over time.

Security vulnerabilities are discovered.

New technologies emerge.

Performance improvements become possible.

Rather than modifying existing RFCs, the IETF typically publishes new RFCs that:

- Update previous specifications.
- Extend existing protocols.
- Replace obsolete technologies.

Older RFCs remain available as part of the historical record.

The newer RFC clearly indicates whether it updates or obsoletes earlier documents.

This approach preserves the complete evolution of Internet technologies.

### RFC Example: Understanding a Protocol Specification

Suppose an engineer wants to understand exactly how TCP establishes a connection.

Rather than relying on third-party documentation, they can consult the official TCP specification.

The RFC defines:

- TCP packet format.
- Sequence numbers.
- Acknowledgements.
- State transitions.
- Timeout behavior.
- Connection establishment.
- Connection termination.
- Error handling.

Because every compliant implementation follows the same specification, engineers worldwide share a common technical reference.

### The Importance of Open Standards

One of the greatest strengths of the Internet is that its core protocols are publicly documented.

Anyone can:

- Read the specifications.
- Develop compatible software.
- Build networking equipment.
- Study protocol behavior.
- Improve existing implementations.

This openness has encouraged innovation for decades.

Companies compete by building better implementations, not by hiding protocol specifications.

As a result, devices from different vendors can communicate seamlessly across the global Internet.

### Key RFCs Every Network Engineer Should Recognize

Although thousands of RFCs have been published, a small number form the foundation of modern networking.

The following RFCs are among the most important.

| RFC | Protocol | Purpose |
|-----:|----------|---------|
| 791 | IPv4 | Defines the Internet Protocol version 4 |
| 793 | TCP | Defines the Transmission Control Protocol |
| 768 | UDP | Defines the User Datagram Protocol |
| 826 | ARP | Defines the Address Resolution Protocol |
| 1035 | DNS | Defines the Domain Name System message format |
| 8446 | TLS 1.3 | Defines the Transport Layer Security version 1.3 protocol |

These RFCs will appear repeatedly throughout this course as each protocol is studied in detail.

### Key Takeaways

- The IETF develops and maintains the technical standards that define the Internet.
- Internet standards are published as RFCs (Requests for Comments).
- Every RFC receives a permanent identification number that is never reused.
- RFCs may define standards, operational practices, informational guidance, or experimental technologies.
- Requirement keywords such as **MUST**, **SHOULD**, and **MAY** have precise meanings defined by Internet standards.
- RFCs serve as the authoritative specifications for Internet protocols.
- Many of the protocols used daily—including IPv4, TCP, UDP, DNS, ARP, and TLS—are formally defined by RFCs published through the IETF.

### Preview

The next part examines other organizations that play critical roles in networking standardization, including the **IEEE**, which develops Ethernet and Wi-Fi standards, and the **Internet Assigned Numbers Authority (IANA)**, which manages protocol registries, port numbers, and global IP address allocations.

## Lesson 1.3 — Protocols, RFCs, and Standards Bodies (Part 3)

### Beyond the IETF

While the IETF defines most Internet protocols, it is not the only organization responsible for networking standards.

Modern computer networks rely on the work of several international organizations, each specializing in a different area of networking.

Among the most important are:

- IEEE
- IANA
- ICANN
- ISO

Each organization has a distinct responsibility, and together they ensure that the Internet remains interoperable, scalable, and globally accessible.

This section focuses on the IEEE and IANA, two organizations encountered frequently by network engineers.

### The Institute of Electrical and Electronics Engineers (IEEE)

The **Institute of Electrical and Electronics Engineers (IEEE)** is one of the world's largest professional engineering organizations.

Unlike the IETF, which primarily develops Internet protocols such as TCP and IP, the IEEE develops standards for technologies operating closer to the physical network.

Its standards cover areas such as:

- Ethernet
- Wi-Fi
- Wireless communication
- Electrical engineering
- Computer engineering
- Industrial networking

Many of the technologies encountered in enterprise networks originate from IEEE working groups.

### The IEEE 802 Project

Networking standards developed by the IEEE are organized under the **IEEE 802** family.

The number **802** refers to the project responsible for Local Area Network (LAN) and Metropolitan Area Network (MAN) technologies.

Numerous networking standards belong to this family.

Some of the most important include:

| Standard | Technology |
|----------|------------|
| IEEE 802.3 | Ethernet |
| IEEE 802.11 | Wi-Fi |
| IEEE 802.1Q | VLAN Tagging |
| IEEE 802.1X | Port-Based Network Access Control |
| IEEE 802.1D | Spanning Tree Protocol (STP) |

Whenever a network engineer refers to "802.3" or "802.11," they are referring to IEEE standards.

### IEEE 802.3 — Ethernet

Ethernet is the dominant wired networking technology used throughout the world.

Originally developed in the 1970s, Ethernet has evolved through numerous revisions while maintaining backward compatibility.

The IEEE 802.3 standard defines many aspects of Ethernet communication, including:

- Frame format
- MAC addressing
- Error detection
- Physical signaling
- Speed specifications
- Media types

Common Ethernet speeds include:

| Standard | Speed |
|-----------|-------|
| Fast Ethernet | 100 Mbps |
| Gigabit Ethernet | 1 Gbps |
| 10 Gigabit Ethernet | 10 Gbps |
| 40 Gigabit Ethernet | 40 Gbps |
| 100 Gigabit Ethernet | 100 Gbps |
| 400 Gigabit Ethernet | 400 Gbps |

Although transmission speeds have increased dramatically over the decades, devices continue to communicate using the standardized Ethernet frame defined by IEEE 802.3.

### Ethernet Frame Standardization

Because every manufacturer follows the same Ethernet specification, a network may contain equipment from many vendors.

For example:

```text
Dell PC

↓

Cisco Switch

↓

Juniper Router

↓

HPE Server
```

Although each device is manufactured by a different company, Ethernet frames remain compatible because every device follows the IEEE 802.3 specification.

This interoperability is one of the primary goals of networking standards.

### IEEE 802.11 — Wi-Fi

The IEEE also develops standards for wireless networking.

These standards belong to the **802.11** family.

Examples include:

| Standard | Marketing Name |
|-----------|----------------|
| 802.11a | Wi-Fi |
| 802.11b | Wi-Fi |
| 802.11g | Wi-Fi |
| 802.11n | Wi-Fi 4 |
| 802.11ac | Wi-Fi 5 |
| 802.11ax | Wi-Fi 6 |
| 802.11be | Wi-Fi 7 |

Although users often refer simply to "Wi-Fi," each generation introduces improvements in:

- Throughput
- Latency
- Range
- Spectral efficiency
- Multi-user communication
- Security

Despite these improvements, all implementations follow the IEEE specifications.

### Ethernet and Wi-Fi

Students often assume Ethernet and Wi-Fi are fundamentally different networking technologies.

In reality, they perform similar functions.

Both provide:

- Local network communication.
- MAC addressing.
- Frame delivery.
- Error detection.

The primary difference lies in the transmission medium.

Ethernet transmits frames over cables.

Wi-Fi transmits frames using radio waves.

Higher-layer protocols such as IP and TCP remain unchanged.

A browser behaves identically whether connected by Ethernet or Wi-Fi.

### IEEE 802.1Q — VLANs

Large networks are often divided into multiple logical networks known as **Virtual Local Area Networks (VLANs)**.

IEEE 802.1Q defines the method for inserting a VLAN identifier into an Ethernet frame.

This allows multiple logical networks to share the same physical switching infrastructure.

For example:

```text
Switch

├── VLAN 10 (Engineering)

├── VLAN 20 (Sales)

└── VLAN 30 (Management)
```

Although all devices may connect to the same switch, VLAN tagging keeps traffic logically separated.

VLANs are covered in detail later in the course.

### IEEE 802.1X — Network Authentication

Modern enterprise networks often require users to authenticate before receiving network access.

IEEE 802.1X defines **Port-Based Network Access Control**.

Instead of allowing every device immediate access, the switch or wireless access point requests authentication.

Typical authentication methods include:

- Username and password
- Digital certificates
- Enterprise credentials

Only after successful authentication is network access granted.

802.1X is commonly deployed in:

- Corporate offices
- Universities
- Government agencies
- Healthcare organizations

It provides an important first line of defense against unauthorized network access.

### Internet Assigned Numbers Authority (IANA)

Many networking values must remain globally unique.

For example:

- IP addresses
- TCP port numbers
- UDP port numbers
- Protocol numbers

If different organizations assigned these values independently, conflicts would quickly occur.

The **Internet Assigned Numbers Authority (IANA)** maintains these global registries.

IANA ensures that Internet numbering resources remain organized and unique.

### Responsibilities of IANA

IANA maintains several important registries.

These include:

- TCP port numbers.
- UDP port numbers.
- IP protocol numbers.
- Autonomous System Number registries.
- DNS root zone management.
- IP address allocation to Regional Internet Registries.

Although IANA manages these registries, it does not assign every address directly to end users.

Instead, it coordinates global allocation through hierarchical management.

### Port Numbers

Every TCP or UDP service uses a numerical identifier known as a **port number**.

Rather than inventing new numbers independently, software developers use values assigned through IANA.

Examples include:

| Service | Port |
|----------|-----:|
| FTP | 21 |
| SSH | 22 |
| Telnet | 23 |
| SMTP | 25 |
| DNS | 53 |
| HTTP | 80 |
| POP3 | 110 |
| IMAP | 143 |
| HTTPS | 443 |

Because these assignments are standardized, every browser knows that HTTPS servers normally listen on port 443.

Likewise, every SSH client expects SSH servers on port 22.

### Port Number Ranges

IANA divides port numbers into three categories.

| Range | Name |
|--------|------|
| 0–1023 | Well-Known Ports |
| 1024–49151 | Registered Ports |
| 49152–65535 | Dynamic / Private Ports |

#### Well-Known Ports

These ports are reserved for widely deployed services.

Examples include:

- HTTP
- HTTPS
- DNS
- SSH

Most servers use these default assignments.

#### Registered Ports

Organizations may register port numbers for proprietary or specialized applications.

Examples include:

- Database servers
- Enterprise software
- Vendor-specific services

#### Dynamic Ports

Dynamic ports are typically assigned temporarily by the operating system.

When a browser connects to:

```
https://example.com
```

the destination port is usually:

```
443
```

The source port, however, is automatically selected from the dynamic range.

Example:

```
Source Port: 52481

Destination Port: 443
```

This temporary assignment allows thousands of simultaneous connections from the same computer.

### IP Address Allocation

Every public IP address on the Internet must be globally unique.

IANA manages the global pool of IP addresses.

Rather than assigning addresses directly to Internet users, IANA delegates large address blocks to **Regional Internet Registries (RIRs)**.

The five Regional Internet Registries are:

| Registry | Region |
|-----------|--------|
| AFRINIC | Africa |
| APNIC | Asia-Pacific |
| ARIN | North America |
| LACNIC | Latin America and Caribbean |
| RIPE NCC | Europe, Middle East, Central Asia |

These organizations then allocate addresses to Internet Service Providers (ISPs), businesses, governments, and other organizations.

### Protocol Numbers

IP packets contain a field called the **Protocol Number**.

This field tells the receiving device which Transport layer protocol is encapsulated inside the IP packet.

IANA maintains the registry of protocol numbers.

Common examples include:

| Number | Protocol |
|---------|----------|
| 1 | ICMP |
| 6 | TCP |
| 17 | UDP |
| 50 | ESP |
| 51 | AH |

For example:

If an IPv4 packet contains:

```
Protocol = 6
```

the operating system immediately knows that the payload contains a TCP segment.

### Why Centralized Registries Matter

Imagine two software companies independently deciding to use port 443 for unrelated services.

Applications would conflict.

Similarly, if two organizations used the same public IP address, Internet routing would fail.

Centralized registries prevent these problems by ensuring that important networking identifiers remain globally unique.

Every compliant networking device therefore interprets addresses, protocol numbers, and port numbers consistently.

### Key Takeaways

- The IEEE develops standards primarily for Local Area Network technologies such as Ethernet and Wi-Fi.
- The IEEE 802 family includes standards such as 802.3 (Ethernet), 802.11 (Wi-Fi), 802.1Q (VLANs), and 802.1X (Network Access Control).
- Ethernet and Wi-Fi perform similar networking functions while using different physical transmission media.
- IANA manages global registries including port numbers, protocol numbers, and IP address allocations.
- Port numbers uniquely identify network services, while protocol numbers identify the Transport protocol carried within an IP packet.
- Standardized registries ensure interoperability across the global Internet by preventing conflicts and maintaining globally unique assignments.

### Preview

The final part of this lesson examines several RFCs that every network engineer should recognize, explains why these documents became foundational Internet standards, and concludes with a practical exercise exploring RFCs, protocol registries, and Wireshark observations.

## Lesson 1.3 — Protocols, RFCs, and Standards Bodies (Part 4)

### Foundational RFCs Every Network Engineer Should Know

More than nine thousand RFCs have been published since the early days of the Internet. While only a small percentage define protocols that are used every day, a handful of RFCs form the foundation of nearly all modern network communication.

Professional network engineers frequently recognize these RFC numbers immediately because they define the protocols encountered throughout daily operations, troubleshooting, security analysis, and protocol development.

The purpose of memorizing these RFCs is not to remember every technical detail they contain. Instead, engineers should associate each RFC with the protocol it defines and understand its importance within the Internet protocol suite.

### RFC 791 — Internet Protocol Version 4 (IPv4)

Published in 1981, **RFC 791** defines **Internet Protocol Version 4 (IPv4)**, the protocol responsible for logical addressing and routing across interconnected networks.

Nearly every packet transmitted across today's Internet contains an IPv4 header or its successor, IPv6.

RFC 791 specifies:

- IPv4 packet format.
- Header fields.
- Fragmentation.
- Time To Live (TTL).
- Protocol identification.
- Source and destination addressing.

A simplified IPv4 packet appears below.

```text
+-------------------------------+
| IPv4 Header                   |
+-------------------------------+
| Transport Layer Payload       |
+-------------------------------+
```

Important fields defined by RFC 791 include:

| Field | Purpose |
|--------|---------|
| Version | Identifies IPv4 |
| Header Length | Length of the IP header |
| Total Length | Size of the packet |
| TTL | Prevents routing loops |
| Protocol | Identifies TCP, UDP, ICMP, etc. |
| Source Address | Sender's IP address |
| Destination Address | Receiver's IP address |

Every router on the Internet examines several of these fields while forwarding packets toward their destination.

Although IPv6 has become increasingly common, IPv4 remains the dominant protocol on many networks.

### RFC 793 — Transmission Control Protocol (TCP)

**RFC 793** defines the **Transmission Control Protocol (TCP)**.

TCP provides reliable, connection-oriented communication between applications.

Unlike IP, TCP ensures that data is delivered:

- Reliably.
- In order.
- Without duplication.

RFC 793 defines:

- Connection establishment.
- Sequence numbers.
- Acknowledgements.
- Flow control.
- Connection termination.
- Retransmission behavior.

One of its best-known features is the **Three-Way Handshake**.

```text
Client                  Server

SYN
----------------------->

          SYN-ACK
<-----------------------

ACK
----------------------->
```

Only after this exchange can application data be transmitted.

TCP is used by applications requiring reliable communication, including:

- HTTP
- HTTPS
- SSH
- FTP
- SMTP

### RFC 768 — User Datagram Protocol (UDP)

**RFC 768** defines the **User Datagram Protocol (UDP)**.

UDP provides a lightweight alternative to TCP.

Unlike TCP, UDP:

- Does not establish a connection.
- Does not guarantee delivery.
- Does not retransmit lost packets.
- Does not guarantee packet ordering.

Its header is intentionally simple.

```text
+----------------------+
| Source Port          |
+----------------------+
| Destination Port     |
+----------------------+
| Length               |
+----------------------+
| Checksum             |
+----------------------+
```

Because of its simplicity, UDP introduces minimal overhead and latency.

Applications commonly using UDP include:

- DNS
- DHCP
- VoIP
- Online gaming
- Video conferencing
- Streaming media

The choice between TCP and UDP depends entirely on application requirements.

### RFC 826 — Address Resolution Protocol (ARP)

Before an Ethernet frame can be transmitted across a local network, the sender must determine the destination device's MAC address.

**RFC 826** defines the **Address Resolution Protocol (ARP)**.

ARP resolves an IPv4 address into its corresponding MAC address.

The process follows a simple request-response pattern.

```text
PC

Who has 192.168.1.1?

---------------------------->

Router

192.168.1.1 is
00:1B:63:84:45:E6

<----------------------------
```

After receiving the reply, the sender stores the mapping in its ARP cache.

Subsequent communications typically use the cached entry without sending another ARP request.

Although IPv6 replaces ARP with Neighbor Discovery Protocol (NDP), ARP remains fundamental to IPv4 networking.

### RFC 1035 — Domain Name System (DNS)

Humans prefer readable names.

Computers communicate using IP addresses.

**RFC 1035** defines much of the **Domain Name System (DNS)** used to translate between these two forms.

Example:

```
example.com

↓

93.184.216.34
```

RFC 1035 specifies:

- DNS message format.
- Resource Records.
- Query structure.
- Response structure.
- Name compression.
- DNS packet fields.

Without DNS, users would need to memorize numerical IP addresses for every website they visit.

### RFC 8446 — Transport Layer Security (TLS) 1.3

Modern web traffic is almost always encrypted.

**RFC 8446** defines **TLS 1.3**, the latest major version of the Transport Layer Security protocol.

TLS provides:

- Confidentiality.
- Integrity.
- Authentication.

It protects application data from interception and modification while it travels across untrusted networks.

A simplified TLS handshake appears below.

```text
Client

Client Hello

---------------------->

Server

Server Hello

Certificate

Finished

<----------------------

Client

Finished

---------------------->
```

Once the handshake completes, application data is encrypted before transmission.

Today, nearly every HTTPS website depends on TLS.

### Recognizing RFC Numbers

Network engineers are not expected to memorize every RFC.

However, recognizing several foundational RFC numbers greatly improves technical communication.

| RFC | Protocol | Layer |
|-----:|----------|-------|
| 791 | IPv4 | Network |
| 793 | TCP | Transport |
| 768 | UDP | Transport |
| 826 | ARP | Link |
| 1035 | DNS | Application |
| 8446 | TLS 1.3 | Application |

These RFCs represent technologies encountered daily in enterprise networks.

### How Vendors Use Standards

Networking vendors rarely invent entirely new communication protocols.

Instead, they implement existing standards.

For example, consider a simple network.

```text
Windows Laptop

↓

Cisco Switch

↓

Juniper Router

↓

Linux Server
```

Although every device originates from a different manufacturer, communication succeeds because each implements the same standards.

The browser generates an HTTP request.

TCP establishes a connection according to RFC 793.

IP forwards packets according to RFC 791.

Ethernet frames follow IEEE 802.3.

The switch forwards frames using Ethernet rules.

The router forwards packets using IP.

The web server interprets HTTP according to Internet standards.

No vendor-specific translation is required.

Standardization enables interoperability.

### Reading Protocol Information in Wireshark

Wireshark allows engineers to observe these standards directly.

Open any captured packet.

Expand the protocol tree.

You may observe:

```text
Frame

Ethernet II

Internet Protocol Version 4

Transmission Control Protocol

Transport Layer Security
```

Each protocol shown corresponds to a published standard.

The packet itself is a real implementation of the specifications defined by the IETF and IEEE.

Understanding protocol standards allows engineers to interpret exactly what Wireshark displays.

### Practical Laboratory

#### Objective

Identify protocols and their corresponding standards within a packet capture.

#### Requirements

- Wireshark
- Internet connection
- Web browser

#### Procedure

1. Start Wireshark.
2. Begin capturing traffic.
3. Visit a secure website.
4. Stop the capture.
5. Select a TCP packet.
6. Expand every protocol layer.

#### Tasks

For each protocol displayed:

- Identify its protocol name.
- Identify its protocol layer.
- Determine which standards organization defines it.
- Determine whether it originates from an RFC or an IEEE standard.

Complete the following table.

| Protocol | Layer | Standards Organization | Standard |
|-----------|-------|------------------------|----------|
| Ethernet | Link | IEEE | IEEE 802.3 |
| IPv4 | Network | IETF | RFC 791 |
| TCP | Transport | IETF | RFC 793 |
| TLS | Application | IETF | RFC 8446 |

Repeat the exercise using a DNS query.

Identify the protocol defined by RFC 1035.

### Common Misconceptions

#### Misconception 1

> Every RFC is an Internet Standard.

Incorrect.

Many RFCs are informational, experimental, or describe operational practices rather than defining standards.

Only some RFCs progress through the standards process.

#### Misconception 2

> The IEEE develops Internet protocols such as TCP and IP.

Incorrect.

TCP, IP, UDP, DNS, and TLS are developed primarily through the IETF.

The IEEE develops technologies such as Ethernet, Wi-Fi, VLANs, and network access control.

#### Misconception 3

> IANA creates networking protocols.

Incorrect.

IANA does not define protocols.

Instead, it manages global registries including:

- Port numbers.
- Protocol numbers.
- IP address allocations.
- DNS root zone information.

#### Misconception 4

> Standards limit innovation.

In practice, the opposite is true.

Open standards encourage innovation because manufacturers compete by improving implementations rather than inventing incompatible protocols.

The existence of common standards has allowed thousands of vendors to develop interoperable networking equipment over several decades.

### Summary

The Internet functions because devices throughout the world implement a common set of open standards.

Protocols define how communication occurs, while standards organizations ensure that these protocols remain consistent, interoperable, and publicly documented.

The IETF develops Internet protocols and publishes them as RFCs. The IEEE defines technologies such as Ethernet and Wi-Fi that operate at the lower layers of the protocol stack. IANA manages the numerical registries required for global interoperability, including port numbers, protocol identifiers, and IP address allocations.

Understanding the responsibilities of these organizations helps explain why equipment from different manufacturers communicates without requiring proprietary translation. Every packet observed in Wireshark is a practical implementation of these standards.

Throughout the remainder of this course, nearly every protocol introduced will trace back to one of the standards discussed in this lesson.

### Key Terms

| Term        | Definition                                                                     |
| ----------- | ------------------------------------------------------------------------------ |
| Protocol    | A formal set of rules governing communication between devices                  |
| Standard    | An agreed specification ensuring interoperability                              |
| IETF        | Organization responsible for Internet protocol development                     |
| RFC         | Request for Comments; official Internet technical publication                  |
| IEEE        | Organization responsible for Ethernet, Wi-Fi, and many networking technologies |
| IANA        | Organization managing Internet protocol registries and numbering resources     |
| RFC 791     | IPv4 specification                                                             |
| RFC 793     | TCP specification                                                              |
| RFC 768     | UDP specification                                                              |
| RFC 826     | ARP specification                                                              |
| RFC 1035    | DNS specification                                                              |
| RFC 8446    | TLS 1.3 specification                                                          |
| IEEE 802.3  | Ethernet standard                                                              |
| IEEE 802.11 | Wi-Fi standard                                                                 |
| IEEE 802.1Q | VLAN tagging standard                                                          |
| IEEE 802.1X | Port-based network access control standard                                     |
