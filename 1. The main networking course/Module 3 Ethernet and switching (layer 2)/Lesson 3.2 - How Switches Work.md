## Part 1 — How a Switch Learns MAC Addresses

A Layer 2 switch makes forwarding decisions using a **MAC address table**, commonly called the **CAM table**.

The central idea is simple:

> **A switch learns where devices are by examining the source MAC address of incoming Ethernet frames.**

Once the switch has learned the location of a destination MAC address, it can forward the frame only through the appropriate port instead of sending it everywhere.

This learning process is the foundation of Ethernet switching.

### What Is a CAM Table?

A **CAM table** is the table a switch uses to associate MAC addresses with the interfaces through which those addresses were learned.

CAM stands for **Content-Addressable Memory**.

A simplified table might look like this:

|VLAN|MAC Address|Port|
|---|---|---|
|10|`00:11:22:33:44:55`|Gi0/1|
|10|`AA:BB:CC:DD:EE:FF`|Gi0/2|
|20|`10:20:30:40:50:60`|Gi0/5|

The switch is effectively maintaining mappings such as:

```
MAC address → VLAN → Switch port
```

The exact implementation varies by switch platform, but the networking concept is the same.

The CAM table allows the switch to answer:

> **"If I need to send a frame to this MAC address, which port should I use?"**

### A Switch Does Not Know Everything Initially

When a switch is first connected to a network, its MAC address table may be empty or contain only a small number of entries.

Consider this topology:

```
        Switch
       /      \
    PC-A      PC-B
```

PC-A:

```
MAC = 00:11:22:33:44:55
Port = Gi0/1
```

PC-B:

```
MAC = AA:BB:CC:DD:EE:FF
Port = Gi0/2
```

Initially, the switch may not know that:

```
00:11:22:33:44:55 → Gi0/1
AA:BB:CC:DD:EE:FF → Gi0/2
```

The switch has to **learn** this information from traffic.

### The Learning Process

Suppose PC-A sends an Ethernet frame.

The frame arrives at the switch through `Gi0/1`:

```
Incoming interface: Gi0/1

Source MAC:
00:11:22:33:44:55

Destination MAC:
AA:BB:CC:DD:EE:FF
```

The first thing the switch does is examine the **source MAC address**.

It sees:

```
Source MAC = 00:11:22:33:44:55
Incoming port = Gi0/1
```

The switch can therefore create an entry:

```
00:11:22:33:44:55 → Gi0/1
```

The switch has now learned where PC-A is located.

This gives us the most important rule of MAC learning:

> **Switches learn source MAC addresses from the port on which the frame arrived.**

### Why the Source MAC Is Used

The source MAC tells the switch:

> **"This frame came from this MAC address."**

The incoming interface tells it:

> **"This MAC address is reachable through this port."**

Together:

```
Source MAC + Incoming port
             ↓
       MAC table entry
```

For example:

```
Frame arrives on Gi0/3

Source MAC:
10:20:30:40:50:60

        ↓

CAM table:
10:20:30:40:50:60 → Gi0/3
```

The switch does not need to know where the source device physically sits in the building. It only needs to know through which switch interface it can reach that MAC address.

### Learning Happens Automatically

MAC learning is normally automatic.

Network administrators do not manually enter every device's MAC address into the switch.

As devices communicate, the switch continuously observes Ethernet frames and updates its table.

Imagine three computers:

```
             Switch
          /     |     \
        PC-A   PC-B   PC-C
```

As traffic arrives, the switch might learn:

```
PC-A → Gi0/1
PC-B → Gi0/2
PC-C → Gi0/3
```

After enough traffic has passed through the switch, its CAM table becomes a representation of the Layer 2 topology it has observed.

### Learning and Forwarding Are Different Operations

A critical distinction is that **learning** and **forwarding** are separate decisions.

When a frame arrives, the switch can perform two conceptual operations:

```
1. Learn the source MAC
2. Determine what to do with the destination MAC
```

For example:

```
Source:
00:11:22:33:44:55

Destination:
AA:BB:CC:DD:EE:FF
```

The switch first learns:

```
00:11:22:33:44:55 → incoming port
```

Then it looks up:

```
AA:BB:CC:DD:EE:FF
```

in the CAM table.

The result determines how the switch handles the frame.

This distinction is extremely important because a switch can **learn the source MAC even when it does not know the destination MAC**.

### The Basic Switch Decision Process

A simplified model of switch operation is:

```
        Ethernet frame arrives
                 ↓
        Read source MAC
                 ↓
        Learn/update source
                 ↓
        Read destination MAC
                 ↓
       Search CAM/MAC table
                 ↓
        ┌────────┴────────┐
        ↓                 ↓
 Destination known   Destination unknown
        ↓                 ↓
   Forward to       Flood within VLAN
   correct port
```

This is the basic decision-making process that will be developed further in Part 2.

### Updating an Existing MAC Entry

Learning is not necessarily a one-time event.

Suppose the switch currently has:

```
00:11:22:33:44:55 → Gi0/1
```

Later, a frame containing that source MAC arrives through `Gi0/5`.

The switch may update the entry:

```
00:11:22:33:44:55 → Gi0/5
```

This allows the switch to adapt when the network changes.

For example, a device might be:

- Moved to another switch port
    
- Connected through a different network path
    
- Associated with a different wireless access point
    
- Migrated as a virtual machine
    
- Reconfigured through a bridge
    

The switch learns from what it currently observes.

### MAC Addresses and VLANs

The CAM table is not simply a global list of MAC addresses.

Switching decisions are made within a **VLAN context**.

Consider:

```
VLAN 10:
00:11:22:33:44:55 → Gi0/1

VLAN 20:
00:11:22:33:44:55 → Gi0/5
```

The same MAC address can potentially appear in different VLAN contexts because VLANs represent separate Layer 2 domains.

Therefore, a more accurate conceptual mapping is:

```
VLAN + MAC address → Port
```

This becomes especially important when studying VLANs and trunking later in the course.

### What a Switch Learns From a Frame

Consider the following frame:

```
Source MAC:
00:AA:BB:CC:DD:01

Destination MAC:
00:AA:BB:CC:DD:02

Incoming port:
Gi0/7

VLAN:
10
```

The switch can learn:

```
VLAN 10
00:AA:BB:CC:DD:01 → Gi0/7
```

Notice what it does **not** necessarily learn from this frame:

```
Destination MAC → outgoing port
```

The destination MAC is used for a lookup.

The source MAC is what provides the information for learning.

This distinction is one of the most important concepts to remember.

### What Happens When the Destination Is Already Known?

Suppose the CAM table contains:

```
VLAN 10
00:AA:BB:CC:DD:02 → Gi0/9
```

A frame arrives on `Gi0/7`:

```
Source:
00:AA:BB:CC:DD:01

Destination:
00:AA:BB:CC:DD:02
```

The switch performs the following operations:

```
Source MAC
    ↓
Learn:
00:AA:BB:CC:DD:01 → Gi0/7

Destination MAC
    ↓
Lookup:
00:AA:BB:CC:DD:02

Result:
Gi0/9
```

The switch can then send the frame out `Gi0/9`.

This is called **known-unicast forwarding**.

The frame does not need to be sent to every port.

### What Happens When the Destination Is Unknown?

Now imagine the switch has learned the source:

```
00:AA:BB:CC:DD:01 → Gi0/7
```

but the destination is not present in the table:

```
00:AA:BB:CC:DD:02
```

The switch does not know which port leads to that MAC address.

Instead of simply discarding the frame, the switch generally **floods the frame** out the other forwarding ports within the relevant VLAN.

Conceptually:

```
             Switch
          /     |     \
        PC-A   PC-B   PC-C
         ↑
      Frame arrives

Destination unknown

         ↓
    Flood to other
    appropriate ports
```

This behavior allows the destination device to receive the frame even though the switch has not learned its location yet.

Part 2 will examine flooding and forwarding in much greater detail.

### Why Flooding Is Necessary

At first, flooding may seem inefficient.

Why not simply discard a frame if the switch does not know where the destination is?

Because doing so would prevent communication with destinations that the switch has not learned yet.

Consider a newly connected device:

```
PC-A ─── Switch ─── PC-B
```

If the switch has never seen PC-B transmit anything, it may not know:

```
PC-B MAC → Gi0/2
```

PC-A sends a frame toward PC-B.

The switch has two choices:

```
Unknown destination
       ↓
Discard
```

or:

```
Unknown destination
       ↓
Flood
```

Flooding allows the frame to reach PC-B.

When PC-B responds, the switch learns PC-B's source MAC and can then forward future traffic more efficiently.

### A Switch Learns Through Normal Traffic

This means a switch's CAM table is constantly being populated and refreshed through ordinary network communication.

Imagine:

```
PC-A → PC-B
PC-C → Server
Printer → PC-A
Server → PC-C
```

Each incoming frame provides source-MAC information.

Over time, the switch builds a table representing the devices it has recently observed.

The CAM table is therefore **dynamic**.

It reflects network activity rather than being a permanent map of every device that exists.

### MAC Table Entries Have a Lifetime

A learned MAC entry does not normally remain in the table forever.

Switches use an **aging mechanism** to remove entries that have not been seen for a certain period.

For example:

```
00:11:22:33:44:55 → Gi0/1
```

may remain in the table while the switch continues to observe traffic from that MAC.

If the device becomes inactive for long enough, the entry can eventually expire.

The exact default aging period depends on the switch platform and configuration, so it should not be treated as a universal fixed value.

### Why Does the CAM Table Age Entries?

Network topology can change.

Devices can:

- Disconnect
    
- Reconnect
    
- Move between ports
    
- Change network paths
    
- Shut down
    
- Be replaced
    
- Appear through virtualization
    

If the switch kept every learned entry forever, its table could contain stale information.

For example:

```
Yesterday:

00:11:22:33:44:55 → Gi0/1
```

The device is then moved:

```
Today:

00:11:22:33:44:55 → Gi0/8
```

The switch needs to eventually forget obsolete information and learn the current location.

Aging helps maintain an accurate forwarding database.

### The Relationship Between Learning, Aging, and Forwarding

These three mechanisms work together:

```
Learning
   ↓
Build MAC → Port information

Aging
   ↓
Remove stale information

Forwarding
   ↓
Use current information to send frames
```

This produces a continuously adapting Layer 2 forwarding system.

### CAM Table as a Network Map

A useful mental model is to think of the CAM table as a **recently learned map of Layer 2 reachability**.

For example:

```
             Switch
          /     |      \
       Gi0/1  Gi0/2   Gi0/3
         │      │       │
       PC-A    PC-B   Server
```

The switch might maintain:

```
PC-A MAC → Gi0/1
PC-B MAC → Gi0/2
Server MAC → Gi0/3
```

If PC-A sends to the server, the switch consults that map.

If the server's MAC is known:

```
Destination MAC
      ↓
CAM lookup
      ↓
Gi0/3
      ↓
Forward
```

If it is unknown:

```
Destination MAC
      ↓
CAM lookup
      ↓
No entry
      ↓
Flood
```

That simple decision is at the heart of Layer 2 switching.

## Summary

A switch uses a **CAM table**, or MAC address table, to associate MAC addresses with switch ports within a VLAN.

The switch builds this table automatically by examining the **source MAC address** of incoming Ethernet frames. If a frame arrives on `Gi0/1` with source MAC `00:11:22:33:44:55`, the switch can learn:

```
00:11:22:33:44:55 → Gi0/1
```

Learning and forwarding are separate operations. The source MAC is used to **learn where a device is**, while the destination MAC is used to **determine where the frame should go**.

When the destination MAC is already known, the switch can perform efficient known-unicast forwarding. When the destination is unknown, the switch generally floods the frame within the appropriate VLAN so that the destination can receive it.

CAM entries are dynamic and can **age out** when the switch stops seeing traffic from a MAC address. This prevents stale information from remaining indefinitely and allows the switch to adapt to topology changes.

The CAM table is therefore a dynamic representation of the Layer 2 devices the switch has recently observed.

## Key Takeaways

- A **CAM table** stores MAC-to-port forwarding information.
    
- The switch learns MAC addresses from the **source MAC of incoming frames**.
    
- The basic learning rule is:
    

```
Source MAC + Incoming Port
            ↓
       CAM table entry
```

- The **source MAC is learned**; the destination MAC is looked up.
    
- A simplified switch process is:
    

```
Receive
→ Learn source
→ Look up destination
→ Forward or flood
```

- A **known destination MAC** allows the switch to forward the frame to the appropriate port.
    
- An **unknown destination MAC** generally causes flooding within the relevant VLAN.
    
- MAC learning happens automatically as traffic passes through the switch.
    
- CAM entries are associated with a **VLAN context** as well as a switch port.
    
- Switches can update a MAC entry if they later observe the same MAC arriving through another port.
    
- Learned MAC entries are **dynamic**, not permanent.
    
- **CAM aging** removes stale entries when they have not been observed for a configured period.
    
- Aging allows switches to adapt to devices moving, disconnecting, reconnecting, or changing network paths.
    
- The CAM table is essentially a **dynamic map of recently learned Layer 2 reachability**.
    
- The next important question is what happens when the destination **is known versus unknown**—this leads directly into **forwarding and flooding**.

### Part 2 — Flooding and Forwarding

### 1. What Happens When a Frame Arrives?

When an Ethernet frame enters a switch, the switch processes the frame and makes a forwarding decision based primarily on:

- The **source MAC address**
    
- The **destination MAC address**
    
- The **VLAN**
    
- The switch's **CAM/MAC address table**
    

A simplified process is:

```
Frame arrives
      ↓
Read source MAC
      ↓
Learn/update source MAC in CAM table
      ↓
Read destination MAC
      ↓
Look up destination in CAM table
      ↓
┌───────────────────────────────┐
│ Is destination MAC known?     │
└───────────────────────────────┘
       ↓ Yes              ↓ No
       ↓                  ↓
Forward to           Flood within
correct port         the VLAN
```

This distinction between **known destinations** and **unknown destinations** is fundamental to understanding switching.

### 2. Known Unicast Forwarding

Suppose a switch has learned the following:

```
VLAN 10

MAC Address          Port
AA:AA:AA:AA:AA:AA    Fa0/1
BB:BB:BB:BB:BB:BB    Fa0/2
CC:CC:CC:CC:CC:CC    Fa0/3
```

If a frame arrives on `Fa0/1` with:

```
Source MAC:      AA:AA:AA:AA:AA:AA
Destination MAC: BB:BB:BB:BB:BB:BB
```

The switch looks up the destination:

```
BB:BB:BB:BB:BB:BB → Fa0/2
```

The switch therefore sends the frame only toward `Fa0/2`.

It does **not** send the frame to every other port.

This is called **known unicast forwarding**.

```
        Host A
          |
        Fa0/1
          |
      +---------+
      | Switch  |
      +---------+
          |
        Fa0/2
          |
        Host B
```

The frame travels only through the required switch path.

This is one of the major reasons switches are much more efficient than traditional hubs.

### 3. Why the Switch Does Not Send a Known Unicast Everywhere

Imagine a switch with 24 ports and 20 connected devices.

If every frame were transmitted through all 20 active ports, the network would generate unnecessary traffic.

Instead, the CAM table allows the switch to determine where the destination device is located.

For example:

```
Destination MAC
       ↓
CAM table lookup
       ↓
Port 12
       ↓
Forward frame to Port 12
```

The other ports do not receive that frame.

This process is often described as **filtering and forwarding**.

The switch:

1. Receives the frame.
    
2. Determines the destination.
    
3. Looks up the destination MAC.
    
4. Selects the appropriate egress port.
    
5. Does not transmit the frame through unrelated ports.
    

### 4. What If the Destination MAC Is Unknown?

A switch cannot forward directly to a destination if it does not know which port leads to that destination.

For example:

```
Source MAC:      AA:AA:AA:AA:AA:AA
Destination MAC: DD:DD:DD:DD:DD:DD
```

But the CAM table contains:

```
AA:AA:AA:AA:AA:AA → Fa0/1
BB:BB:BB:BB:BB:BB → Fa0/2
CC:CC:CC:CC:CC:CC → Fa0/3
```

There is no entry for:

```
DD:DD:DD:DD:DD:DD
```

The destination is therefore an **unknown unicast**.

The switch cannot safely choose a single destination port.

Instead, it **floods** the frame.

### 5. Unknown Unicast Flooding

Unknown unicast flooding means the switch sends the frame out multiple eligible ports within the relevant VLAN, except the port on which the frame arrived.

For example:

```
             Host B
               |
             Fa0/2
               |
               |
Host A ---- Switch ---- Host C
 Fa0/1        |          Fa0/3
              |
            Host D
             Fa0/4
```

If Host A sends a frame to an unknown MAC address, the switch may send copies out:

```
Fa0/2
Fa0/3
Fa0/4
```

but not back out:

```
Fa0/1
```

because that is the ingress port.

Conceptually:

```
                 ┌──→ Port 2
                 │
Incoming Frame ──┼──→ Port 3
                 │
                 └──→ Port 4
```

The switch is effectively making the frame available to the other devices in the relevant Layer 2 domain.

If the destination device receives the frame, it can process it. Other devices normally discard frames that are not addressed to them.

### 6. Why Flooding Is Necessary

Flooding may initially seem inefficient, but it solves an important problem.

A switch learns dynamically.

When a device first communicates, the switch may not yet know where that device is located.

Suppose Host A wants to communicate with Host D.

Initially:

```
CAM table:

Host A → Fa0/1
Host D → unknown
```

Host A sends:

```
A → D
```

The switch knows where A is, but not D.

Therefore:

```
A → Switch
       ├──→ Fa0/2
       ├──→ Fa0/3
       └──→ Fa0/4
```

When Host D responds, the switch sees D's source MAC on the incoming frame.

It can then learn:

```
Host D → Fa0/4
```

Now the switch knows where D is.

Future traffic can be forwarded directly:

```
A → Switch → Fa0/4 → D
```

This is the basic learning cycle:

```
Unknown destination
        ↓
      Flood
        ↓
Destination responds
        ↓
Learn source MAC
        ↓
Destination becomes known
        ↓
Direct forwarding
```

### 7. Broadcast Flooding

Flooding is not limited to unknown unicast traffic.

Ethernet also has **broadcast frames**.

The Ethernet broadcast destination is:

```
FF:FF:FF:FF:FF:FF
```

A broadcast frame is intended for all devices in the relevant Layer 2 broadcast domain.

A switch therefore floods broadcast traffic within the appropriate VLAN.

For example:

```
             Host B
               ↑
               |
Host A → Switch ┼──→ Host C
               |
               ↓
             Host D
```

If Host A sends a broadcast frame, the switch forwards it to the other eligible ports belonging to that VLAN.

This behavior is normal and necessary for protocols such as **ARP** in IPv4.

### 8. Unknown Unicast vs Broadcast

These two types of flooding are related but should not be confused.

|Traffic type|Destination known?|Switch behavior|
|---|---|---|
|Known unicast|Yes|Forward to specific port|
|Unknown unicast|No|Flood within VLAN|
|Broadcast|Not applicable|Flood within VLAN|
|Multicast|Depends on configuration|Forward/flood according to multicast handling|

For example:

```
Known unicast:
A → B
     ↓
   Port 2 only
```

```
Unknown unicast:
A → Unknown
     ↓
   Multiple eligible ports
```

```
Broadcast:
A → FF:FF:FF:FF:FF:FF
     ↓
   Multiple eligible ports
```

The key question for known unicast traffic is:

> **Does the switch know which port leads to the destination MAC in the relevant VLAN?**

### 9. Flooding Is VLAN-Scoped

A very important concept is that flooding does **not** normally mean sending the frame through every physical port on the entire switch.

Flooding occurs within the relevant **VLAN**.

Suppose a switch has:

```
VLAN 10:
Fa0/1
Fa0/2
Fa0/3

VLAN 20:
Fa0/4
Fa0/5
Fa0/6
```

If a device in VLAN 10 sends an unknown unicast frame, the switch floods it among eligible ports in **VLAN 10**.

It does not simply flood the frame into VLAN 20.

Conceptually:

```
              Switch
          ┌─────────────┐
VLAN 10 → │ Ports 1-3   │ ← flooding stays here
          │             │
VLAN 20 → │ Ports 4-6   │
          └─────────────┘
```

This is one reason VLANs are important for controlling Layer 2 broadcast and flooding domains.

### 10. The Ingress Port Is Not Used for Flooding

When a switch receives a frame, the incoming interface is called the **ingress port**.

Ports through which the switch sends the frame are **egress ports**.

If a frame arrives on:

```
Fa0/1
```

and must be flooded, the switch does not normally send the same frame back out `Fa0/1`.

Instead:

```
Ingress:
Fa0/1
   ↓
 Switch
   ↓
Egress:
Fa0/2
Fa0/3
Fa0/4
```

This prevents the switch from unnecessarily returning the frame to the device that transmitted it.

### 11. What Happens When a Destination Is Learned Later?

CAM entries can change over time.

Suppose the switch initially has:

```
AA:AA:AA:AA:AA:AA → Fa0/1
BB:BB:BB:BB:BB:BB → Fa0/2
```

A frame arrives from:

```
BB:BB:BB:BB:BB:BB
```

on `Fa0/5`.

The switch can update its knowledge:

```
BB:BB:BB:BB:BB:BB → Fa0/5
```

The previous association with `Fa0/2` is no longer the current location.

This matters when devices move between switch ports, when links change, or when network topology changes.

The CAM table is therefore not a permanent inventory.

It is a **dynamic forwarding database**.

### 12. CAM Aging and Flooding

CAM entries normally have an aging mechanism.

If a learned MAC address is not seen for a period of time, its dynamic entry can expire.

For example:

```
Before aging:

AA:AA:AA:AA:AA:AA → Fa0/1
```

After the entry expires:

```
AA:AA:AA:AA:AA:AA → unknown
```

If another device then sends a frame to that MAC address, the switch no longer has a specific port to use.

The result may be:

```
Destination unknown
        ↓
Unknown-unicast flooding
        ↓
Destination responds
        ↓
Source MAC learned again
        ↓
Direct forwarding resumes
```

This is normal switch behavior.

Aging prevents stale information from remaining indefinitely in the forwarding table.

### 13. Switch vs Hub

Understanding flooding becomes much easier when compared with a hub.

A traditional Ethernet hub operates essentially at the physical layer.

If a signal enters one port, the hub repeats it to other ports.

A switch, however, makes Layer 2 forwarding decisions based on MAC addresses.

|Hub|Switch|
|---|---|
|Primarily Layer 1|Primarily Layer 2|
|Does not build a MAC table|Builds a MAC/CAM table|
|Repeats traffic broadly|Forwards known unicast selectively|
|No MAC-based forwarding decision|Uses destination MAC for forwarding|
|Shared collision domain in traditional Ethernet|Each switch port is its own collision domain in typical full-duplex Ethernet|

A switch can temporarily **behave similarly to a hub for particular traffic**, especially unknown unicast or broadcast traffic, because it must flood that traffic.

But this does not make the switch a hub.

The difference is that the switch is making a forwarding decision based on Layer 2 information.

### 14. A Complete Example

Consider three hosts:

```
Host A
MAC: AA:AA:AA:AA:AA:AA
        |
      Fa0/1
        |
        v
    +---------+
    | Switch  |
    +---------+
        |
      Fa0/2
        |
Host B
MAC: BB:BB:BB:BB:BB:BB
```

Initially:

```
CAM table:

AA:AA:AA:AA:AA:AA → Fa0/1
BB:BB:BB:BB:BB:BB → unknown
```

Host A sends a frame to Host B:

```
Source:      AA:AA:AA:AA:AA:AA
Destination: BB:BB:BB:BB:BB:BB
```

The switch performs a destination lookup.

```
BB:BB:BB:BB:BB:BB
        ↓
     Not found
        ↓
Unknown unicast
        ↓
     Flood
```

Host B receives the frame and responds.

The response contains:

```
Source:      BB:BB:BB:BB:BB:BB
Destination: AA:AA:AA:AA:AA:AA
```

The switch sees the source MAC:

```
BB:BB:BB:BB:BB:BB
```

arriving on `Fa0/2`.

It learns:

```
BB:BB:BB:BB:BB:BB → Fa0/2
```

Now the CAM table is:

```
MAC Address          Port
AA:AA:AA:AA:AA:AA    Fa0/1
BB:BB:BB:BB:BB:BB    Fa0/2
```

The next frame from A to B is therefore forwarded directly:

```
Host A
   |
 Fa0/1
   |
Switch
   |
 Fa0/2
   |
Host B
```

No flooding is required.

### 15. The Core Switching Logic

You can summarize the switch's basic Layer 2 behavior as:

```
1. Receive Ethernet frame
2. Read source MAC
3. Learn/update source MAC → ingress port
4. Read destination MAC
5. Determine VLAN context
6. Look up destination MAC
7. If known:
      forward toward the correct port
8. If unknown:
      flood within the VLAN
9. If broadcast:
      flood within the VLAN
```

The most important distinction is:

```
SOURCE MAC
    ↓
LEARN WHERE IT IS

DESTINATION MAC
    ↓
DECIDE WHERE TO SEND
```

This simple mechanism is the foundation of Ethernet switching.

## Summary

A switch uses its CAM/MAC address table to determine where Layer 2 destinations are located. When a frame arrives, the switch learns the **source MAC address** from the ingress port and then uses the **destination MAC address** to make a forwarding decision.

If the destination MAC is known, the switch performs **known-unicast forwarding** and sends the frame only toward the appropriate port. If the destination MAC is unknown, the switch performs **unknown-unicast flooding** within the relevant VLAN. Broadcast traffic is also flooded within its VLAN.

Flooding allows communication to work before the switch has learned the destination's location. When the destination responds, the switch can learn its source MAC and subsequently forward traffic directly.

CAM entries are dynamic and can age out. When an entry disappears, traffic destined for that MAC may temporarily become unknown and therefore be flooded again.

## Key Takeaways

- A switch learns **source MAC addresses** from incoming Ethernet frames.
    
- The CAM table maps MAC addresses to forwarding information, normally within a **VLAN context**.
    
- The **destination MAC** is used to determine where a frame should go.
    
- **Known unicast** traffic is forwarded to the specific destination port.
    
- **Unknown unicast** traffic is flooded within the relevant VLAN.
    
- **Broadcast** traffic is flooded within the relevant VLAN.
    
- Flooding normally excludes the frame's **ingress port**.
    
- Flooding does not mean the frame is sent indiscriminately into every VLAN.
    
- CAM aging removes stale dynamic entries.
    
- When an entry ages out, traffic to that destination may temporarily be flooded again.
    
- A switch may flood some traffic, but unlike a hub, it uses a **MAC address table to make selective forwarding decisions**.

### Part 3 — MAC Flooding and Port Security

### 1. Why the CAM Table Matters for Security

The CAM table is essential for normal switching because it tells the switch where Layer 2 destinations can be reached.

However, the same mechanism creates a security concern.

A switch has finite hardware resources for storing forwarding information. If an attacker can cause a switch to learn a very large number of different source MAC addresses, the switch may eventually have difficulty maintaining normal MAC-to-port mappings.

This type of attack is commonly called a **MAC flooding attack** or **CAM table overflow attack**.

The important concept is:

```
Normal traffic
     ↓
Learn source MAC addresses
     ↓
CAM table grows normally
     ↓
Known destinations → direct forwarding
```

An attack attempts to turn this into:

```
Large number of fake source MAC addresses
     ↓
CAM table resources consumed
     ↓
Some legitimate destination entries may be unavailable
     ↓
More destinations become unknown
     ↓
Unknown-unicast flooding increases
```

### 2. What Is MAC Flooding?

**MAC flooding** is an attack in which a large number of Ethernet frames with different source MAC addresses are sent toward a switch in an attempt to consume CAM-table resources.

The attacker does not necessarily need to know the real MAC addresses of the other devices.

The basic idea is to generate many apparently different source addresses:

```
Frame 1 → Source MAC: 02:00:00:00:00:01
Frame 2 → Source MAC: 02:00:00:00:00:02
Frame 3 → Source MAC: 02:00:00:00:00:03
Frame 4 → Source MAC: 02:00:00:00:00:04
...
Frame N → Source MAC: different
```

The switch attempts to learn these source addresses.

Conceptually:

```
Many unique source MACs
          ↓
     MAC learning
          ↓
      CAM table
          ↓
      Resources
       consumed
```

The exact behavior depends on the switch hardware, software, configuration, and available forwarding resources.

### 3. CAM Table Overflow

A CAM table is not infinitely large.

A switch has a finite amount of hardware memory and forwarding resources for Layer 2 entries.

If an attacker generates enough unique source MAC addresses, the switch may reach a resource limit.

A simplified representation is:

```
CAM capacity

[████████████████████] 100%
```

Once available resources become constrained, the switch may no longer be able to maintain all expected dynamic MAC entries.

This can result in legitimate destination MAC addresses becoming unknown to the switch.

For example, before the attack:

```
AA:AA:AA:AA:AA:AA → Fa0/1
BB:BB:BB:BB:BB:BB → Fa0/2
CC:CC:CC:CC:CC:CC → Fa0/3
```

After sufficient table pressure, some entries may no longer be available:

```
AA:AA:AA:AA:AA:AA → unknown
BB:BB:BB:BB:BB:BB → Fa0/2
CC:CC:CC:CC:CC:CC → unknown
```

Traffic destined for an unknown unicast MAC can then be flooded within the relevant VLAN.

### 4. Why CAM Overflow Can Increase Flooding

Recall the normal forwarding process:

```
Destination MAC
       ↓
CAM lookup
       ↓
Known?
 ┌─────┴─────┐
Yes         No
 ↓           ↓
Forward     Flood
```

A CAM-table attack attempts to make the **"No"** condition occur more frequently.

For example:

```
Before:
Host A → Host B
         ↓
      Known MAC
         ↓
      Port 2 only
```

Under CAM-table pressure:

```
Host A → Host B
         ↓
   MAC not available
         ↓
 Unknown unicast
         ↓
 Flood within VLAN
```

This can increase unnecessary traffic and reduce the confidentiality benefits normally provided by selective Layer 2 forwarding.

### 5. Important Limitation: Flooding Does Not Automatically Mean Full Traffic Capture

A common misconception is:

> “If the CAM table is full, an attacker automatically sees all traffic.”

That is too simplistic.

CAM-table pressure can cause increased flooding, but it does not guarantee that every frame will be delivered to the attacker's port.

The actual result depends on:

- Switch hardware
    
- Switch software
    
- VLAN configuration
    
- Forwarding architecture
    
- Security features
    
- Port configuration
    
- Traffic patterns
    
- How the switch handles resource exhaustion
    

Therefore, MAC flooding should be understood primarily as a technique for **disrupting normal Layer 2 forwarding and increasing flooding**, not as a guaranteed method of capturing every frame.

Modern switches also implement protections that can make this attack less effective.

### 6. The Role of `macof`

A commonly known tool for generating large numbers of Ethernet frames with varying source MAC addresses is `**macof**`, associated with the dsniff toolset.

In an authorized lab, a command such as:

```
sudo macof -i eth0
```

can be used to demonstrate the concept of generating many frames with changing source MAC addresses.

This should only be used on a network you own or are explicitly authorized to test.

The important learning objective is not the command itself.

The important mechanism is:

```
Many unique source MAC addresses
            ↓
Switch learns them
            ↓
CAM resources become heavily used
            ↓
Legitimate entries may be displaced/unavailable
            ↓
Unknown destinations increase
            ↓
Flooding can increase
```

### 7. Why Attackers Use MAC Flooding

MAC flooding can be used for several security objectives.

#### Denial of Service

The most direct goal is to interfere with normal Layer 2 forwarding.

Excessive flooding can consume:

- Switch resources
    
- Link bandwidth
    
- Host processing capacity
    

This can contribute to degraded network performance.

#### Increased Traffic Exposure

If unknown-unicast flooding increases, more frames may be transmitted through ports that normally would not receive them.

This can increase the opportunity for traffic observation in environments where other protections are weak.

However, this should not be confused with guaranteed packet interception.

#### Testing Network Defenses

Security professionals may intentionally generate controlled MAC-learning pressure to determine whether network protections such as port security are correctly configured.

This belongs in an authorized testing environment.

### 8. Why MAC Addresses Are Not Strong Authentication

MAC flooding is easier to understand when you recognize a larger security principle:

**A MAC address is an identifier, not proof of identity.**

A device can potentially:

- Change its MAC address
    
- Randomize its MAC address
    
- Use a virtual MAC address
    
- Spoof another device's MAC address
    
- Generate many different source MAC addresses
    

Therefore, a security policy such as:

```
“MAC AA:AA:AA:AA:AA:AA is trusted.”
```

does not prove that the legitimate device is actually present.

The switch is primarily observing what a frame claims its source MAC address is.

This is fundamentally different from cryptographic authentication.

### 9. MAC Filtering and Its Limitations

Some networks use **MAC filtering** to permit or deny devices based on their MAC addresses.

For example:

```
Allowed MACs:

AA:AA:AA:AA:AA:AA
BB:BB:BB:BB:BB:BB
CC:CC:CC:CC:CC:CC
```

At first glance, this may appear to provide strong access control.

It does not.

An attacker who can change their network interface's MAC address may be able to impersonate an allowed address.

Conceptually:

```
Authorized device:
AA:AA:AA:AA:AA:AA
        ↓
Allowed

Attacker:
Original MAC
        ↓
Change/spoof MAC
        ↓
AA:AA:AA:AA:AA:AA
        ↓
May appear to match the allowed address
```

This is why **MAC filtering should not be treated as strong authentication**.

It can still have operational value in some environments, but it should not be the primary mechanism for proving device or user identity.

### 10. Port Security

A stronger Layer 2 control is **switch port security**.

Port security allows an administrator to place restrictions on the MAC addresses that can appear on a switch port.

One important control is the **maximum number of MAC addresses** allowed on a port.

For example:

```
Port Fa0/1
Maximum allowed MAC addresses: 2
```

The switch can then restrict the number of source MAC addresses learned or authorized on that port according to its configuration.

This is particularly useful for reducing the effectiveness of MAC flooding from an ordinary access port.

### 11. Maximum MAC Addresses Per Port

Consider a workstation port:

```
Fa0/1
   |
Workstation
```

An administrator might expect only one endpoint to be connected.

The port can therefore be configured with a maximum of one secure MAC address.

Conceptually:

```
Fa0/1
Maximum MACs = 1
```

If the port suddenly attempts to use many different source MAC addresses:

```
MAC 1 ✓
MAC 2 ✗
MAC 3 ✗
MAC 4 ✗
...
```

the switch can treat this as a port-security violation.

The maximum should be chosen according to the actual topology.

For example, a port connected to a single workstation may reasonably use a low limit, while a port connected to an authorized downstream device or specialized infrastructure may legitimately require more.

### 12. Port-Security Violation Actions

When a port-security rule is violated, the switch needs to decide what to do.

Common Cisco IOS violation modes include:

- **protect**
    
- **restrict**
    
- **shutdown**
    

Their exact behavior can vary by platform and software version, so always verify the vendor documentation for the device being configured.

Conceptually:

```
Unauthorized MAC detected
          ↓
Port-security violation
          ↓
┌─────────┼──────────┐
Protect  Restrict  Shutdown
```

#### Protect

The switch can drop traffic from unauthorized source MAC addresses while keeping the port operational.

The exact handling of logging/counters depends on platform behavior.

#### Restrict

The switch can drop traffic from unauthorized source MAC addresses while also providing additional violation information such as counters or logging, depending on the platform.

#### Shutdown

The switch places the port into a disabled/error state in typical Cisco implementations.

This is generally the most disruptive response, but it can provide strong containment.

### 13. Cisco IOS Configuration Example

On Cisco IOS, a basic port-security configuration can look like:

```
interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security violation shutdown
```

This configuration conceptually means:

```
Access port
     ↓
Port security enabled
     ↓
Maximum 2 secure MAC addresses
     ↓
Violation → shutdown
```

The exact syntax and available options depend on the switch vendor and operating system.

The important concepts are:

```
Maximum MACs
+
Violation action
=
Port-security policy
```

### 14. Sticky MAC Addresses

Many Cisco switches also support **sticky MAC learning**.

A simplified configuration example is:

```
interface FastEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
```

With sticky learning, the switch can dynamically learn a MAC address and treat the learned address as a secure address according to the port-security configuration.

Conceptually:

```
First expected device
        ↓
MAC learned
        ↓
MAC becomes secure/sticky
        ↓
Unexpected MAC appears
        ↓
Port-security violation
```

Sticky MAC is convenient, but administrators should understand how the configuration is stored and persisted on their specific platform.

### 15. Port Security vs MAC Filtering

These controls are related but not equivalent.

|Feature|MAC Filtering|Port Security|
|---|---|---|
|Primary idea|Allow/deny based on MAC|Restrict MAC behavior on a port|
|Limits MAC count|Usually not the primary purpose|Yes|
|Helps against MAC flooding|Weak|Much stronger mitigation|
|Prevents MAC spoofing completely|No|No|
|Can define violation actions|Depends on implementation|Yes|
|Strong authentication|No|No|
|Useful Layer 2 control|Yes, in limited cases|Yes|

Port security is not magic.

An attacker may still spoof an allowed MAC address if the control only checks the address itself.

Its major advantage for this lesson is that it can **limit how many MAC addresses can appear on an access port**, making large-scale source-MAC generation much harder from that port.

### 16. Port Security Does Not Replace Authentication

A strong network security design should distinguish between:

```
Port security
     ↓
Controls Layer 2 behavior
```

and:

```
802.1X
     ↓
Provides port-based network access control
     ↓
Authentication framework
```

Port security is useful for controlling the number and behavior of MAC addresses on a port.

It does not establish strong cryptographic identity by itself.

For environments requiring stronger access control, **802.1X** can be used with an appropriate authentication method and authentication infrastructure.

The important principle is:

> **A forwarding restriction is not the same thing as authentication.**

### 17. Security Design Principle

When securing a Layer 2 network, ask what each control actually protects against.

For example:

```
MAC filtering
    ↓
Basic MAC-based allow/deny
    ↓
Weak against spoofing
```

```
Port security
    ↓
Limits MAC behavior per port
    ↓
Useful against excessive MAC learning
```

```
802.1X
    ↓
Port-based network access control
    ↓
Can provide stronger authentication
```

No single Layer 2 feature should be treated as a complete security solution.

### 18. The Complete Attack-and-Defense Model

The entire concept can be summarized as:

```
Normal operation
      ↓
Switch learns source MACs
      ↓
CAM table maps MACs to ports
      ↓
Known unicast → direct forwarding
```

An attacker attempts:

```
Many unique source MACs
      ↓
Excessive MAC learning
      ↓
CAM resources pressured
      ↓
Some destinations may become unknown
      ↓
More unknown-unicast flooding
```

A defensive configuration can respond:

```
Access port
      ↓
Port security
      ↓
Maximum MAC addresses
      ↓
Violation detection
      ↓
Protect / Restrict / Shutdown
```

For stronger access control:

```
802.1X
  ↓
Authentication
  ↓
Authorized network access
```

This illustrates an important security principle:

> **Good network security controls the behavior that creates risk, rather than simply assuming that an observed MAC address represents a trusted device.**

## Summary

A CAM table is a finite switch resource used to maintain Layer 2 forwarding information. A MAC flooding attack attempts to consume these resources by generating frames with many different source MAC addresses. If legitimate MAC entries become unavailable or unknown, the switch may perform more unknown-unicast flooding, increasing traffic and potentially degrading network performance.

The commonly known `macof` tool can demonstrate this behavior in an authorized security lab, but the important concept is the relationship between excessive source-MAC learning, CAM resources, and flooding.

MAC filtering is not strong authentication because MAC addresses can be changed or spoofed. **Port security** provides a stronger Layer 2 defense against excessive MAC learning by allowing administrators to restrict the number of MAC addresses associated with a port and define actions when violations occur.

Cisco IOS, for example, can configure port security with a maximum MAC count and a violation mode such as `shutdown`. Sticky MAC learning can also be used where appropriate.

Port security is a Layer 2 control, not a replacement for authentication. For stronger network-access control, technologies such as **802.1X** should be considered.

## Key Takeaways

- CAM tables contain finite forwarding resources.
    
- **MAC flooding** attempts to consume CAM resources with many unique source MAC addresses.
    
- CAM-table pressure can cause legitimate destinations to become unknown.
    
- Unknown destinations can result in increased **unknown-unicast flooding**.
    
- CAM flooding does **not** guarantee that an attacker will see all network traffic.
    
- `macof` can be used to demonstrate MAC flooding in an authorized lab.
    
- MAC addresses are identifiers, not strong proof of identity.
    
- **MAC filtering is weak against MAC spoofing.**
    
- **Port security** can limit the number of MAC addresses allowed on a switch port.
    
- Common port-security violation actions include **protect, restrict, and shutdown**, depending on platform.
    
- Cisco IOS examples include:
    
    - `switchport port-security`
        
    - `switchport port-security maximum`
        
    - `switchport port-security violation`
        
- Sticky MAC addresses can automatically learn and secure MAC addresses according to the port-security policy.
    
- Port security helps control Layer 2 behavior but does not replace strong authentication.
    
- **802.1X** provides a stronger model for port-based network access control.
    
- The key security principle is to understand exactly what a control verifies and what attack it actually mitigates.