## Checkpoint Quiz — Module 1 (Lessons 1.1–1.4)

This checkpoint assesses your understanding of everything covered so far:

- How the Internet works
- Networks and the Internet
- The protocol stack
- Encapsulation and decapsulation
- Protocols and standards
- RFCs, IEEE, and IANA
- Packet switching
- Packets and frames
- MTU
- Bandwidth and throughput
- Wireshark basics

This quiz is designed to test understanding rather than memorization.

**Total Questions:** 50

**Passing Score:** 80% (40/50)

---

# Part A — Multiple Choice

### Question 1

Which statement best describes the Internet?

A. A single worldwide computer

B. A collection of interconnected networks using common protocols

C. A network owned by governments

D. A collection of web servers

---

### Question 2

Which protocol is primarily responsible for delivering web pages?

A. DNS

B. HTTP

C. ARP

D. ICMP

---

### Question 3

Which layer is responsible for reliable delivery of data?

A. Application

B. Transport

C. Network

D. Physical

---

### Question 4

Which protocol provides logical addressing?

A. Ethernet

B. TCP

C. IP

D. HTTP

---

### Question 5

Which layer creates frames?

A. Application

B. Transport

C. Network

D. Link

---

### Question 6

Which device forwards packets between different networks?

A. Hub

B. Switch

C. Router

D. Access Point

---

### Question 7

Which address changes at every router hop?

A. Destination IP

B. Source IP

C. Destination MAC

D. Source Port

---

### Question 8

Which address normally remains unchanged from sender to receiver?

A. Destination MAC

B. Source MAC

C. Destination IP

D. Ethernet Type

---

### Question 9

Which protocol uses port 80 by default?

A. HTTPS

B. SSH

C. HTTP

D. DNS

---

### Question 10

What does MTU stand for?

A. Maximum Transfer Unit

B. Maximum Transmission Unit

C. Minimum Transmission Unit

D. Maximum Tunnel Unit

---

### Question 11

The standard Ethernet MTU is:

A. 512 bytes

B. 1024 bytes

C. 1500 bytes

D. 9000 bytes

---

### Question 12

Which organization develops TCP and IP?

A. IEEE

B. ISO

C. IETF

D. ICANN

---

### Question 13

Which organization defines Ethernet?

A. IEEE

B. IETF

C. IANA

D. W3C

---

### Question 14

Which organization manages port numbers?

A. IEEE

B. ISO

C. IANA

D. ICANN

---

### Question 15

RFC 793 defines:

A. UDP

B. IPv4

C. TCP

D. DNS

---

### Question 16

RFC 791 defines:

A. IPv4

B. HTTP

C. Ethernet

D. TLS

---

### Question 17

RFC 768 defines:

A. ARP

B. UDP

C. ICMP

D. SMTP

---

### Question 18

Which protocol resolves an IPv4 address to a MAC address?

A. DNS

B. ARP

C. ICMP

D. DHCP

---

### Question 19

Which RFC defines DNS?

A. RFC 768

B. RFC 791

C. RFC 1035

D. RFC 8446

---

### Question 20

Which RFC defines TLS 1.3?

A. RFC 791

B. RFC 8446

C. RFC 826

D. RFC 793

---

# Part B — True or False

### Question 21

Packets and frames are the same thing.

---

### Question 22

Routers rebuild Ethernet frames at every hop.

---

### Question 23

TCP is connection-oriented.

---

### Question 24

UDP guarantees packet delivery.

---

### Question 25

Ethernet operates at the Link layer.

---

### Question 26

The Internet uses circuit switching.

---

### Question 27

Packet switching allows many users to share network resources.

---

### Question 28

Bandwidth is the actual speed you observe during file downloads.

---

### Question 29

Goodput excludes protocol overhead.

---

### Question 30

Wireshark can display the Physical layer signals.

---

# Part C — Short Answer

### Question 31

Define a protocol.

---

### Question 32

What is encapsulation?

---

### Question 33

What is decapsulation?

---

### Question 34

Explain the difference between a packet and a frame.

---

### Question 35

Why does the Internet use packet switching instead of circuit switching?

---

### Question 36

What is statistical multiplexing?

---

### Question 37

Why do routers replace Ethernet frames?

---

### Question 38

Explain the difference between bandwidth and throughput.

---

### Question 39

What is the purpose of MTU?

---

### Question 40

Name the five layers of the Internet model in order from top to bottom.

---

# Part D — Scenario Questions

### Question 41

A browser sends an HTTP request.

List the protocol data units (PDUs) created as the data travels down the protocol stack.

---

### Question 42

A packet travels through five routers.

How many Ethernet frames are created during the journey?

Explain your reasoning.

---

### Question 43

A network link has a bandwidth of 1 Gbps, but a speed test reports 920 Mbps.

Does this necessarily indicate a problem? Why or why not?

---

### Question 44

You capture a packet in Wireshark and see:

```text
Frame
Ethernet II
Internet Protocol Version 4
Transmission Control Protocol
Hypertext Transfer Protocol
```

Match each protocol to its corresponding Internet layer.

---

### Question 45

A computer needs to send 4500 bytes of application data across an Ethernet network with an MTU of 1500 bytes.

Will the data fit into a single packet? Explain what happens.

---

# Part E — Wireshark Analysis

Answer these questions after capturing traffic to **http://neverssl.com**.

### Question 46

Find an HTTP GET request.

What destination port does it use?

---

### Question 47

Locate the IPv4 header.

Write down:

- Source IP
- Destination IP

---

### Question 48

Locate the Ethernet header.

Write down:

- Source MAC
- Destination MAC

---

### Question 49

Measure the frame length.

Is it equal to the Ethernet MTU?

Explain.

---

### Question 50

Starting from the Frame entry in Wireshark, list every protocol you observe until you reach the HTTP request.

---

# Bonus Challenge

Without looking at your notes, answer the following:

- What is RFC 791?
- What is RFC 793?
- What is RFC 768?
- What is RFC 826?
- What is RFC 1035?
- What is RFC 8446?
- What does IEEE 802.3 define?
- What does IEEE 802.11 define?
- What does IEEE 802.1Q define?
- What does IEEE 802.1X define?

If you can answer all of these correctly from memory, you have a solid grasp of the foundational concepts covered in Module 1.