## Module 1 — How the Internet Actually Works

## Lesson 1.4 — Packets, Frames, and Data Flow (Part 1)

### Introduction

Every second, the Internet transports billions of pieces of information between computers, smartphones, servers, routers, and countless other devices. A single webpage may require hundreds of individual requests, while a video stream may consist of millions of packets transmitted over several minutes.

Despite this enormous scale, the Internet does not establish a dedicated physical connection between every pair of communicating devices. Instead, it divides data into small units called **packets**, which are forwarded independently across a shared network.

This design is one of the fundamental reasons why the Internet is scalable, fault-tolerant, and capable of supporting billions of simultaneous users.

In this lesson, we begin by examining two fundamentally different approaches to network communication: **circuit switching** and **packet switching**.

Understanding why the Internet adopted packet switching instead of circuit switching explains many behaviors observed in modern networks, including routing, congestion, latency, and bandwidth sharing.

### Learning Objectives

After completing this lesson, students should be able to:

- Explain the difference between circuit switching and packet switching.
- Describe why the Internet uses packet switching.
- Understand statistical multiplexing.
- Explain why packet switching scales better than dedicated circuits.
- Describe how packets travel independently through a network.

### Two Ways to Build a Network

Historically, communication networks have been built using one of two approaches:

- Circuit Switching
- Packet Switching

Although both methods allow information to travel between two endpoints, they operate very differently.

Understanding these differences helps explain why the Internet functions the way it does today.

### Circuit Switching

Circuit switching establishes a **dedicated communication path** between two devices before any information is transmitted.

Once established, that path remains reserved exclusively for the duration of the communication session.

No other user may utilize the reserved resources, even if they are temporarily idle.

Traditional telephone networks are the classic example of circuit switching.

Consider two telephones connected through several telephone exchanges.

```text
Telephone A

↓

Exchange 1

↓

Exchange 2

↓

Exchange 3

↓

Telephone B
```

Before either person can speak, the telephone network reserves a complete end-to-end path.

Once the circuit is established:

- Bandwidth is reserved.
- Intermediate switches dedicate resources.
- Communication proceeds continuously.
- Other users cannot use that reserved capacity.

The circuit remains allocated until one party hangs up.

### Characteristics of Circuit Switching

Circuit-switched networks possess several important characteristics.

#### Dedicated Resources

Every conversation reserves its own communication path.

Even during periods of silence, the reserved bandwidth cannot be shared with other users.

For example:

```text
Call Established

Reserved Capacity

████████████████
```

Whether the users are speaking or remaining silent, the network resources remain occupied.

#### Predictable Performance

Because resources are reserved, communication quality remains relatively stable.

Latency is generally consistent.

Bandwidth remains available throughout the connection.

This predictability made circuit switching well suited for early voice communication.

#### Limited Capacity

The greatest disadvantage of circuit switching is inefficiency.

Suppose a network contains enough capacity for 100 simultaneous calls.

If all 100 circuits are reserved, a new caller receives a busy signal.

Even if many existing callers are completely silent, their reserved bandwidth cannot be reassigned.

The network therefore wastes considerable resources.

### The Problem with Circuit Switching

Human communication is naturally bursty.

During a conversation:

- One person speaks.
- The other listens.
- Both remain silent briefly.
- The conversation resumes.

The communication channel is not used continuously.

Data communication behaves similarly.

When loading a webpage, the browser sends a request.

It then waits for the server.

The server responds.

The browser processes the response.

Periods of activity alternate with periods of inactivity.

If every Internet connection required a dedicated circuit, enormous amounts of bandwidth would remain unused most of the time.

### Packet Switching

Packet switching takes a fundamentally different approach.

Instead of reserving an entire communication path, data is divided into small pieces called **packets**.

Each packet is transmitted independently across a shared network.

Multiple users simultaneously share the same communication links.

No permanent circuit exists.

A packet-switched network appears conceptually as follows.

```text
        Router

      /    |    \

User A   User B   User C

      \    |    /

        Internet
```

Rather than dedicating one path per user, every communication link carries packets belonging to many different users.

### What Is a Packet?

A packet is a small unit of transmitted data.

Each packet contains:

- Control information.
- Source address.
- Destination address.
- Payload.

Conceptually:

```text
+-----------------------+
| Header                |
+-----------------------+
| Payload               |
+-----------------------+
```

The header tells the network where the packet should travel.

The payload contains the actual user data.

Large files are divided into many packets before transmission.

The receiving device later reconstructs the original data.

### An Example

Suppose a user downloads a 20 MB file.

The file is not transmitted as one enormous block.

Instead, it is divided into thousands of smaller packets.

```text
File

↓

Packet 1

Packet 2

Packet 3

Packet 4

...

Packet N
```

Each packet travels independently through the network.

Some packets may even take different routes before arriving at the destination.

The receiving computer reassembles the packets into the original file.

### Sharing the Network

One of packet switching's greatest advantages is resource sharing.

Consider three users accessing the Internet simultaneously.

```text
User A

User B

User C

↓

Shared Router

↓

Internet
```

Rather than allocating dedicated bandwidth to each user, packets from all three users are transmitted whenever capacity becomes available.

The communication link is therefore utilized much more efficiently.

### Statistical Multiplexing

Packet switching relies on a principle known as **statistical multiplexing**.

Instead of permanently reserving bandwidth, the network dynamically shares available capacity among active users.

Suppose ten users each possess a 10 Mbps connection.

It is highly unlikely that every user will continuously transmit data at the maximum rate.

Most users alternate between:

- Reading.
- Typing.
- Waiting.
- Downloading.
- Watching videos.
- Remaining idle.

Packet switching exploits this behavior.

Whenever one user becomes inactive, another user's packets utilize the available bandwidth.

This dynamic allocation dramatically improves overall network efficiency.

### An Everyday Analogy

Imagine a supermarket with dedicated checkout lanes.

Each customer permanently reserves one cashier.

Even if the customer leaves temporarily to collect another item, the cashier remains unavailable.

This resembles circuit switching.

Now imagine a single queue.

Whenever a cashier becomes available, the next customer proceeds.

No cashier remains idle while customers are waiting.

This resembles packet switching.

The second approach generally serves more customers with fewer resources.

Packet-switched networks operate according to the same principle.

### Why Statistical Multiplexing Works

Computer traffic is extremely unpredictable.

A user reading a webpage generates almost no network traffic.

Moments later, they begin downloading a file.

Another user finishes a video stream and becomes idle.

A third user starts a video conference.

Because usage constantly changes, permanently reserving bandwidth would waste enormous capacity.

Packet switching allows bandwidth to be allocated only when it is actually needed.

This significantly increases network utilization.

### Comparing Circuit Switching and Packet Switching

The differences between the two approaches can be summarized as follows.

| Feature | Circuit Switching | Packet Switching |
|---------|-------------------|------------------|
| Communication Path | Dedicated | Shared |
| Resource Reservation | Required | Not Required |
| Bandwidth Utilization | Fixed | Dynamic |
| Scalability | Limited | Excellent |
| Fault Tolerance | Lower | Higher |
| Efficiency | Lower | Higher |
| Typical Use | Traditional Telephone Networks | Internet |

Circuit switching emphasizes predictable performance through dedicated resources.

Packet switching emphasizes efficient resource sharing and scalability.

The rapid growth of the Internet would not have been possible using circuit-switched networking.

### Why the Internet Uses Packet Switching

The Internet connects billions of devices that communicate unpredictably.

Users continuously start and stop downloads, browse websites, stream media, send emails, and participate in video conferences.

If every communication session required a dedicated circuit, the Internet would require vastly more physical infrastructure than exists today.

Packet switching allows communication links to be shared among millions of simultaneous users.

Only devices actively transmitting data consume bandwidth.

Idle devices consume almost none.

This makes packet switching dramatically more efficient for general-purpose data communication.

### Key Takeaways

- Circuit switching establishes a dedicated communication path before data transmission begins.
- Packet switching divides data into packets that share network resources dynamically.
- The Internet uses packet switching because it provides significantly better scalability and resource utilization.
- Statistical multiplexing allows many users to efficiently share the same communication links.
- Large files are divided into many packets, each of which may travel independently through the network.
- Packet switching accommodates the bursty nature of modern Internet traffic far more effectively than circuit switching.

### Preview

In the next part, we will examine how packets actually move through the Internet, distinguish between **packets** and **frames**, and follow data as routers remove and rebuild Link-layer frames at every hop while preserving the end-to-end IP packet.

## Lesson 1.4 — Packets, Frames, and Data Flow (Part 2)

### From the Application to the Network

In the previous lesson, data was shown moving through the protocol stack using encapsulation. An application generates data, the Transport layer creates a segment, the Network layer creates a packet, and the Link layer creates a frame before the Physical layer transmits the bits across the communication medium.

Understanding encapsulation is only the first step. Equally important is understanding **how those packets actually travel across the Internet**.

One common misconception is that the exact Ethernet frame created by the sender travels unchanged all the way to the destination.

This is not true.

Only the **IP packet** is intended to travel from the source host to the destination host. Ethernet frames exist only for communication across a **single physical link**.

This distinction is one of the most important concepts in networking.

### End-to-End Communication

Suppose a computer in Paris communicates with a web server in Tokyo.

The communication path might look like this:

```text
PC

↓

Home Router

↓

ISP Router

↓

Core Router

↓

International Gateway

↓

ISP Router

↓

Server
```

Although this appears to be one continuous journey, it is actually composed of many smaller communications.

Each communication occurs only between neighboring devices.

The computer sends a frame to the home router.

The home router sends another frame to the ISP router.

The ISP router builds another frame for the next router.

This process repeats until the packet finally reaches the destination.

### The Difference Between a Packet and a Frame

Students often use the terms **packet** and **frame** interchangeably.

In networking, they refer to different Protocol Data Units operating at different layers.

| Layer | Protocol Data Unit |
|--------|--------------------|
| Network | Packet |
| Link | Frame |

Although a frame contains a packet, they are not the same thing.

Think of the relationship like this:

```text
Ethernet Frame

┌──────────────────────────────┐
│ Ethernet Header              │
│                              │
│   IP Packet                  │
│   ┌──────────────────────┐   │
│   │ IP Header            │   │
│   │                      │   │
│   │ TCP Segment          │   │
│   └──────────────────────┘   │
│                              │
│ Ethernet Trailer             │
└──────────────────────────────┘
```

Each frame carries exactly one Network-layer packet.

### Packets Are End-to-End

The IP packet represents the logical communication between the sender and the receiver.

The source IP address remains the sender.

The destination IP address remains the receiver.

For example:

```text
Source IP

192.168.1.10

↓

Destination IP

93.184.216.34
```

These addresses identify the two endpoints of communication.

Every router examines the destination IP address to determine where the packet should be forwarded next.

The IP packet continues toward the destination until it finally arrives.

### Frames Are Hop-by-Hop

Unlike packets, Ethernet frames never travel across the entire Internet.

A frame exists only between two directly connected devices.

Consider the following network.

```text
PC

↓

Switch

↓

Router

↓

Router

↓

Server
```

The PC creates an Ethernet frame for the first router.

When the router receives the frame, it removes the Ethernet header and trailer.

The original frame is destroyed.

The router then creates a completely new Ethernet frame addressed to the next router.

This process repeats at every hop.

### Visualizing Frame Replacement

Suppose a packet travels through three routers.

The communication can be represented as follows.

```text
PC

Frame A

↓

Router 1

Frame B

↓

Router 2

Frame C

↓

Router 3

Frame D

↓

Server
```

Notice that the IP packet remains logically the same.

Only the surrounding Ethernet frame changes.

Each network segment has its own frame.

### Why Routers Replace Frames

Each physical network has different devices connected to it.

Every network interface possesses its own MAC address.

Suppose Router 1 forwards a packet to Router 2.

The destination MAC address cannot remain the web server's MAC address because the server is not directly connected.

Instead, Router 1 creates a frame addressed to Router 2.

For example:

First network:

```text
Destination MAC

Router 1
```

Second network:

```text
Destination MAC

Router 2
```

Third network:

```text
Destination MAC

Server
```

The destination MAC address changes at every hop.

The destination IP address does not.

### Encapsulation at Every Hop

Each router performs the same sequence of operations.

1. Receive a frame.
2. Verify the Frame Check Sequence (FCS).
3. Remove the Link-layer header and trailer.
4. Examine the IP packet.
5. Determine the next hop.
6. Build a new Ethernet frame.
7. Transmit the frame.

Conceptually:

```text
Receive Frame

↓

Remove Ethernet Header

↓

Read IP Packet

↓

Lookup Destination

↓

Build New Ethernet Frame

↓

Transmit
```

This process occurs millions of times every second across the Internet.

### The Router Does Not Modify the Application Data

An important observation is that routers do **not** inspect the application payload during normal forwarding.

For example, suppose a packet contains:

```http
GET /index.html HTTP/1.1
```

The router does not read:

- HTML
- CSS
- Images
- JavaScript
- HTTP requests

Instead, it focuses almost entirely on the Network layer.

Its primary responsibilities are:

- Reading the destination IP address.
- Selecting the next hop.
- Forwarding the packet.

This separation allows routers to forward enormous amounts of traffic efficiently.

### Hop-by-Hop Communication

The phrase **hop** refers to one movement of a packet between two neighboring Layer 3 devices.

Example:

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

The packet travels through four hops.

Each hop consists of a separate Link-layer communication.

Network engineers frequently use the term when troubleshooting.

For example:

> "The packet is lost after the third hop."

Utilities such as **traceroute** display each hop that a packet traverses on its path to the destination.

### Switching vs Routing

Another common source of confusion is the difference between switches and routers.

Although both forward traffic, they make forwarding decisions differently.

A switch examines:

- Destination MAC address.

A router examines:

- Destination IP address.

This distinction reflects the layers at which they primarily operate.

| Device | Primary Layer | Uses |
|----------|---------------|------|
| Switch | Link | MAC addresses |
| Router | Network | IP addresses |

Switches move frames within a local network.

Routers move packets between different networks.

### Example Journey

Consider a browser requesting a webpage.

```text
Browser

↓

TCP Segment

↓

IP Packet

↓

Ethernet Frame
```

The frame reaches the first router.

The router removes the Ethernet frame.

```text
Ethernet Removed

↓

IP Packet
```

The router determines the next destination.

It then creates a new frame.

```text
New Ethernet Frame

↓

IP Packet
```

The packet continues toward the destination.

This process repeats until the web server receives the packet.

### Why This Design Is Important

Separating packets from frames provides enormous flexibility.

The same IP packet can travel across many different technologies.

For example:

```text
Ethernet

↓

Fiber

↓

Wi-Fi

↓

MPLS

↓

Ethernet
```

Although each network uses different Link-layer technologies, the IP packet continues uninterrupted.

This layered architecture allows different networking technologies to coexist while maintaining global interoperability.

### Observing This in Wireshark

When capturing packets with Wireshark, the software displays the frame that exists on your local network interface.

For example:

```text
Frame

↓

Ethernet II

↓

Internet Protocol Version 4

↓

Transmission Control Protocol

↓

Hypertext Transfer Protocol
```

Notice that Wireshark begins with the Ethernet frame because that is what your computer actually receives.

If the packet is forwarded by a router, that router constructs a different Ethernet frame before sending it onward.

You only observe the frame present on your own network segment.

### Key Takeaways

- An IP packet provides end-to-end communication between the source and destination hosts.
- An Ethernet frame provides hop-by-hop communication between directly connected devices.
- Routers remove the incoming Link-layer frame and construct a new frame for the next network segment.
- The destination IP address usually remains unchanged throughout the journey, while the destination MAC address changes at every hop.
- Switches forward frames using MAC addresses, whereas routers forward packets using IP addresses.
- The separation between packets and frames allows IP traffic to traverse many different physical network technologies without modification.

### Preview

In the next part, we will examine **Maximum Transmission Unit (MTU)**, understand why Ethernet typically uses an MTU of **1500 bytes**, explore what happens when packets exceed this limit, and distinguish between **bandwidth**, **throughput**, and **goodput** when measuring network performance.

## Lesson 1.4 — Packets, Frames, and Data Flow (Part 3)

### Maximum Transmission Unit (MTU)

When an application sends data across a network, it cannot transmit an unlimited amount of information in a single packet. Every network technology defines a maximum amount of payload that can be carried in one frame.

This limit is called the **Maximum Transmission Unit (MTU)**.

The MTU specifies the maximum number of bytes that can be placed inside the payload of a Layer 2 frame.

For Ethernet, the standard MTU is:

```text
1500 bytes
```

This means an Ethernet frame can carry an IP packet whose maximum payload is 1500 bytes.

Anything larger must be handled differently.

### Why Does an MTU Exist?

Every physical networking technology has practical limitations.

Network devices must:

- Allocate memory for buffers.
- Process frames efficiently.
- Detect transmission errors.
- Share the communication medium fairly.

If frames were allowed to become arbitrarily large:

- One transmission could monopolize the communication medium.
- Error recovery would become expensive.
- Latency would increase.
- Memory requirements would grow significantly.

By limiting frame size, networks maintain predictable performance while reducing the cost of retransmitting damaged data.

### Ethernet's 1500-Byte MTU

The standard Ethernet frame contains several fields in addition to the payload.

A simplified Ethernet frame is shown below.

```text
+----------------------------------------+
| Ethernet Header (14 Bytes)             |
+----------------------------------------+
| IP Packet (Up to 1500 Bytes)           |
+----------------------------------------+
| Frame Check Sequence (4 Bytes)         |
+----------------------------------------+
```

Notice that the MTU applies only to the payload carried inside the Ethernet frame.

The Ethernet header and trailer are **not included** in the MTU value.

### Total Ethernet Frame Size

A typical Ethernet frame consists of:

| Component | Size |
|-----------|------|
| Ethernet Header | 14 Bytes |
| Payload (MTU) | 1500 Bytes |
| Frame Check Sequence | 4 Bytes |

Total frame size:

```text
14 + 1500 + 4 = 1518 Bytes
```

Additional technologies such as VLAN tagging increase the total frame size slightly by inserting extra fields.

### MTU at Different Layers

The MTU is a **Layer 2 property**.

Different network technologies define different MTU values.

Examples include:

| Technology | Typical MTU |
|------------|------------:|
| Ethernet | 1500 Bytes |
| PPPoE | 1492 Bytes |
| IPv6 Minimum MTU | 1280 Bytes |
| Jumbo Ethernet Frames | 9000 Bytes (Typical) |

Whenever packets move between different network technologies, the smallest MTU along the path becomes the limiting factor.

### Sending a Large File

Suppose an application wants to transmit a file containing:

```text
5000 Bytes
```

Since Ethernet allows only:

```text
1500 Bytes
```

per packet payload, the operating system divides the data into multiple packets.

Conceptually:

```text
5000 Bytes

↓

Packet 1 (1500)

Packet 2 (1500)

Packet 3 (1500)

Packet 4 (500)
```

Each packet is transmitted independently across the network.

The receiving device later reconstructs the original file using information provided by the Transport layer.

### What Happens When a Packet Is Too Large?

Suppose a host creates an IP packet larger than the MTU supported by the outgoing network.

Two different outcomes are possible.

#### Fragmentation

In IPv4, routers may divide an oversized packet into multiple smaller packets.

This process is called **fragmentation**.

Example:

```text
Original Packet

3000 Bytes

↓

Fragment 1

1500 Bytes

↓

Fragment 2

1500 Bytes
```

Each fragment receives its own IP header.

The receiving host reassembles the fragments into the original packet.

Although fragmentation allows communication to continue, it introduces several disadvantages.

### Problems with Fragmentation

Fragmentation increases network overhead.

Instead of forwarding one packet, the network forwards several.

Each fragment requires:

- Its own IP header.
- Separate forwarding.
- Separate buffering.
- Separate processing.

If even one fragment is lost, the receiving host cannot reconstruct the original packet.

This often requires retransmission of the entire Transport-layer segment.

Because fragmentation reduces efficiency, modern networks attempt to avoid it whenever possible.

### Path MTU Discovery

Instead of allowing routers to fragment packets repeatedly, modern operating systems attempt to determine the largest packet size that can traverse the complete path.

This process is called **Path MTU Discovery (PMTUD)**.

The sender initially assumes a large packet size.

If a router encounters a packet that exceeds the next network's MTU and fragmentation is not permitted, it returns an ICMP error indicating that the packet is too large.

The sender then reduces its packet size.

Eventually, the sender discovers the maximum packet size supported along the entire path.

Example:

```text
Host

↓

1500 Bytes

↓

Router

↓

MTU Too Large

↓

ICMP Error

↓

Host

↓

1400 Bytes

↓

Successful Delivery
```

This allows communication without fragmentation.

### IPv4 vs IPv6 Fragmentation

IPv4 and IPv6 handle fragmentation differently.

IPv4 allows routers to fragment packets during forwarding.

IPv6 does **not**.

If an IPv6 packet is too large for a network, the router discards it and informs the sender.

The sender must reduce the packet size before retransmission.

This design simplifies routers and improves forwarding performance.

### Jumbo Frames

Some high-speed enterprise and data center networks use **Jumbo Frames**.

Instead of the standard:

```text
1500 Bytes
```

they may support MTUs around:

```text
9000 Bytes
```

Larger frames reduce protocol overhead because more application data is carried within each frame.

Advantages include:

- Fewer packets.
- Lower CPU utilization.
- Reduced protocol overhead.
- Improved throughput for large data transfers.

However, every device along the communication path must support the larger MTU.

Otherwise, communication problems occur.

### Bandwidth

Another commonly misunderstood concept is **bandwidth**.

Bandwidth describes the **maximum theoretical capacity** of a communication link.

It is usually measured in:

- bits per second (bps)
- kilobits per second (Kbps)
- megabits per second (Mbps)
- gigabits per second (Gbps)

Examples:

| Link | Bandwidth |
|------|----------:|
| Fast Ethernet | 100 Mbps |
| Gigabit Ethernet | 1 Gbps |
| Fiber Connection | 10 Gbps |

Bandwidth represents the largest amount of data that could be transmitted under ideal conditions.

It does **not** describe the amount of data actually delivered.

### Water Pipe Analogy

Bandwidth is often compared to the diameter of a water pipe.

Imagine two pipes.

```text
Small Pipe

|

|

|

Large Pipe

||||||
||||||
||||||
```

The larger pipe can transport more water each second.

Similarly, a higher-bandwidth network link can carry more data each second.

However, the actual amount of water flowing depends on many additional factors.

The same principle applies to computer networks.

### Throughput

**Throughput** is the actual amount of useful data successfully transmitted across the network during a given period.

Unlike bandwidth, throughput reflects real-world conditions.

Factors affecting throughput include:

- Congestion.
- Packet loss.
- Network latency.
- Processing delays.
- Retransmissions.
- Protocol overhead.

For example:

A network may have:

```text
Bandwidth

1 Gbps
```

but achieve only:

```text
Throughput

820 Mbps
```

The remaining capacity may be consumed by overhead, congestion, or retransmissions.

### Goodput

An even more precise measurement is **goodput**.

Goodput measures only the useful application data delivered to the receiving application.

It excludes:

- Protocol headers.
- Retransmissions.
- Duplicate packets.
- Control messages.

Relationship:

```text
Bandwidth

↓

Throughput

↓

Goodput
```

Each value is less than or equal to the one above it.

### Comparing the Three Measurements

| Measurement | Includes Overhead | Represents |
|-------------|------------------|------------|
| Bandwidth | No | Maximum theoretical capacity |
| Throughput | Yes | Actual delivered traffic |
| Goodput | No | Useful application data received |

These three terms are often confused, but they describe different aspects of network performance.

### Why Throughput Is Usually Lower

Several factors prevent throughput from reaching the theoretical bandwidth.

Examples include:

- Ethernet headers.
- IP headers.
- TCP headers.
- Encryption overhead.
- Packet retransmissions.
- Congestion.
- Flow control.
- Processing limitations.

Even on an ideal Gigabit Ethernet network, actual throughput is typically somewhat below 1 Gbps.

This difference is completely normal.

### Practical Example

Suppose you purchase:

```text
Internet Connection

500 Mbps
```

Running a speed test reports:

```text
470 Mbps
```

This does **not** necessarily indicate a problem.

The difference is usually explained by:

- Protocol overhead.
- Network conditions.
- ISP infrastructure.
- Measurement methodology.

Only significant deviations below expected performance suggest congestion or network faults.

### Key Takeaways

- The MTU defines the maximum Layer 2 payload that can be transmitted in a single frame.
- Standard Ethernet uses an MTU of **1500 bytes**.
- Large data transfers are divided into multiple packets before transmission.
- IPv4 supports fragmentation, while IPv6 requires the sender to reduce packet size instead of allowing routers to fragment packets.
- Path MTU Discovery determines the largest packet size that can successfully traverse a network path without fragmentation.
- Jumbo Frames increase efficiency on high-speed networks but require end-to-end support.
- **Bandwidth** is the theoretical capacity of a communication link.
- **Throughput** is the actual rate of successful data transfer.
- **Goodput** measures only the useful application data delivered to the receiving application.

### Preview

In the final part of this lesson, you will capture real traffic using **Wireshark**, observe packets and frames directly, identify every layer involved in an HTTP request to **neverssl.com**, and reinforce the concepts of encapsulation, MTU, and protocol layering through practical analysis.

## Lesson 1.4 — Packets, Frames, and Data Flow (Part 4)

### Observing Data Flow in the Real World

Throughout this lesson, we have discussed packets, frames, encapsulation, MTU, bandwidth, and throughput from a theoretical perspective.

However, networking becomes much easier to understand when you can observe these concepts directly.

One of the most valuable tools available to network engineers is **Wireshark**.

Wireshark allows you to capture and inspect packets moving through a network interface in real time.

Instead of imagining protocol headers and packet structures, you can see exactly what your computer is transmitting and receiving.

In this lab, we will capture an HTTP request to **neverssl.com** and identify every layer involved in the communication process.

### Why Use neverssl.com?

Most modern websites use HTTPS.

HTTPS adds an additional encryption layer (TLS), which can obscure some of the underlying application-layer communication.

The website:

```text
http://neverssl.com
```

intentionally uses plain HTTP.

This makes it ideal for learning because Wireshark can display the HTTP request and response directly.

Students can observe:

- Ethernet frames
- IP packets
- TCP segments
- HTTP requests
- HTTP responses

without dealing with encryption.

### Lab Objectives

After completing this exercise, students should be able to:

- Capture network traffic using Wireshark.
- Identify all five layers of the Internet model.
- Distinguish between packets and frames.
- Locate source and destination addresses.
- Identify source and destination ports.
- Recognize encapsulation in a real packet.
- Follow an HTTP request from the browser to the server.

### Lab Requirements

Software:

- Wireshark

Hardware:

- Internet-connected computer

Knowledge:

- Basic understanding of the protocol stack
- Familiarity with IP addresses and ports

### Step 1 — Launch Wireshark

Open Wireshark.

You should see a list of available network interfaces.

Examples:

```text
Ethernet

Wi-Fi

VPN Adapter

Virtual Interface
```

Select the interface currently connected to the Internet.

If unsure, observe which interface displays active traffic.

### Step 2 — Start Capturing

Double-click the active network interface.

Wireshark immediately begins collecting packets.

You will likely see thousands of packets appearing rapidly.

This is normal.

Modern operating systems constantly exchange network traffic.

### Step 3 — Generate Traffic

Open a web browser.

Navigate to:

```text
http://neverssl.com
```

Wait until the page loads completely.

Return to Wireshark.

Stop the capture using:

```text
Capture → Stop
```

or the red square button.

You now possess a packet capture containing the HTTP session.

### Step 4 — Filter the Traffic

The capture may contain hundreds or thousands of packets.

To focus on HTTP traffic, enter the following display filter:

```text
http
```

Press Enter.

Only HTTP packets should remain visible.

You should see entries similar to:

```text
GET /

HTTP/1.1 200 OK
```

These packets represent the browser requesting a webpage and the server responding.

### Step 5 — Select an HTTP Packet

Click an HTTP GET request.

The lower portion of Wireshark displays a hierarchical protocol tree.

It may resemble:

```text
Frame

Ethernet II

Internet Protocol Version 4

Transmission Control Protocol

Hypertext Transfer Protocol
```

This hierarchy directly represents protocol encapsulation.

### Identifying the Five Layers

The packet can now be mapped to the Internet protocol stack.

| Internet Layer | Wireshark Protocol |
|---------------|-------------------|
| Application | HTTP |
| Transport | TCP |
| Network | IPv4 |
| Link | Ethernet |
| Physical | Electrical Signals / Radio Waves |

Remember that Wireshark cannot display the Physical layer directly.

The Physical layer exists outside the captured packet.

### Layer 1 — Physical

The Physical layer converts bits into signals.

Examples include:

- Copper Ethernet signals
- Fiber optic light pulses
- Wi-Fi radio transmissions

Although Wireshark does not display these signals directly, every captured packet originated as physical-layer transmissions.

### Layer 2 — Link Layer

Expand:

```text
Ethernet II
```

You should see fields similar to:

```text
Destination MAC

Source MAC

Type
```

Example:

```text
Destination:
20:47:47:11:82:5A

Source:
A8:5E:45:3B:91:7C
```

These addresses identify devices on the local network segment.

Questions:

- Which device sent the frame?
- Which device received the frame?
- Is the destination your default gateway?

### Layer 3 — Network Layer

Expand:

```text
Internet Protocol Version 4
```

You should observe:

```text
Source Address

Destination Address

Time To Live

Protocol
```

Example:

```text
Source:
192.168.1.100

Destination:
34.117.59.81
```

These addresses identify the endpoints of the communication.

Unlike MAC addresses, IP addresses remain relevant beyond the local network.

### Layer 4 — Transport Layer

Expand:

```text
Transmission Control Protocol
```

Typical fields include:

```text
Source Port

Destination Port

Sequence Number

Acknowledgment Number

Flags
```

Example:

```text
Source Port:
52934

Destination Port:
80
```

Interpretation:

```text
52934
```

Temporary client port.

```text
80
```

HTTP server port.

The browser selected the source port automatically.

The server listens on the well-known HTTP port.

### Layer 5 — Application Layer

Expand:

```text
Hypertext Transfer Protocol
```

You should observe an HTTP request similar to:

```http
GET / HTTP/1.1

Host: neverssl.com
```

This is the actual application data generated by the browser.

Everything below it exists solely to deliver this request successfully.

### Following Encapsulation

The packet structure can be visualized as:

```text
Ethernet Frame

└── IPv4 Packet

    └── TCP Segment

        └── HTTP Data
```

Or:

```text
Frame

↓

Packet

↓

Segment

↓

Application Data
```

This mirrors the encapsulation process studied earlier.

Each layer wraps the data from the layer above.

### Understanding the Journey

Suppose your browser generated:

```http
GET /
```

The communication sequence was:

```text
HTTP Request

↓

TCP Segment

↓

IP Packet

↓

Ethernet Frame

↓

Bits on the Wire
```

The server performed decapsulation:

```text
Bits

↓

Ethernet Frame

↓

IP Packet

↓

TCP Segment

↓

HTTP Request
```

The original HTTP request was reconstructed exactly.

### Examining Packet Sizes

Select the Frame entry at the top of the protocol tree.

You will see information similar to:

```text
Frame Length:
742 Bytes
```

Notice that this value is significantly smaller than the Ethernet MTU.

Most web requests are relatively small.

Only large file transfers approach the MTU limit.

### Looking for MTU Effects

To observe larger packets:

1. Download a large file.
2. Capture the traffic.
3. Observe packet lengths.

You will often see values close to:

```text
1514 Bytes
```

or

```text
1518 Bytes
```

These packets are approaching the Ethernet MTU limit.

### Packet vs Frame in Wireshark

Students frequently confuse these two concepts.

Wireshark makes the distinction clear.

The top-level entry:

```text
Frame
```

represents the Layer 2 structure actually received by your network interface.

Inside that frame is:

```text
Internet Protocol Version 4
```

which represents the Layer 3 packet.

The frame contains the packet.

The packet does not contain the frame.

### Packet Flow Through a Router

Suppose the packet traverses three routers.

Conceptually:

```text
Frame A

↓

Router

↓

Frame B

↓

Router

↓

Frame C

↓

Router

↓

Frame D
```

The IP packet remains logically the same.

The Ethernet frame changes at every hop.

Wireshark only shows the frame visible on your local network segment.

### Common Misconceptions

#### Misconception 1

> A packet and a frame are the same thing.

Incorrect.

A frame is a Link-layer structure.

A packet is a Network-layer structure.

The frame carries the packet.

#### Misconception 2

> Ethernet frames travel unchanged across the Internet.

Incorrect.

Routers remove and rebuild frames at every hop.

Only the packet continues end-to-end.

#### Misconception 3

> Bandwidth and throughput mean the same thing.

Incorrect.

Bandwidth is theoretical capacity.

Throughput is actual delivered traffic.

#### Misconception 4

> Every packet is exactly 1500 bytes.

Incorrect.

1500 bytes is the Ethernet MTU limit.

Many packets are significantly smaller.

### Practical Exercises

#### Exercise 1

Capture traffic while loading:

```text
http://neverssl.com
```

Identify:

- Source MAC Address
- Destination MAC Address
- Source IP Address
- Destination IP Address
- Source Port
- Destination Port

#### Exercise 2

Determine:

- Which protocol belongs to the Application layer?
- Which protocol belongs to the Transport layer?
- Which protocol belongs to the Network layer?
- Which protocol belongs to the Link layer?

#### Exercise 3

Measure:

- Frame length
- IP packet length
- TCP payload size

Compare the values.

Explain why they differ.

#### Exercise 4

Locate:

```text
GET / HTTP/1.1
```

within the packet.

Identify every protocol header surrounding it.

### Lesson Summary

The Internet relies on packet switching to efficiently share network resources among billions of devices. Unlike circuit-switched networks, packet-switched networks divide data into packets that travel independently through a shared infrastructure.

Packets operate at the Network layer and provide end-to-end communication, while frames operate at the Link layer and provide hop-by-hop delivery. Routers forward packets by removing incoming frames, examining the destination IP address, and constructing new frames for the next network segment.

Every network technology imposes a Maximum Transmission Unit (MTU), which limits the amount of data that may be carried within a frame. Ethernet typically uses an MTU of 1500 bytes. Larger transfers are divided into multiple packets, and modern networks use Path MTU Discovery to avoid inefficient fragmentation.

Network performance is measured using concepts such as bandwidth, throughput, and goodput. Bandwidth represents theoretical capacity, throughput reflects actual delivered traffic, and goodput measures useful application data received by the application.

Wireshark allows engineers to observe these concepts directly by capturing real packets and displaying the protocol layers involved in communication. Through packet analysis, the abstract concepts of networking become visible and measurable.

### Key Terms

| Term | Definition |
|--------|------------|
| Packet Switching | Network communication using independently forwarded packets |
| Circuit Switching | Communication using dedicated end-to-end paths |
| Statistical Multiplexing | Dynamic sharing of network resources among users |
| Packet | Network-layer Protocol Data Unit |
| Frame | Link-layer Protocol Data Unit |
| MTU | Maximum Transmission Unit |
| Fragmentation | Splitting large packets into smaller pieces |
| Path MTU Discovery | Process of finding the largest usable packet size |
| Bandwidth | Maximum theoretical capacity of a link |
| Throughput | Actual delivered traffic rate |
| Goodput | Useful application data delivered to the application |
| Hop | One movement between neighboring Layer 3 devices |
| Encapsulation | Adding protocol headers as data moves down the stack |
| Decapsulation | Removing protocol headers as data moves up the stack |
| Wireshark | Packet capture and protocol analysis tool |
