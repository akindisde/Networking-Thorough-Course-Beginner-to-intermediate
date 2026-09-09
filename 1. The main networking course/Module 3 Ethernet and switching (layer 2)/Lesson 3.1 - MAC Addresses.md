## Part 1 — MAC Address Fundamentals

A **MAC address** is a Layer 2 identifier used by a network interface for communication on a local network. Ethernet switches use MAC addresses to determine where Ethernet frames should be forwarded.

To understand why MAC addresses exist, it helps to separate the responsibilities of different networking layers.

An **IP address** identifies a Layer 3 endpoint and allows networks to route traffic toward a destination.

A **MAC address** identifies a Layer 2 interface for delivery across the current local network segment.

For example, a computer might have:

- IPv4 address: `192.168.1.20`
    
- MAC address: `00:1A:2B:3C:4D:5E`
    

These addresses describe the same network interface, but they serve different purposes.

### MAC Addresses Operate at Layer 2

Ethernet operates primarily at **Layer 2 of the OSI model**.

When a host sends an Ethernet frame, the frame contains:

- A **source MAC address**
    
- A **destination MAC address**
    
- The Layer 3 packet being transported, such as an IPv4 or IPv6 packet
    

A switch examines these MAC addresses to make forwarding decisions.

A simplified communication path looks like this:

```text
Application
    ↓
TCP / UDP
    ↓
IP
    ↓
Ethernet
    ↓
Physical network
```

MAC addresses belong to the Ethernet portion of this process.

This is why a switch can forward traffic without needing to understand the application data inside the packet.

### The 48-Bit MAC Address

The most common MAC address format is **48 bits**, also known as **EUI-48**.

48 bits equals:

```text
48 bits ÷ 8 = 6 bytes
```

Therefore, a traditional MAC address contains **6 octets**.

An example is:

```text
00:1A:2B:3C:4D:5E
```

Each pair represents one octet:

```text
00    1A    2B    3C    4D    5E
│     │     │     │     │     │
1     2     3     4     5     6
octet octet octet octet octet octet
```

Each octet contains 8 bits:

```text
8 × 6 = 48 bits
```

### Why Hexadecimal Is Used

A MAC address could technically be written entirely in binary:

```text
00000000 00011010 00101011 00111100 01001101 01011110
```

That is difficult for humans to read and remember.

Hexadecimal makes the same 48 bits much more compact:

```text
00:1A:2B:3C:4D:5E
```

One hexadecimal digit represents **4 bits**.

Therefore:

```text
48 bits ÷ 4 = 12 hexadecimal digits
```

A standard 48-bit MAC address therefore contains **12 hexadecimal digits**, usually grouped into six pairs.

Common representations include:

```text
00:1A:2B:3C:4D:5E
00-1A-2B-3C-4D-5E
001A.2B3C.4D5E
```

The representation changes, but the underlying 48-bit value is the same.

### Understanding the First Octet

The first octet is particularly important because two of its bits have special meanings.

Consider:

```text
00
```

In binary:

```text
00000000
```

The two least significant bits of the first octet are:

```text
76543210
||||||||
00000000
      ^^
      ||
      |└─ I/G bit
      └── U/L bit
```

These two bits are:

- **I/G bit** — Individual/Group
    
- **U/L bit** — Universal/Local
    

They provide information about how the MAC address is being used.

### The I/G Bit: Unicast vs Multicast

The **I/G bit** determines whether the MAC address represents an individual interface or a group.

If the bit is:

```text
0 → Individual / Unicast
1 → Group / Multicast
```

A normal unicast MAC address therefore has the I/G bit set to `0`.

For example:

```text
00:1A:2B:3C:4D:5E
```

The first octet is:

```text
00 = 00000000
```

The I/G bit is `0`, so this is a unicast address.

A multicast MAC address has the I/G bit set to `1`.

This distinction becomes important when studying Ethernet frame delivery in Part 2.

### The U/L Bit: Universal vs Local

The second important bit is the **U/L bit**.

It indicates whether the address is:

- **Universally administered**
    
- **Locally administered**
    

Conceptually:

```text
U/L = 0 → Universally administered
U/L = 1 → Locally administered
```

A universally administered address is normally associated with an IEEE-assigned address block.

A locally administered address is intended to be configured or generated locally rather than treated as a globally assigned hardware identity.

Locally administered addresses are common in situations such as:

- Virtual machines
    
- Containers
    
- Network virtualization
    
- Manually configured interfaces
    
- Privacy/randomized MAC addresses
    
- Certain wireless client implementations
    

This is one reason you should **not assume that every MAC address uniquely identifies a physical device manufacturer**.

### OUI and IEEE Address Allocation

The first 24 bits of a traditional globally administered MAC address are commonly associated with an **OUI**, or **Organizationally Unique Identifier**.

For example:

```text
00:1A:2B:3C:4D:5E
└─────────┘
   OUI
```

Historically, the OUI identifies an organization that has been allocated an address block by the **IEEE**.

People often describe this as the "manufacturer portion" of a MAC address.

That description is useful for beginners, but it is not always literally correct.

An OUI identifies an **organization or allocated address block**, not necessarily the physical manufacturer of the specific device currently using the address.

For example, the address may belong to:

- A hardware vendor
    
- A virtualization platform
    
- A network equipment company
    
- An organization using its own assigned address space
    
- Another entity that has received an IEEE allocation
    

Furthermore, locally administered and randomized MAC addresses may not correspond to a traditional vendor OUI at all.

### The Remaining Address Bits

In the traditional globally administered format, the remaining portion of the address is allocated within the organization's assigned block.

A simplified representation is:

```text
48-bit MAC address

|       24 bits       |       24 bits       |
|---------------------|---------------------|
| Organization block  | Address assignment  |
```

The exact allocation structure can vary depending on the type of IEEE identifier and address assignment.

Therefore, it is better to think of the address as an **IEEE-managed or locally managed identifier space**, rather than assuming:

> "First half = manufacturer, second half = serial number."

A MAC address is **not necessarily a serial number**.

### How MAC Addresses Are Assigned

A network interface needs a Layer 2 address before it can participate normally in Ethernet communication.

Traditionally, network hardware manufacturers receive address blocks from IEEE and assign addresses to interfaces.

A simplified process looks like:

```text
IEEE
  ↓
Organization receives address allocation
  ↓
Organization assigns individual addresses
  ↓
Network interface receives a MAC address
```

However, modern networking is more complicated.

MAC addresses can also be:

- Generated by operating systems
    
- Assigned manually
    
- Generated for virtual interfaces
    
- Randomized for privacy
    
- Changed by administrators
    
- Changed temporarily by software
    

Therefore, the MAC address currently visible on an interface is not necessarily the permanent address originally assigned to the physical hardware.

### Permanent MAC vs Current MAC

A useful distinction is between the address associated with the physical or virtual interface and the address the operating system is **currently presenting to the network**.

For example, a wireless device may use MAC randomization when scanning for or connecting to wireless networks.

Similarly, a virtual machine may receive a MAC address generated by the virtualization platform.

This means that:

> **A MAC address should not automatically be treated as a permanent identity.**

This becomes extremely important when discussing Layer 2 security later in the course.

### MAC Address Types

At a high level, Ethernet MAC addresses can be categorized according to their delivery purpose.

#### Unicast

A **unicast MAC address** identifies an individual Layer 2 interface.

Example:

```text
00:1A:2B:3C:4D:5E
```

A unicast frame is intended for a specific destination interface.

#### Multicast

A **multicast MAC address** represents a group rather than one individual interface.

Multiple hosts can therefore participate in receiving multicast traffic.

Multicast is commonly used by networking protocols and applications that need to communicate with multiple receivers.

#### Broadcast

The Ethernet broadcast address is:

```text
FF:FF:FF:FF:FF:FF
```

A frame sent to this address is intended for all devices within the relevant Layer 2 broadcast domain.

Broadcast behavior becomes particularly important when learning about ARP, DHCP, switching, and network segmentation.

### MAC Address vs IP Address

One of the most common beginner mistakes is treating a MAC address and an IP address as interchangeable.

They are not.

|Characteristic|MAC Address|IP Address|
|---|---|---|
|Primary layer|Layer 2|Layer 3|
|Common Ethernet format|48-bit hexadecimal|IPv4: 32-bit / IPv6: 128-bit|
|Main purpose|Local Layer 2 delivery|Layer 3 addressing/routing|
|Used by switches|Yes|Not for normal Layer 2 forwarding decisions|
|Used by routers|Not as an end-to-end identifier|Yes|
|Changes across routers|Yes, Ethernet MACs are replaced per link|IP addressing generally remains relevant end-to-end, subject to routing/NAT|

The important idea is that **MAC addresses provide local delivery**, while **IP addresses provide logical network addressing and routing**.

### A MAC Address Is Not a User Identity

Another important misconception is:

> "If I know someone's MAC address, I know who they are."

Not necessarily.

A MAC address identifies a Layer 2 interface, not a human being.

It does not inherently tell you:

- Who is using the device
    
- Who owns the device
    
- Whether the device is trustworthy
    
- Whether the address is currently genuine
    
- Whether the address belongs to physical hardware
    
- Whether the same address was used elsewhere
    

A MAC address can also be observed in network traffic and, depending on the environment, changed or spoofed.

This limitation becomes central to the security discussion in Part 3.

### Why MAC Addresses Matter to Network Engineers

MAC addresses are fundamental to understanding:

- Ethernet
    
- Switching
    
- VLANs
    
- ARP
    
- Wireless networking
    
- Network troubleshooting
    
- Packet captures
    
- Layer 2 attacks
    
- Network access control
    

When examining a packet capture, for example, the MAC addresses can immediately tell you which Layer 2 interfaces were involved in a particular Ethernet frame.

When troubleshooting a switch, the MAC address table can tell you which interface the switch associates with a particular MAC address.

When investigating a security incident, unexpected MAC addresses or MAC movements between switch ports can provide useful evidence.

Understanding the structure and limitations of MAC addresses is therefore not just theoretical knowledge. It is a foundation for working with real Layer 2 networks.

## Summary

A MAC address is a **Layer 2 identifier used for local network communication**, particularly with Ethernet. The traditional MAC address is a **48-bit EUI-48 address**, represented as six hexadecimal octets.

The first octet contains two important control bits: the **I/G bit**, which distinguishes unicast from multicast addressing, and the **U/L bit**, which distinguishes universally administered from locally administered addresses.

The first 24 bits of a traditional globally administered address are commonly associated with an **IEEE OUI**, which identifies an organization or allocated address block. It should not automatically be interpreted as a permanent manufacturer identity.

MAC addresses can be assigned by hardware vendors, operating systems, virtualization platforms, or administrators, and they can be randomized or changed. As a result, a MAC address is useful for Layer 2 communication and identification, but it is **not a strong identity or authentication mechanism by itself**.

## Key Takeaways

- A **MAC address operates at Layer 2**.
    
- Traditional MAC addresses are **48 bits / 6 octets**.
    
- MAC addresses are commonly written as **12 hexadecimal digits**.
    
- One hexadecimal digit represents **4 bits**.
    
- The **I/G bit** indicates unicast or multicast.
    
- The **U/L bit** indicates universal or local administration.
    
- An **OUI** identifies an IEEE-assigned organizational address block; it is not necessarily the physical device manufacturer.
    
- MAC addresses are **not necessarily permanent**.
    
- Virtual, randomized, and locally administered MAC addresses are common in modern networks.
    
- A MAC address identifies a **network interface**, not a person or a trustworthy device.
    
- MAC addresses are primarily relevant to **local Layer 2 communication**.
    
- Understanding MAC addressing is essential before learning **Ethernet frames, switching, MAC tables, ARP, VLANs, and Layer 2 security**.

## Part 2 — MAC Addresses in Ethernet Communication

Part 1 established what a MAC address is and how it is structured. Now we can look at what actually happens when devices use MAC addresses to communicate.

A MAC address becomes operationally important when it is placed inside an **Ethernet frame**.

When a computer sends data across an Ethernet LAN, the information does not simply travel from one MAC address to another by itself. The operating system, Layer 3 protocols, Ethernet, and switches all participate in the process.

A simplified view is:

```text
Application data
      ↓
Transport segment
      ↓
IP packet
      ↓
Ethernet frame
      ↓
Physical transmission
```

The Ethernet frame provides the Layer 2 delivery information needed to move the packet across the current local network.

### Ethernet Frames

An **Ethernet frame** is the Layer 2 container used to transport network-layer data across an Ethernet network.

Among other fields, an Ethernet frame contains:

```text
+-------------------+-------------------+
| Destination MAC   | Source MAC        |
+-------------------+-------------------+
| Ethernet header / type information    |
+---------------------------------------+
| Payload                               |
| Usually an IP packet                  |
+---------------------------------------+
| Frame Check Sequence (FCS)            |
+---------------------------------------+
```

The two most important fields for understanding MAC addressing are:

- **Destination MAC address**
    
- **Source MAC address**
    

The source address identifies the Layer 2 interface that transmitted the frame onto the current Ethernet link.

The destination address identifies the Layer 2 destination for that frame.

For example:

```text
Source MAC:
00:11:22:33:44:55

Destination MAC:
AA:BB:CC:DD:EE:FF
```

The switch receiving this frame can use these addresses to make a forwarding decision.

### Source and Destination MAC Addresses

Consider two computers connected to a switch:

```text
PC-A                         PC-B
MAC: 00:11:22:33:44:55       MAC: AA:BB:CC:DD:EE:FF
       │                            │
       └────────── Switch ─────────┘
```

If PC-A sends an Ethernet frame to PC-B:

```text
Source MAC      = 00:11:22:33:44:55
Destination MAC = AA:BB:CC:DD:EE:FF
```

The switch examines the frame.

It does not normally need to inspect the application data to determine where the frame should go.

Instead, it can use the **destination MAC address**.

This is the basic mechanism behind Ethernet switching.

### Three Major Ethernet Delivery Types

Ethernet communication can be divided into three important delivery categories:

1. **Unicast**
    
2. **Broadcast**
    
3. **Multicast**
    

Understanding the difference is essential because switches handle each type differently.

### Unicast

A **unicast** frame is intended for one specific Layer 2 destination.

For example:

```text
PC-A → PC-B
```

The Ethernet frame might contain:

```text
Source:
00:11:22:33:44:55

Destination:
AA:BB:CC:DD:EE:FF
```

The switch attempts to forward the frame only toward the port where it believes the destination MAC exists.

This is the normal form of host-to-host communication on an Ethernet LAN.

### Broadcast

A **broadcast** frame is intended for all devices within the relevant Layer 2 broadcast domain.

The Ethernet broadcast address is:

```text
FF:FF:FF:FF:FF:FF
```

For example:

```text
Destination:
FF:FF:FF:FF:FF:FF
```

A switch generally floods a broadcast frame out all appropriate ports in the same VLAN except the port on which the frame arrived.

Broadcasts are therefore capable of reaching many devices.

Protocols such as **ARP** and **DHCP** make important use of broadcast communication in IPv4 networks.

However, routers normally separate broadcast domains. A Layer 2 broadcast does not automatically propagate through a router into another IP network.

### Multicast

A **multicast** frame is addressed to a group rather than one individual interface.

Instead of:

```text
One sender → One receiver
```

or:

```text
One sender → Everyone
```

multicast provides:

```text
One sender → Interested group of receivers
```

Multicast MAC addresses have the I/G bit set to indicate group addressing.

Switch behavior for multicast can depend on configuration and features such as **IGMP snooping**. Without appropriate multicast awareness, a switch may treat certain multicast traffic similarly to flooded traffic within the VLAN.

This makes multicast an important topic when designing networks carrying services such as:

- Video distribution
    
- IPTV
    
- Routing protocols
    
- Service discovery
    
- Real-time applications
    

### What Happens When a Host Sends a Frame?

Suppose PC-A wants to send an IPv4 packet to another host on the same LAN.

PC-A has:

```text
IP address:
192.168.1.10

MAC address:
00:11:22:33:44:55
```

The destination host has:

```text
IP address:
192.168.1.20

MAC address:
AA:BB:CC:DD:EE:FF
```

PC-A needs to construct an Ethernet frame.

It needs to know:

> Which MAC address corresponds to `192.168.1.20`?

For IPv4 Ethernet communication, this is where **ARP** becomes important.

### ARP: Connecting IPv4 and MAC Addresses

**ARP**, or Address Resolution Protocol, allows an IPv4 host to discover the MAC address associated with an IPv4 address on the local network.

Conceptually:

```text
IPv4 address:
192.168.1.20

        ↓ ARP

MAC address:
AA:BB:CC:DD:EE:FF
```

If PC-A does not already know the destination MAC address, it can send an ARP request.

The request essentially asks the local network:

> Who has `192.168.1.20`?

The device using that IPv4 address responds with its MAC address.

PC-A can then construct the Ethernet frame:

```text
Source MAC:
00:11:22:33:44:55

Destination MAC:
AA:BB:CC:DD:EE:FF
```

The IPv4 packet is then carried inside that Ethernet frame.

This illustrates an important relationship:

```text
IP address
    ↓
ARP
    ↓
MAC address
    ↓
Ethernet frame
    ↓
Switch
```

ARP is specifically associated with **IPv4**.

IPv6 does not use ARP. IPv6 uses **Neighbor Discovery Protocol (NDP)**, which performs related neighbor-resolution functions using ICMPv6.

### What the Switch Actually Learns

When an Ethernet frame enters a switch port, the switch examines the **source MAC address**.

Suppose PC-A sends:

```text
Source MAC:
00:11:22:33:44:55
```

through switch port `Gi0/1`.

The switch can learn:

```text
00:11:22:33:44:55 → Gi0/1
```

It stores this information in its **MAC address table**, also called the forwarding database.

A simplified table might look like:

|MAC Address|Switch Port|
|---|---|
|`00:11:22:33:44:55`|`Gi0/1`|
|`AA:BB:CC:DD:EE:FF`|`Gi0/2`|
|`10:20:30:40:50:60`|`Gi0/3`|

The switch is effectively building a map:

```text
MAC address → Where I can reach it
```

This learning process is one of the fundamental mechanisms of Ethernet switching.

### Why the Switch Learns from the Source MAC

The switch learns from the **source**, not the destination.

Suppose a frame arrives on `Gi0/1` with:

```text
Source MAC:
00:11:22:33:44:55
```

The switch knows:

> "I received traffic from `00:11:22:33:44:55` through Gi0/1."

Therefore, it can associate that MAC address with that port.

The destination MAC is then used for the forwarding decision.

This gives the basic process:

```text
1. Receive frame
        ↓
2. Learn source MAC
        ↓
3. Look up destination MAC
        ↓
4. Decide where to forward
```

### Known Unicast Forwarding

Suppose the switch already knows:

```text
AA:BB:CC:DD:EE:FF → Gi0/2
```

A frame arrives:

```text
Source:
00:11:22:33:44:55

Destination:
AA:BB:CC:DD:EE:FF
```

The switch looks up the destination.

It finds:

```text
AA:BB:CC:DD:EE:FF → Gi0/2
```

The switch can therefore forward the frame out `Gi0/2`.

It does not need to send the frame to every port.

This is what makes switched Ethernet substantially more efficient than a simple shared medium.

### Unknown Unicast

What happens if the switch does not yet know where the destination MAC is?

For example:

```text
Destination:
AA:BB:CC:DD:EE:FF
```

but the MAC address table contains no entry for that address.

The switch treats this as an **unknown unicast**.

It generally floods the frame out the other appropriate ports within the same VLAN, except the port on which the frame arrived.

If the destination device receives the frame, it can respond. The switch can then learn where that MAC address is located from the response traffic.

Eventually, the switch may have a table entry such as:

```text
AA:BB:CC:DD:EE:FF → Gi0/2
```

and subsequent frames can be forwarded directly.

### MAC Address Table Aging

MAC address table entries are not necessarily permanent.

Switches typically associate learned MAC addresses with an **aging timer**.

If a switch stops seeing traffic from a learned MAC address for a period of time, it can remove the entry.

For example:

```text
MAC                         Port
00:11:22:33:44:55           Gi0/1
```

If the device disconnects or moves and the switch no longer sees traffic from that address, the entry can eventually expire.

This allows the switch to adapt to changing network topology.

### MAC Address Movement

A switch may sometimes learn the same MAC address on different ports.

For example:

```text
00:11:22:33:44:55 → Gi0/1
```

and later:

```text
00:11:22:33:44:55 → Gi0/5
```

This can happen for legitimate reasons, such as:

- A device physically moved
    
- A wireless client moved between access points
    
- A virtual machine migrated
    
- A network topology changed
    

But repeated or unexpected **MAC address movement** can also indicate a configuration problem or a security issue.

Possible causes include:

- Layer 2 loops
    
- Incorrect cabling
    
- Virtualization behavior
    
- Bridging problems
    
- MAC spoofing
    

Therefore, MAC-table behavior can be useful when troubleshooting or investigating a network.

### The Layer 2 Boundary

MAC addresses are used for **local Layer 2 forwarding**.

This does not mean they disappear after one physical cable or one switch.

A Layer 2 network can contain multiple interconnected switches:

```text
PC-A
  │
Switch 1
  │
Switch 2
  │
Switch 3
  │
PC-B
```

As long as the devices are participating in the same Layer 2 domain, Ethernet frames can be forwarded through multiple switches.

Therefore, saying that MAC addresses are "one-hop" addresses needs some precision.

A better way to understand it is:

> **MAC addressing applies to the current Layer 2 forwarding domain and the current link-layer delivery process.**

A frame can traverse several switches while remaining within the same Layer 2 domain.

The important boundary is the **router**.

### What Happens When a Packet Crosses a Router?

Consider:

```text
PC-A
192.168.1.10
      │
      │ LAN
      ▼
Router
      │
      │ Another network
      ▼
PC-B
192.168.2.20
```

PC-A wants to communicate with PC-B.

PC-A determines that `192.168.2.20` is **not on its local subnet**.

Therefore, PC-A does not try to send the Ethernet frame directly to PC-B's MAC address.

Instead, it sends the frame to its **default gateway**.

For example:

```text
PC-A MAC:
00:11:22:33:44:55

Router LAN MAC:
AA:AA:AA:AA:AA:AA
```

The first Ethernet frame might therefore look conceptually like:

```text
Source MAC:
00:11:22:33:44:55

Destination MAC:
AA:AA:AA:AA:AA:AA
```

The router receives the frame.

The router removes the Ethernet header and processes the IP packet.

It then forwards the IP packet toward the next network.

A **new Ethernet frame** is created on the outgoing interface.

For example:

```text
Source MAC:
BB:BB:BB:BB:BB:BB

Destination MAC:
CC:CC:CC:CC:CC:CC
```

The exact addresses depend on the next Layer 2 network.

The important concept is:

```text
LAN 1                         LAN 2

MAC A → MAC Router    →    MAC Router → MAC B
          │                         │
          └──── Router ─────────────┘
```

The Ethernet source and destination MAC addresses are therefore **replaced at the Layer 2 boundary**.

### IP Addresses and MAC Addresses Work Together

This creates one of the most important relationships in networking.

Suppose:

```text
Source IP:
192.168.1.10

Destination IP:
192.168.2.20
```

The IP packet is trying to reach `192.168.2.20`.

But on the first Ethernet network, the frame may be addressed to the router:

```text
Source MAC:
PC-A

Destination MAC:
Router
```

The router then creates another Ethernet frame:

```text
Source MAC:
Router's outgoing interface

Destination MAC:
Next-hop device
```

The Layer 3 destination remains relevant to routing, while the Layer 2 addresses are rewritten for each Ethernet segment.

A useful mental model is:

```text
IP:
"Where should the packet ultimately go?"

MAC:
"Where should this frame go on this local Layer 2 network?"
```

### Same Subnet vs Different Subnet

This distinction is critical.

If the destination is on the **same IP subnet**, the host generally needs the destination's MAC address.

For example:

```text
PC-A:
192.168.1.10/24

PC-B:
192.168.1.20/24
```

PC-A determines that PC-B is local.

Conceptually:

```text
PC-A
  │
  │ Destination MAC = PC-B
  ▼
Switch
  │
  ▼
PC-B
```

If the destination is on a **different IP subnet**:

```text
PC-A:
192.168.1.10/24

PC-B:
192.168.2.20/24
```

PC-A determines that the destination is remote.

It therefore sends the Ethernet frame to the MAC address of the **default gateway**, not directly to PC-B.

Conceptually:

```text
PC-A
  │
  │ Destination MAC = Router
  ▼
Switch
  │
  ▼
Router
  │
  ▼
Other network
```

This distinction explains many common networking problems.

### VLANs and MAC Addressing

A VLAN creates a separate Layer 2 broadcast domain.

For example:

```text
VLAN 10
192.168.10.0/24

VLAN 20
192.168.20.0/24
```

Even if both VLANs exist on the same physical switch, they represent separate Layer 2 domains.

A switch maintains MAC address information in the context of VLANs.

Conceptually:

```text
VLAN 10:
00:11:22:33:44:55 → Gi0/1

VLAN 20:
00:11:22:33:44:55 → Gi0/8
```

The same MAC address could potentially appear in different VLAN contexts without representing the same Layer 2 forwarding domain.

Communication between VLANs requires a Layer 3 device such as a router or Layer 3 switch.

This reinforces the central idea:

> **MAC addresses provide Layer 2 delivery; routing provides Layer 3 communication between different networks.**

### Why MAC Addresses Are Important in Packet Analysis

When analyzing Ethernet traffic with a tool such as Wireshark, the Ethernet header provides valuable information.

You can examine:

```text
Destination MAC
Source MAC
EtherType
Payload
```

For example, an IPv4 Ethernet frame might conceptually show:

```text
Ethernet II
    Destination: AA:BB:CC:DD:EE:FF
    Source: 00:11:22:33:44:55
    Type: IPv4
```

An ARP frame will show Ethernet MAC addresses while carrying ARP information about IPv4 addresses.

By comparing the Ethernet addresses with the IP addresses, you can determine:

- Which interface sent the frame
    
- Which interface was the Layer 2 destination
    
- Whether the frame was broadcast or unicast
    
- Whether traffic is local or being sent toward a gateway
    
- Whether MAC addresses are changing as traffic crosses routers
    
- Whether unexpected MAC addresses appear in the traffic
    

This is an essential troubleshooting and security-analysis skill.

### The Complete Local Communication Process

Putting the concepts together, consider a host communicating with another host on the same LAN.

```text
1. Application creates data
          ↓
2. Transport layer creates segment
          ↓
3. IP creates packet
          ↓
4. Host determines destination is local
          ↓
5. ARP/NDP resolves the Layer 3 neighbor
          ↓
6. Ethernet creates a frame
          ↓
7. Switch receives the frame
          ↓
8. Switch learns the source MAC
          ↓
9. Switch looks up the destination MAC
          ↓
10. Switch forwards the frame
          ↓
11. Destination host receives the frame
```

For a remote destination, the process changes at Layer 2:

```text
Host
  ↓
Ethernet frame → Default Gateway
  ↓
Router
  ↓
New Ethernet frame
  ↓
Next Layer 2 network
  ↓
Destination / Next Hop
```

This is the foundation of Ethernet switching and routed communication.

## Summary

MAC addresses are used inside **Ethernet frames** to provide Layer 2 delivery. Every Ethernet frame normally contains a source MAC and destination MAC, allowing switches to make forwarding decisions.

Ethernet supports **unicast, broadcast, and multicast** delivery. The broadcast address is `FF:FF:FF:FF:FF:FF`.

Switches learn source MAC addresses and associate them with ports in a **MAC address table**. When a destination MAC is known, the switch can forward a unicast frame directly toward the appropriate port. Unknown unicast and broadcast traffic may be flooded within the relevant VLAN.

For IPv4, **ARP maps local IPv4 addresses to MAC addresses**. IPv6 uses **NDP** for related neighbor-resolution functions.

MAC addresses operate within the current **Layer 2 domain**. When traffic crosses a router, the Ethernet frame is terminated and a new Layer 2 frame is created on the next network. The IP packet remains part of the Layer 3 forwarding process, while the MAC addresses change according to the next Layer 2 segment.

Understanding this relationship between **IP addresses, MAC addresses, Ethernet frames, ARP/NDP, switches, VLANs, and routers** is essential for understanding how real networks forward traffic.

## Key Takeaways

- An **Ethernet frame** contains source and destination MAC addresses.
    
- **Unicast** targets one interface, **broadcast** targets the local broadcast domain, and **multicast** targets a group.
    
- `FF:FF:FF:FF:FF:FF` is the Ethernet **broadcast MAC address**.
    
- Switches learn **source MAC addresses** and associate them with switch ports.
    
- The switch uses the **destination MAC address** to determine where to forward a frame.
    
- A known destination MAC can be forwarded directly to its associated port.
    
- Unknown unicast traffic may be **flooded** within the relevant VLAN.
    
- Broadcast traffic is generally flooded within the Layer 2 broadcast domain.
    
- **ARP** resolves IPv4 addresses to MAC addresses on the local network.
    
- **IPv6 uses NDP**, not ARP.
    
- A Layer 2 domain can span **multiple switches**.
    
- A **VLAN represents a separate Layer 2 broadcast domain**.
    
- Hosts communicating within the same subnet generally use the destination host's MAC address.
    
- Hosts communicating with a different subnet generally send the Ethernet frame to the **default gateway's MAC address**.
    
- Routers **terminate Ethernet frames and create new frames** on outgoing Layer 2 networks.
    
- MAC addresses are therefore fundamentally **local/hop-by-hop Layer 2 addressing**, while IP addressing supports Layer 3 communication across networks.
    
- MAC address tables are valuable for **switching, troubleshooting, packet analysis, and security investigations**.

## Part 3 — MAC Addresses and Layer 2 Security

MAC addresses are essential for Ethernet communication, but they have an important security limitation:

> **A MAC address is an identifier, not proof of identity.**

A switch can use a MAC address to decide where to forward a frame, but it cannot inherently determine whether the device using that MAC address is the legitimate device that was originally assigned it.

This distinction is extremely important when designing secure Layer 2 networks.

### Why MAC Addresses Cannot Be Trusted as Strong Identity

A MAC address may look like a unique identifier:

```text
00:11:22:33:44:55
```

It is tempting to think:

> "This MAC belongs to this computer, therefore this computer is authorized."

That conclusion is unsafe.

A MAC address can be:

- Observed from network traffic
    
- Configured manually
    
- Changed by software
    
- Randomized by an operating system
    
- Generated for a virtual machine
    
- Spoofed by an attacker
    

Therefore:

```text
MAC address ≠ authenticated identity
```

A network can recognize an address without actually knowing who or what is behind that address.

This is the fundamental weakness behind many basic MAC-based security mechanisms.

### What Is MAC Spoofing?

**MAC spoofing** is the act of changing the MAC address presented by a network interface so that it appears to use a different address.

For example, suppose an interface normally presents:

```text
00:11:22:33:44:55
```

An administrator might configure it in an authorized test environment to present:

```text
02:11:22:33:44:55
```

The operating system and network interface then use the new address for Layer 2 communication.

MAC spoofing does not magically give an attacker control over another device. It simply changes the Layer 2 address the attacker's interface presents to the network.

This distinction matters.

### MAC Spoofing in an Authorized Lab

On Linux, an administrator can temporarily change an interface's MAC address using standard networking tools.

For example, in a controlled lab:

```bash
sudo ip link set dev eth0 down
sudo ip link set dev eth0 address 02:11:22:33:44:55
sudo ip link set dev eth0 up
```

The exact interface name may differ:

```text
eth0
ens33
enp0s3
wlan0
```

This is useful for understanding how systems behave when their Layer 2 identity changes.

Only perform this type of testing on systems and networks you own or are explicitly authorized to test.

### Why Attackers Use MAC Spoofing

There are several reasons an attacker might change a MAC address.

One reason is to **evade weak access controls**.

Suppose a network administrator creates a simple allowlist:

```text
Authorized MAC addresses:

00:11:22:33:44:55
AA:BB:CC:DD:EE:FF
10:20:30:40:50:60
```

The administrator's assumption is:

> "Only these devices can connect."

But if the network relies exclusively on MAC addresses, an attacker may observe an authorized MAC address and configure their own interface to use it.

The attacker is then presenting the same Layer 2 identifier.

This demonstrates the central problem:

```text
Observed MAC
     ↓
Copied MAC
     ↓
Weak identity control
```

The network may see the expected address without knowing that a different physical or logical device is actually using it.

### MAC Spoofing Does Not Equal Full Device Impersonation

It is important not to overstate what MAC spoofing accomplishes.

Changing your MAC address does **not automatically** provide:

- The legitimate user's credentials
    
- Access to encrypted application sessions
    
- The original device's private keys
    
- The original device's IP address
    
- Control of the original device
    
- Access to services requiring additional authentication
    

MAC spoofing primarily changes the **Layer 2 identity presented by the interface**.

Its usefulness therefore depends heavily on what security controls exist elsewhere in the network.

A network using strong authentication may gain little from MAC spoofing.

A network relying on MAC addresses as its primary authorization mechanism may be significantly more vulnerable.

### MAC Filtering

One common Layer 2 security technique is **MAC filtering**.

The administrator creates a list of permitted MAC addresses and allows only those addresses to use a particular network connection.

Conceptually:

```text
Switch / Access Point
        │
        ├── MAC A → Allowed
        ├── MAC B → Allowed
        ├── MAC C → Allowed
        └── MAC D → Denied
```

This can provide some operational control, but it should not be confused with strong authentication.

### Why MAC Filtering Is Weak

MAC filtering depends on the assumption:

> "A device using an approved MAC address is an approved device."

But the network can observe the MAC address while having no cryptographic proof that the device is legitimate.

If an attacker can determine an allowed MAC address, they may be able to configure their interface to present the same address.

Therefore:

```text
MAC filtering
      ↓
Checks "What address are you presenting?"
      ↓
Does not necessarily prove "Who are you?"
```

This is why MAC filtering should not be considered a replacement for authentication.

### MAC Filtering Still Has Some Uses

Calling MAC filtering "useless" would also be inaccurate.

It can still have practical value for:

- Basic administrative control
    
- Simple device management
    
- Reducing accidental connections
    
- Small controlled environments
    
- Network inventory
    
- Operational policies
    

It can also act as one layer in a broader defense strategy.

The problem occurs when an organization treats it as a **strong security boundary**.

A mature network should assume that MAC addresses can be observed and potentially changed.

### Switch Port Security

Another Layer 2 control is **switch port security**.

Port security allows a switch to place restrictions on the MAC addresses that can appear on a particular physical port.

For example:

```text
Gi0/10
    ↓
Allowed MAC:
00:11:22:33:44:55
```

The switch may be configured to allow only a particular MAC address, or a limited number of MAC addresses, on that port.

Depending on the platform and configuration, a violation can result in actions such as:

- Dropping offending frames
    
- Logging the violation
    
- Restricting the port
    
- Shutting down the port
    

Port security is generally stronger operationally than a simple MAC allowlist, because it can associate the allowed address with a **specific switch port**.

However, it still relies on MAC addresses.

Therefore:

> **Port security improves Layer 2 control, but MAC addresses themselves are still not strong authentication credentials.**

### The MAC Table vs Authentication

It is important to distinguish two completely different concepts.

A switch's MAC address table answers:

> **"Where did I learn this MAC address?"**

An authentication system answers:

> **"Has this device or user successfully demonstrated that it is authorized?"**

For example:

```text
MAC Table:

00:11:22:33:44:55 → Gi0/10
```

This tells the switch that traffic from this MAC was observed on `Gi0/10`.

It does not prove:

- Who is operating the device
    
- Whether the device is authorized
    
- Whether the MAC address was changed
    
- Whether another device is impersonating it
    

This distinction is fundamental to network security.

## 802.1X — Port-Based Network Access Control

For stronger access control, enterprise networks commonly use **802.1X**.

802.1X provides **port-based network access control**.

Instead of trusting a MAC address, the network requires a device to authenticate before granting normal network access.

The architecture typically contains three roles:

```text
Supplicant
     │
     │ EAP
     ▼
Authenticator
     │
     │ Authentication exchange
     ▼
Authentication Server
```

These roles are usually:

### Supplicant

The **supplicant** is the client requesting network access.

Examples include:

- A laptop
    
- A desktop
    
- A smartphone
    
- An IP phone
    
- Another network-capable device
    

The supplicant runs software that participates in the authentication process.

### Authenticator

The **authenticator** is the network device controlling access.

On a wired network, this is typically a switch.

On a wireless network, the access point or WLAN infrastructure performs the authenticator role.

The authenticator controls whether the client receives normal network access.

### Authentication Server

The authentication server validates the authentication attempt.

In enterprise deployments, **RADIUS** is commonly used as the backend authentication protocol between the network access device and the authentication infrastructure.

The overall model can therefore look like:

```text
Client
Supplicant
    │
    │ Authentication
    ▼
Switch / AP
Authenticator
    │
    │ RADIUS
    ▼
Authentication Server
```

### EAP and 802.1X

802.1X commonly uses **EAP**, the Extensible Authentication Protocol, to carry authentication methods.

EAP itself is not one single authentication mechanism.

Instead, it supports different EAP methods.

Examples include methods based on:

- Certificates
    
- User credentials
    
- Tunneled authentication
    

The security properties therefore depend significantly on which EAP method is deployed and how it is configured.

This is an important distinction:

> **802.1X provides the access-control framework; the chosen EAP method determines how authentication is performed.**

### What Happens During 802.1X Authentication?

A simplified wired sequence looks like:

```text
1. Device connects to switch
          ↓
2. Switch places port in restricted state
          ↓
3. Client begins authentication
          ↓
4. Authentication exchange occurs
          ↓
5. Switch forwards authentication information
          ↓
6. Authentication server validates credentials
          ↓
7. Authentication succeeds or fails
          ↓
8. Network access is granted or denied
```

Before successful authentication, the port can restrict normal network access.

After successful authentication, the network can authorize the client.

This creates a fundamentally different security model from MAC filtering.

### MAC Filtering vs 802.1X

Consider the difference.

#### MAC Filtering

```text
Device
  ↓
"What MAC are you using?"
  ↓
Is MAC on allowlist?
  ↓
Allow / Deny
```

The control depends on an identifier that can potentially be copied.

#### 802.1X

```text
Device
  ↓
"Authenticate"
  ↓
Authentication exchange
  ↓
Authentication server validates identity
  ↓
Allow / Deny
```

The control is based on an authentication mechanism rather than simply trusting a visible Layer 2 address.

This is why 802.1X is substantially stronger for enterprise access control.

### Why 802.1X Is More Difficult to Bypass

An attacker who changes their MAC address does not automatically possess the credentials or cryptographic material required by a properly configured 802.1X deployment.

For example, if an organization uses certificate-based authentication, simply copying:

```text
00:11:22:33:44:55
```

does not provide the attacker's system with the legitimate client's private key and certificate credentials.

The attacker has copied an address, not the authentication secret.

This illustrates a broader security principle:

> **Identifiers should not be confused with authenticators.**

A MAC address is primarily an identifier.

A password, certificate, or cryptographic credential can serve as an authenticator.

### MAC Spoofing and Network Detection

Although MAC spoofing can bypass weak controls, it can also produce detectable anomalies.

For example, network administrators may observe:

```text
00:11:22:33:44:55 → Gi0/10
```

and later:

```text
00:11:22:33:44:55 → Gi0/24
```

If the same MAC repeatedly appears on different ports, this can indicate:

- A legitimate device movement
    
- A Layer 2 topology change
    
- Virtual machine mobility
    
- A bridging issue
    
- A switching loop
    
- MAC spoofing
    

The correct interpretation depends on the environment.

Security monitoring should therefore look at **behavior and context**, not just the MAC address itself.

### Layer 2 Security Is Defense in Depth

A secure network should not depend on one identifier or one control.

A more mature architecture can combine:

```text
802.1X
   +
VLAN segmentation
   +
Switch port security
   +
DHCP / IP controls
   +
Network monitoring
   +
Endpoint security
   +
Layer 3 access controls
```

Each control addresses a different part of the security problem.

For example:

- **802.1X** controls network admission.
    
- **VLANs** provide Layer 2 segmentation.
    
- **Port security** restricts unexpected MAC behavior.
    
- **Layer 3 ACLs** control IP-level communication.
    
- **Monitoring** detects anomalies.
    
- **Endpoint security** protects the device itself.
    

This layered approach is much more effective than attempting to make a MAC address serve as a complete security identity.

### A Red-Team Perspective

From a red-team perspective, MAC addresses should be treated as **observable and potentially manipulable information**.

When assessing a network under explicit authorization, a tester may examine questions such as:

- Are network access decisions based solely on MAC addresses?
    
- Can an unauthorized device connect by changing its MAC?
    
- Are switch ports protected against unexpected addresses?
    
- Is 802.1X deployed?
    
- What happens when authentication fails?
    
- Are authenticated devices placed into appropriate VLANs?
    
- Are suspicious MAC movements monitored?
    
- Are Layer 2 controls backed by Layer 3 and endpoint controls?
    

The important lesson is not simply:

> "MAC spoofing works."

The deeper lesson is:

> **A security control is only as strong as the property it actually verifies.**

If a control verifies only a MAC address, it verifies only the presented Layer 2 identifier.

It does not automatically verify the identity of the device, user, or organization behind that identifier.

### Security Model to Remember

The entire lesson can be reduced to three concepts:

```text
MAC Address
    ↓
Layer 2 identification
    ↓
Useful for forwarding
```

```text
MAC Filtering
    ↓
Layer 2 access restriction
    ↓
Weak because MACs can be observed and changed
```

```text
802.1X
    ↓
Port-based network access control
    ↓
Authentication before normal network access
```

Understanding this progression makes it much easier to understand why modern enterprise networks do not rely on MAC addresses alone for admission control.

## Summary

MAC addresses are essential for Ethernet forwarding, but they are **not strong security identities**. A device can potentially change or spoof the MAC address it presents to the network.

**MAC spoofing** is the act of changing an interface's Layer 2 address. Attackers may use it to bypass weak controls that assume a specific MAC address represents an authorized device. However, spoofing a MAC does not automatically provide the legitimate device's credentials, private keys, sessions, or other authentication material.

**MAC filtering** and basic MAC-based controls can provide useful administrative restrictions, but they should not be treated as strong authentication. **Switch port security** provides additional Layer 2 enforcement but still depends on MAC-based identification.

**802.1X** provides a stronger model through port-based network access control. It uses a supplicant, authenticator, and authentication server, with EAP-based authentication methods and commonly RADIUS as the backend authentication protocol.

The fundamental security principle is:

> **A MAC address tells the network what Layer 2 identity is being presented; authentication provides evidence that the entity presenting that identity is authorized.**

## Key Takeaways

- A MAC address is an **identifier**, not proof of identity.
    
- MAC addresses can be **changed, randomized, virtualized, or spoofed**.
    
- **MAC spoofing** changes the Layer 2 address presented by an interface.
    
- MAC spoofing can be useful against **weak MAC-based access controls**.
    
- Spoofing a MAC does **not automatically compromise the original device**.
    
- **MAC filtering is not strong authentication** because MAC addresses can be observed and potentially copied.
    
- **Switch port security** can restrict which MAC addresses appear on particular ports, but it still relies on MAC-based identification.
    
- A switch's MAC table tells it **where a MAC was learned**, not whether that MAC is trustworthy.
    
- **802.1X provides port-based network access control**.
    
- The three common 802.1X roles are:
    
    - **Supplicant** — client requesting access
        
    - **Authenticator** — switch or access point controlling access
        
    - **Authentication server** — validates authentication
        
- **EAP** provides the framework for authentication methods used with 802.1X.
    
- **RADIUS** is commonly used between the authenticator and authentication infrastructure.
    
- The security of 802.1X depends on the **authentication method and its configuration**.
    
- Certificate-based authentication can provide substantially stronger protection against simple MAC spoofing.
    
- Layer 2 security should use **defense in depth**, combining authentication, segmentation, port controls, monitoring, and higher-layer security.
    
- The key distinction is:

```text
Identifier ≠ Authentication
```

- MAC addresses are excellent for **Ethernet forwarding**, but they should not be trusted as the sole basis for **network identity or authorization**.

