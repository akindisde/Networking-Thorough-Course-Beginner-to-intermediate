## Lesson 2.3 — Network Devices — Part 1

### The Layer 1 and Layer 2 World

Network devices exist because different problems have to be solved at different points in a network.

A physical medium gives us a way to carry signals, but a network needs devices that can receive those signals, interpret information, and decide what to do with traffic.

In this lesson, we begin with two important devices:

```
Hub
Switch
```

They may look similar because both can connect multiple Ethernet devices.

Functionally, however, they are very different.

```
Hub
→ Layer 1
→ Repeats signals

Switch
→ Layer 2
→ Makes forwarding decisions using MAC addresses
```

Understanding this difference is one of the foundations of Ethernet networking.

### Hub

A **hub** is a Layer 1 networking device.

Layer 1 is the **Physical layer**.

A hub does not understand:

```
MAC addresses
IP addresses
TCP
UDP
HTTP
```

It operates on the physical signal.

Its basic job is:

```
Receive a signal
      ↓
Repeat the signal
      ↓
Send it out other ports
```

A hub does not make an intelligent forwarding decision.

### How a Hub Behaves

Imagine four computers connected to a hub:

```
PC-A
  |
  |
PC-B — HUB — PC-C
  |
  |
PC-D
```

Suppose PC-A sends an Ethernet frame.

The hub receives the physical signal from PC-A.

It does not inspect the destination MAC address and ask:

```
"Which port contains the destination?"
```

Instead, it repeats the signal toward the other connected ports.

Conceptually:

```
PC-A
  ↓
HUB
 / | \
↓  ↓  ↓
B  C  D
```

All of those devices receive the physical transmission.

Only the intended destination is supposed to process the frame as its own traffic.

The other devices effectively ignore it at the Ethernet layer because the destination MAC address does not identify them.

### Why This Is Inefficient

Suppose PC-A wants to communicate only with PC-B.

With a hub:

```
PC-A
  ↓
HUB
  ↓
PC-B
PC-C
PC-D
```

PC-C and PC-D still receive the transmission.

The hub has no knowledge of which device should receive it.

This creates unnecessary traffic on the shared medium.

A switch solves this problem by making forwarding decisions.

### Hubs and Shared Ethernet

A hub creates a shared Ethernet environment.

All devices connected through the hub effectively participate in the same shared collision domain.

This matters because Ethernet originally used mechanisms designed to allow multiple devices to share a common medium.

The classic mechanism is:

```
CSMA/CD
```

which stands for:

```
Carrier Sense Multiple Access with Collision Detection
```

The basic idea was:

```
Listen before transmitting
        ↓
Transmit if the medium appears available
        ↓
Detect collisions
        ↓
Wait
        ↓
Retry
```

Modern switched full-duplex Ethernet does not operate this way in normal operation.

Understanding the historical model is still useful because it explains why collision domains are important.

### Collision

A **collision** occurs when two devices transmit onto the same shared medium at overlapping times and their signals interfere with each other.

Conceptually:

```
PC-A ────────┐
             ↓
            HUB
             ↑
PC-B ────────┘
```

If both transmit simultaneously:

```
PC-A →→→
         X
PC-B →→→
```

their transmissions collide.

The resulting signal cannot be correctly interpreted as the original two transmissions.

The devices must recover according to the Ethernet collision-handling mechanism.

### Collision Domain

A **collision domain** is a portion of a network in which devices can potentially interfere with each other's transmissions through collisions.

With a traditional hub:

```
        HUB
     /   |   \
   PC-A PC-B PC-C
```

all connected devices share the same collision domain.

A transmission from one device can interfere with a simultaneous transmission from another.

This is one of the major weaknesses of hubs.

### One Hub, One Collision Domain

A useful mental model is:

```
HUB
 |
 +-- PC-A
 |
 +-- PC-B
 |
 +-- PC-C
 |
 +-- PC-D

All devices
      ↓
Same collision domain
```

Adding more devices to the hub does not create separate collision domains.

It increases the number of devices sharing the same collision domain.

This can increase contention and reduce efficiency.

### Switch

A **switch** is fundamentally different.

A traditional Ethernet switch operates primarily at:

```
Layer 2 — Data Link
```

Instead of simply repeating the physical signal everywhere, the switch examines Ethernet frames and makes forwarding decisions.

The central piece of information is the:

```
MAC address
```

A switch can therefore determine where an Ethernet frame should be sent.

### MAC Addresses

Every Ethernet interface has a MAC address associated with it.

A MAC address identifies a network interface at the Data Link layer.

A simplified example is:

```
00:11:22:33:44:55
```

Another device might have:

```
AA:BB:CC:DD:EE:FF
```

An Ethernet frame contains, among other fields:

```
Source MAC
Destination MAC
```

For example:

```
Source MAC:
00:11:22:33:44:55

Destination MAC:
AA:BB:CC:DD:EE:FF
```

A switch can examine these addresses and use them to make a forwarding decision.

### Switch Forwarding

Imagine this network:

```
PC-A
  |
  |
SWITCH
 /    \
PC-B  PC-C
```

Suppose PC-A sends a frame to PC-B.

The frame contains:

```
Source MAC = PC-A
Destination MAC = PC-B
```

The switch receives the frame.

It examines the destination MAC address.

If the switch knows which port leads to PC-B, it forwards the frame only through that port.

Conceptually:

```
PC-A
  ↓
SWITCH
  ↓
PC-B
```

PC-C does not need to receive the frame.

This is a fundamental improvement over a hub.

### The Key Difference

Compare the two:

```
HUB

Incoming signal
      ↓
Repeat
      ↓
Multiple ports
```

versus:

```
SWITCH

Incoming frame
      ↓
Read Ethernet information
      ↓
Check destination MAC
      ↓
Select forwarding port
      ↓
Forward
```

The hub repeats.

The switch makes a forwarding decision.

### The CAM Table

A switch needs a way to remember which MAC addresses are reachable through which ports.

This information is stored in a table commonly called the:

```
CAM table
```

CAM stands for:

```
Content-Addressable Memory
```

In networking discussions, you may also hear:

```
MAC address table
```

or simply:

```
MAC table
```

For the purposes of this course, think of these as referring to the switch's learned mapping between MAC addresses and switch ports.

A simplified table might look like:

|MAC Address|Port|
|---|---|
|00:11:22:33:44:55|Gi0/1|
|AA:BB:CC:DD:EE|Gi0/2|
|10:20:30:40:50:60|Gi0/3|

The switch uses this information to determine where frames should be forwarded.

### How a Switch Learns

A switch does not necessarily start with a complete table.

It learns MAC addresses from incoming frames.

Suppose PC-A sends a frame into port 1.

The frame contains:

```
Source MAC = 00:11:22:33:44:55
```

The switch receives the frame on:

```
Port 1
```

It can learn:

```
00:11:22:33:44:55 → Port 1
```

Conceptually:

```
Incoming Frame
      ↓
Read Source MAC
      ↓
Associate MAC with Incoming Port
      ↓
Store in MAC/CAM Table
```

This process is called **MAC address learning**.

### Why the Source MAC Is Important

When a frame arrives, the switch can learn from the source.

For example:

```
Frame arrives on Port 4

Source MAC:
AA:AA:AA:AA:AA:AA
```

The switch can record:

```
AA:AA:AA:AA:AA:AA → Port 4
```

The destination MAC is then used to determine where the frame should go.

This gives the switch two important pieces of information:

```
Source MAC
→ Learn where the sender is

Destination MAC
→ Decide where the frame should go
```

This distinction is fundamental to understanding Ethernet switching.

### What If the Destination MAC Is Unknown?

A switch may receive a frame whose destination MAC address is not currently in its MAC table.

For example:

```
Destination MAC:
AA:BB:CC:DD:EE:FF
```

but the switch has no entry for that address.

The switch cannot yet determine the specific destination port.

In this situation, for a normal Ethernet data frame, the switch will generally **flood** the frame out the other relevant ports in the VLAN, except the port on which the frame arrived.

Conceptually:

```
PC-A
  ↓
SWITCH
 / | \
↓  ↓  ↓
B  C  D
```

This may look similar to hub behavior, but the reason is different.

A hub always repeats the signal because it has no Layer 2 intelligence.

A switch floods an unknown unicast because it does not yet know which port contains the destination.

Once the destination device responds and the switch learns its MAC address, future frames can be forwarded more selectively.

### Known Unicast

Once the switch knows the destination MAC:

```
Destination MAC
        ↓
CAM Table
        ↓
Port 2
```

the switch can forward the frame specifically to Port 2.

```
PC-A
  ↓
SWITCH
  ↓
PC-B
```

This is called forwarding a **known unicast**.

The switch does not need to send the frame to every port.

### Broadcast Traffic

There is another important case:

```
Broadcast
```

An Ethernet broadcast frame uses the destination MAC:

```
FF:FF:FF:FF:FF:FF
```

A Layer 2 switch normally floods broadcast frames throughout the relevant VLAN.

Conceptually:

```
          SWITCH
        /   |   \
       ↓    ↓    ↓
     PC-A  PC-B  PC-C
```

Broadcast behavior is different from a known unicast.

This distinction becomes important later when we study ARP, IP networks, and broadcast domains.

### Unknown Unicast vs Broadcast

Do not confuse these two cases.

```
Unknown Unicast

Destination is one specific MAC
but the switch does not know its port.

→ Flood within the VLAN
```

versus:

```
Broadcast

Destination MAC is:
FF:FF:FF:FF:FF:FF

→ Flood within the VLAN
```

The resulting forwarding behavior can look similar, but the reasons are different.

### Switches and Collision Domains

One of the major advantages of a switch is that each switch port can form a separate collision domain.

Consider:

```
        SWITCH
       /   |   \
     PC-A PC-B PC-C
```

Conceptually:

```
PC-A ←→ Port 1
       Collision Domain 1

PC-B ←→ Port 2
       Collision Domain 2

PC-C ←→ Port 3
       Collision Domain 3
```

Each Ethernet segment connected to a switch port is isolated from the others with respect to collisions.

This is fundamentally different from a hub.

With a hub:

```
All ports
   ↓
One shared collision domain
```

With a switch:

```
Each port
   ↓
Separate collision domain
```

### Full-Duplex Ethernet

Modern switched Ethernet commonly operates in:

```
Full-duplex
```

Full-duplex communication means a link can transmit and receive simultaneously.

Conceptually:

```
Device A  ←────────→  Switch
```

Both directions can operate at the same time.

Because the devices are not competing for a shared half-duplex medium in the traditional sense, collisions are not expected on normal full-duplex Ethernet links.

This is another reason modern switched Ethernet is fundamentally different from the old hub-based shared-medium model.

### Hub vs Switch

The comparison should be memorized conceptually, not merely as a table.

|   |   |   |
|---|---|---|
|Property|Hub|Switch|
|Primary Layer|Layer 1|Layer 2|
|Main Operation|Repeats signal|Forwards frames|
|Understands MAC addresses|No|Yes|
|Uses MAC/CAM table|No|Yes|
|Normal forwarding decision|None|Yes|
|Shared collision domain|Yes|Ports are separated|
|Typical modern use|Obsolete|Standard Ethernet device|

The most important row is:

```
Hub → repeats
Switch → forwards
```

### Why Hubs Became Obsolete

Hubs were simple.

They were also inefficient.

Consider a network with:

```
20 devices
```

connected through a hub.

If one device transmits, the signal is repeated to the other devices.

The shared medium creates:

```
More contention
More unnecessary traffic
Potential collisions
Reduced efficiency
```

A switch can isolate each port and forward traffic based on MAC addresses.

Therefore:

```
Hub
→ Shared medium
→ Collisions
→ Inefficient

Switch
→ Segmented links
→ MAC-based forwarding
→ Much more efficient
```

This is why switches replaced hubs in normal Ethernet networks.

### A Critical Mental Model

Do not think:

```
Switch = smarter hub
```

That description is incomplete.

A switch is not simply a hub with more intelligence.

The underlying forwarding model is different.

A hub operates on:

```
Physical signals
```

A switch operates on:

```
Ethernet frames
+
MAC addresses
```

This difference is what allows a switch to make forwarding decisions.

### Tracing a Frame Through a Switch

Suppose:

```
PC-A MAC = AA:AA:AA:AA:AA:AA
PC-B MAC = BB:BB:BB:BB:BB:BB
```

and:

```
PC-A → Switch Port 1
PC-B → Switch Port 2
```

PC-A sends:

```
Source:
AA:AA:AA:AA:AA:AA

Destination:
BB:BB:BB:BB:BB:BB
```

The switch receives the frame on Port 1.

First, it can learn:

```
AA:AA:AA:AA:AA:AA → Port 1
```

Then it checks:

```
BB:BB:BB:BB:BB:BB
```

against its MAC table.

If it finds:

```
BB:BB:BB:BB:BB:BB → Port 2
```

it forwards the frame to Port 2.

The logical process is:

```
Receive frame
      ↓
Learn source MAC
      ↓
Read destination MAC
      ↓
Search CAM/MAC table
      ↓
Destination known?
   /          \
 Yes           No
 ↓              ↓
Forward       Flood
to port       within VLAN
```

This process is one of the core behaviors of Ethernet switching.

### What the Switch Does Not Do

A normal Layer 2 switch does not need to understand the IP address to make a basic Ethernet forwarding decision.

Suppose a frame contains:

```
Destination MAC:
BB:BB:BB:BB:BB:BB
```

The switch can forward it based on that MAC address without needing to understand the application or transport protocol inside the frame.

The hierarchy is:

```
Physical Layer
→ Signals

Data Link Layer
→ Ethernet frames
→ MAC addresses

Network Layer
→ IP packets
→ IP addresses
```

The switch's traditional forwarding decision is primarily based on the Data Link layer.

### Why This Matters for the Next Lesson

We have now reached an important boundary.

A switch can connect devices within a Layer 2 Ethernet network.

But eventually, traffic may need to leave that network.

For example:

```
PC-A
  |
Switch
  |
Local Network
  |
 ???
  |
Another Network
```

A switch is not the device that fundamentally determines how traffic moves between different IP networks.

That is the job of the router.

The next part will build this distinction carefully.

### Key Takeaways

```
A hub is a Layer 1 device.

A hub repeats physical signals to its other ports.

A hub does not understand MAC addresses.

A hub creates a shared collision domain.

Multiple devices connected to the same hub
can contend for the same shared medium.

A switch is primarily a Layer 2 device.

A switch receives Ethernet frames.

A switch examines MAC addresses.

A switch learns source MAC addresses from incoming frames.

The learned MAC-to-port mappings are stored in a
CAM/MAC address table.

A known destination MAC allows the switch to forward
the frame to the appropriate port.

An unknown unicast is generally flooded within the VLAN.

Broadcast frames are also flooded within the VLAN.

Switch ports separate collision domains.

Modern switched Ethernet commonly operates full-duplex,
so collisions are not expected on normal full-duplex links.

Hubs became obsolete because switched Ethernet
provides much more efficient communication.
```

### Final Mental Model

The most important model from this part is:

```
HUB

Signal
  ↓
Repeat
  ↓
Multiple Ports

Layer 1
No MAC intelligence
Shared collision domain
```

versus:

```
SWITCH

Frame
  ↓
Read Source MAC
  ↓
Learn MAC → Port
  ↓
Read Destination MAC
  ↓
Check CAM/MAC Table
  ↓
Forward to Correct Port
```

If you remember only one sentence from this part, remember:

```
A hub repeats signals; a switch reads Ethernet frames
and uses MAC addresses to decide where to forward them.
```

## Lesson 2.3 — Network Devices — Part 2

### The Layer 3 World

A switch is excellent at moving Ethernet frames within a Layer 2 network.

But networks do not exist as one enormous Ethernet segment.

Real networks are divided into separate networks.

Eventually, traffic needs to move from:

```
One Network
    ↓
Another Network
```

This is where the **router** becomes essential.

The central distinction is:

```
Switch
→ Primarily Layer 2
→ Uses MAC addresses
→ Connects devices within a Layer 2 network

Router
→ Layer 3
→ Uses IP addresses
→ Connects different IP networks
```

This distinction is one of the most important concepts in networking.

### Router

A **router** is a Layer 3 networking device.

Layer 3 is the:

```
Network layer
```

The most important addressing system at this layer is:

```
IP addressing
```

A router examines the destination IP address of a packet and determines where that packet should be forwarded.

Conceptually:

```
Incoming Packet
      ↓
Read Destination IP
      ↓
Consult Routing Table
      ↓
Select Next Hop / Interface
      ↓
Forward Packet
```

A router therefore performs a fundamentally different job from a basic Layer 2 switch.

### Why We Need Routers

Imagine two separate networks:

```
Network A

PC-A
  |
Switch
  |
10.0.1.0/24
```

and:

```
Network B

PC-B
  |
Switch
  |
10.0.2.0/24
```

These are different IP networks.

For PC-A to communicate with PC-B, something must connect the two networks.

That device is the router.

Conceptually:

```
10.0.1.0/24
      |
    Switch
      |
     PC-A
      |
      |
    Router
      |
      |
    Switch
      |
     PC-B
      |
10.0.2.0/24
```

The router sits between the networks.

### The Core Rule

The key lesson of this section is:

```
Same subnet
→ Switch handles local Layer 2 delivery

Different subnet
→ Router is needed for Layer 3 forwarding
```

This rule should become automatic when analyzing network traffic.

When a host needs to communicate with another IP address, one of the first questions should be:

```
Is the destination in my local subnet?
```

If yes:

```
Local communication
→ Layer 2 delivery
→ Switch
```

If no:

```
Remote communication
→ Layer 3 forwarding
→ Router
```

### What Is a Subnet?

A subnet is a defined portion of an IP address space.

For example:

```
192.168.1.0/24
```

represents one IPv4 network.

Another network might be:

```
192.168.2.0/24
```

These are separate networks.

A host in:

```
192.168.1.0/24
```

does not consider a host in:

```
192.168.2.0/24
```

to be local.

The subnet mask or prefix length determines this relationship.

### Why the Host Needs to Know

Suppose a computer has:

```
IP address:
192.168.1.10

Subnet:
192.168.1.0/24
```

It wants to communicate with:

```
192.168.1.20
```

The destination is inside the same:

```
192.168.1.0/24
```

network.

The host therefore treats the destination as local.

Now suppose it wants to communicate with:

```
192.168.2.20
```

That destination belongs to:

```
192.168.2.0/24
```

which is a different network.

The host therefore needs to send the traffic toward a router.

### Default Gateway

The device that a host uses to reach other networks is normally called its:

```
Default gateway
```

For example:

```
PC:
IP address      = 192.168.1.10
Subnet mask     = 255.255.255.0
Default gateway = 192.168.1.1
```

The default gateway is normally an interface on a router or Layer 3 device connected to the local network.

Conceptually:

```
PC
 |
 | Local Ethernet
 |
Switch
 |
 |
Router
 |
 | Other Network
 |
Internet
```

When the destination is outside the local subnet, the host sends the traffic toward the default gateway.

### Local Destination

Consider:

```
PC-A:
192.168.1.10/24

PC-B:
192.168.1.20/24
```

Both belong to:

```
192.168.1.0/24
```

Therefore:

```
PC-A
  ↓
Switch
  ↓
PC-B
```

A router is not fundamentally required for this local delivery.

The hosts can communicate across the local Layer 2 network.

### Remote Destination

Now consider:

```
PC-A:
192.168.1.10/24

Server:
192.168.2.20/24
```

These addresses belong to different subnets.

The path becomes conceptually:

```
PC-A
  ↓
Switch
  ↓
Router
  ↓
Switch / Network
  ↓
Server
```

The switch handles local Ethernet delivery toward the router.

The router handles Layer 3 forwarding between networks.

This division of responsibility is fundamental.

### MAC Addresses and IP Addresses Work Together

One common beginner mistake is to think:

```
MAC addresses replace IP addresses
```

or:

```
IP addresses replace MAC addresses
```

They solve different problems.

At a high level:

```
MAC address
→ Layer 2 local delivery

IP address
→ Layer 3 network-to-network delivery
```

When a packet travels across a routed network, both addressing systems can be involved.

The IP addresses identify the endpoints at Layer 3.

The Ethernet MAC addresses identify the local Layer 2 interfaces used on each Ethernet segment.

### A Packet Crossing a Router

Consider:

```
PC-A
192.168.1.10
        |
        |
     Router
        |
        |
Server
192.168.2.20
```

PC-A is communicating with:

```
192.168.2.20
```

The IP packet is addressed toward that destination.

But the packet must first travel across the local Ethernet network to reach the router.

Therefore, the Ethernet frame on the first link has a local Layer 2 destination.

Conceptually:

```
IP Packet

Source IP:
192.168.1.10

Destination IP:
192.168.2.20
```

The Ethernet frame carrying that packet on the first segment has:

```
Source MAC:
PC-A MAC

Destination MAC:
Router's local-interface MAC
```

The router receives the frame.

### What the Router Does

The router removes the incoming Layer 2 framing and processes the Layer 3 packet.

It examines:

```
Destination IP
```

Then it consults its routing table.

Conceptually:

```
Ethernet Frame
      ↓
Router
      ↓
Remove / process Layer 2 framing
      ↓
Inspect IP packet
      ↓
Read Destination IP
      ↓
Routing Table
      ↓
Select outgoing interface
      ↓
Create new Layer 2 frame
      ↓
Forward
```

This is an important concept:

```
The IP packet travels end-to-end.

The Layer 2 frame is local to each link.
```

At every routed hop, the Layer 2 framing can change.

### The Router Rebuilds the Frame

Suppose the packet travels:

```
PC-A → Router → Server
```

On the first Ethernet segment:

```
Source MAC:
PC-A

Destination MAC:
Router
```

After the router forwards the packet onto the next Ethernet segment:

```
Source MAC:
Router

Destination MAC:
Server
```

The Layer 2 addresses have changed.

The Layer 3 destination remains the destination IP of the packet.

Conceptually:

```
FRAME 1

MAC:
PC-A → Router

IP:
PC-A → Server
```

then:

```
FRAME 2

MAC:
Router → Server

IP:
PC-A → Server
```

This illustrates why MAC addresses are local to the Layer 2 network while IP addresses provide the broader Layer 3 addressing structure.

### Routing Table

A router needs information about where different networks can be reached.

That information is stored in a:

```
Routing table
```

A simplified routing table might look like:

|Destination Network|Next Hop / Interface|
|---|---|
|192.168.1.0/24|LAN interface|
|192.168.2.0/24|Interface toward Network B|
|10.0.0.0/8|Interface toward internal network|
|0.0.0.0/0|Upstream router|

The router compares the destination IP address against the routes in the table.

It then chooses the appropriate route.

### Longest Prefix Match

When multiple routes could match a destination, routers generally use the most specific matching route.

This is called:

```
Longest Prefix Match
```

For example, a routing table might contain:

```
10.0.0.0/8
10.1.0.0/16
10.1.2.0/24
```

A packet destined for:

```
10.1.2.50
```

matches all three prefixes.

The router chooses:

```
10.1.2.0/24
```

because it is the most specific matching route.

This concept becomes increasingly important when studying routing in detail.

### Default Route

A router may not have a specific route for every possible destination.

Instead, it can have a:

```
Default route
```

For IPv4, this is commonly represented as:

```
0.0.0.0/0
```

Conceptually:

```
Specific route exists?
       |
      Yes
       ↓
Use specific route

       No
       ↓
Use default route
```

This allows a network device to forward unknown destinations toward an upstream router.

### Router Interfaces

A router normally has multiple interfaces or logical interfaces.

For example:

```
Router

GigabitEthernet0/0
192.168.1.1/24

GigabitEthernet0/1
192.168.2.1/24
```

These interfaces connect the router to different networks.

Conceptually:

```
Network A
192.168.1.0/24
      |
      |
192.168.1.1
   Router
192.168.2.1
      |
      |
Network B
192.168.2.0/24
```

The router is therefore a point where different Layer 3 networks meet.

### A Router Is Not Just a "Bigger Switch"

This distinction matters.

A switch primarily answers:

```
Which local Layer 2 port should receive this Ethernet frame?
```

A router primarily answers:

```
Which Layer 3 path should this IP packet take
to reach its destination network?
```

These are different decisions.

A switch operates primarily with:

```
MAC addresses
Ethernet frames
```

A router operates primarily with:

```
IP addresses
IP packets
Routing tables
```

### The Same-Subnet Test

When troubleshooting a network, start with the IP addressing.

Suppose:

```
Host A:
192.168.10.10/24

Host B:
192.168.10.20/24
```

They are in the same subnet.

Therefore:

```
Local delivery
→ Layer 2
→ Switch
```

Now:

```
Host A:
192.168.10.10/24

Host B:
192.168.20.20/24
```

They are in different subnets.

Therefore:

```
Remote delivery
→ Layer 3
→ Router
```

This simple test is extremely useful.

### Why the Default Gateway Matters

Consider:

```
PC:
192.168.10.10/24

Default gateway:
192.168.10.1
```

The PC can communicate with local hosts without sending traffic through the default gateway.

For a remote destination:

```
8.8.8.8
```

the host recognizes that:

```
8.8.8.8
```

is not part of:

```
192.168.10.0/24
```

It therefore sends the traffic toward:

```
192.168.10.1
```

The router then makes the next Layer 3 forwarding decision.

### Local vs Remote Communication

Keep this model:

```
LOCAL

Host
  ↓
Ethernet
  ↓
Switch
  ↓
Destination Host
```

versus:

```
REMOTE

Host
  ↓
Ethernet
  ↓
Switch
  ↓
Router
  ↓
Other Network
  ↓
Switch
  ↓
Destination Host
```

The router becomes necessary because the destination is outside the local IP network.

### Router vs Switch Summary

|   |   |   |
|---|---|---|
|Property|Switch|Router|
|Primary Layer|Layer 2|Layer 3|
|Main Address|MAC|IP|
|Main Data Unit|Ethernet frame|IP packet|
|Main Table|MAC/CAM table|Routing table|
|Main Job|Local forwarding|Network-to-network forwarding|
|Typical Scope|Local Layer 2 network|Multiple IP networks|
|Broadcast Behavior|Forwards broadcasts within VLAN|Does not normally forward Layer 2 broadcasts between interfaces|

The most important conceptual distinction is:

```
Switch
→ Where is the destination MAC?

Router
→ Where is the destination IP network?
```

### Broadcast Domains

The concept of a **broadcast domain** becomes important when comparing switches and routers.

A Layer 2 broadcast is normally propagated through the relevant switched VLAN.

A router does not normally forward Layer 2 broadcast traffic from one interface to another.

Therefore, routers separate broadcast domains.

Conceptually:

```
Broadcast Domain A
        |
      Router
        |
Broadcast Domain B
```

A broadcast generated in Domain A does not simply cross the router into Domain B as the same Layer 2 broadcast.

This is another major reason routers are used to separate networks.

### Why Routers Matter for Network Design

Without routing, a network would have difficulty scaling into many separate IP networks.

Routers allow engineers to create boundaries between networks.

For example:

```
Users
10.10.10.0/24
       |
     Router
       |
Servers
10.10.20.0/24
       |
     Router
       |
Internet
```

These boundaries allow networks to be:

```
Separated
Addressed independently
Routed independently
Controlled independently
```

This becomes especially important for:

```
Security
Performance
Address management
Network segmentation
Internet connectivity
```

### A Simple Packet Walk

Consider:

```
PC-A
192.168.1.10/24
```

communicating with:

```
Server
192.168.2.20/24
```

The process is:

```
1. PC-A examines the destination IP.

2. PC-A determines that 192.168.2.20
   is outside its local subnet.

3. PC-A decides that the packet must be
   sent toward its default gateway.

4. PC-A uses Layer 2 delivery to reach
   the router's local interface.

5. The switch forwards the Ethernet frame
   toward the router using MAC information.

6. The router receives the frame.

7. The router examines the destination IP.

8. The router consults its routing table.

9. The router selects an outgoing interface
   or next hop.

10. The router creates appropriate Layer 2
    framing for the next network.

11. The packet continues toward the destination.
```

This is the fundamental interaction between:

```
Host
Switch
Router
```

### The Most Important Rule in This Lesson

Memorize the logic, not just the sentence:

```
Destination in my subnet?
        |
     Yes
        ↓
Local Layer 2 delivery
        ↓
Switch
```

versus:

```
Destination outside my subnet?
        |
       Yes
        ↓
Layer 3 forwarding
        ↓
Default Gateway
        ↓
Router
```

This distinction will appear repeatedly throughout networking.

### Key Takeaways

```
A router is primarily a Layer 3 device.

Routers use IP addresses to make forwarding decisions.

Routers connect different IP networks.

A routing table contains information about reachable networks.

A router examines the destination IP and selects
an appropriate route.

A host determines whether a destination is local
or remote using its IP address and subnet information.

Same subnet generally means local Layer 2 delivery.

Different subnet means the host must use a router
to reach the remote network.

The default gateway is the host's normal path
to destinations outside its local subnet.

Switches use MAC addresses for Layer 2 forwarding.

Routers use IP addresses for Layer 3 forwarding.

A packet can remain end-to-end while its Layer 2 frame
changes at each routed hop.

Routers separate Layer 2 broadcast domains.
```

### Final Mental Model

Keep this diagram in your head:

```
SAME SUBNET

PC-A
  |
  ↓
Switch
  |
  ↓
PC-B

MAC addresses
Layer 2
Local network
```

And:

```
DIFFERENT SUBNETS

PC-A
  |
  ↓
Switch
  |
  ↓
Router
  |
  ↓
Switch
  |
  ↓
PC-B

MAC addresses
   ↓
local Ethernet delivery

IP addresses
   ↓
network-to-network delivery
```

The single most important sentence from this part is:

```
If the destination is on the same subnet, Layer 2 can
deliver the traffic locally; if the destination is on a
different subnet, Layer 3 routing is required.
```

## Lesson 2.3 — Network Devices — Part 3

### Devices Around the Network

So far, we have established three important roles:

```
Hub
→ Layer 1
→ Repeats physical signals

Switch
→ Layer 2
→ Forwards Ethernet frames using MAC addresses

Router
→ Layer 3
→ Forwards IP packets between networks
```

Real networks contain additional devices that build on these fundamental functions.

Two especially important devices are:

```
Access Point
Firewall
```

We also need to understand something students encounter constantly in real life:

```
The home router
```

A device sitting next to a home Internet connection may be called a "router", but internally it commonly combines several networking functions into one physical appliance.

### Access Point

An **Access Point**, usually abbreviated:

```
AP
```

provides wireless network access to client devices.

Examples of wireless clients include:

```
Laptop
Phone
Tablet
Wireless printer
IoT device
```

An access point connects wireless clients to a network infrastructure.

Conceptually:

```
          Wi-Fi
     /      |      \
  Laptop  Phone  Tablet
     \      |      /
          Access
          Point
             |
             |
          Ethernet
             |
           Switch
```

The access point acts as a bridge between the wireless side and the wired Ethernet network.

### What an Access Point Does

At a high level:

```
Wireless client
      ↓
Wi-Fi
      ↓
Access Point
      ↓
Ethernet
      ↓
Switch
      ↓
Network
```

The access point allows wireless stations to participate in the Layer 2 network.

This is why a basic access point is generally associated with:

```
Layer 2
```

rather than being treated as a router.

### Access Point vs Router

These two devices are frequently confused because many home devices contain both functions.

A dedicated access point primarily provides:

```
Wireless connectivity
+
Layer 2 bridging
```

A router provides:

```
Layer 3 routing
```

A simplified comparison is:

|Device|Primary Role|
|---|---|
|Access Point|Connects Wi-Fi clients to the Layer 2 network|
|Switch|Connects Ethernet devices within a Layer 2 network|
|Router|Connects different Layer 3 networks|

A dedicated AP does not need to route between IP networks.

For example:

```
Laptop
  |
 Wi-Fi
  |
 AP
  |
Ethernet
  |
Switch
  |
LAN
```

The AP is extending network connectivity from wired Ethernet into the wireless medium.

### Wireless and Ethernet as Two Access Methods

Think of an access point as providing another way for a device to enter the same local network.

For example:

```
                 LAN
                  |
             +----+----+
             |         |
          Switch       AP
          /   \        |
        PC    PC     Wi-Fi
                     /   \
                  Phone Laptop
```

The wired PCs use Ethernet.

The phone and laptop use Wi-Fi.

The access point bridges the wireless clients into the network infrastructure.

### Wireless Is Still a Shared Medium

An important connection to the previous module is that wireless communication uses radio waves.

Unlike a dedicated Ethernet cable between two devices, radio transmission propagates through physical space.

Therefore, multiple nearby wireless devices can share the same radio environment.

Conceptually:

```
        Radio Environment

Laptop  )))))))))
Phone   )))))))))
Tablet  )))))))))
          |
          |
          AP
```

The access point coordinates wireless communication, but the medium itself is fundamentally different from a point-to-point Ethernet cable.

This is why wireless networking has its own mechanisms for controlling access to the medium.

### Access Point and MAC Addresses

Wireless clients still operate with Layer 2 addressing.

A wireless interface has a MAC address.

Therefore, an access point participates in Layer 2 communication.

A simplified model is:

```
Wireless Client
MAC A
    |
    | Wi-Fi
    ↓
Access Point
    |
    | Ethernet
    ↓
Switch
```

The Layer 2 network can therefore extend across the wireless and wired portions.

### Multiple Access Points

Larger networks often use multiple access points.

For example:

```
                    Switch
                  /   |   \
                 /    |    \
               AP1   AP2   AP3
                |     |     |
             Wi-Fi  Wi-Fi  Wi-Fi
```

This allows wireless coverage to extend across a building.

Enterprise wireless systems may centrally manage these access points, but the fundamental concept remains:

```
AP
→ Provides wireless network access
→ Bridges wireless clients into the network
```

### Firewall

A **firewall** controls network traffic according to security policy.

At its simplest, a firewall answers questions such as:

```
Should this traffic be allowed?
Should this traffic be blocked?
```

A firewall therefore introduces a security decision into traffic forwarding.

Conceptually:

```
Network A
    |
    ↓
Firewall
    |
    ↓
Network B
```

Traffic attempts to cross the boundary.

The firewall evaluates the traffic against configured policy.

```
Traffic
   ↓
Firewall
   ↓
Policy decision
 /           \
Allow        Deny
 ↓             ↓
Forward       Drop
```

### Firewall as a Security Boundary

Firewalls are commonly placed between networks with different trust levels.

For example:

```
Internet
    |
    ↓
Firewall
    |
    ↓
Internal Network
```

The Internet is generally considered an untrusted or less-trusted zone.

The internal network is generally more trusted.

The firewall becomes the policy enforcement point between them.

### Zones

A useful way to think about a firewall is through:

```
Security zones
```

For example:

```
Internet
   |
   |
[ Firewall ]
   |
   +---- Internal LAN
   |
   +---- DMZ
```

Different zones can have different security policies.

A simplified policy might say:

```
Internet → Internal
Deny unless explicitly permitted

Internal → Internet
Allow according to policy

Internet → DMZ
Allow selected services

DMZ → Internal
Restrict heavily
```

The exact policy depends on the organization's requirements.

The important concept is:

```
Traffic crossing a security boundary
can be inspected and controlled.
```

### Firewall vs Router

A router and firewall are not inherently the same thing.

A router's fundamental job is:

```
Forward packets between networks.
```

A firewall's fundamental job is:

```
Enforce security policy on traffic.
```

A device can perform both functions.

For example:

```
             Router
               +
             Firewall
               |
               ↓
            Network
```

But conceptually they solve different problems.

A router asks:

```
Where should this packet go?
```

A firewall asks:

```
Should this traffic be allowed to cross this boundary?
```

A modern security appliance may answer both questions.

### Packet Forwarding vs Security Policy

This distinction is important.

Suppose a packet arrives at a device.

A routing decision might determine:

```
Destination network:
10.20.0.0/24

Outgoing interface:
LAN2
```

That answers:

```
Where should it go?
```

A firewall policy may then determine:

```
Source:
Internet

Destination:
10.20.0.50

Protocol:
TCP

Destination port:
22

Policy:
DENY
```

That answers:

```
Should it be allowed?
```

Routing and security policy are different decisions.

### Stateful Firewalls

Modern firewalls commonly track the state of connections.

For example, suppose an internal client initiates a TCP connection to a server on the Internet.

The firewall can track the connection.

Conceptually:

```
Internal Client
      |
      | SYN
      ↓
 Firewall
      |
      ↓
 Internet Server
```

The firewall records information about the connection.

When the response comes back:

```
Internet Server
      |
      | SYN/ACK
      ↓
 Firewall
      |
      ↓
Internal Client
```

the firewall can recognize that the return traffic belongs to an established connection.

This is broadly what is meant by a:

```
Stateful firewall
```

The exact implementation varies between platforms.

### Why Firewalls Matter

Without traffic-control boundaries, every reachable service can potentially be exposed to traffic from networks that should not have access.

Firewalls allow administrators to define policies such as:

```
Allow
Deny
Restrict
Log
Inspect
```

This makes them central components of network security.

### The Home Router

Now we can combine the concepts.

A typical home device may be called:

```
Home router
```

but it commonly combines several functions.

A simplified home network might look like:

```
                Internet
                    |
                    |
              [ Home Box ]
              /    |     \
             /     |      \
          Wi-Fi   LAN    Routing
```

The physical device may contain multiple logical networking functions.

Common functions include:

```
Router
Ethernet switch
Wireless access point
Firewall
NAT gateway
DHCP server
```

This is why calling the entire appliance simply a "router" can be misleading.

### Function 1: Router

The device routes between networks.

For example:

```
Home LAN
192.168.1.0/24
       |
       |
 Home Router
       |
       |
ISP Network
```

The device provides Layer 3 forwarding between the local network and the upstream network.

### Function 2: Ethernet Switch

The device commonly has several LAN Ethernet ports.

For example:

```
             Home Device
          +---------------+
          | Ethernet      |
          | switch        |
          +---------------+
           |    |    |
          PC   TV  Console
```

Those ports provide local Layer 2 connectivity.

Internally, there is a switching function connecting the Ethernet ports.

### Function 3: Wireless Access Point

The same appliance commonly provides Wi-Fi.

Conceptually:

```
             Home Device
          +---------------+
          | Wi-Fi AP      |
          +---------------+
             )))  )))
            Phone Laptop
```

This is the access-point function.

It connects wireless clients to the local network.

### Function 4: Firewall

The appliance commonly includes firewall functionality.

Conceptually:

```
Internet
   |
   ↓
Firewall
   |
   ↓
Home LAN
```

The firewall can enforce rules controlling traffic between the WAN and LAN.

### Function 5: NAT

Many residential gateways also perform:

```
NAT
```

which stands for:

```
Network Address Translation
```

A common home configuration uses private IPv4 addresses internally.

For example:

```
Laptop:
192.168.1.20

Phone:
192.168.1.21

PC:
192.168.1.22
```

The home gateway can translate these internal addresses when traffic goes toward the public Internet.

A simplified conceptual path is:

```
Private LAN
192.168.1.20
      |
      ↓
Home Gateway
      |
      ↓
Public Internet
```

NAT is an important subject in its own right and will be studied more deeply later.

For now, understand it as one of the common functions built into a residential gateway.

### Function 6: DHCP Server

The home gateway may also provide:

```
DHCP
```

DHCP allows clients to obtain network configuration automatically.

A device joining the network may receive:

```
IP address
Subnet mask / prefix
Default gateway
DNS server information
```

For example:

```
Laptop joins Wi-Fi
       ↓
DHCP request
       ↓
Home gateway
       ↓
Configuration supplied
```

This is another service that is often hidden inside the same physical appliance.

### One Box, Many Functions

A typical home gateway can therefore be visualized as:

```
                 HOME GATEWAY

       +--------------------------------+
       |                                |
       |  Router                        |
       |  Ethernet Switch               |
       |  Wireless Access Point         |
       |  Firewall                      |
       |  NAT                            |
       |  DHCP Server                   |
       |                                |
       +--------------------------------+
              |      |      |
            Wi-Fi   LAN    WAN
```

The important lesson is:

```
Physical device ≠ single networking function
```

One physical appliance can contain many logical networking components.

### Why This Matters

If you look at a home gateway and think:

```
"This is just a router."
```

you will miss a large part of what the device actually does.

Instead, ask:

```
What functions are inside this appliance?
```

You may find:

```
Layer 2 switching
Layer 3 routing
Wireless bridging
Firewall filtering
NAT
DHCP
DNS forwarding
VPN functionality
```

The exact feature set depends on the device.

### Device vs Function

This distinction is important throughout networking.

A physical box is a hardware platform.

A networking function is a logical role performed by software, hardware, or both.

For example:

```
One physical appliance
        ↓
Multiple logical functions
```

Conversely, in larger networks:

```
One logical function
        ↓
Dedicated specialized hardware
```

For example, an enterprise may deploy:

```
Dedicated switches
Dedicated routers
Dedicated wireless access points
Dedicated firewalls
```

instead of combining everything into one appliance.

### Home Network Example

Consider a typical home:

```
                 Internet
                    |
                    |
             ISP connection
                    |
              [Home Gateway]
              /     |      \
             /      |       \
          Wi-Fi    LAN      WAN
          /  \      |
       Phone Laptop Switch
                       |
                    Desktop
```

Inside the home gateway:

```
Router
→ Connects home network to ISP network

Switch
→ Connects wired LAN devices

Access Point
→ Connects Wi-Fi devices

Firewall
→ Controls traffic between security zones

NAT
→ Translates private/public addressing

DHCP
→ Automatically configures clients
```

The user sees one box.

The network engineer sees multiple functions.

### Access Point, Switch, Router, Firewall

Keep these roles separate in your mental model:

```
ACCESS POINT

"How do wireless devices join the network?"

→ Wi-Fi
→ Layer 2 bridging
```

```
SWITCH

"Which local Ethernet port should receive this frame?"

→ MAC address
→ Layer 2 forwarding
```

```
ROUTER

"Which network should this IP packet be sent toward?"

→ IP address
→ Layer 3 forwarding
```

```
FIREWALL

"Should this traffic be allowed across this boundary?"

→ Security policy
→ Filtering / inspection
```

These questions are more useful than memorizing device names.

### A Single Traffic Flow

Suppose a laptop connected through Wi-Fi wants to reach a server on another network.

The logical path could be:

```
Laptop
  ↓
Wi-Fi
  ↓
Access Point
  ↓
Ethernet / Switch
  ↓
Router
  ↓
Firewall Policy
  ↓
Upstream Network
  ↓
Destination
```

In a home gateway, many of these functions may occur inside the same physical device.

In an enterprise network, they may be implemented by several different devices.

The architecture can change while the underlying networking functions remain recognizable.

### Key Takeaways

```
An access point provides wireless network access.

A basic access point operates primarily at Layer 2
and bridges wireless clients into a wired network.

Wireless clients still use MAC addresses at Layer 2.

A firewall enforces security policy on network traffic.

A firewall can allow, deny, restrict, log, or inspect traffic
according to configured policy.

Routing and firewalling are different functions.

A router answers:
"Where should the packet go?"

A firewall answers:
"Should this traffic be allowed?"

A typical home gateway may combine several functions:

Router
Ethernet switch
Wireless access point
Firewall
NAT
DHCP server

One physical networking appliance can therefore perform
multiple logical networking functions.
```

### Final Mental Model

Think in terms of questions:

```
ACCESS POINT
"How does this wireless device join the LAN?"
        ↓
Wi-Fi / Layer 2
```

```
SWITCH
"Which local Ethernet port should receive this frame?"
        ↓
MAC / Layer 2
```

```
ROUTER
"Which network should this packet travel toward?"
        ↓
IP / Layer 3
```

```
FIREWALL
"Should this traffic cross this security boundary?"
        ↓
Security policy
```

And for a home gateway:

```
ONE PHYSICAL BOX

        ┌──────────────────────┐
        │ Router               │
        │ Switch               │
        │ Access Point         │
        │ Firewall             │
        │ NAT                  │
        │ DHCP                 │
        └──────────────────────┘
```

The most important lesson is:

```
Do not identify a network device only by its physical box.
Identify the networking functions it performs.
```

That mental model will make enterprise networks, cloud networks, security appliances, and home networks much easier to understand.

## Lesson 2.3 — Network Devices — Part 4

### Lab: Hub vs Switch and the Complete Device Mental Model

This part turns the concepts from the previous sections into an experiment.

The objective is not simply to memorize:

```
Hub = Layer 1
Switch = Layer 2
Router = Layer 3
AP = Wireless Layer 2
Firewall = Traffic control
```

The objective is to observe what these devices actually do and then use that behavior to reason about real networks.

The primary lab uses:

```
Cisco Packet Tracer
```

You will build two small Ethernet networks:

```
Network A
→ Hub

Network B
→ Switch
```

You will then compare their behavior.

### Lab Objectives

By the end of the lab, you should be able to:

```
Identify a hub as a Layer 1 device.

Identify a switch as a Layer 2 device.

Explain why a hub repeats traffic.

Explain why a switch forwards traffic selectively.

Observe a switch learning MAC addresses.

Read a switch MAC address table.

Explain the difference between known-unicast
and unknown-unicast forwarding.

Explain why a hub creates one shared collision domain.

Explain why switch ports provide separate collision domains.

Explain why modern switched Ethernet normally operates
without collisions on full-duplex links.

Explain when a router becomes necessary.
```

### Part 1 — Build the Hub Network

Open Cisco Packet Tracer.

Create a simple topology containing:

```
3 PCs
1 Hub
```

Connect them:

```
PC-A
  |
  |
 HUB
 / \
PC-B PC-C
```

Use Ethernet connections appropriate for the Packet Tracer devices.

The exact interface names may differ depending on the device models you select.

The important topology is:

```
PC-A
  |
  |
Hub
 / \
PC-B PC-C
```

### Configure the PCs

Give the PCs addresses in the same IPv4 subnet.

For example:

```
PC-A
IP: 192.168.10.10
Mask: 255.255.255.0
```

```
PC-B
IP: 192.168.10.20
Mask: 255.255.255.0
```

```
PC-C
IP: 192.168.10.30
Mask: 255.255.255.0
```

A default gateway is not required for communication among these three hosts because they are in the same subnet.

The important point is:

```
192.168.10.0/24
```

is the local network.

### Verify Connectivity

From PC-A, open the command prompt and test:

```
ping 192.168.10.20
```

Then:

```
ping 192.168.10.30
```

The pings should succeed if the topology and addressing are correct.

You have now demonstrated basic local communication through the hub.

### Enter Simulation Mode

Packet Tracer has a simulation mode that allows you to observe packets as they move through the topology.

Switch from:

```
Realtime
```

to:

```
Simulation
```

The interface may vary slightly between Packet Tracer versions, but the objective is the same:

```
Slow down network events
Observe packets
Inspect their movement
```

Generate traffic from PC-A to PC-B.

For example:

```
ping 192.168.10.20
```

Observe what happens at the hub.

### What You Should Observe

The hub does not examine the destination MAC address and select one port.

Instead, the hub repeats the signal toward its other ports.

Conceptually:

```
PC-A
  |
  ↓
HUB
 / \
↓   ↓
B   C
```

The intended destination is:

```
PC-B
```

but PC-C also receives the transmission at the physical level.

This is the behavior that defines the hub's Layer 1 operation.

### Lab Question 1

Answer:

```
PC-A sends traffic specifically to PC-B.
Why does PC-C still receive the transmission
when the network uses a hub?
```

Expected reasoning:

```
Because the hub does not make a Layer 2 forwarding
decision based on the destination MAC address.

It repeats the physical signal toward its other ports.
```

Do not describe the hub as "checking the MAC and deciding to flood."

That would incorrectly attribute Layer 2 intelligence to a Layer 1 device.

### Lab Question 2

How many collision domains exist in this hub topology?

```
PC-A
  |
 HUB
 / \
PC-B PC-C
```

Answer:

```
One collision domain.
```

All three devices share the same collision domain.

### Part 2 — Build the Switch Network

Create a second topology:

```
3 PCs
1 Switch
```

Connect them:

```
PC-A
  |
  |
SWITCH
 /   \
PC-B PC-C
```

Configure the same addressing scheme:

```
PC-A
192.168.10.10/24

PC-B
192.168.10.20/24

PC-C
192.168.10.30/24
```

This keeps the IP layer constant.

The device changes.

That is important because you want to isolate the effect of the networking device.

### Generate Traffic

From PC-A:

```
ping 192.168.10.20
```

Then:

```
ping 192.168.10.30
```

Observe the traffic in Simulation mode.

You should see behavior that differs from the hub topology.

The switch can use Layer 2 information to determine where a known destination is connected.

### The Switch Learns

When frames enter the switch, the switch learns from their source MAC addresses.

Conceptually:

```
Frame arrives on Port 1

Source MAC:
AA:AA:AA:AA:AA:AA

Switch learns:

AA:AA:AA:AA:AA:AA → Port 1
```

After more traffic has passed, the switch can build a table similar to:

```
MAC Address              Port

AA:AA:AA:AA:AA:AA        Port 1
BB:BB:BB:BB:BB:BB        Port 2
CC:CC:CC:CC:CC:CC        Port 3
```

The actual MAC addresses and interface names in Packet Tracer will differ.

The concept is what matters:

```
MAC address → switch port
```

### Inspect the MAC Address Table

On the switch, enter the appropriate CLI context and inspect the MAC address table.

On Cisco IOS devices, a commonly used command is:

```
show mac address-table
```

Depending on the Packet Tracer switch model and IOS version, the displayed columns may include information such as:

```
VLAN
MAC Address
Type
Ports
```

You are looking for learned dynamic MAC entries.

The important relationship is:

```
MAC address
      ↓
Specific switch port
```

### Lab Question 3

Why can the switch forward a frame specifically toward PC-B?

Answer:

```
Because the switch can learn the MAC address
associated with the port where PC-B is connected.
```

The switch therefore has information that a hub does not.

### Known Unicast

Suppose the switch knows:

```
PC-B MAC → Port 2
```

PC-A sends:

```
Destination MAC = PC-B MAC
```

The switch can perform:

```
Receive frame
      ↓
Read destination MAC
      ↓
Find MAC in table
      ↓
Port 2
      ↓
Forward through Port 2
```

This is a:

```
Known unicast
```

The switch does not need to send the frame to PC-C.

### Unknown Unicast

Now consider a situation where the switch does not know the destination MAC.

For example:

```
Destination:
DD:DD:DD:DD:DD:DD
```

but the MAC address table contains no entry for it.

The switch cannot determine which port contains the destination.

It therefore floods the frame within the relevant Layer 2 domain, excluding the incoming port.

Conceptually:

```
          SWITCH
         /      \
        ↓        ↓
      PC-B      PC-C
```

This is why a switch can sometimes appear to behave like a hub.

The crucial difference is:

```
Hub
→ Always repeats

Switch
→ Selectively forwards when destination is known
→ Floods when destination is unknown
```

### Lab Question 4

A student says:

> "A switch and a hub are basically the same because both can send traffic to multiple ports."

Is that statement correct?

No.

The forwarding logic is fundamentally different.

A hub:

```
Layer 1
Signal repetition
No MAC-based forwarding
```

A switch:

```
Layer 2
Frame processing
MAC-based forwarding
MAC learning
```

The fact that a switch may flood an unknown destination does not make it equivalent to a hub.

### Collision Domain Comparison

Now compare the two topologies.

Hub:

```
       HUB
     /  |  \
   PC-A PC-B PC-C

One shared collision domain
```

Switch:

```
      SWITCH
     /  |  \
   PC-A PC-B PC-C

Separate collision domains per switch port
```

This is one of the major architectural advantages of switching.

### Full-Duplex Observation

Modern Ethernet switch links normally operate in:

```
Full-duplex
```

This means:

```
Transmit
+
Receive
```

can occur simultaneously.

The link does not behave like the old shared half-duplex hub medium.

Therefore, normal full-duplex switched Ethernet does not have the collision behavior associated with traditional shared Ethernet.

### Lab Question 5

Why does replacing a hub with a switch improve Ethernet performance?

A strong answer should include several points:

```
The switch separates collision domains.

The switch can forward known unicast frames
only toward the appropriate port.

The switch allows full-duplex operation on normal
point-to-point Ethernet links.

The network no longer depends on a single shared
collision domain for all connected hosts.
```

### Part 3 — Add a Router

Now extend the topology.

Create two IP networks:

```
Network A
192.168.10.0/24

Network B
192.168.20.0/24
```

A simplified topology:

```
PC-A
192.168.10.10
    |
  Switch
    |
    |
  Router
    |
    |
  Switch
    |
PC-B
192.168.20.20
```

Configure the router interfaces:

```
Interface toward Network A:
192.168.10.1/24

Interface toward Network B:
192.168.20.1/24
```

Configure:

```
PC-A
IP: 192.168.10.10
Mask: 255.255.255.0
Gateway: 192.168.10.1
```

and:

```
PC-B
IP: 192.168.20.20
Mask: 255.255.255.0
Gateway: 192.168.20.1
```

### Test the Local Network

From PC-A:

```
ping 192.168.10.1
```

This tests connectivity to the router interface on the local subnet.

Then:

```
ping 192.168.20.20
```

This tests communication with a host on a different subnet.

### Trace the Path

The second ping should conceptually follow:

```
PC-A
  ↓
Switch
  ↓
Router
  ↓
Switch
  ↓
PC-B
```

This is the key device relationship:

```
Same subnet
→ Switch

Different subnet
→ Router
```

The switches provide local Layer 2 connectivity.

The router provides Layer 3 connectivity between the two IP networks.

### Lab Question 6

Why can PC-A not simply send the frame directly to PC-B's MAC address?

Because PC-B is on another IP network.

The local host needs to send the packet toward the router.

The router then forwards the packet into the destination network.

The important distinction is:

```
PC-A → Router
```

is local Layer 2 delivery on the first network.

Then:

```
Router → PC-B
```

is local Layer 2 delivery on the second network.

The IP packet can remain associated with the same end-to-end communication while the Layer 2 frame is rebuilt for each link.

### Part 4 — Build the Complete Mental Model

At this point, stop thinking about devices as isolated definitions.

Instead, ask what problem each device solves.

### Hub

Question:

```
How can I repeat a physical signal to multiple ports?
```

Answer:

```
Hub
```

Layer:

```
Layer 1
```

Primary information:

```
Physical signal
```

Forwarding intelligence:

```
None
```

### Switch

Question:

```
Which local Ethernet port should receive this frame?
```

Answer:

```
Switch
```

Layer:

```
Layer 2
```

Primary information:

```
MAC address
```

Table:

```
CAM / MAC address table
```

### Router

Question:

```
Which network should this IP packet travel toward?
```

Answer:

```
Router
```

Layer:

```
Layer 3
```

Primary information:

```
IP address
```

Table:

```
Routing table
```

### Access Point

Question:

```
How can wireless clients participate in the local network?
```

Answer:

```
Access Point
```

Primary role:

```
Wireless Layer 2 connectivity / bridging
```

Medium:

```
Radio
```

### Firewall

Question:

```
Should this traffic be allowed across this security boundary?
```

Answer:

```
Firewall
```

Primary role:

```
Traffic inspection and policy enforcement
```

The firewall may operate at multiple layers depending on its design and capabilities.

### One Network, Multiple Functions

A real network may contain all of these:

```
                    Internet
                       |
                    Firewall
                       |
                     Router
                       |
                    Switch
                  /   |   \
                PC   Server  AP
                              |
                         Wi-Fi Clients
```

A home network may collapse many of these roles into one physical device:

```
                 ISP
                  |
           [Home Gateway]
          /      |       \
       Wi-Fi    LAN      Routing
         |       |          |
       Phone    PC       Internet
```

The physical architecture changes.

The logical functions remain.

### Troubleshooting Through Device Roles

Suppose a computer cannot communicate with another computer.

Do not immediately blame the "network."

Break the problem into functions.

Ask:

```
1. Is the physical link working?

2. Can the device communicate at Layer 2?

3. Is the destination on the same subnet?

4. If it is remote, is the default gateway reachable?

5. Does the router have a route to the destination network?

6. Is a firewall blocking the traffic?

7. Is the destination service actually listening?
```

This approach prevents random troubleshooting.

### Example: Same-Subnet Failure

Suppose:

```
PC-A:
192.168.1.10/24

PC-B:
192.168.1.20/24
```

and they cannot communicate.

The first devices to investigate are:

```
NIC
Cable / Wi-Fi
Switch
Host firewall
ARP / Layer 2 behavior
```

You would not immediately assume that an Internet router is the problem.

Why?

Because the destination is local:

```
192.168.1.0/24
```

The traffic does not fundamentally need Layer 3 routing to reach the other host.

### Example: Different-Subnet Failure

Now:

```
PC-A:
192.168.1.10/24

PC-B:
192.168.2.20/24
```

The troubleshooting path changes.

You should consider:

```
PC-A
↓
Local switch
↓
Default gateway
↓
Router
↓
Destination network
```

Questions become:

```
Can PC-A reach its gateway?

Does the router have a route to 192.168.2.0/24?

Is the return route correct?

Are firewall policies allowing the traffic?
```

This is much more systematic.

### Device Selection Exercise

For each requirement, identify the primary device or function.

#### Requirement 1

"Connect several wired computers within the same local Ethernet network."

Answer:

```
Switch
```

#### Requirement 2

"Connect wireless laptops to the wired LAN."

Answer:

```
Access Point
```

#### Requirement 3

"Connect two different IP networks."

Answer:

```
Router
```

#### Requirement 4

"Repeat a physical Ethernet signal to multiple ports without Layer 2 forwarding intelligence."

Answer:

```
Hub
```

#### Requirement 5

"Block incoming traffic according to security policy."

Answer:

```
Firewall
```

### Integrated Scenario

Consider this network:

```
                         Internet
                            |
                         Firewall
                            |
                          Router
                            |
                         Switch
                    /       |       \
                  PC      Server     AP
                                      |
                                  Wi-Fi
                                  /   \
                               Phone Laptop
```

Now identify each function.

```
Firewall
→ Controls traffic according to policy

Router
→ Connects the internal network to other networks

Switch
→ Provides local wired Layer 2 connectivity

Access Point
→ Provides wireless Layer 2 connectivity

PC / Server / Phone / Laptop
→ End hosts
```

If the physical deployment uses a single home gateway, several of these functions may be inside one box.

### The Layered Device Model

The entire lesson can now be condensed into:

```
PHYSICAL

Hub
→ Signals
```

```
DATA LINK

Switch
→ Ethernet frames
→ MAC addresses
```

```
WIRELESS DATA LINK

Access Point
→ Wi-Fi
→ Layer 2 bridging
```

```
NETWORK

Router
→ IP packets
→ IP addresses
→ Routing table
```

```
SECURITY

Firewall
→ Traffic policy
→ Inspection
→ Allow / deny decisions
```

These are functional roles.

They may be implemented by:

```
Separate physical devices
```

or:

```
Multiple functions inside one appliance
```

### Final Lab Checklist

Before considering this lesson complete, verify that you can answer all of these without looking at the notes.

```
What layer does a traditional hub operate at?

What does a hub actually do with an incoming signal?

What is a collision domain?

Why does a hub create a shared collision domain?

What layer does an Ethernet switch primarily operate at?

What is a MAC address?

What is a CAM/MAC address table?

How does a switch learn MAC addresses?

What is a known unicast?

What is an unknown unicast?

Why does a switch sometimes flood traffic?

Why are switch ports separate collision domains?

What is full-duplex Ethernet?

What layer does a router operate at?

What is a routing table?

What is a default gateway?

How does a host determine whether a destination is local?

Why is a router needed for a different subnet?

What is an access point?

Why is an access point primarily associated with Layer 2?

What does a firewall do?

How is firewalling different from routing?

Why can one home device contain a router, switch,
access point, firewall, NAT, and DHCP server?
```

### Final Mental Model

The complete device model should now look like this:

```
                NETWORK DEVICES

Hub
│
├── Layer 1
├── Repeats signals
└── Shared collision domain


Switch
│
├── Layer 2
├── Ethernet frames
├── MAC addresses
├── CAM/MAC table
└── Local forwarding


Access Point
│
├── Wireless Layer 2
├── Wi-Fi
└── Bridges wireless clients to the LAN


Router
│
├── Layer 3
├── IP addresses
├── Routing table
└── Connects different networks


Firewall
│
├── Security boundary
├── Traffic inspection
├── Policy enforcement
└── Allow / deny decisions
```

The most important operational rule is:

```
Same subnet
→ Local Layer 2 delivery
→ Switch / AP

Different subnet
→ Layer 3 forwarding
→ Router
```

And the most important architectural rule is:

```
One physical box can perform many networking functions.
```

If you can look at a network diagram and identify which device is responsible for:

```
physical signal handling
local Ethernet forwarding
wireless access
network-to-network forwarding
security policy
```

then you have moved beyond memorizing device names and started thinking like a network engineer.