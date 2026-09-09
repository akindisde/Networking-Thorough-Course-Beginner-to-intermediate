### Part 1 — VLAN Fundamentals and 802.1Q

### 1. The Problem VLANs Solve

A traditional Layer 2 network can be built with one physical switch connecting many devices.

For example:

```
             +----------------+
             |    Switch      |
             +----------------+
              |  |  |  |  |
              |  |  |  |  |
             PC PC PC PC PC
```

All of these devices may belong to the same Layer 2 broadcast domain.

This becomes a problem in larger networks.

Imagine an enterprise with:

- Employees
    
- Servers
    
- Printers
    
- IP phones
    
- Guest devices
    
- Network-management systems
    

If everything is placed into one Layer 2 network, devices from completely different roles share the same Layer 2 domain.

This creates several problems:

- Broadcast traffic reaches too many devices.
    
- Network segmentation becomes difficult.
    
- Security boundaries are weaker.
    
- Troubleshooting becomes harder.
    
- Different network policies cannot be cleanly applied.
    
- Guest and internal traffic may share the same Layer 2 domain.
    

A **VLAN**, or **Virtual Local Area Network**, solves this by allowing one physical switching infrastructure to contain multiple logical Layer 2 networks.

Instead of:

```
One switch
    ↓
One Layer 2 network
```

we can have:

```
One physical switch
        ↓
┌───────┼────────┬────────┐
↓       ↓        ↓        ↓
VLAN 10 VLAN 20  VLAN 30  VLAN 40
Users   Servers  Printers Guest
```

The physical infrastructure is shared, but the Layer 2 broadcast domains are logically separated.

### 2. What Is a VLAN?

A **VLAN** is a logical Layer 2 network created within switching infrastructure.

Each VLAN represents a separate Layer 2 broadcast domain.

For example:

```
VLAN 10 → Users
VLAN 20 → Servers
VLAN 30 → Printers
VLAN 40 → Guests
```

Devices in VLAN 10 are normally isolated at Layer 2 from devices in VLAN 20.

A broadcast generated in VLAN 10 does not normally cross into VLAN 20.

Conceptually:

```
              Switch
        ┌─────────────────┐
        │                 │
VLAN 10 │ PC1   PC2   PC3 │
        │                 │
        ├─────────────────┤
VLAN 20 │ Server1 Server2 │
        │                 │
        └─────────────────┘
```

The switch may be one physical device, but logically it contains multiple Layer 2 networks.

### 3. VLANs Create Separate Broadcast Domains

One of the most important properties of VLANs is **broadcast-domain separation**.

Without VLANs:

```
PC1 ─┐
PC2 ─┤
PC3 ─┤── Switch ── One broadcast domain
PC4 ─┤
PC5 ─┘
```

With VLANs:

```
             Switch
          ┌───────────┐
VLAN 10   │ PC1 PC2   │
          ├───────────┤
VLAN 20   │ PC3 PC4   │
          └───────────┘
```

A broadcast from PC1 in VLAN 10 remains within VLAN 10 at Layer 2.

It does not automatically reach PC3 and PC4 in VLAN 20.

This is a major reason VLANs are used in enterprise networks.

### 4. VLAN IDs

Each VLAN is identified by a **VLAN ID**.

The VLAN ID is a numerical identifier.

Examples:

```
VLAN 10
VLAN 20
VLAN 30
VLAN 100
VLAN 200
```

The number itself does not inherently determine the purpose.

For example:

```
VLAN 10 = Users
```

is an administrative design decision.

Another organization could use:

```
VLAN 10 = Management
```

The important point is that the network administrator defines the VLAN-to-purpose mapping.

A well-designed enterprise network normally uses a documented and consistent VLAN numbering scheme.

### 5. VLANs on a Physical Switch

A switch can have ports assigned to different VLANs.

For example:

```
Switch

Fa0/1 → VLAN 10
Fa0/2 → VLAN 10
Fa0/3 → VLAN 10

Fa0/4 → VLAN 20
Fa0/5 → VLAN 20
Fa0/6 → VLAN 20
```

This produces:

```
VLAN 10
PC1 ──┐
PC2 ──┼── Switch
PC3 ──┘

VLAN 20
Server1 ──┐
Server2 ──┼── Switch
Printer ──┘
```

Although all devices connect to the same physical switch, the VLAN configuration determines which Layer 2 broadcast domain each port belongs to.

### 6. VLANs and Layer 2 Isolation

VLAN separation provides **Layer 2 isolation**, not complete security by itself.

For example:

```
VLAN 10
10.10.10.0/24

VLAN 20
10.10.20.0/24
```

A device in VLAN 10 cannot simply send an ordinary Layer 2 Ethernet frame directly to a device in VLAN 20.

The VLAN boundary prevents normal Layer 2 forwarding between them.

However, if routing is configured between the VLANs, communication can occur through Layer 3:

```
VLAN 10
   ↓
Layer 3 router/switch
   ↓
VLAN 20
```

Therefore:

> **VLANs separate Layer 2 domains; they do not automatically create an absolute security boundary against all traffic.**

This distinction becomes important when we study inter-VLAN routing later.

### 7. How Can Multiple VLANs Cross One Link?

Now consider a more realistic network.

There are two switches:

```
Switch A                     Switch B

PC1 ── VLAN 10              VLAN 10 ── PC3
PC2 ── VLAN 20              VLAN 20 ── PC4
       |                            |
       +──────────── Link ──────────+
```

The link between the switches must carry traffic for multiple VLANs.

If the link simply transmitted ordinary untagged Ethernet frames, the receiving switch would have difficulty determining which VLAN each frame belongs to.

This is where **VLAN tagging** becomes important.

The most widely used Ethernet VLAN tagging mechanism is **IEEE 802.1Q**.

### 8. IEEE 802.1Q

**IEEE 802.1Q** defines a VLAN tagging mechanism that allows Ethernet frames to carry VLAN information.

A VLAN tag is inserted into the Ethernet frame so that network devices can identify the VLAN associated with the frame.

Conceptually:

```
Normal Ethernet frame:

+----------+----------+-------------+------+
| Dest MAC | Src MAC  | EtherType   | Data |
+----------+----------+-------------+------+

802.1Q-tagged frame:

+----------+----------+----------+-------------+------+
| Dest MAC | Src MAC  | 802.1Q   | EtherType   | Data |
+----------+----------+----------+-------------+------+
                         tag
```

The 802.1Q tag is **4 bytes** long.

It contains several fields, including the VLAN Identifier.

### 9. The 4-Byte 802.1Q Tag

The 802.1Q tag is inserted between the source MAC address and the original EtherType/Length field.

Conceptually:

```
+-----------+-----------+------------+-------------+------+
| Dest MAC  | Source MAC| 802.1Q Tag | EtherType   | Data |
+-----------+-----------+------------+-------------+------+
                         4 bytes
```

The tag contains:

- **TPID** — Tag Protocol Identifier
    
- **TCI** — Tag Control Information
    

The TCI contains:

- **PCP** — Priority Code Point
    
- **DEI** — Drop Eligible Indicator
    
- **VID** — VLAN Identifier
    

A simplified representation is:

```
802.1Q Tag
┌────────────────────────────────────┐
│ TPID                                │
├──────────┬─────┬───────────────────┤
│ PCP      │ DEI │ VLAN ID           │
└──────────┴─────┴───────────────────┘
```

The VLAN Identifier is the field that identifies the VLAN.

### 10. VLAN ID Field

The VLAN ID, or **VID**, is a **12-bit field** within the 802.1Q tag.

A 12-bit field can represent values from:

```
0–4095
```

However, not every value represents a normal usable VLAN.

In the traditional 802.1Q VLAN numbering model:

- VLAN ID `0` is reserved for priority tagging and does not identify a VLAN.
    
- VLAN ID `4095` is reserved.
    
- VLAN IDs `1–4094` are the usable VLAN-ID range, subject to platform and standards considerations.
    

Therefore, you should not simply assume that all 4096 possible values are ordinary VLANs.

### 11. How 802.1Q Tagging Works

Consider a device in VLAN 10 connected to Switch A.

The frame enters Switch A through an access port associated with VLAN 10.

The switch knows:

```
Incoming port → VLAN 10
```

If the frame needs to cross a trunk link, the switch can transmit it with an 802.1Q tag identifying VLAN 10.

Conceptually:

```
PC
 |
 | Untagged
 v
Access Port
 |
Switch A
 |
 | VLAN 10 tagged
 v
Trunk
 |
Switch B
```

Switch B reads the VLAN information in the tag and knows:

```
This frame belongs to VLAN 10.
```

It can then forward the frame within VLAN 10.

The tag therefore allows multiple VLANs to share the same physical trunk link.

### 12. Multiple VLANs on One Trunk

A trunk can carry frames belonging to multiple VLANs.

For example:

```
Switch A                     Switch B

VLAN 10 ─┐                 ┌─ VLAN 10
VLAN 20 ─┼─── Trunk ───────┼─ VLAN 20
VLAN 30 ─┤                 ├─ VLAN 30
VLAN 40 ─┘                 └─ VLAN 40
```

The physical connection is only one link, but logically it carries traffic for several VLANs.

Frames can be identified by their VLAN tags:

```
Frame A → VLAN 10
Frame B → VLAN 20
Frame C → VLAN 30
Frame D → VLAN 40
```

This is much more scalable than requiring a separate physical link for every VLAN.

### 13. Access Ports

An **access port** is normally associated with a single VLAN and is typically used for end devices.

Examples include:

- PCs
    
- Servers
    
- Printers
    
- IP phones, depending on configuration
    
- Other endpoint devices
    

For example:

```
Switch port Fa0/1
        ↓
Access VLAN 10
        ↓
PC
```

The endpoint normally sends and receives ordinary **untagged Ethernet frames**.

The switch associates those frames with the configured access VLAN.

Conceptually:

```
PC
 |
 | Untagged Ethernet
 |
 v
Access Port
 |
 +── VLAN 10
```

The endpoint does not normally need to understand the switch's VLAN configuration.

### 14. Access Port Example

Suppose:

```
Fa0/1 → Access VLAN 10
Fa0/2 → Access VLAN 10
Fa0/3 → Access VLAN 20
Fa0/4 → Access VLAN 20
```

Then:

```
PC1 ── Fa0/1 ── VLAN 10
PC2 ── Fa0/2 ── VLAN 10

PC3 ── Fa0/3 ── VLAN 20
PC4 ── Fa0/4 ── VLAN 20
```

PC1 and PC2 are in the same Layer 2 broadcast domain.

PC3 and PC4 are in another.

### 15. Trunk Ports

A **trunk port** is designed to carry traffic belonging to multiple VLANs over a single physical link.

Typical uses include:

- Switch-to-switch links
    
- Switch-to-router links for router-on-a-stick
    
- Switch-to-other Layer 3 infrastructure, depending on design
    

Conceptually:

```
Switch A
    |
    | Trunk
    |
Switch B
```

The trunk can carry:

```
VLAN 10
VLAN 20
VLAN 30
VLAN 40
```

Unlike a typical access port, a trunk needs a mechanism to identify the VLAN associated with each frame.

That is where 802.1Q tagging is used.

### 16. Access vs Trunk

The difference is fundamental.

|Access Port|Trunk Port|
|---|---|
|Normally carries one VLAN|Carries multiple VLANs|
|Typically used for endpoints|Typically used between network devices|
|Endpoint traffic is normally untagged|VLAN information is normally carried using 802.1Q tagging|
|Belongs to one access VLAN|Has a set of allowed/active VLANs|
|Common for PCs and printers|Common for switch-to-switch links|

Conceptually:

```
Access:

PC ── untagged ── Switch
                   |
                 VLAN 10
```

```
Trunk:

Switch A ── tagged VLAN 10/20/30 ── Switch B
```

The word **normally** is important.

Network equipment and endpoint devices can have more advanced configurations, so the exact tagging behavior depends on the topology and configuration.

### 17. Native VLAN

802.1Q trunks have a special concept called the **native VLAN**.

The native VLAN is the VLAN associated with traffic that is transmitted **untagged** on an 802.1Q trunk.

Conceptually:

```
Trunk
 ├── VLAN 10 → tagged
 ├── VLAN 20 → tagged
 ├── VLAN 30 → tagged
 └── Native VLAN → untagged
```

This means that not every frame crossing a trunk must necessarily contain an 802.1Q tag.

The native VLAN provides a defined VLAN context for untagged traffic on the trunk.

### 18. Why the Native VLAN Matters

The native VLAN is important because both ends of a trunk need to agree about how untagged traffic should be interpreted.

Suppose:

```
Switch A native VLAN = 99
Switch B native VLAN = 1
```

An untagged frame sent across the trunk could be interpreted differently by the two switches.

This creates a **native VLAN mismatch**.

Native VLAN mismatches can cause:

- Connectivity problems
    
- Unexpected traffic placement
    
- Troubleshooting difficulties
    
- Security concerns
    

Therefore, trunk configurations should be designed consistently.

### 19. Why Changing the Native VLAN From VLAN 1 Matters

VLAN 1 has historically been the default VLAN on many enterprise switches.

Leaving the native VLAN as VLAN 1 is not automatically a vulnerability, but using VLAN 1 as the native VLAN can make the network harder to segment cleanly because VLAN 1 often has special default behavior and may be used by legacy control protocols or default switch configurations.

A common security design is to use a dedicated, otherwise-unused VLAN as the native VLAN.

For example:

```
VLAN 999 → Native VLAN
```

while production VLANs use:

```
VLAN 10 → Users
VLAN 20 → Servers
VLAN 30 → Printers
VLAN 40 → Voice
```

The exact VLAN number is an administrative choice.

The important principle is:

> **Do not treat the native VLAN as an ordinary user-access VLAN.**

Changing the native VLAN alone does not secure a network. It is one component of a broader trunk-hardening strategy.

### 20. Native VLAN and Untagged Traffic

Consider a trunk carrying:

```
VLAN 10
VLAN 20
VLAN 30
Native VLAN 999
```

Conceptually:

```
VLAN 10 → tagged
VLAN 20 → tagged
VLAN 30 → tagged
VLAN 999 → untagged
```

The receiving switch interprets an untagged frame arriving on the trunk as belonging to the native VLAN.

This is why native VLAN configuration must match on both ends.

A mismatch can effectively place untagged traffic into different VLANs on different sides of the link.

### 21. VLANs Do Not Automatically Provide Routing

A common misconception is:

> “If I create VLAN 10 and VLAN 20, the devices can communicate between them automatically.”

They cannot communicate directly at Layer 2.

For example:

```
VLAN 10
PC1
 |
 +── Switch

VLAN 20
PC2
 |
 +── Switch
```

The switch keeps these VLANs separate.

To communicate between them, the network needs a **Layer 3 routing function**.

That topic will be covered in Part 2.

Conceptually:

```
VLAN 10
    ↓
Layer 3 routing
    ↓
VLAN 20
```

### 22. The Bigger Picture

VLANs allow physical infrastructure to be separated into logical networks.

The basic hierarchy is:

```
Physical infrastructure
        ↓
Switch
        ↓
VLANs
        ↓
Separate Layer 2 broadcast domains
        ↓
Trunks carry multiple VLANs
        ↓
802.1Q identifies VLAN membership
        ↓
Layer 3 routing can connect VLANs
```

This is one of the fundamental building blocks of enterprise network design.

### 23. Key Concepts to Remember

A few distinctions should become automatic:

```
VLAN
↓
Logical Layer 2 network
```

```
Access port
↓
One VLAN
↓
Typically untagged endpoint traffic
```

```
Trunk port
↓
Multiple VLANs
↓
802.1Q tagging
```

```
Native VLAN
↓
Untagged VLAN traffic on an 802.1Q trunk
```

```
802.1Q
↓
4-byte VLAN tag
↓
12-bit VLAN ID field
```

And most importantly:

```
VLANs separate Layer 2 domains.
Routing connects different VLANs at Layer 3.
```

## Summary

VLANs allow a single physical switching infrastructure to support multiple logical Layer 2 networks. Each VLAN represents a separate broadcast domain, allowing organizations to separate users, servers, printers, guests, voice devices, and management traffic without requiring a separate physical switch for every network.

IEEE **802.1Q** provides the standard VLAN-tagging mechanism used to identify VLAN traffic across trunk links. The 802.1Q tag is **4 bytes** and contains fields including the **12-bit VLAN Identifier (VID)**.

**Access ports** are normally assigned to one VLAN and are commonly used for endpoint devices, whose traffic is typically untagged. **Trunk ports** carry multiple VLANs over one physical link and normally use 802.1Q tags to identify VLAN membership.

The **native VLAN** is the VLAN associated with untagged traffic on an 802.1Q trunk. A dedicated native VLAN is commonly used instead of VLAN 1 as part of a cleaner and more secure trunk design, although changing the native VLAN by itself is not a complete security control.

VLANs provide Layer 2 separation, but communication between different VLANs requires Layer 3 routing.

## Key Takeaways

- A **VLAN** is a logical Layer 2 network.
    
- Each VLAN normally represents a separate **broadcast domain**.
    
- VLANs allow multiple logical networks to share the same physical switching infrastructure.
    
- A **VLAN ID** identifies a VLAN.
    
- IEEE **802.1Q** is the standard VLAN-tagging mechanism used on Ethernet trunks.
    
- An 802.1Q tag is **4 bytes** long.
    
- The VLAN Identifier (**VID**) is a **12-bit** field.
    
- VLAN ID `0` and `4095` are reserved; the traditional usable VLAN-ID range is `1–4094`.
    
- An **access port** normally carries one VLAN and is commonly used for endpoint devices.
    
- Endpoint traffic on an access port is normally **untagged**.
    
- A **trunk port** carries multiple VLANs over one physical link.
    
- 802.1Q tagging allows switches to identify which VLAN a tagged frame belongs to.
    
- The **native VLAN** is associated with untagged traffic on an 802.1Q trunk.
    
- Both ends of a trunk should have a consistent native VLAN configuration.
    
- Using a dedicated native VLAN instead of VLAN 1 is a common trunk-hardening practice.
    
- VLANs provide **Layer 2 separation**, not automatic Layer 3 security.
    
- Communication between different VLANs requires **inter-VLAN routing**.

### Part 2 — Enterprise VLAN Design and Inter-VLAN Routing

### 1. Why Enterprise Networks Use Multiple VLANs

Creating VLANs is not only about dividing a switch into smaller broadcast domains.

In an enterprise network, VLANs are normally designed around **roles, security requirements, and operational boundaries**.

A typical organization may separate:

```
VLAN 10 → Management
VLAN 20 → Servers
VLAN 30 → Users
VLAN 40 → Printers
VLAN 50 → Guest
VLAN 60 → VoIP
```

Each VLAN represents a separate Layer 2 network.

This allows administrators to apply different:

- IP addressing schemes
    
- Security policies
    
- Access-control policies
    
- Quality-of-service policies
    
- Monitoring policies
    
- Routing rules
    

The VLAN structure therefore becomes part of the network's overall architecture.

### 2. Enterprise VLAN Template

A common enterprise design separates networks by function.

One example is:

|VLAN|Purpose|Typical devices|
|---|---|---|
|10|Management|Switches, routers, APs, controllers|
|20|Servers|Application and infrastructure servers|
|30|Users|Employee workstations|
|40|Printers|Network printers|
|50|Guest|Guest laptops and mobile devices|
|60|VoIP|IP phones and voice endpoints|

The exact VLAN numbers are not universal.

What matters is that the design is:

- Consistent
    
- Documented
    
- Scalable
    
- Easy to troubleshoot
    
- Aligned with security requirements
    

For example, another organization could use VLAN 100 for users and VLAN 200 for servers.

The number is not the security boundary.

The **VLAN membership and associated policies** are.

### 3. Management VLAN

A **management VLAN** is commonly used for administrative access to network infrastructure.

Typical devices include:

- Switch management interfaces
    
- Wireless access points
    
- Wireless controllers
    
- Network appliances
    
- Other infrastructure components
    

For example:

```
VLAN 10
Management

10.10.10.0/24
```

Network devices can have management IP addresses within this network.

Conceptually:

```
Administrator
      |
      ↓
Management network
      |
 ┌────┼────┐
 ↓    ↓    ↓
SW1  SW2   AP1
```

The goal is to prevent ordinary user traffic from sharing the same management network.

Management access should also be controlled by appropriate Layer 3 security policies and management-plane protections.

### 4. Server VLAN

Servers are commonly placed into one or more dedicated VLANs.

For example:

```
VLAN 20
Servers
10.10.20.0/24
```

This creates a clear Layer 2 boundary between servers and ordinary user devices.

For example:

```
Users VLAN
     |
     | Layer 3 policy
     ↓
Servers VLAN
```

The router or Layer 3 switch can then enforce policies between these networks.

This is much more flexible than allowing every device to communicate freely within one large broadcast domain.

### 5. User VLAN

The user VLAN contains ordinary employee endpoint devices such as:

- Desktop computers
    
- Laptops
    
- Corporate workstations
    

For example:

```
VLAN 30
Users
10.10.30.0/24
```

In a larger organization, there may be multiple user VLANs rather than one global user VLAN.

For example:

```
VLAN 30 → Users-Floor1
VLAN 31 → Users-Floor2
VLAN 32 → Users-Floor3
```

This can reduce broadcast-domain size and allow different policies where necessary.

### 6. Printer VLAN

Printers are often placed into their own VLAN.

For example:

```
VLAN 40
Printers
10.10.40.0/24
```

This provides a useful security boundary.

Users may need to reach printers, but printers generally do not need unrestricted access to user devices.

A Layer 3 firewall or ACL can therefore implement a policy such as:

```
Users → Printers
Allowed as required

Printers → Users
Restricted
```

The VLAN itself creates the Layer 2 separation.

The Layer 3 security policy determines what communication is actually permitted between the networks.

### 7. Guest VLAN

Guest devices should normally be separated from internal corporate networks.

For example:

```
VLAN 50
Guest
10.10.50.0/24
```

A typical security policy might be:

```
Guest VLAN
     |
     ├──→ Internet: allowed
     |
     └──→ Internal networks: denied
```

This is a good example of why VLANs and routing work together.

The VLAN provides Layer 2 separation.

The firewall or Layer 3 device enforces the Layer 3 policy.

### 8. VoIP VLAN

Voice traffic is often placed into a dedicated VLAN.

For example:

```
VLAN 60
VoIP
10.10.60.0/24
```

Separating voice traffic allows the network to apply specific policies such as:

- QoS
    
- Access control
    
- Monitoring
    
- Voice-specific DHCP options
    
- Traffic prioritization
    

Voice VLANs can be deployed using switch features designed for IP telephony.

The exact configuration depends on the switch, phone, and network architecture.

### 9. VLAN Segmentation Is Not the Same as Security

A common mistake is to assume:

> “Different VLANs cannot communicate, so VLANs are a firewall.”

That is incorrect.

VLANs provide Layer 2 separation.

Once routing is introduced, traffic can move between VLANs.

For example:

```
VLAN 30 Users
       |
       ↓
Layer 3 switch
       |
       ↓
VLAN 20 Servers
```

The Layer 3 device can route the traffic.

Security then depends on what policies are applied to that routed traffic.

A secure design therefore combines:

```
VLAN segmentation
       +
Layer 3 routing
       +
ACLs / firewall policies
       +
Authentication and other controls
```

### 10. Why Inter-VLAN Routing Is Needed

Suppose:

```
PC1
VLAN 30
IP: 10.10.30.10

Server1
VLAN 20
IP: 10.10.20.10
```

The two devices belong to different IP networks:

```
10.10.30.0/24
10.10.20.0/24
```

PC1 cannot deliver the packet directly as an ordinary Layer 2 Ethernet frame to Server1.

Instead, PC1 sends the packet to its default gateway:

```
PC1
10.10.30.10
    |
    | Ethernet
    ↓
Default Gateway
10.10.30.1
    |
    | Routing
    ↓
10.10.20.1
    |
    ↓
Server1
10.10.20.10
```

The function that connects different VLANs at Layer 3 is called **inter-VLAN routing**.

### 11. The Default Gateway's Role

Every IP subnet normally needs a Layer 3 gateway when devices must communicate outside their local subnet.

For example:

```
VLAN 30
10.10.30.0/24

Default gateway:
10.10.30.1
```

A host might have:

```
IP address:      10.10.30.10
Subnet mask:     255.255.255.0
Default gateway: 10.10.30.1
```

If the host wants to reach:

```
10.10.30.20
```

the destination is local to its subnet.

If it wants to reach:

```
10.10.20.10
```

the destination is outside its local subnet.

The host therefore sends the Ethernet frame toward its default gateway.

This is the beginning of inter-VLAN routing.

### 12. Router on a Stick

One traditional method of inter-VLAN routing is called **Router on a Stick**.

The basic architecture is:

```
                 Router
              +----------+
              |          |
              | Router   |
              +----------+
                   |
                   | One physical link
                   | carrying multiple VLANs
                   |
                 Trunk
                   |
              +----------+
              |  Switch  |
              +----------+
               /   |                /     |              VLAN 10 VLAN 20 VLAN 30
```

The router uses one physical interface with multiple logical subinterfaces.

Each subinterface is associated with a VLAN.

For example:

```
Router interface:

G0/0.10 → VLAN 10
G0/0.20 → VLAN 20
G0/0.30 → VLAN 30
```

The switch-facing interface is configured as a trunk.

### 13. Router-on-a-Stick Traffic Flow

Suppose a host in VLAN 30 wants to communicate with a server in VLAN 20.

The process is conceptually:

```
Host VLAN 30
      |
      ↓
Access port
      |
      ↓
Switch
      |
      | 802.1Q-tagged VLAN 30
      ↓
Trunk
      |
      ↓
Router VLAN 30 subinterface
      |
      ↓
Routing decision
      |
      ↓
Router VLAN 20 subinterface
      |
      | VLAN 20
      ↓
Trunk
      |
      ↓
Switch
      |
      ↓
Server VLAN 20
```

The router removes the Layer 2 framing it received and creates a new Layer 2 frame for the outgoing VLAN.

The IP packet is routed between the two networks.

### 14. Router-on-a-Stick Cisco Configuration

A simplified Cisco IOS example might look like:

```
interface GigabitEthernet0/0
 no shutdown

interface GigabitEthernet0/0.10
 encapsulation dot1Q 10
 ip address 10.10.10.1 255.255.255.0

interface GigabitEthernet0/0.20
 encapsulation dot1Q 20
 ip address 10.10.20.1 255.255.255.0

interface GigabitEthernet0/0.30
 encapsulation dot1Q 30
 ip address 10.10.30.1 255.255.255.0
```

The corresponding switch port connected to the router must operate as a trunk and carry the required VLANs.

The important relationship is:

```
Router subinterface
        ↕
802.1Q VLAN ID
        ↕
Switch VLAN
```

The exact commands vary by vendor and platform.

### 15. Limitations of Router on a Stick

Router on a Stick is useful and easy to understand, but it has limitations.

All inter-VLAN traffic uses the router-facing link.

For example:

```
VLAN 10 ─┐
VLAN 20 ─┼── Trunk ── Router
VLAN 30 ─┘
```

If many VLANs generate heavy inter-VLAN traffic, that single physical link can become a bottleneck.

The architecture also introduces an additional routing hop through the external router.

For small networks and learning environments, this design can be perfectly appropriate.

For larger networks, a Layer 3 switch is often a better solution.

### 16. Layer 3 Switch

A **Layer 3 switch** combines high-speed Layer 2 switching with Layer 3 routing capabilities.

Instead of sending inter-VLAN traffic to an external router, the switch itself can perform the routing.

Conceptually:

```
             Layer 3 Switch
          +------------------+
          | Switching        |
          | + Routing        |
          +------------------+
            |      |      |
          VLAN 10 VLAN 20 VLAN 30
```

This can provide efficient inter-VLAN routing directly within the switching infrastructure.

### 17. SVI — Switched Virtual Interface

A Layer 3 switch commonly uses an **SVI**, or **Switched Virtual Interface**, as the Layer 3 interface for a VLAN.

For example:

```
VLAN 10
SVI:
10.10.10.1/24

VLAN 20
SVI:
10.10.20.1/24

VLAN 30
SVI:
10.10.30.1/24
```

Each SVI acts as the default gateway for hosts in its VLAN.

Conceptually:

```
VLAN 10
   |
   ↓
SVI 10.10.10.1
   |
   ↓
Layer 3 routing
   |
   ↓
SVI 10.10.20.1
   |
   ↓
VLAN 20
```

The switch can route directly between the VLAN interfaces.

### 18. SVI Configuration Example

A simplified Cisco IOS example is:

```
interface Vlan10
 ip address 10.10.10.1 255.255.255.0
 no shutdown

interface Vlan20
 ip address 10.10.20.1 255.255.255.0
 no shutdown

interface Vlan30
 ip address 10.10.30.1 255.255.255.0
 no shutdown
```

The Layer 3 routing function must also be enabled on platforms where it is not enabled by default.

For Cisco IOS Layer 3 switches, this commonly involves:

```
ip routing
```

The exact requirements vary by switch model and operating system.

### 19. Router on a Stick vs Layer 3 Switch

Both designs provide inter-VLAN routing, but their architecture is different.

|Feature|Router on a Stick|Layer 3 Switch|
|---|---|---|
|Routing device|External router|Switch itself|
|VLAN interfaces|Router subinterfaces|SVIs|
|Switch-router link|Trunk|Not required for internal VLAN routing|
|Typical performance|Depends on router/link|Usually high for supported hardware|
|Scalability|More limited|Generally better|
|Configuration complexity|Relatively simple|More Layer 3 functionality|
|Common use|Small networks, labs|Enterprise networks|

A simplified comparison:

```
Router on a Stick:

VLANs
  ↓
Switch
  ↓
Trunk
  ↓
Router
  ↓
Routing
```

```
Layer 3 Switch:

VLANs
  ↓
Layer 3 Switch
  ↓
SVIs
  ↓
Routing
```

### 20. Layer 2 and Layer 3 Boundaries

A VLAN is fundamentally a Layer 2 concept.

A subnet is fundamentally a Layer 3 concept.

They are often designed together:

```
VLAN 10
        ↕
10.10.10.0/24

VLAN 20
        ↕
10.10.20.0/24
```

This creates a clean relationship:

```
VLAN
↓
Layer 2 broadcast domain

IP subnet
↓
Layer 3 network
```

The Layer 3 gateway provides the transition between them.

### 21. A Complete Enterprise Example

Consider an organization using:

```
VLAN 10 → Management
VLAN 20 → Servers
VLAN 30 → Users
VLAN 40 → Printers
VLAN 50 → Guests
VLAN 60 → VoIP
```

The IP addressing plan could be:

```
VLAN 10 → 10.10.10.0/24
VLAN 20 → 10.10.20.0/24
VLAN 30 → 10.10.30.0/24
VLAN 40 → 10.10.40.0/24
VLAN 50 → 10.10.50.0/24
VLAN 60 → 10.10.60.0/24
```

A Layer 3 switch could provide:

```
VLAN 10 SVI → 10.10.10.1
VLAN 20 SVI → 10.10.20.1
VLAN 30 SVI → 10.10.30.1
VLAN 40 SVI → 10.10.40.1
VLAN 50 SVI → 10.10.50.1
VLAN 60 SVI → 10.10.60.1
```

Now the network has clearly defined Layer 2 and Layer 3 boundaries.

Security policies can then control traffic such as:

```
Users → Servers       Allowed as required
Users → Printers      Allowed as required
Guests → Internet     Allowed
Guests → Internal     Denied
VoIP → Required       Allowed
Management → Devices  Restricted to administrators
```

The exact policy depends on the organization.

### 22. Why VLAN Design Should Follow Function

Poor VLAN design often creates unnecessary complexity.

For example:

```
VLAN 10 → Random mixture of users,
          printers and servers
```

is harder to secure than:

```
VLAN 10 → Management
VLAN 20 → Servers
VLAN 30 → Users
VLAN 40 → Printers
```

Functional segmentation makes it easier to answer questions such as:

- Who should access this network?
    
- Which services should be reachable?
    
- Which traffic should be blocked?
    
- Which devices should receive priority?
    
- Where should monitoring be applied?
    

VLAN design is therefore both a technical and architectural decision.

### 23. Important Design Principle

Do not create VLANs simply because VLANs exist.

Create them when there is a meaningful reason to separate traffic.

Good reasons include:

- Different security requirements
    
- Different administrative ownership
    
- Different trust levels
    
- Different traffic characteristics
    
- Broadcast-domain control
    
- Quality-of-service requirements
    
- Regulatory or organizational segmentation
    

The objective is not to maximize the number of VLANs.

The objective is to create **useful and manageable boundaries**.

## Summary

Enterprise networks commonly use VLANs to separate devices according to function, such as management, servers, users, printers, guests, and VoIP. This segmentation creates clear Layer 2 broadcast domains and provides a foundation for applying different Layer 3 security, routing, and quality-of-service policies.

VLANs cannot communicate with one another directly at Layer 2. **Inter-VLAN routing** is required when traffic must move between VLANs. The default gateway for each VLAN provides the Layer 3 entry point for hosts that need to reach another subnet.

Two major inter-VLAN routing designs are **Router on a Stick** and **Layer 3 switching**. Router on a Stick uses one router interface with multiple 802.1Q subinterfaces connected to a switch trunk. A Layer 3 switch performs routing internally and commonly uses **SVIs** as the Layer 3 interfaces and default gateways for VLANs.

Router on a Stick is useful for smaller networks and learning environments, but the shared trunk can become a bottleneck as inter-VLAN traffic grows. Layer 3 switches generally provide better scalability and performance for enterprise environments.

The key design principle is that VLANs should be created for meaningful architectural, operational, or security boundaries rather than simply increasing the number of networks.

## Key Takeaways

- Enterprise VLANs are commonly organized by **function and trust level**.
    
- A typical template can include:
    
    - Management
        
    - Servers
        
    - Users
        
    - Printers
        
    - Guest
        
    - VoIP
        
- VLAN numbers are administrative identifiers; their numbers do not inherently determine their purpose.
    
- A VLAN provides a **Layer 2 broadcast domain**.
    
- An IP subnet provides a **Layer 3 network**.
    
- VLANs do not automatically provide complete security.
    
- Traffic between VLANs requires **inter-VLAN routing**.
    
- Hosts use a **default gateway** to reach destinations outside their local IP subnet.
    
- **Router on a Stick** uses one physical router interface with multiple VLAN subinterfaces.
    
- Router-on-a-Stick subinterfaces use **802.1Q VLAN identification**.
    
- A **Layer 3 switch** can route between VLANs without sending internal inter-VLAN traffic through an external router.
    
- An **SVI** provides a Layer 3 interface associated with a VLAN.
    
- SVIs commonly serve as the default gateways for their VLANs.
    
- Router on a Stick is useful for smaller or simpler environments but can create a shared-link bottleneck.
    
- Layer 3 switching is generally more scalable for enterprise networks.
    
- VLAN segmentation and Layer 3 security policies should be designed together.
    
- A Guest VLAN should normally have controlled access to internal networks.
    
- A Management VLAN should be protected from ordinary user access.
    
- VLAN design should create meaningful boundaries rather than unnecessary complexity.

