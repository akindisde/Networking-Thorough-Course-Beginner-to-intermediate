## Part 1 - Why Layer 2 Networks Need STP

### Why Layer 2 Networks Need Redundancy

Redundant links are an important part of network design. If a switch or link fails, an alternate path can keep devices connected.

Consider three switches connected in a triangle:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

There are multiple physical paths between the switches. If the SW1–SW2 link fails, traffic can potentially use SW1–SW3–SW2 instead.

The fundamental reason for redundancy is:

```text
Primary path fails
       ↓
Alternate path remains
       ↓
Connectivity continues
```

The problem is that Ethernet Layer 2 forwarding was not designed to operate with unrestricted forwarding loops.

A switch forwards Ethernet frames based primarily on destination MAC addresses. When a switch does not know the destination MAC address, it may flood the frame out multiple ports. Broadcast frames are also intentionally flooded throughout the local Layer 2 broadcast domain.

With redundant links, this flooding behavior can create a loop.

### The Layer 2 Loop Problem

Return to the triangle:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

Suppose SW1 receives an Ethernet broadcast.

The Ethernet broadcast destination is:

```text
FF:FF:FF:FF:FF:FF
```

SW1 forwards the broadcast through its other active ports. The frame can therefore reach both SW2 and SW3.

```text
        SW1
       /   \
      ↓     ↓
    SW2─────SW3
```

SW2 can then forward the frame toward SW3.

SW3 can forward another copy toward SW2.

Because the switches are connected in a loop, the same traffic can continue circulating.

```text
SW1 → SW2 → SW3 → SW1 → SW2 → SW3 → ...
```

An Ethernet frame does not have the Layer 3 TTL mechanism used by IP to limit the number of routed hops.

At Layer 2, switches can therefore continue forwarding traffic while the forwarding loop remains active.

This is the fundamental danger of a Layer 2 loop.

### Why Broadcast Traffic Is Especially Dangerous

Broadcast traffic is designed to reach all devices in a Layer 2 broadcast domain.

A broadcast frame uses:

```text
Destination MAC:
FF:FF:FF:FF:FF:FF
```

A switch normally floods this frame out all appropriate ports except the port on which it arrived.

In a loop-free topology:

```text
          SW1
         /   \
        /     \
      SW2     SW3
```

the broadcast can spread through the network and reach the participating devices without circulating indefinitely.

In a topology containing a forwarding loop:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

the broadcast can repeatedly circulate.

The resulting traffic is known as a **broadcast storm**.

A broadcast storm can consume:

- Link bandwidth
    
- Switch forwarding resources
    
- CPU resources
    
- Memory
    
- Host processing capacity
    

As the storm grows, legitimate network traffic can become delayed or dropped.

### Broadcast Storms

A broadcast storm occurs when excessive broadcast traffic overwhelms a network.

The basic progression is:

```text
Layer 2 loop
     ↓
Broadcast frame enters loop
     ↓
Switches flood the frame
     ↓
Copies circulate through redundant paths
     ↓
Traffic increases
     ↓
Network resources become exhausted
     ↓
Legitimate traffic is affected
```

The severity depends on the topology, traffic volume, switch behavior, and available resources.

A small Layer 2 loop can therefore have consequences far beyond the two ports directly involved.

### Unknown-Unicast Flooding

Broadcast traffic is not the only traffic that can contribute to a Layer 2 loop.

A switch normally maintains a MAC address table that maps MAC addresses to ports.

For example:

```text
MAC Address          Port
-------------------------
00:11:22:33:44:55    Fa0/1
00:AA:BB:CC:DD:EE    Fa0/2
```

If the switch knows where the destination MAC is located, it can forward the frame toward the appropriate port.

However, if the destination MAC is unknown, the switch may perform **unknown-unicast flooding** within the relevant VLAN.

For example:

```text
Destination MAC:
00:55:66:77:88:99

MAC table:
No matching entry
```

The switch does not know which port contains the destination, so it floods the frame similarly to broadcast traffic.

If the network contains a Layer 2 loop, these flooded frames can also circulate.

Therefore:

```text
Broadcast flooding
+
Unknown-unicast flooding
+
Layer 2 loop
=
Potentially severe traffic amplification
```

### Duplicate Frames

A Layer 2 loop can cause the same Ethernet frame to reach a destination through multiple paths.

Consider:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

A frame sent from SW1 may reach SW2 directly and SW3 directly.

If both switches forward the frame through the redundant connection, additional copies can reach the same devices.

The receiver may therefore see duplicate frames.

The problem is not simply that one frame was delivered twice. The repeated forwarding can continue as the frame circulates.

This can produce large amounts of unnecessary traffic and make normal network behavior unreliable.

### MAC Address Flapping

Switches learn MAC addresses by examining the **source MAC address** of received frames.

Suppose a switch learns:

```text
MAC-A → Port 1
```

This means the switch believes that the device using MAC-A is reachable through Port 1.

Now imagine a Layer 2 loop causes another copy of a frame sourced by MAC-A to arrive through Port 2.

The switch may update its table:

```text
MAC-A → Port 2
```

Later, another copy may arrive through Port 1:

```text
MAC-A → Port 1
```

The entry can repeatedly move:

```text
Port 1 → Port 2 → Port 1 → Port 2 → ...
```

This behavior is called **MAC address flapping**.

MAC flapping is an important symptom when diagnosing Layer 2 loops.

A switch is essentially receiving contradictory information:

```text
"MAC-A is here."
       ↓
     Port 1

"MAC-A is here."
       ↓
     Port 2
```

The switch cannot maintain a stable mapping while the topology is continuously producing frames from different directions.

### Why Layer 2 Does Not Simply Use TTL

IP packets have a Time To Live (TTL) field in IPv4 and a Hop Limit field in IPv6.

Routers decrement this value as packets traverse Layer 3 hops.

Ethernet frames do not use the same mechanism.

A switch does not normally decrement a Layer 2 hop counter every time it forwards a frame.

Therefore, Layer 2 needs another mechanism to prevent loops.

This is the problem that **Spanning Tree Protocol (STP)** was designed to solve.

### The Basic STP Idea

STP allows a network to maintain physical redundancy while preventing redundant paths from forwarding simultaneously.

Consider the triangle topology:

```text
Physical topology:

        SW1
       /   \
      /     \
    SW2─────SW3
```

Without STP, all three links may participate in forwarding and form a loop.

STP logically blocks one redundant path:

```text
Logical forwarding topology:

        SW1
       /   \
      /     \
    SW2     SW3

SW2─────SW3
  blocked
```

The physical cable still exists.

The important distinction is:

```text
Physical topology ≠ active forwarding topology
```

The network keeps the redundant link available, but that link does not forward ordinary Layer 2 traffic while it is in a non-forwarding state.

If an active link later fails, STP can change the topology and allow the redundant path to become active.

### Redundancy Without a Forwarding Loop

The purpose of STP can therefore be expressed as:

```text
Physical redundancy
       +
Controlled Layer 2 forwarding
       ↓
Loop-free logical topology
```

STP does not remove the redundant cable.

It removes the redundant **forwarding path** from the active topology.

A network can therefore have:

```text
3 physical links
```

while STP creates:

```text
2 active forwarding links
1 blocked redundant link
```

The blocked link remains useful because it provides an alternate path if the active topology changes.

### STP as a Distributed Decision

STP is not a single central controller telling every switch what to do.

Switches communicate with one another using **Bridge Protocol Data Units (BPDUs)**.

Through BPDU exchange, switches learn information about the spanning-tree topology and participate in determining which paths should forward and which paths should remain blocked.

The process can be simplified as:

```text
Switches exchange BPDUs
          ↓
Determine the root bridge
          ↓
Calculate best paths toward root
          ↓
Select forwarding ports
          ↓
Block redundant paths
          ↓
Loop-free Layer 2 topology
```

The detailed election and port-selection process is covered in Part 2.

### Physical Versus Logical Topology

Network engineers must distinguish between the topology that exists physically and the topology that is actively forwarding traffic.

For example:

```text
Physical:

        SW1
       /   \
      /     \
    SW2─────SW3
```

This contains a physical loop.

After STP makes its decisions:

```text
Forwarding:

        SW1
       /   \
      /     \
    SW2     SW3
```

The active topology is a tree.

A tree has no cycles.

That is where the name **Spanning Tree Protocol** comes from: STP constructs a loop-free spanning tree across the Layer 2 topology.

### What Happens During a Network Failure?

Redundancy would be useless if the backup link could never become active.

Suppose the active topology is:

```text
        SW1
       /   \
      /     \
    SW2     SW3

SW2─────SW3
 blocked
```

Now suppose the SW1–SW3 link fails:

```text
        SW1
         |
         |
       SW2     SW3
```

The network has lost the direct forwarding path between SW1 and SW3.

The previously redundant SW2–SW3 link can become part of the active topology:

```text
        SW1
         |
         |
       SW2─────SW3
```

The exact transition behavior depends on the STP version and topology.

The important principle is:

```text
Normal state:
Redundant path blocked

Failure:
Topology changes

Recovery:
Redundant path becomes forwarding
```

This is the reason STP is both a loop-prevention mechanism and a network availability mechanism.

### The Core Problem STP Solves

The entire problem can be summarized as:

```text
Redundant links are necessary
for resilience.

But redundant forwarding paths
can create Layer 2 loops.

Layer 2 loops cause:
    • Broadcast storms
    • Unknown-unicast flooding
    • Duplicate frames
    • MAC address flapping
    • Excessive bandwidth consumption
    • Switch and host resource exhaustion

STP solves this by creating
a loop-free logical topology
while preserving physical redundancy.
```

### Part 1 Mental Model

Keep the following model in mind before studying how STP works internally:

```text
                 PHYSICAL
              REDUNDANT LINKS
                    ↓
             Layer 2 Loop
                    ↓
        ┌─────────────────────────┐
        │ Broadcast storms        │
        │ MAC flapping             │
        │ Duplicate frames         │
        │ Excessive traffic        │
        └─────────────────────────┘
                    ↓
                   STP
                    ↓
        ┌─────────────────────────┐
        │ Loop-free logical tree  │
        │ +                       │
        │ Redundant backup paths  │
        └─────────────────────────┘
```

The essential idea is:

**Redundancy is good. Uncontrolled Layer 2 redundancy is dangerous. STP keeps the redundancy while preventing the forwarding loop.**

### Part 1 Checkpoint

Before moving to Part 2, you should be able to explain:

1. Why are redundant links useful?
    
2. Why can redundant links create a Layer 2 loop?
    
3. Why are broadcast frames particularly dangerous in a loop?
    
4. What is a broadcast storm?
    
5. What is unknown-unicast flooding?
    
6. Why can Layer 2 loops produce duplicate frames?
    
7. What is MAC address flapping?
    
8. Why does Ethernet not automatically stop a looping frame using IP-style TTL?
    
9. What is the difference between a physical topology and an active forwarding topology?
    
10. What is the fundamental purpose of STP?
    

The key relationship is:

```text
Redundant links
      ↓
Potential Layer 2 loop
      ↓
STP
      ↓
Loop-free forwarding topology
      ↓
Redundancy remains available for failures
```

## Summary

Spanning Tree Protocol (STP) solves a fundamental problem in Ethernet networks: **redundancy can improve reliability, but redundant Layer 2 forwarding paths can create loops**.

A network may intentionally connect switches with redundant links so that traffic can continue if a link fails. However, Ethernet switches can flood broadcast and unknown-unicast frames, and Ethernet frames do not have an IP-style TTL that automatically stops them from circulating. When redundant links form a forwarding loop, frames can circulate repeatedly through the network.

A Layer 2 loop can produce several serious problems:

- **Broadcast storms** — broadcast frames are repeatedly flooded and can consume network resources.
    
- **Unknown-unicast flooding** — frames whose destination MAC is not known can also be flooded through the loop.
    
- **Duplicate frames** — the same frame can reach a destination through multiple paths.
    
- **MAC address flapping** — switches repeatedly learn the same source MAC address on different ports because frames are arriving from multiple directions.
    
- **Resource exhaustion** — bandwidth, switch CPU, memory, and host resources can be consumed by looping traffic.
    

STP prevents these problems by creating a **loop-free logical forwarding topology** while keeping the physical redundant links available.

For example, a physical triangle:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

can become a logical tree:

```text
        SW1
       /   \
      /     \
    SW2     SW3
```

The SW2–SW3 link still physically exists, but STP places it into a non-forwarding role. If an active link later fails, STP can change the topology and allow the redundant path to become active.

STP accomplishes this through communication between switches using **BPDUs (Bridge Protocol Data Units)**. The switches collectively determine the appropriate loop-free topology rather than relying on a single centralized controller.

The most important distinction is:

```text
Physical topology
    ↓
Contains redundant links

Logical forwarding topology
    ↓
Must not contain Layer 2 loops
```

This is why STP is called **Spanning Tree Protocol**: it creates a tree-like, loop-free forwarding topology across the physical Layer 2 network.

## Key Takeaways

1. **Redundancy is necessary for resilience.**  
    Multiple physical paths allow a network to survive link failures.
    
2. **Redundancy at Layer 2 can create loops.**  
    Ethernet switching does not inherently prevent frames from circulating through a loop.
    
3. **Broadcasts are especially dangerous.**  
    Broadcast frames are intentionally flooded, so a Layer 2 loop can turn them into a broadcast storm.
    
4. **Unknown-unicast traffic can also contribute to loops.**  
    A switch may flood traffic when it does not know the destination MAC address.
    
5. **Layer 2 loops cause multiple symptoms.**  
    Watch for broadcast storms, duplicate frames, excessive traffic, and MAC address flapping.
    
6. **MAC flapping is an important troubleshooting clue.**  
    If the same MAC address repeatedly appears on different switch ports, a Layer 2 loop or topology problem may be involved.
    
7. **Ethernet does not use IP TTL to stop Layer 2 loops.**  
    STP provides a different mechanism for preventing forwarding loops.
    
8. **STP blocks redundant forwarding paths, not necessarily physical links.**  
    The cable remains available as a backup.
    
9. **Physical and logical topology are different.**  
    A network can have a physical triangle while STP creates a loop-free logical tree.
    
10. **STP provides both loop prevention and redundancy.**  
    Under normal conditions, a redundant path can remain blocked; after a failure, it can become part of the forwarding topology.

## Part 2 - How STP Builds a Loop-Free Topology

### 1. What STP Does

Spanning Tree Protocol (STP) prevents Layer 2 loops by logically organizing switches into a **loop-free tree topology**.

The physical network can contain redundant links:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

STP does not remove the physical links. Instead, it decides which ports should forward traffic and which redundant ports should remain non-forwarding.

The result is:

```text
        SW1
       /   \
      /     \
    SW2     SW3

SW2─────SW3
 blocked
```

The network still has physical redundancy, but the active forwarding topology contains no loop.

STP achieves this through a distributed process in which switches exchange **Bridge Protocol Data Units (BPDUs)** and make decisions based on the information contained in those BPDUs.

### 2. Bridge Protocol Data Units

BPDUs are control frames used by STP-enabled switches to exchange spanning-tree information.

They allow switches to communicate information such as:

- Which switch is considered the root bridge
    
- The sender's Bridge ID
    
- The path cost toward the root
    
- Information about the topology
    

The general process is:

```text
Switches exchange BPDUs
        ↓
Compare Bridge IDs
        ↓
Elect root bridge
        ↓
Determine best paths to root
        ↓
Select forwarding ports
        ↓
Place redundant paths into non-forwarding roles
```

STP is therefore a **distributed control protocol**. Each switch participates in calculating the topology.

### 3. The Bridge ID

A central concept in STP is the **Bridge ID (BID)**.

The Bridge ID is used when switches determine which switch should become the root bridge.

Conceptually, the Bridge ID consists of:

```text
Bridge ID
├── Bridge Priority
└── MAC Address
```

The switch with the **lowest Bridge ID** becomes the root bridge.

Priority is considered before the MAC address.

For example:

```text
SW1:
Priority = 32768
MAC      = 00:11:22:33:44:01

SW2:
Priority = 32768
MAC      = 00:11:22:33:44:02

SW3:
Priority = 32768
MAC      = 00:11:22:33:44:03
```

If the priorities are equal, the lowest MAC address wins.

Therefore:

```text
SW1 → Root Bridge
```

The root bridge becomes the reference point for the spanning-tree calculations.

### 4. Root Bridge Election

The first major STP decision is the election of the **root bridge**.

Every switch initially considers itself a potential root and advertises its Bridge ID.

The switches compare the information they receive.

The switch with the lowest Bridge ID becomes the root.

The simplified process is:

```text
SW1 ── BID 10
SW2 ── BID 20
SW3 ── BID 30

Lowest BID
    ↓
SW1 becomes root
```

Once the root bridge has been determined, the other switches calculate their best paths toward it.

The root bridge is important because STP decisions are based around reaching this common reference point.

### 5. Bridge Priority

Bridge priority is a configurable value that can influence which switch becomes the root bridge.

A network administrator can intentionally configure a particular switch with a lower priority so that it becomes the preferred root.

For example:

```text
Core-SW1
Priority = low
       ↓
Preferred root
```

while access switches may retain their default or higher priority.

This allows the STP topology to be designed rather than left entirely to automatic election.

The root bridge should normally be a switch that is well positioned to serve as the logical center of the Layer 2 topology.

### 6. Root Ports

After the root bridge is selected, every non-root switch needs to determine its best path toward the root.

The port providing that best path is called the **root port**.

A simplified topology:

```text
              SW1
             ROOT
            /    \
           /      \
         SW2──────SW3
```

SW2 needs to reach SW1.

If its direct connection to SW1 is the best path, that interface becomes SW2's root port.

Similarly:

```text
SW2 → root port → SW1
SW3 → root port → SW1
```

The root bridge itself does not have a root port because it is already the root.

A useful rule is:

**Every non-root switch selects one root port. The root bridge has no root port.**

### 7. Path Cost

STP needs a way to determine which path toward the root is better.

It uses **path cost**.

Path cost is associated with the bandwidth characteristics of links. Higher-bandwidth links generally have lower STP costs than lower-bandwidth links.

Consider:

```text
             SW1
             ROOT
            /    \
       Fast link  Slow link
          /          \
        SW2          SW3
```

STP can prefer the path with the lower total cost.

The path cost toward the root can be thought of as:

```text
Total path cost =
sum of the costs of the links along the path
```

For example:

```text
Path A:
SW2 → SW1

Cost = 4


Path B:
SW2 → SW3 → SW1

Cost = 4 + 19
     = 23
```

STP would prefer Path A because its total cost is lower.

The exact cost values depend on the STP implementation and version.

### 8. Root Path Cost

Each switch effectively determines how expensive it is to reach the root bridge.

Imagine:

```text
          SW1
          ROOT
         /    \
      cost 4  cost 4
       /        \
     SW2        SW3
       \        /
        \ cost 19
         \    /
          ───
```

SW2 can reach SW1 directly at cost 4.

The alternative route through SW3 would have a higher total cost:

```text
SW2 → SW3 → SW1
```

Therefore, SW2 prefers its direct connection to the root.

This concept becomes important when several redundant paths exist.

### 9. Designated Ports

STP also needs to determine which port should forward traffic on each network segment.

A **designated port** is the port selected to provide the preferred forwarding path for a given Layer 2 segment.

In a simplified topology:

```text
          SW1
          ROOT
         /    \
        /      \
      SW2──────SW3
```

The root bridge's ports toward SW2 and SW3 are forwarding ports and serve as designated ports for their respective segments.

On the SW2–SW3 segment, STP must decide which side should be preferred.

The switch with the better path toward the root wins the designated-port decision.

### 10. Redundant Ports

Once STP has selected the root bridge, root ports, and designated ports, some ports may be left without a forwarding role.

These ports are used to eliminate loops.

For example:

```text
          SW1
          ROOT
         /    \
        /      \
      SW2──────SW3
        \______/
        redundant
```

STP may logically block one side of the redundant connection:

```text
          SW1
          ROOT
         /    \
        /      \
      SW2──────SW3
             X
          blocked
```

The physical connection remains.

The important change is that the port does not participate in normal forwarding.

### 11. The Resulting Spanning Tree

The final topology should contain no forwarding cycles.

Physical topology:

```text
        SW1
       /   \
      /     \
    SW2─────SW3
```

STP forwarding topology:

```text
        SW1
       /   \
      /     \
    SW2     SW3
```

The result is a tree.

A tree has no Layer 2 forwarding loop.

The redundant physical link can remain available as a backup path.

### 12. STP Port States

Traditional STP uses several port states to control how a port participates in the network.

The classic sequence is:

```text
Blocking
   ↓
Listening
   ↓
Learning
   ↓
Forwarding
```

There is also a **Disabled** state when a port is administratively or operationally disabled.

The four active STP states have different purposes.

### 13. Blocking

In the **blocking** state, the port does not forward normal data frames.

Its primary purpose is to prevent a Layer 2 loop.

A blocked port can still participate in STP control operations by processing appropriate BPDUs.

Conceptually:

```text
Data traffic:
      ✕

STP information:
      ✓
```

This allows the switch to maintain knowledge about the topology without creating an active forwarding loop.

### 14. Listening

In the **listening** state, the switch is actively evaluating the spanning-tree topology.

The port does not yet forward normal user traffic.

The switch can process BPDUs and determine whether the port should eventually become part of the forwarding topology.

Conceptually:

```text
Normal data:
      ✕

STP topology calculation:
      ✓
```

### 15. Learning

In the **learning** state, the switch begins learning source MAC addresses.

Normal data forwarding has not yet begun.

The switch can therefore start building its MAC address table before the port transitions to forwarding.

```text
MAC learning:
      ✓

Normal forwarding:
      ✕
```

This helps the switch prepare its forwarding information before the port becomes fully active.

### 16. Forwarding

In the **forwarding** state, the port is fully participating in normal Layer 2 forwarding.

It can:

- Forward data frames
    
- Learn source MAC addresses
    
- Process STP information
    

The forwarding state is therefore the normal active state for a selected Layer 2 path.

```text
MAC learning:
      ✓

Data forwarding:
      ✓

STP participation:
      ✓
```

### 17. Why the Port States Matter

The traditional state progression provides controlled topology establishment.

```text
Blocking
   ↓
Listening
   ↓
Learning
   ↓
Forwarding
```

The switch does not immediately begin forwarding through every newly connected path.

Instead, STP first evaluates the topology and establishes which paths should be active.

This prevents the network from immediately creating a forwarding loop when topology information is changing.

### 18. Topology Changes

Networks are not static.

Links can fail:

```text
        SW1
       /   \
      /     \
    SW2     SW3
      \     /
       \   /
```

If an active path fails, STP must recalculate the topology.

For example:

```text
Normal:

SW1 ─── SW3
  \
   \
   SW2 ─── SW3
         blocked
```

If the SW1–SW3 path fails:

```text
SW1      SW3
 |
 |
SW2─────SW3
```

the previously redundant connection can become active.

This provides fault tolerance while maintaining loop prevention.

The exact speed of this process depends heavily on the STP version being used.

### 19. Traditional STP and Convergence

Traditional IEEE 802.1D STP was designed primarily for loop prevention and could take significantly longer to converge after a topology change.

During convergence, some ports may temporarily remain in non-forwarding states while switches determine the new topology.

This can result in noticeable connectivity interruption.

The classic STP process is therefore often associated with relatively slow convergence compared with modern alternatives.

### 20. RSTP

**Rapid Spanning Tree Protocol (RSTP)**, standardized as IEEE 802.1w and incorporated into later versions of the IEEE 802.1D standard, was designed to provide much faster convergence.

The fundamental purpose remains the same:

```text
Prevent Layer 2 loops
+
Maintain redundant paths
```

But RSTP improves the mechanisms used to transition ports and react to topology changes.

The practical result is substantially faster recovery from many failures.

Conceptually:

```text
STP:
Failure → topology recalculation → slower recovery

RSTP:
Failure → faster topology recalculation → faster recovery
```

RSTP is therefore generally preferred over legacy 802.1D STP when the network supports it.

### 21. STP vs RSTP vs PVST+

These technologies should not be treated as completely unrelated protocols.

They solve the same fundamental Layer 2 loop-prevention problem but differ in operation and deployment model.

|Technology|Main characteristic|
|---|---|
|STP / 802.1D|Traditional spanning tree, slower convergence|
|RSTP / 802.1w|Faster convergence|
|PVST+|Cisco implementation using a separate spanning-tree instance per VLAN|

The important concepts are:

```text
STP
↓
Loop prevention

RSTP
↓
Loop prevention + faster convergence

PVST+
↓
Spanning tree calculated separately for each VLAN
```

### 22. Per-VLAN Spanning Tree

A VLAN represents a separate Layer 2 broadcast domain.

With **PVST+**, each VLAN can have its own spanning-tree topology.

This means different VLANs can potentially use different forwarding paths.

For example:

```text
VLAN 10:
SW1 → SW2 → SW3

VLAN 20:
SW1 → SW3 → SW2
```

This allows the network to make more efficient use of redundant links than forcing every VLAN to use exactly the same active topology.

It also means STP design becomes more complex because each VLAN has its own spanning-tree instance.

### 23. Why Per-VLAN STP Matters

Consider two redundant paths:

```text
          SW1
         /   \
        /     \
      SW2─────SW3
```

If every VLAN uses the same blocked path, one physical link may remain underutilized.

With per-VLAN spanning tree, different VLANs can potentially select different paths:

```text
VLAN 10:
SW1 → SW2 → SW3

VLAN 20:
SW1 → SW3 → SW2
```

This can provide better traffic distribution.

The tradeoff is increased control-plane complexity.

### 24. Complete STP Decision Process

The complete conceptual process can be represented as:

```text
                 BPDUs
                   ↓
          Elect Root Bridge
                   ↓
       Calculate Root Path Costs
                   ↓
         Select Root Ports
                   ↓
       Select Designated Ports
                   ↓
    Identify redundant/non-forwarding ports
                   ↓
       Construct loop-free tree
```

A more detailed decision hierarchy is:

```text
1. Lowest Bridge ID
        ↓
   Root Bridge

2. Best path toward root
        ↓
   Root Port

3. Best path on each segment
        ↓
   Designated Port

4. Remaining redundant path
        ↓
   Non-forwarding/blocking port
```

The exact tie-breaking rules become more detailed when multiple paths have equal costs, but the fundamental logic remains the same.

### 25. STP's Core Design Principle

STP is essentially solving an optimization problem with one strict requirement:

```text
Maintain connectivity
while
preventing Layer 2 forwarding loops
```

The network wants as many useful paths as possible, but it cannot allow all redundant paths to forward simultaneously when they create cycles.

Therefore:

```text
Physical topology:
     Redundant

STP logical topology:
     Loop-free
```

This distinction is one of the most important concepts in Layer 2 switching.

### 26. Part 2 Mental Model

The complete STP model can be remembered as:

```text
              Physical topology
                     ↓
               Switches exchange
                    BPDUs
                     ↓
              Elect root bridge
                     ↓
          Calculate path costs
                     ↓
              Select root ports
                     ↓
          Select designated ports
                     ↓
       Block redundant forwarding paths
                     ↓
           Loop-free spanning tree
                     ↓
         Failure occurs later
                     ↓
        Recalculate / reconverge
                     ↓
       Redundant path can activate
```

The key idea is:

**STP does not eliminate redundancy. It controls redundancy so that only a loop-free set of paths forwards traffic at a given time.**

### Part 2 Checkpoint

You should now be able to answer the following:

1. What is the purpose of STP?
    
2. What are BPDUs?
    
3. What is a Bridge ID?
    
4. How is the root bridge elected?
    
5. Why does bridge priority matter?
    
6. What is a root port?
    
7. Why does the root bridge not have a root port?
    
8. What is STP path cost?
    
9. What is a designated port?
    
10. Why does STP place some ports into a non-forwarding state?
    
11. What are the traditional STP port states?
    
12. What happens during blocking?
    
13. What happens during listening?
    
14. What happens during learning?
    
15. What happens during forwarding?
    
16. Why was RSTP introduced?
    
17. What is the major difference between STP and RSTP?
    
18. What does PVST+ provide?
    
19. Why can per-VLAN spanning tree improve link utilization?
    
20. What happens to a redundant path when an active path fails?
    

The central sequence to remember is:

```text
BPDUs
  ↓
Root Bridge
  ↓
Root Ports
  ↓
Designated Ports
  ↓
Block Redundant Paths
  ↓
Loop-Free Tree
  ↓
Topology Failure
  ↓
Recalculate
  ↓
Activate Backup Path
```

## Summary

STP creates a **loop-free Layer 2 forwarding topology** while preserving physical redundancy. Switches exchange **BPDUs** to share spanning-tree information and collectively determine the topology.

The first major decision is the election of the **root bridge**, which is the switch with the lowest Bridge ID. The Bridge ID is primarily determined by bridge priority and MAC address. After the root is selected, each non-root switch determines its best path toward the root using **STP path cost**.

Each non-root switch selects a **root port**, representing its best path toward the root. STP also selects **designated ports** for Layer 2 segments. Ports that are not needed for the active loop-free topology can be placed into a non-forwarding state, preventing redundant paths from creating a loop.

Traditional STP uses the states **blocking, listening, learning, and forwarding** as ports transition toward normal operation. When the topology changes, STP can recalculate the tree and activate a previously redundant path.

**RSTP** improves the convergence speed of traditional STP, allowing networks to recover from topology changes much faster. **PVST+** extends the concept by maintaining a separate spanning-tree instance for each VLAN, allowing different VLANs to potentially use different forwarding paths.

The overall process is:

```text
BPDUs
   ↓
Elect Root Bridge
   ↓
Calculate Best Paths
   ↓
Select Root Ports
   ↓
Select Designated Ports
   ↓
Block Redundant Paths
   ↓
Loop-Free Forwarding Topology
   ↓
Failure
   ↓
Recalculate / Reconverge
   ↓
Activate Backup Path
```

## Key Takeaways

1. **BPDUs are the communication mechanism of STP.**  
    Switches exchange spanning-tree information with each other.
    
2. **The root bridge is the reference point for the spanning tree.**  
    It is elected using the lowest Bridge ID.
    
3. **Bridge priority can influence root bridge selection.**  
    Network administrators can intentionally make a particular switch the preferred root.
    
4. **Every non-root switch selects a root port.**  
    The root port provides the best path toward the root bridge.
    
5. **STP uses path cost to compare paths.**  
    The preferred path generally has the lowest total cost.
    
6. **Designated ports provide the preferred forwarding path for a segment.**
    
7. **Redundant ports may be placed into a non-forwarding state.**  
    This is how STP breaks Layer 2 loops without physically removing the redundant link.
    
8. **Traditional STP uses blocking, listening, learning, and forwarding states.**
    
9. **RSTP provides faster convergence than traditional STP.**  
    This makes it better suited to modern networks where rapid recovery is important.
    
10. **PVST+ creates a separate spanning-tree instance per VLAN.**  
    Different VLANs can therefore have different STP topologies.
    
11. **STP is dynamic.**  
    When an active link fails, the spanning-tree topology can be recalculated and a previously redundant path can become active.
    
12. **The core STP decision process is:**
    

```text
Root Bridge
    ↓
Root Ports
    ↓
Designated Ports
    ↓
Non-forwarding Redundant Ports
    ↓
Loop-Free Tree
```

**The central lesson:** STP turns a physically redundant Layer 2 network into a **controlled, loop-free forwarding topology**, while keeping redundant links available for failover.

## Part 3 - STP Security and Protection

### 1. Why STP Is a Security Concern

STP is designed to protect network availability by preventing Layer 2 loops. However, STP also depends on switches trusting the control information they receive from other devices.

This creates a security concern.

If an unauthorized device is able to participate in STP and send specially crafted or strategically generated BPDUs, it may influence the spanning-tree topology.

The fundamental trust relationship is:

```text
Switch
   ↓
Receives BPDU
   ↓
Uses STP information
   ↓
May change topology
```

This means that STP is not only a switching mechanism. It is also a **control-plane protocol**, and its control messages need to be protected.

### 2. The STP Manipulation Threat

An attacker who gains access to a Layer 2 network may attempt to influence STP by introducing a device that participates in the spanning-tree process.

The general attack concept is:

```text
Unauthorized device
        ↓
Sends STP BPDUs
        ↓
Influences STP calculations
        ↓
Topology changes
        ↓
Traffic may take an unintended path
```

The attacker does not necessarily need to break the encryption of the traffic.

Instead, the goal can be to manipulate the **Layer 2 forwarding topology** itself.

### 3. Root Bridge Takeover

One particularly important attack is attempting to become the root bridge.

Recall that STP selects the root bridge using the Bridge ID.

```text
Lowest Bridge ID
       ↓
Root Bridge
```

If an unauthorized switch advertises a superior Bridge ID, legitimate switches may consider it a better root candidate.

Conceptually:

```text
Normal:

       CORE-SW
         ROOT
        /    \
      SW1    SW2
```

An unauthorized device attempts to influence the election:

```text
       ATTACKER
       "better"
          ROOT
        /      \
      SW1      SW2
```

If successful, the attacker has influenced the logical topology of the network.

This is commonly referred to as **STP root manipulation** or **root bridge takeover**.

### 4. Why Becoming Root Matters

The root bridge is the reference point used by STP when determining paths.

Changing the root can therefore change which links and ports are preferred.

For example:

```text
Before:

        CORE
        ROOT
       /    \
     SW1    SW2
       \    /
        \  /
```

After an attacker becomes root:

```text
      ATTACKER
         ROOT
        /    \
      SW1    SW2
```

Traffic may now follow a topology that was not intended by the network administrator.

The security impact depends on the network architecture, VLAN configuration, switch protections, and where the unauthorized device is connected.

### 5. Topology Influence

STP manipulation does not necessarily require completely taking over the root election.

An attacker may attempt to influence the topology in other ways.

The general concept is:

```text
Unauthorized STP information
          ↓
Switches recalculate topology
          ↓
Port roles may change
          ↓
Traffic paths may change
```

Possible consequences include:

- Unexpected traffic paths
    
- Loss of redundancy
    
- Network instability
    
- Traffic interception opportunities
    
- Increased exposure to denial-of-service conditions
    
- Difficult troubleshooting
    

STP is therefore part of the network's **control plane**, and control-plane manipulation can affect the data plane.

### 6. Potential Man-in-the-Middle Positioning

If an attacker can influence the Layer 2 topology so that traffic passes through an attacker-controlled device, the attacker may create conditions favorable to a **man-in-the-middle (MITM)** position.

Conceptually:

```text
Normal:

Host A ───────── Switch ───────── Host B
```

Potential manipulated topology:

```text
Host A ─── Switch ─── Attacker ─── Switch ─── Host B
```

The important point is that STP manipulation itself does not automatically decrypt traffic.

If the application uses strong end-to-end encryption such as properly configured TLS, an attacker positioned on the Layer 2 path generally cannot simply read the encrypted application payload.

However, gaining a strategically useful forwarding position can still be dangerous because it can enable traffic observation, disruption, or attacks against other protocols.

Therefore:

**Layer 2 topology manipulation can create opportunities for MITM attacks, but STP manipulation does not by itself defeat cryptographic protection.**

### 7. Unauthorized Switches

Another security concern is an unauthorized switch being connected to an access port.

For example:

```text
Corporate switch
      |
      |
Access port
      |
Unauthorized switch
```

The unauthorized switch may introduce:

- Additional Layer 2 paths
    
- Unknown devices
    
- STP BPDUs
    
- Potential loops
    
- Rogue network services
    
- Additional attack surfaces
    

This is why edge ports should not blindly trust STP control traffic from end-user devices.

### 8. BPDU Guard

**BPDU Guard** is a defensive mechanism designed primarily for ports that should connect to end devices rather than other switches.

The basic security model is:

```text
Access port
     ↓
Expected:
PC / printer / phone
     ↓
Should NOT receive BPDUs
```

If the port receives a BPDU, BPDU Guard treats that as a violation.

Conceptually:

```text
Access Port
     ↓
BPDU received
     ↓
BPDU Guard triggered
     ↓
Port protected / disabled according to platform behavior
```

The exact operational response depends on the switch vendor and configuration, but the objective is to prevent an unexpected device from participating in STP through an edge port.

### 9. Why BPDU Guard Is Effective at the Edge

Consider an access switch:

```text
             Switch
          /    |    \
         /     |     \
       PC1    PC2    PC3
```

These ports should normally connect to endpoint devices.

If someone connects another switch:

```text
             Switch
               |
        Unauthorized switch
             /     \
           PC      PC
```

the unauthorized switch can participate in STP.

BPDU Guard provides a mechanism to detect this condition by monitoring for BPDUs on ports where they should not appear.

The security principle is:

**Do not allow an edge port intended for an endpoint to unexpectedly become part of the spanning-tree control topology.**

### 10. PortFast

**PortFast** is commonly used on access ports connected to end devices.

An endpoint does not normally need to participate in spanning-tree calculations.

For example:

```text
Switch ───── PC
```

When the PC connects, there is generally no reason to make the PC wait through the traditional STP transition process before the port can begin normal operation.

PortFast allows an edge port to transition rapidly toward forwarding.

Conceptually:

```text
Normal endpoint port:

Link comes up
     ↓
STP transition
     ↓
Forwarding

With PortFast:

Link comes up
     ↓
Rapidly enters forwarding
```

PortFast is therefore primarily an **edge-port behavior**, not a replacement for STP.

### 11. PortFast and BPDU Guard Together

PortFast and BPDU Guard are commonly used together on access ports.

The design principle is:

```text
Access port
    ↓
PortFast
    +
BPDU Guard
```

PortFast provides fast transition for legitimate endpoint devices.

BPDU Guard protects the same port if a device unexpectedly sends a BPDU.

For example:

```text
Normal:

Switch ───── PC
            ↓
        PortFast
            ↓
       Fast forwarding


Unexpected:

Switch ───── Rogue Switch
            ↓
         BPDU sent
            ↓
       BPDU Guard
            ↓
          Protect
```

These mechanisms address different problems:

```text
PortFast
→ Faster edge-port activation

BPDU Guard
→ Protects edge ports from unexpected BPDUs
```

### 12. Where BPDU Guard Should Be Used

BPDU Guard is most appropriate on ports that are intended to be **edge ports**.

Examples include ports connected to:

- Desktop computers
    
- Laptops
    
- Printers
    
- IP phones
    
- Other endpoint devices
    

It should not be blindly enabled on links that are intentionally connecting switches.

For example:

```text
PC ───── Access Port
       BPDU Guard ✓


Switch ───── Switch
       BPDU Guard
       depends on design
```

A switch-to-switch link is expected to exchange BPDUs, so treating every BPDU as an error would be inappropriate there.

### 13. Root Guard

**Root Guard** addresses a different STP security problem.

BPDU Guard protects an edge port from unexpected BPDUs.

Root Guard protects a designated part of the topology from becoming an unexpected path toward a superior root.

The basic idea is:

```text
Trusted STP region
        |
        |
   Protected port
        |
   Other switch
```

The administrator can use Root Guard where the connected switch should **never be allowed to become the root or influence the topology in that direction**.

If a superior BPDU is received where it should not be, Root Guard can place the affected port into a protective state according to the implementation.

### 14. BPDU Guard vs Root Guard

These two mechanisms are often confused.

|Feature|Primary purpose|
|---|---|
|BPDU Guard|Protect edge ports from unexpected BPDUs|
|Root Guard|Prevent an unexpected downstream device from becoming root through a protected port|
|PortFast|Quickly transition an edge port toward forwarding|

A useful mental model is:

```text
BPDU Guard
"What is a BPDU doing on this endpoint port?"

Root Guard
"Why is this neighboring switch trying to become a better root?"

PortFast
"This port connects to an endpoint, so activate it quickly."
```

### 15. Bridge Priority as a Design Control

Bridge priority is not merely an election parameter. It can also be used as part of deliberate STP design.

A network administrator can establish a predictable hierarchy:

```text
Primary core switch
       ↓
Lowest preferred priority
       ↓
Primary root
```

and:

```text
Secondary core switch
       ↓
Next preferred priority
       ↓
Backup root
```

This creates a more predictable topology.

A well-designed network should not rely on whichever switch happens to have the lowest MAC address becoming root.

Instead, the intended root and backup root should be deliberately selected.

### 16. Root Bridge Placement

The root bridge should generally be positioned where the topology benefits from having it as the logical reference point.

For example:

```text
             Core
          /         \
       Access       Access
       /   \         /   \
      PC   PC       PC   PC
```

Making an appropriate core or distribution switch the root can produce predictable paths.

Poor root placement can result in:

- Longer Layer 2 paths
    
- Unnecessary traffic across links
    
- Inefficient use of redundant links
    
- Less predictable failure behavior
    

Therefore, STP security and STP design are closely related.

### 17. Defensive STP Design

A secure STP deployment should establish clear trust boundaries.

A simplified design is:

```text
                    Core
                 /        \
              Dist        Dist
             /              \
         Access            Access
        /  |  \            / |  \
      PC  PC  PC          PC PC PC
```

The network should distinguish between:

```text
Switch-to-switch links
        ↓
Trusted STP participation
```

and:

```text
Switch-to-endpoint links
        ↓
Restricted STP participation
```

Typical defensive controls include:

- Deliberately configuring the root bridge
    
- Configuring a predictable secondary root
    
- Using PortFast on appropriate edge ports
    
- Using BPDU Guard on appropriate edge ports
    
- Using Root Guard where root protection is required
    
- Avoiding unnecessary Layer 2 extensions
    
- Monitoring STP topology changes
    
- Investigating unexpected BPDUs
    
- Controlling physical access to switching infrastructure
    

### 18. STP Security Is About Trust Boundaries

The central security concept is **trust**.

A switch-to-switch link is normally expected to exchange STP control information:

```text
SW1 ←→ BPDUs ←→ SW2
```

An endpoint port normally should not:

```text
PC ─── Access Port
       ↓
    No BPDUs expected
```

Therefore, network security controls should reflect the intended role of each port.

The more clearly the network defines:

```text
Trusted infrastructure ports
```

versus:

```text
Untrusted edge ports
```

the easier it becomes to protect the STP control plane.

### 19. STP Security Mental Model

The complete security model can be summarized as:

```text
                 STP
                  ↓
          Controls topology
                  ↓
          BPDUs influence
          STP decisions
                  ↓
       ┌──────────┴──────────┐
       ↓                     ↓
 Legitimate switch       Rogue device
       ↓                     ↓
 Normal topology        BPDU manipulation
                             ↓
                    Root/topology influence
                             ↓
                    Traffic path changes
```

Defensive controls:

```text
Edge Port
   ↓
PortFast + BPDU Guard

Protected STP Boundary
   ↓
Root Guard

Network Design
   ↓
Intentional Root Bridge
+
Secondary Root
```

### 20. Final Mental Model

STP security can be remembered through three questions:

```text
1. Who should be allowed to participate in STP?
        ↓
   Control BPDUs at the edge

2. Who should be allowed to become root?
        ↓
   Control root placement
   + Root Guard where appropriate

3. Which ports connect to endpoints?
        ↓
   Use PortFast
   + BPDU Guard
```

The resulting design is:

```text
                Planned Root
                    ROOT
                   /    \
                  /      \
              Switch    Switch
              /   \      /   \
            PC    PC    PC    PC
            ↑                 ↑
       PortFast +        PortFast +
       BPDU Guard        BPDU Guard
```

The infrastructure links participate normally in STP, while edge ports are protected from unexpected STP participation.

## Summary

STP protects Layer 2 networks from forwarding loops, but because it relies on BPDUs and distributed topology decisions, it also introduces a control-plane security consideration. An unauthorized switch that can participate in STP may attempt to influence the topology, including attempting to become the root bridge.

A successful STP manipulation can change forwarding paths and potentially create conditions useful for traffic interception or disruption. It does not, by itself, defeat encryption such as TLS, but controlling the Layer 2 path can provide an attacker with a strategically valuable position.

The primary defensive mechanisms are **BPDU Guard, Root Guard, and PortFast**. BPDU Guard protects edge ports from unexpected BPDUs. Root Guard prevents an unexpected neighboring device from becoming an authoritative root through a protected topology boundary. PortFast allows endpoint ports to transition rapidly toward forwarding and is commonly paired with BPDU Guard.

STP should also be deliberately designed by selecting an appropriate root bridge and secondary root rather than allowing root selection to happen unpredictably.

## Key Takeaways

1. **STP is a control-plane protocol.**  
    Its BPDUs influence how switches construct the Layer 2 topology.
    
2. **BPDUs must be treated as trusted control information.**  
    Unauthorized devices should not be allowed to manipulate STP unnecessarily.
    
3. **STP manipulation can influence the root bridge.**  
    An attacker may attempt a root bridge takeover by advertising a superior Bridge ID.
    
4. **Changing the root can change traffic paths.**  
    This can create instability, inefficient paths, or opportunities for traffic interception.
    
5. **STP manipulation does not automatically break encryption.**  
    A manipulated Layer 2 path and decrypted application traffic are separate issues.
    
6. **BPDU Guard protects edge ports.**  
    If a port intended for an endpoint receives a BPDU, BPDU Guard can protect the network by taking defensive action.
    
7. **PortFast is for edge ports.**  
    It allows endpoint-facing ports to transition rapidly toward forwarding.
    
8. **PortFast and BPDU Guard complement each other.**
    

```text
PortFast
→ Fast edge activation

BPDU Guard
→ Protection from unexpected BPDUs
```

9. **Root Guard protects the STP root hierarchy.**  
    It prevents an unexpected neighboring device from becoming an authoritative root through a protected port.
    
10. **BPDU Guard and Root Guard solve different problems.**
    

```text
BPDU Guard → unexpected BPDU on an edge port

Root Guard → unexpected superior root information
```

11. **The root bridge should be intentionally selected.**  
    STP design should define a primary and, where appropriate, secondary root.
    
12. **STP security depends on trust boundaries.**  
    Switch-to-switch infrastructure links and endpoint-facing access ports should be treated differently.
    
13. **The main security principle is simple:**
    

```text
Control who can participate in STP
        +
Control who can become root
        +
Protect endpoint ports
        ↓
More predictable and secure Layer 2 topology
```

**The central lesson:** STP prevents Layer 2 loops, but the STP control plane itself must be protected. A secure design deliberately controls **BPDUs, root selection, and edge-port behavior**.