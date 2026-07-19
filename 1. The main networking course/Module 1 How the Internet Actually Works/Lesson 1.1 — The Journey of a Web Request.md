# Module 1 — How the Internet Actually Works

## Lesson 1.1 — The Journey of a Web Request (Part 1)

### Introduction

Every interaction with the Internet begins with a simple action. A user opens a web browser, types a website address such as `google.com`, and presses **Enter**. Within a fraction of a second, the requested webpage appears on the screen. Although this process feels instantaneous, it involves dozens of protocols, multiple devices, several operating system components, and thousands of exchanged packets.

Understanding this sequence is one of the most important milestones in networking education. Almost every networking concept introduced later in this course—including Ethernet, ARP, IP addressing, routing, DNS, TCP, TLS, HTTP, switching, firewalls, and network security—represents one step in this communication process.

Rather than studying these technologies independently, this lesson presents the complete communication process from beginning to end. Later modules revisit each stage individually, examining the underlying protocols, packet structures, configurations, and troubleshooting techniques in greater detail, but still, we will have a proper introduction to what they do from this moment building up the knowledge about them.

By the end of this lesson, students should understand the overall journey of a web request and recognize how different networking protocols *cooperate* to deliver a webpage.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Cooperate|Terminology]]

### Learning Objectives

After completing this lesson, students should be able to:

- Describe the complete journey of a web request.
- Explain why multiple networking protocols are required.
- Identify the role of the browser, operating system, router, switch, DNS server, and web server.
- Distinguish between application names and IP addresses.
- Understand the purpose of DNS resolution.
- Recognize where future networking topics fit within the communication process.

### The Big Picture

Consider the following action; A user enters the following address into a browser:

```
https://www.google.com
```

After pressing **Enter**, the browser <u>eventually</u> displays Google's homepage.

Although this appears to be a single action, the *[[Operating system]]* performs many independent tasks before any webpage can be displayed.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Operating system (OS)|Definitions]]

A simplified overview is shown below.

```text
User types URL and hit enter
 │
Browser
 │
DNS Lookup
 │
Local Network
 │
Router
 │
Internet
 │
Google Server
 │
HTTP Response
 │
Browser Rendering
 │
Webpage Displayed
```

Each box in this diagram represents multiple networking technologies working together.

### A Communication Problem

Humans and computers communicate differently; Humans prefer names because they are meaningful and easy to remember. Examples include:

- google.com
- microsoft.com
- openai.com
- wikipedia.org

Computers, however, communicate using <u><mark style="background:#fff88f">numerical addresses known as IP addresses.</mark></u>

For example:

```
142.250.190.78
```

or

```
2607:f8b0:4004:c1b::66
```

A computer cannot directly communicate using the name **google.com**. Before any connection can be established, the name must first be translated into an IP address.

This translation process is performed by the *[[Domain Name System (DNS)]]*, one of the most fundamental services on the Internet, and that's why the browser does a DNS lookup first thing.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Domain name system (DNS)|Definitions]]

### Breaking Down a *[[Uniform Resource Locator (URL)]]*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Uniform resource locator (URL)|Definitions]]

Before contacting any remote server, the browser analyzes the address entered by the user. Consider the following URL :

```
https://www.google.com/search?q=networking
```

This address consists of several components :

| Component | Value          | Purpose                |
| --------- | -------------- | ---------------------- |
| Protocol  | https          | Communication protocol |
| Hostname  | www.google.com | Target server          |
| Path      | /search        | Requested resource     |
| Query     | q=networking   | Additional parameters  |

The browser *interprets* each component independently.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Interpret|Terminology]]

The hostname identifies the server.
The protocol determines how communication should occur.
The path specifies which resource is requested.
The query string supplies additional information to the server.
At this stage, no network communication has occurred.
The browser is simply interpreting the information supplied by the user.

### Browser Processing

A modern *[[Web browser]]* performs significantly more work than displaying web pages.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Web browser|Definitions]]

It functions as <u>sophisticated</u> networking applications responsible for managing multiple communication tasks simultaneously.

Immediately after the URL is entered, the browser begins several checks, these include:

- Verifying URL syntax
- Checking *browser cache*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Browser cache|Definitions]]

![[Cybersecurity journey/1. Networking/Q&A#❔ - Why does the browser check its cache processing a URL ?|Q&A]]

- Checking *DNS cache*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - DNS cache|Definitions]]

- Looking for existing *[[Transmission Control Protocol (TCP)]]* connections

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Transmission Control Protocol (TCP)|Definitions]]

![[Cybersecurity journey/1. Networking/Q&A#❔ - Why does a browser look for existing TCP connecting processnig a URL ?|Q&A]]

- Applying browser security policies
- Determining whether HTTPS is required

Some of these checks may eliminate unnecessary network communication. For example, if the requested webpage is already stored in the browser cache and remains valid, downloading the page again may not be necessary.

Similarly, if the IP address of the website is already stored in the local DNS cache, a new DNS lookup can be avoided.

*Caching* significantly reduces network traffic and improves browsing performance.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Caching|Terminology]]

### Browser Cache

Browsers store previously downloaded resources locally. These may include:

- HTML pages
- Images
- CSS stylesheets
- JavaScript files
- Fonts

If a previously downloaded resource remains valid, the browser can reuse it without downloading it again. For example:

```
First Visit :
Browser
      │
Download Logo.png
      │
Store Locally
```

Later:

```
Second Visit :
Browser
      │
Read Logo.png from Local Cache
```

No Internet communication is required, because the resource needed is, now, local (in your machine).

Caching improves :
- Page loading speed
- User experience
- Server performance
- Bandwidth utilization

Later modules will examine *[[HTTP caching]]* in detail.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - HTTP caching|Definitions]]

### DNS Cache

Operating systems also maintain a DNS cache; suppose a computer recently visited Google, instead of asking a DNS server again, the operating system may already know Google's IP address.

```
google.com
↓
142.250.x.x
```

If the cached *DNS record* has not expired, the operating system immediately returns the stored IP address.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - DNS record|Definitions]]

This avoids another DNS request.

Every DNS record contains a value known as the ***Time To Live (TTL)***.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Time-To-Live (TTL)|Definitions]]

The TTL determines how long a record may remain cached before it <u>must</u> be requested again.

Caching allows frequently visited websites to load more quickly while reducing the workload placed on DNS servers.

### Why DNS Exists

Imagine attempting to browse the Internet without DNS, Instead of typing:

```
google.com
```

Users would need to remember addresses such as :

```
142.250.190.78
```

- Every website would require memorizing one or more IP addresses.
- Furthermore, if a company changed servers, every user would need to learn the new address.

DNS solves this problem by separating names from addresses. Users remember names. Computers communicate using IP addresses. DNS acts as the translator between these two worlds.

### What Is DNS?

The Domain Name System (DNS) is a distributed database responsible for translating domain names into IP addresses, it is often compared to a telephone directory, instead of looking up telephone numbers by name, DNS looks up IP addresses by domain name.

Example:

```
google.com
↓
142.250.190.78
```

DNS does **not** transport webpages.
DNS does **not** establish network connections.
DNS performs only one task : **Name resolution.**

Once the IP address has been discovered, DNS has completed its work, <mark style="background:#fff88f">other protocols take over</mark>.

### Where Does the DNS Query Go?

When the operating system cannot answer from its local cache, it sends a *DNS query* to a *DNS server*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - DNS query|Definitions]]

![[Cybersecurity journey/1. Networking/Definitions#🧠 - DNS server|Definitions]]

This server is usually operated by:

- The *[[Internet Service Provider (ISP)]]*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Internet service provider (ISP)|Definitions]]

- Google Public DNS
- Cloudflare
- Quad9
- OpenDNS

Example:

```
Computer
↓
DNS Server
↓
Returns IP Address
```

The computer does not contact Google immediately, instead, it first asks another server:

> "What is the IP address of google.com?"

The DNS server responds with the requested address, <u>Only</u> after receiving this response can communication with Google's servers begin.

### Recursive DNS Resolution

Most computers communicate with a recursive DNS resolver. The resolver performs the difficult work of locating the correct server. 

![[Cybersecurity journey/1. Networking/Q&A#❔ - Why is it called "recursive" DNS resolution ?|Q&A]]


The process is simplified below :

```text
Computer
     │
Recursive DNS Resolver
     │
Root DNS Server
     │
.com DNS Server
     │
google.com Authoritative DNS Server
     │
IP Address Returned
```

Although several servers may participate in the lookup, the client typically communicates only with the recursive resolver, the resolver performs the remaining steps on behalf of the client.

> This process is explored in detail in Module 8.

### Multiple IP Addresses

<u>Large websites rarely use a single server</u>, instead, DNS may return multiple IP addresses. Example:

```
google.com
↓
142.250.x.x
↓
142.251.x.x
↓
172.217.x.x
```

This provides several advantages.

- *Load balancing*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Load balancing|Definitions]]

- Geographic distribution
- Redundancy
- High availability

Users in different countries may receive different IP addresses even when requesting the same website, this allows users to connect to nearby data centers, reducing *latency*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Latency|Definitions]]

### At This Point in the Journey

After DNS resolution has completed, the computer finally knows the destination IP address.

However, communication still cannot begin.

Several important questions remain unanswered.

- How does the computer send data across the local network?
- How does it discover the router?
- How does it determine the destination MAC address?
- How do switches forward the packet?
- How do routers deliver the packet across the Internet?

These questions are answered by the protocols operating below DNS.

The next part of this lesson begins at the moment the operating system has obtained the destination IP address and must prepare the packet for transmission across the local Ethernet network.

### Key Takeaways

- Browsers perform significant processing before any network communication begins.
- URLs contain multiple components, including the protocol, hostname, path, and query string.
- Browser and DNS caches reduce unnecessary network traffic.
- Computers communicate using IP addresses rather than domain names.
- DNS translates human-readable names into numerical IP addresses.
- Recursive DNS resolvers perform name resolution on behalf of client devices.
- DNS completes its work before any connection to the destination server is established.
- Obtaining an IP address is only the beginning of the communication process.

### Preview

Part 2 begins immediately after DNS resolution. The following topics will be covered:

- Default gateways
- ARP resolution
- MAC addresses
- Ethernet frame construction
- Switch forwarding
- Router forwarding
- Packet traversal across the Internet
- Arrival at the destination network

## Lesson 1.1 — The Journey of a Web Request (Part 2)

### From DNS to Data Transmission

At the end of the DNS resolution process, the operating system knows the IP address of the destination server. Suppose the DNS server returned the following address for `google.com`.

```
142.250.190.78
```

Knowing the destination IP address does not mean communication can begin immediately. The computer still needs to determine <u>how the packet should leave the local network.</u>

At this point, the operating system asks an important question:

> Is the destination located on my local network, or is it on a remote network?

The answer determines the next step.

### *Local Network* vs Remote Network

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Local Area Network (LAN)|Definitions]]

Every device connected to a network belongs to an IP network, sometimes called a *subnet*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Subnet|Definitions]]

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Subnet mask|Definitions]]

Consider the following example.

```
PC
IP Address: 192.168.1.10
Subnet Mask: 255.255.255.0
```

This *[[Subnet mask]]* indicates that every address from :

```
192.168.1.1
```

to

```
192.168.1.254
```

<u>belongs to the same local network</u>.
Suppose the destination is :

```
192.168.1.50
```

Because both devices belong to the same subnet, communication can occur directly; in simple words, they won't need to much work to talk to each other since they're so close in terms of "do I need to deal with <u>routing</u> and other things to get to the other device", these are concept that will covered in the future parts.

Now consider a different destination.

```
142.250.190.78
```

This address belongs to an entirely different network.

The computer cannot send packets directly to Google because Google is not connected to the local *Ethernet network*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Ethernet network|Definitions]]

> Just a note, Networking concepts can get from 0 to 100 very fast in terms of density and complexity, but you should not worry about some terms that you don't understand yet, just know they exist and they won't affect your understanding of the current material that you're reading, at the end of the lessons and whole chapters, labs will be a great tool to visualize and really stabilize what you've learned so far.

Instead, the packet must first be delivered to another device known as the **default gateway**.

### The Default Gateway

A *default gateway* is a router responsible for forwarding packets to other networks, every computer connected to a network normally has one configured.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Default gateway|Definitions]]

Example:

```
PC
IP Address:
192.168.1.10
Default Gateway:
192.168.1.1
```

Whenever the destination exists outside the local subnet, packets are transmitted to the default gateway.

The router then determines the next hop toward the destination.

A simplified network appears below :

![[Pasted image 20260718185048.png]]

Focus on the fact that a the router acts as the exit point from the local network, more specifically; the *interface* `G0/1` facing the LAN is the one configured to be the default gateway of the LAN.

For now, the WAN, which means wide area network, just means the internet.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Interface|Definitions]]

### Another Problem

The computer now knows that the router should receive the packet. However, Ethernet networks do not deliver *frames* using IP addresses. Ethernet uses **MAC addresses** for that.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Ethernet frame|Definitions]]

![[Cybersecurity journey/1. Networking/Q&A#❔ - Why not just call it data, why should we call it a frame or another name ?|Q&A]]

You noticed that inside the LAN we have another piece of hardware called a switch, for now, you should just know it doesn't need IP addresses to communicate, it need *MAC addresses*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Media Access Control (MAC) address|Definitions]]

The computer therefore knows:

```
Destination IP
↓
192.168.1.1
```

but still needs:

```
Destination MAC
↓
??
```

Without the router's MAC address, the computer cannot <u>build</u> an Ethernet frame.

Another protocol is required, a protocol that specializes in getting the destination MAC address knowing the IP address.

### [[Address Resolution Protocol (ARP)]]

The *Address Resolution Protocol (ARP)* maps IP addresses to MAC addresses <mark style="background:#fff88f">on local Ethernet networks.</mark>

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Address Resolution Protocol (ARP)|Definitions]]

Its purpose is simple :

```
Known:
IP Address
Need:
MAC Address
```

ARP performs this translation <u>automatically</u>.

#### The ARP Request

Suppose the computer wants to reach :

```
192.168.1.1
```

but does not know its MAC address, The operating system sends an ARP Request. Conceptually, the message says:

> "Who owns IP address 192.168.1.1?"

Because the computer does not know which device owns this address, the request is <mark style="background:#fff88f">sent to every device on the local network.</mark> This is known as a *broadcast*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Broadcast|Definitions]]

```text
PC
↓
ARP Request
↓
All Devices
```

Every connected device receives the request.

#### The ARP Reply

Each device examines the requested IP address. Only the router recognizes the address as its own (because in the example we set its address as 192.168.1.1). The router replies :

```
192.168.1.1
↓
AA:BB:CC:DD:EE:FF
```

The reply is transmitted only to the requesting computer. Unlike the broadcast request, the reply is a *unicast transmission*. The computer now knows the router's MAC address.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Unicast transmission|Definitions]]

#### The ARP Cache

To avoid repeating this process for every packet, operating systems store ARP results temporarily, This table is called the **ARP cache**. Example :

| IP Address   | MAC Address       |
| ------------ | ----------------- |
| 192.168.1.1  | AA:BB:CC:DD:EE:FF |
| 192.168.1.15 | 98:71:23:55:8A:21 |

Future packets destined for the same router reuse this cached information until the entry expires. This significantly reduces unnecessary broadcast traffic.

### Building the Packet

The application has produced data that must be transmitted across the network. Before transmission, the operating system adds *protocol headers*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Protocol header|Definitions]]

Here's a quick illustration of protocols and their headers (information related) that they add to the original data that you wanna send over the internet or any network, mind not the complexity, just observe how rich the information added to your data to reliably deliver it to the destination successfully: 

![[Pasted image 20260718230825.png]]

Protocols add information so they control many aspects of the process of sending and receiving, that information is the headers, and adding headers to data is called *[[Encapsulation]]*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Encapsulation|Definitions]]

Mind that a protocol can add a header and the whole result now is still called data, so when another protocol receives that data, it can add headers on top of the headers already their. More on this in the future.

At a high level :

```
Application Data
↓
TCP Segment
↓
IP Packet
↓
Ethernet Frame
↓
Bits
```

<mark style="background:#fff88f">Each protocol adds information required for its specific task.</mark> Protocols belong to layers, and when data leaves a layer, its conventional name changes, for example : raw data is just data until it leaves the transport layer, and now its a segment.

### Ethernet Frame Construction

The Ethernet header contains information <u>used only on the local network.</u> A simplified Ethernet frame appears below :

```text
+----------------------------------------------+
| Destination MAC Address                      |
+----------------------------------------------+
| Source MAC Address                           |
+----------------------------------------------+
| EtherType                                    |
+----------------------------------------------+
| Payload (IP Packet)                          |
+----------------------------------------------+
| Frame Check Sequence (FCS)                   |
+----------------------------------------------+
```

Notice that the destination is **the router's MAC address**, not Google's MAC address. Google's MAC address is unknown and <u>irrelevant</u> because MAC addresses matter only within a local Ethernet network.

### IP Packet Construction

Inside the Ethernet frame is an IP packet. A simplified packet appears below :

```text
+--------------------------------+
| Source IP Address              |
+--------------------------------+
| Destination IP Address         |
+--------------------------------+
| TTL                            |
+--------------------------------+
| Protocol                       |
+--------------------------------+
| Data                           |
+--------------------------------+
```

Unlike the Ethernet header, the destination IP remains Google's IP address.

```
Destination IP
↓
142.250.190.78
```

The IP address remains unchanged while the packet travels across the Internet.

### Ethernet vs IP

Students often confuse these two addressing systems. Their responsibilities are completely different :

| Ethernet | IP |
|-----------|----|
| Local communication | End-to-end communication |
| Uses MAC addresses | Uses IP addresses |
| Changes every hop | Remains constant |
| Layer 2 | Layer 3 |

This distinction is one of the most important concepts in networking.

### The First Transmission

The computer converts the Ethernet frame into electrical, optical, or radio signals depending on the physical medium. The signals travel to the first networking device. In most networks, this device is an Ethernet [[Switch]].

#### What the Switch Does

Switches operate at Layer 2 of the OSI model. Unlike hubs, switches examine only the Ethernet header.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What do we mean when we say a device operates at a certain layer ?|Q&A]]

The switch reads :

```
Destination MAC Address
```

It <u>can not</u> examine :
- IP addresses
- TCP ports
- HTTP requests

The switch simply determines which port leads to the destination MAC address. If the destination is already known, the frame is forwarded only through the appropriate *physical port*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Physical port|Definitions]]

If the destination is unknown, the frame is temporarily flooded (sent through) to every port except the incoming one.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Flooded|Terminology]]

#### The Frame Reaches the Router

Eventually the frame reaches the default gateway, The router receives the Ethernet frame and performs several actions :
1. Verifies the frame integrity.
2. Removes the Ethernet header.
3. Reads the destination IP address.
4. Searches the *routing table*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Routing table|Definitions]]

5. Determines the next hop (next router).

Notice that the original Ethernet frame no longer exists. <mark style="background:#fff88f">Routers do not forward Ethernet frames</mark>, they remove the original ones and build entirely new ones.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Forwarding data|Definitions]]

#### Routing Decisions

Every router maintains a routing table. The routing table tells the router where packets should be sent. Conceptually :

```
Destination
↓
Routing Table
↓
Outgoing Interface
↓
Next Router
```

Each router repeats this process independently. The router does not know the complete Internet path. It only knows the <mark style="background:#fff88f">best</mark> next hop.

#### Rebuilding the Ethernet Frame

After selecting the outgoing interface, the router <u>constructs an entirely new Ethernet frame.</u> This new frame contains different MAC addresses.

For example :
Original frame :

```
Source MAC
↓
PC
```

```
Destination MAC
↓
Router
```

New frame:

```
Source MAC
↓
Router
```

```
Destination MAC
↓
next router
```

The IP packet inside the frame remains almost unchanged. Only a few fields, such as the Time To Live (TTL), are modified.

### Hop-by-Hop Communication

Communication across the Internet occurs one hop at a time, a hop is simply a router. Each router performs the same sequence :

```
Receive Frame
↓
Remove Ethernet Header
↓
Read IP Packet
↓
Routing Decision
↓
Create New Ethernet Frame
↓
Forward
```

This process repeats until the destination network is reached. The Internet is therefore not a single direct connection. Instead, it consists of thousands of independent routers forwarding packets one hop at a time, here is a diagram that clears things up for you :

![[Pasted image 20260718235613.png]]

> This diagram is built using what you now know, but the internet and networks in general are very complex, you're learning networking layer by layer and piece by piece, until you have the big picture.

### Key Concepts Introduced

This section introduced several concepts that become major topics later in the course :

| Concept | Future Module |
|----------|---------------|
| Default Gateway | Routing |
| ARP | Ethernet & Switching |
| MAC Address | Ethernet |
| Ethernet Frames | Layer 2 |
| IP Packets | Layer 3 |
| Routing Tables | Routing |
| Encapsulation | Protocol Stack |

Each concept will later be explored in detail, including packet structures, protocol operation, configuration, and troubleshooting.

### Key Takeaways

- DNS provides the destination IP address but does not transport data.
- Computers determine whether a destination is local or remote.
- Remote destinations are reached through the default gateway.
- ARP resolves local IP addresses into MAC addresses.
- Ethernet frames require MAC addresses, not IP addresses.
- IP packets remain inside Ethernet frames during transmission.
- Switches forward frames using MAC addresses.
- Routers forward packets using IP addresses.
- Routers remove and rebuild Ethernet frames at every hop.
- Internet communication is a hop-by-hop forwarding process rather than a direct connection.

### Preview

Part 3 begins when the packet finally reaches Google's server. The following topics will be covered:

- TCP three-way handshake
- Establishing a reliable connection
- TLS handshake and encryption
- HTTP requests
- HTTP responses
- Downloading webpage resources
- Browser rendering
- Displaying the final webpage

## Lesson 1.1 — The Journey of a Web Request (Part 3)

### Arrival at the Destination Network

After traversing multiple routers across the Internet, the packet eventually reaches the network that contains Google's servers.

The final router performs the same forwarding process as every previous router. It examines the destination IP address, consults its routing table, determines that the destination is directly connected, constructs a new Ethernet frame, and forwards the packet to the destination server.

At this point, the packet has successfully completed its journey across the Internet. However, communication between the client and the server has not yet begun.

The server has received a packet, but before any webpage can be exchanged, both devices must establish a <u>reliable communication channel.</u>

### Why a Connection Is Needed

Sending a webpage is different from broadcasting a radio signal. The browser expects :
- Every packet to arrive.
- Packets to arrive in the correct order.
- Missing packets to be retransmitted.
- Corrupted packets to be discarded.
- Communication to continue until the transfer is complete.

The Internet itself provides none of these guarantees. *[[Internet protocol (IP)]]* simply delivers packets on a best-effort basis. To provide reliability, another protocol is required; That protocol is the **[[Transmission Control Protocol (TCP)]].**

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Internet protocol (IP)|Definitions]]

### Transmission Control Protocol (TCP)

TCP is a *connection-oriented* transport protocol operating at Layer 4 of the OSI model.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Connection-oriented|Definitions]]

Its primary responsibilities include :
- Establishing connections
- Reliable delivery
- Packet sequencing
- Flow control
- Error detection
- Retransmission of lost segments
- Connection termination

Most web applications use TCP because reliability is more important than speed.

### Before Any Data Is Sent

Neither the client nor the server immediately begins transmitting webpage data. Instead, they first establish a TCP connection. This process is known as the **TCP Three-Way Handshake**.

### The TCP Three-Way Handshake

The handshake establishes communication between two devices. It synchronizes *sequence numbers* and confirms that both sides are ready to exchange data.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Sequence number|Definitions]]

The process consists of three packets :

```text
Client                     Server
  SYN  -------------------->
       <-------------------  SYN-ACK
  ACK  -------------------->
```

Only after these three packets have been exchanged can application data be transmitted.

#### Step 1 — SYN

The browser (the client) requests a new connection. The client sends a packet with the SYN (Synchronize) flag (a field in the TCP header) set.

Conceptually, the message says :

> "I would like to establish a TCP connection."

The packet contains an initial sequence number that identifies the first byte of data the client intends to send.

No webpage data is transmitted during this step.

#### Step 2 — SYN-ACK

The server receives the SYN packet. <mark style="background:#fff88f">If the requested service</mark> is available, the server responds with :
- SYN
- ACK

The SYN indicates that the server is also willing to establish a connection.

The ACK acknowledges receipt of the client's sequence number. Conceptually, the message becomes:

> "I received your request, and I am ready."

#### Step 3 — ACK

The client acknowledges the server's response. After this acknowledgement is received, both devices consider the TCP session established.

Communication can now begin, here is a diagram for you to best visualize it :

![[Pasted image 20260719131124.png]]

### Why Three Packets?

A common question is why TCP requires three packets instead of one. The answer is reliability. Both devices must independently confirm :
- They can transmit.
- They can receive.
- They agree on sequence numbers.

Without this confirmation, neither side can safely begin transferring data.

### Port Numbers

TCP connections are identified using **port numbers**. An IP address identifies a computer. A port identifies a specific application running on that computer.

Example :

```
142.250.190.78
```

This identifies Google's server.

```
Port 443
```

This identifies the HTTPS service on that server.
Together they form :

```
142.250.190.78:443
```

Likewise, the client selects a <u>temporary source port</u>.

![[Cybersecurity journey/1. Networking/Q&A#❔ - How are the temporary port number allocated to the applications by the OS ?|Q&A]]

Example :

```
192.168.1.10:52318
```

A complete TCP connection therefore consists of :

```
Client IP
Client Port
↓
Server IP
Server Port
```

This combination uniquely identifies the communication session.

### Why HTTPS?

Most modern websites use HTTPS instead of HTTP.
HTTPS provides :
- Confidentiality
- Integrity
- Authentication

Without HTTPS :
- Passwords could be intercepted.
- Cookies could be stolen.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Cookie|Definitions]]

- Data could be modified in transit.
- Users could unknowingly communicate with fraudulent servers.

Before HTTP communication begins, encryption must first be established.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What's the difference between http and https for a beginner|Q&A]]

### The TLS Handshake

HTTPS uses *[[Transport Layer Security (TLS)]]*.

TLS establishes an encrypted communication channel before any webpage data is exchanged.

The handshake performs several tasks. It :
- Negotiates encryption algorithms
- Verifies the server's identity
- Exchanges encryption keys
- Creates secure session keys

Only after this process completes does encrypted communication begin.

### Server Certificate

One of the server's first responses during the TLS handshake is its [[Digital certificate]].

The certificate contains information including :
- Server name.
- Public key.
- Certificate issuer.
- Expiration date.
- Digital signature.

The browser verifies that the certificate :
- Matches the requested domain.
- Has not expired.
- Was issued by a trusted *Certificate Authority (CA)*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Certificate Authority (CA)|Definitions]]

If verification fails, the browser displays a security warning.

### Encryption Begins

After successful certificate validation, both client and server derive identical *session keys*.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Session key|Definitions]]

From this point onward :
- HTTP requests become encrypted.
- HTTP responses become encrypted.
- Cookies become encrypted.
- Authentication credentials become encrypted.

Anyone *intercepting* packets can still observe IP addresses and packet sizes, but the application data itself is unreadable.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Intercept|Terminology]]

### The HTTP Request

With TCP established and TLS completed, the browser finally sends the first HTTP request. A simplified request appears below :

```http
GET / HTTP/1.1
Host: www.google.com
User-Agent: Chrome
Accept: text/html
```

This request asks the server to send the homepage.
Although shown in plain text here, HTTPS encrypts this request before transmission.

### The Server Processes the Request

The web server receives the request. Several internal operations may occur, For example :
- Authentication (who are you)
- Authorization (what you're authorized to do after you're authenticated)
- Database queries
- Session validation
- Business logic execution
- Content generation

Dynamic websites often generate pages in real time rather than retrieving static HTML files.

The complexity of this processing depends entirely on the application.

### The HTTP Response

Once processing is complete, the server returns an HTTP response. Example :

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 12546
```

The response contains :
- Status code (what happened while processing your request ?)
- Response headers
- HTML document

Again, this information is encrypted by TLS before leaving the server.

### HTTP Status Codes

Servers use status codes to describe the result of a request. Some common examples include :

| Code | Meaning               |
| ---- | --------------------- |
| 200  | OK                    |
| 301  | Moved Permanently     |
| 302  | Temporary Redirect    |
| 304  | Not Modified          |
| 400  | Bad Request           |
| 401  | Unauthorized          |
| 403  | Forbidden             |
| 404  | Not Found             |
| 500  | Internal Server Error |
| 503  | Service Unavailable   |

These codes become valuable during troubleshooting.

### Downloading the HTML

The browser receives the encrypted response.
TLS decrypts the data.
TCP reorders any out-of-sequence segments.
The browser now possesses the HTML document.
However, displaying the page immediately is impossible.
The HTML references many additional resources. Example:

```html
<link rel="stylesheet">
<script src="app.js">
<img src="logo.png">
```

Each resource requires additional HTTP requests, and the cycle restarts, in the same TCP connection.

### Additional Resource Requests

Modern webpages rarely consist of a single file. Instead, browsers request :
- HTML
- CSS
- JavaScript
- Fonts
- Images
- Videos
- Icons

Each resource may generate another HTTP request. Large websites routinely require dozens or even hundreds of requests before rendering is complete.

### Browser Rendering

Once sufficient resources have been downloaded, the browser begins *rendering* the page.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Page rendering|Definitions]]

The rendering pipeline generally includes :
1. HTML parsing.
2. DOM construction.
3. CSS parsing.
4. CSSOM construction.
5. JavaScript execution.
6. Layout calculation.
7. Painting.
8. Compositing.
9. Displaying pixels on the screen.

Although this occurs extremely quickly, many independent software components participate.

### The User Sees the Webpage

Only after all previous stages have completed does the webpage appear. The complete journey has involved :
- Browser processing
- DNS
- Routing
- Ethernet
- ARP
- TCP
- TLS
- HTTP
- Browser rendering

A single page load may involve hundreds of packets exchanged over multiple network connections.

### Complete Journey Overview

```text
User Types URL
        │
Browser Processes URL
        │
Browser Cache
        │
DNS Cache
        │
DNS Resolution
        │
Destination IP Obtained
        │
Default Gateway Selected
        │
ARP Resolution
        │
Ethernet Frame Created
        │
Switch
        │
Router
        │
Internet
        │
Destination Router
        │
Destination Server
        │
TCP Handshake
        │
TLS Handshake
        │
HTTP Request
        │
HTTP Response
        │
Browser Rendering
        │
Webpage Displayed
```

Here is a diagram to visualize it better : 

![[Pasted image 20260719153534.png]]

### Why This Lesson Matters

Every major topic covered throughout this course fits somewhere within this communication process. Later modules examine each stage individually. For example :

| Lesson | Stage of the Journey |
|---------|----------------------|
| Ethernet | Frame transmission |
| Switching | Local forwarding |
| ARP | IP-to-MAC resolution |
| IP Addressing | Packet delivery |
| Routing | Internet forwarding |
| TCP | Reliable transport |
| DNS | Name resolution |
| HTTP | Application communication |
| Security | TLS, firewalls, VPNs |

By understanding the overall communication flow first, each future lesson becomes part of a larger, *coherent* picture rather than an isolated topic.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Coherent|Terminology]]

### Key Takeaways

- TCP establishes a reliable communication channel before data transfer.
- The TCP Three-Way Handshake synchronizes both endpoints.
- Port numbers identify individual services running on hosts.
- HTTPS relies on TLS to provide confidentiality, integrity, and authentication.
- Digital certificates verify the identity of remote servers.
- HTTP requests are transmitted only after encryption has been established.
- Modern webpages consist of many independent resources requiring multiple HTTP requests.
- Browser rendering transforms downloaded resources into the visual webpage presented to the user.
- A simple webpage request involves <u>cooperation between multiple protocols operating across several layers of the TCP/IP stack</u>.

## Lesson 1.1 — The Journey of a Web Request (Part 4)

### Verifying the Journey with Wireshark

The communication process described throughout this lesson can be observed directly using Wireshark. Rather than treating networking protocols as abstract concepts, packet analysis allows engineers to verify exactly what occurs on the network.

Capturing packets while loading a website reveals nearly every protocol involved in the communication process.

A typical capture may include:

- ARP
- DNS
- TCP
- TLS
- HTTP or HTTPS
- ICMP (occasionally)

Observing these packets provides insight into protocol behavior and greatly simplifies troubleshooting.

---

# Preparing a Packet Capture

Before capturing traffic, ensure that:

- Wireshark is installed.
- The correct network interface is selected.
- Internet connectivity is available.

Start packet capture before opening the browser.

Once capture has started:

1. Open a browser.
2. Navigate to a website that has not recently been visited.
3. Wait until the page fully loads.
4. Stop packet capture.

The capture now contains every packet exchanged during the communication process.

---

# Identifying ARP Packets

If the ARP cache is empty, one of the first packets visible should be an ARP request.

Example:

```
Who has 192.168.1.1?

Tell 192.168.1.10
```

The router replies:

```
192.168.1.1

is at

AA:BB:CC:DD:EE:FF
```

This confirms that the operating system successfully resolved the router's MAC address before transmitting IP packets.

If no ARP packets appear, the MAC address was likely already present in the ARP cache.

---

# Identifying DNS Packets

Next, locate DNS packets.

Apply the following display filter.

```
dns
```

A typical sequence appears as:

```
Standard Query

↓

Standard Response
```

The query asks:

```
google.com
```

The response contains:

```
142.250.x.x
```

The DNS exchange confirms that the browser successfully translated the hostname into an IP address.

---

# Identifying TCP Packets

Apply the filter:

```
tcp
```

Near the beginning of the connection, the following sequence should appear.

```
SYN

↓

SYN, ACK

↓

ACK
```

This is the TCP Three-Way Handshake discussed earlier.

Selecting each packet reveals:

- Source Port
- Destination Port
- Sequence Number
- Acknowledgement Number
- TCP Flags
- Window Size

These fields become increasingly important during later troubleshooting exercises.

---

# Identifying TLS Packets

Because nearly every modern website uses HTTPS, TLS packets should appear immediately after the TCP handshake.

Filter:

```
tls
```

Common packets include:

- Client Hello
- Server Hello
- Certificate
- Change Cipher Spec
- Application Data

The certificate packet contains information identifying the remote server.

Later security modules examine these packets in greater detail.

---

# Identifying HTTP Traffic

If HTTPS is disabled or a non-encrypted website is used, HTTP packets become visible.

Filter:

```
http
```

Example request:

```
GET /

Host: example.com
```

Example response:

```
HTTP/1.1 200 OK
```

With HTTPS, the HTTP payload is encrypted and no longer readable.

This demonstrates one of the primary benefits of TLS.

---

# Following a TCP Stream

One of Wireshark's most useful features is **Follow TCP Stream**.

Right-click any TCP packet.

Select:

```
Follow

↓

TCP Stream
```

Wireshark reconstructs the entire application conversation.

For unencrypted HTTP traffic, this reveals:

- Complete requests.
- Complete responses.
- HTML pages.
- Cookies.
- Headers.

When HTTPS is used, only encrypted data is visible.

---

# Timeline of a Typical Web Request

Although exact timings differ depending on network conditions, the sequence generally follows this order.

```text
User Types URL
        │
        ▼
Browser Checks Cache
        │
        ▼
DNS Lookup
        │
        ▼
ARP Request
        │
        ▼
TCP Handshake
        │
        ▼
TLS Handshake
        │
        ▼
HTTP Request
        │
        ▼
HTTP Response
        │
        ▼
Additional Requests
        │
        ▼
Browser Rendering
```

This timeline represents the communication flow examined throughout this lesson.

---

# Common Failure Points

A webpage may fail to load for many reasons.

Understanding where communication stops greatly simplifies troubleshooting.

---

## Failure 1 — DNS Resolution

Symptoms:

- Browser reports server cannot be found.
- IP connectivity exists.
- Websites cannot be accessed by name.

Possible causes:

- DNS server unavailable.
- Incorrect DNS configuration.
- Firewall blocking DNS.

Verification:

```
nslookup google.com
```

---

## Failure 2 — ARP

Symptoms:

- Unable to communicate with the default gateway.
- Local devices unreachable.

Possible causes:

- Duplicate IP addresses.
- Incorrect subnet mask.
- ARP cache corruption.
- Switch issues.

Verification:

Windows:

```
arp -a
```

Linux:

```
ip neigh
```

---

## Failure 3 — Routing

Symptoms:

- Local communication works.
- Internet inaccessible.

Possible causes:

- Missing default gateway.
- Incorrect routing table.
- ISP outage.

Verification:

```
traceroute

or

tracert
```

---

## Failure 4 — TCP

Symptoms:

- Packets arrive.
- Connection never establishes.

Possible causes:

- Firewall blocking TCP.
- Closed server port.
- Server unavailable.

Verification:

Wireshark:

```
SYN

↓

No SYN-ACK
```

---

## Failure 5 — TLS

Symptoms:

- Browser security warnings.
- HTTPS connection refused.

Possible causes:

- Expired certificate.
- Invalid certificate.
- Incorrect server configuration.

Verification:

Inspect browser certificate information.

---

## Failure 6 — HTTP

Symptoms:

- Connection established.
- Server responds with errors.

Examples:

```
404 Not Found
```

```
500 Internal Server Error
```

```
403 Forbidden
```

The network functions correctly.

The problem exists within the web application.

---

# Layer-by-Layer Troubleshooting

Professional network engineers troubleshoot from the bottom upward.

```text
Layer 1

↓

Layer 2

↓

Layer 3

↓

Layer 4

↓

Layer 7
```

Questions asked at each layer include:

**Layer 1**

Is the cable connected?

Is Wi-Fi connected?

Are interfaces up?

---

**Layer 2**

Can ARP resolve?

Are switches forwarding frames?

Is the correct VLAN configured?

---

**Layer 3**

Can the destination IP be reached?

Is the routing table correct?

Does the default gateway respond?

---

**Layer 4**

Does TCP establish?

Are ports open?

Is a firewall blocking traffic?

---

**Layer 7**

Does the application respond?

Are HTTP requests successful?

Is authentication required?

This structured methodology prevents random guessing during troubleshooting.

---

# Security Considerations

Every stage of communication introduces potential security risks.

Understanding these risks is essential for secure network design.

| Stage   | Possible Attack     |
| ------- | ------------------- |
| DNS     | DNS Spoofing        |
| ARP     | ARP Poisoning       |
| TCP     | SYN Flood           |
| HTTP    | Session Hijacking   |
| TLS     | Certificate Forgery |
| Routing | Route Hijacking     |

Future modules explore these attacks together with the defensive technologies used to mitigate them.

---

# Practical Laboratory

## Objective

Observe the complete communication process while accessing a website.

---

## Requirements

- Internet connection
- Wireshark
- Modern web browser

---

## Procedure

1. Open Wireshark.
2. Select the active network interface.
3. Start packet capture.
4. Clear the browser cache (optional).
5. Visit a website.
6. Wait until loading completes.
7. Stop packet capture.

---

## Tasks

Identify:

- ARP Request
- DNS Query
- DNS Response
- TCP Handshake
- TLS Handshake
- HTTP Request (if visible)
- HTTP Response (if visible)

Record:

- Source IP
- Destination IP
- Source Port
- Destination Port

---

## Challenge

Repeat the experiment.

Visit the same website again.

Compare both captures.

Questions:

- Did another DNS query occur?
- Was another ARP request required?
- Did TCP establish again?
- Were fewer packets transmitted?

Explain the differences using browser, DNS, and ARP caching.

---

# Summary

Typing a web address into a browser initiates a complex sequence of events involving numerous protocols and networking devices.

The browser first interprets the URL and checks local caches. If necessary, DNS translates the hostname into an IP address. ARP resolves the MAC address of the default gateway, allowing an Ethernet frame to be constructed and transmitted across the local network. Routers forward the IP packet across the Internet until it reaches the destination server.

Once the packet arrives, TCP establishes a reliable connection, TLS negotiates encryption, and HTTP exchanges the requested resources. The browser downloads additional content, processes HTML, CSS, and JavaScript, and finally renders the webpage visible to the user.

Every networking technology covered throughout the remainder of this course represents one component of this communication process. Understanding the complete journey provides the context necessary for studying individual protocols in greater depth.

---

# Key Terms

| Term | Definition |
|------|------------|
| URL | Uniform Resource Locator identifying a web resource |
| DNS | Resolves domain names into IP addresses |
| ARP | Resolves IP addresses into MAC addresses |
| MAC Address | Physical Layer 2 hardware address |
| IP Address | Logical Layer 3 network address |
| Default Gateway | Router used to reach remote networks |
| Ethernet Frame | Layer 2 data unit |
| IP Packet | Layer 3 data unit |
| TCP Segment | Layer 4 data unit |
| TLS | Encryption protocol used by HTTPS |
| HTTP | Protocol used to transfer web resources |
| Browser Cache | Locally stored web resources |
| DNS Cache | Locally stored DNS records |

---

# Looking Ahead

The next lesson begins with the physical foundation of networking. Before packets, IP addresses, or routing can exist, data must travel across a physical medium. The following module examines how information is represented electrically, optically, and wirelessly, introducing signals, transmission media, bandwidth, latency, and the devices that form the physical layer of modern computer networks.