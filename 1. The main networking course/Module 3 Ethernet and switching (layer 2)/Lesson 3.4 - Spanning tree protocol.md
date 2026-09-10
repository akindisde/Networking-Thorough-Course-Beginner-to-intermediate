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

