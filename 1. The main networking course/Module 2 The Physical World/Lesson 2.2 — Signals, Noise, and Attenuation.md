## Lesson 2.2 — Signals, Noise, and Attenuation — Part 1

### From Bits to Physical Signals

In networking, we often talk about data as bits:

```
1011010010110100
```

But a physical network cannot literally send an abstract 1 or 0 through a cable or through the air.

At some point, those bits have to be represented as a physical phenomenon that can travel from one device to another.

This is the job of the physical layer.

The basic process is:

```
Digital Information
        ↓
Physical-Layer Encoding
        ↓
Physical Signal
        ↓
Transmission Medium
        ↓
Physical Signal
        ↓
Receiver
        ↓
Recovered Information
```

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Physical layer encoding|Definitions]]

<u>The physical signal depends on the medium being used.</u>

The three important cases in this lesson are:

```
Copper
Bits → Electrical Signal

Fiber
Bits → Light

Wireless
Bits → Radio Waves
```

The logical information can be the same, but the physical representation is completely different.

### Why Networking Needs Physical Signals

Consider a computer sending a packet to another computer.

At the networking layers above the physical layer, we can think about:

```
Application Data
        ↓
Transport Segment
        ↓
Network Packet
        ↓
Ethernet Frame
```

Eventually, however, the frame must be converted into something that can physically travel between the two devices.

For copper Ethernet, this means electrical signaling.

For fiber Ethernet, this means optical signaling.

For Wi-Fi, this means radio-frequency electromagnetic signaling.

The physical layer therefore forms the connection between the logical networking system and the real physical world.

```
Logical Networking
        ↓
Physical Representation
        ↓
Physical Medium
```

### Copper — Electrical Signaling

Twisted-pair Ethernet uses copper conductors to carry electrical signals.

A transmitter generates controlled electrical changes on the conductors.

A simplified visualization might look like:

```
Voltage

  ^
  |
  |     ┌───┐       ┌───┐
  |     │   │       │   │
  |─────┘   └───────┘   └────→ Time
  |
```

This diagram is intentionally simplified.

Modern Ethernet does not simply use a rule such as:

```
1 = high voltage
0 = low voltage
```

Ethernet physical layers use specific signaling, encoding, modulation, and decoding techniques depending on the Ethernet standard.

The important concept is that the digital information is transformed into an electrical waveform that can propagate through the copper conductors.

```
Bits
 ↓
Encoding
 ↓
Electrical Signal
 ↓
Copper
```

At the other end, the receiving interface observes the electrical signal and uses the appropriate physical-layer technology to recover the transmitted information.

### [[Differential Signaling]]

Twisted-pair Ethernet commonly uses **differential signaling**.

Rather than simply measuring one conductor against ground, the receiver examines the electrical difference between conductors in a pair.

Conceptually:

```
Wire A:  +V
Wire B:  -V
Difference between A and B
          ↓
        Signal
```

The two conductors carry related electrical signals.

The receiver uses their difference to determine the transmitted signal.

This is important because external interference can affect both conductors in a similar way.

If unwanted electrical energy is introduced similarly onto both conductors, the receiver can reject much of that common interference when determining the difference between them.

This property contributes to the ability of twisted-pair Ethernet to operate reliably in environments containing electrical noise.

![[Pasted image 20260913074111.png]]

### Why the Cable Has Pairs

An Ethernet twisted-pair cable contains:

```
8 conductors
↓
4 twisted pairs
```

Each pair consists of two conductors.

Conceptually:

```
Pair 1
Conductor A ╲╱╲╱╲╱╲╱
Conductor B ╱╲╱╲╱╲╱╲

Pair 2
Conductor A ╲╱╲╱╲╱╲╱
Conductor B ╱╲╱╲╱╲╱╲
```

The twisting is not decorative.

It is part of the <u>electrical engineering of the cable.</u>

The physical geometry of the conductors affects how signals interact with each other and with external electromagnetic fields.

This becomes particularly important when discussing:

```
Noise
Interference
Crosstalk
Signal-to-noise ratio
```

Those concepts will be examined in the next part.

### Fiber — Optical Signaling

Fiber optic networking uses light instead of electrical signaling through copper.

The transmitter converts electrical information into an optical signal.

A simplified representation might look like:

```
Light

ON     OFF     ON     ON     OFF

████   ____    ████   ████   ____
```

Again, real optical communication is more sophisticated than simply turning a light source on and off for every individual bit.

Different optical technologies use different forms of encoding and modulation.

The fundamental concept is:

```
Electrical Information
        ↓
Optical Transmitter
        ↓
Light
        ↓
Fiber
        ↓
Optical Receiver
        ↓
Electrical Information
```

The fiber itself does not understand IP, TCP, HTTP, or any other protocol.

It provides a physical path through which the optical signal travels.

### What Actually Travels Through Fiber?

The information is represented by properties of an optical signal.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What are the properties of an optical signal ?|Q&A]]

The physical medium is typically a glass fiber consisting of structures such as:

```
Cladding
    ↓
Core
```

The optical signal propagates through the fiber's core.

The important distinction for this lesson is:

```
Copper → Electrical energy
Fiber → Optical energy
```

This difference gives fiber very different physical characteristics from copper.

For example, fiber is not affected by electromagnetic interference in the same way that metallic conductors are.

Fiber also supports extremely long-distance communication and very high data rates.

Those properties will become increasingly important as we study physical networks.

### Wireless — Radio Signals

Wireless networking removes the physical cable between the transmitter and receiver.

Instead, the transmitter generates a radio-frequency electromagnetic signal.

The antenna radiates electromagnetic energy into the surrounding environment.

The receiving antenna detects part of that energy.

The simplified process is:

```
Digital Information
        ↓
Radio Transmitter
        ↓
Electromagnetic Wave
        ↓
Free Space
        ↓
Radio Receiver
        ↓
Recovered Information
```

Wi-Fi is therefore not sending packets through empty space as abstract data.

The wireless interface converts the information into a radio signal that can propagate through the environment.

### Radio Is Not Confined to a Cable

This creates a major physical difference between wireless and wired networking.

With a cable:

```
Device A
   |
Cable
   |
Device B
```

The physical transmission medium is relatively confined.

With wireless:

```
             Device B
                ↑
             ↗     ↖
          ↗           ↖
       ↗                 ↖
Device A  )))))))))))))))))
```

The radio signal propagates through the surrounding space.

This means the signal can potentially be detected by devices that are not part of the intended communication.

This is one of the reasons wireless networks require strong security mechanisms such as encryption.

The physical medium itself does not provide the same containment as a cable.

### Comparing the Three

At the physical level:

|Medium|Physical Signal|Transmission Environment|
|---|---|---|
|Twisted-pair copper|Electrical signal|Copper conductors|
|Fiber optic|Optical signal|Fiber|
|Wireless|Electromagnetic/radio signal|Free space|

The same general networking architecture can operate over all three.

For example, an application may send data through TCP/IP without caring whether the final link is:

```
Ethernet over copper
Ethernet over fiber
Wi-Fi over radio
```

The lower layers provide the necessary translation.

### One Message, Different Physical Forms

Imagine a computer sending an HTTP request.

At the application level:

```
HTTP Data
```

At the transport layer:

```
TCP Segment
```

At the network layer:

```
IP Packet
```

At the link layer:

```
Ethernet Frame
```

At the physical layer, the representation depends on the medium.

Over copper:

```
Ethernet Frame
        ↓
Electrical Encoding
        ↓
Electrical Signal
        ↓
Copper
```

Over fiber:

```
Ethernet Frame
        ↓
Optical Encoding
        ↓
Optical Signal
        ↓
Fiber
```

Over Wi-Fi:

```
Network Data
        ↓
802.11 Frame
        ↓
Radio Encoding/Modulation
        ↓
Radio Signal
        ↓
Free Space
```

The higher-level information can remain conceptually the same while the physical representation changes.

### The Receiver Has a Difficult Job

The transmitter knows what it sent.

The receiver only observes what arrives.

<mark style="background:#fff88f">Those are not necessarily identical.</mark>

A simplified model is:

```
Transmitted Signal
        ↓
     Medium
        ↓
Attenuation
        +
Noise
        +
Interference
        +
Other Physical Effects
        ↓
Received Signal
        ↓
Receiver
        ↓
Recovered Data
```

This is one of the most important ideas in physical networking.

The receiver has to distinguish the intended signal from everything else that exists around it.

If the signal arrives cleanly, the receiver has a relatively easy task.

If the signal has been weakened or contaminated by interference, the receiver has a much harder task.

This leads directly into the next major concepts:

```
Attenuation
Noise
Interference
Crosstalk
Signal-to-Noise Ratio
```

### The Fundamental Physical-Layer Problem

The entire problem can be reduced to this:

```
How can the receiver reliably determine
what the transmitter actually sent?
```

The transmitter creates a physical signal.

The signal travels through a physical medium.

The medium is imperfect.

The signal may weaken.

Other energy may interfere with it.

The receiver must still reconstruct the intended information.

That means <u>networking is constrained by physical signal quality.</u>

The protocols we study later can provide mechanisms for detection, retransmission, error handling, congestion control, and recovery.

But those mechanisms operate on top of a physical system that has fundamental limitations.

### Key Takeaways

1. Bits are logical representations
2. Bits must be encoded into physical signals before they can travel through a network
3. Copper carries electrical signals
4. Fiber carries optical signals
5. Wireless uses electromagnetic radio signals
6. The physical layer connects logical networking to the real physical world
7. Twisted-pair Ethernet uses pairs of conductors  and differential signaling
8. The receiver must recover the intended signal from an imperfect physical environment
9. Distance, noise, interference, and the properties of the medium all affect signal quality

The central mental model for this part is:

```
Bits
 ↓
Physical-Layer Encoding
 ↓
Signal
 ↓
Medium
 ↓
Signal Degradation / Interference
 ↓
Receiver
 ↓
Recovered Bits
```

The next question is what happens to that signal while it travels.

A signal does not remain perfectly unchanged.

It weakens with distance, encounters unwanted energy, and can interfere with other signals.

That is where **attenuation, noise, crosstalk, and SNR** become critical.

## Lesson 2.2 — Signals, Noise, and Attenuation — Part 2

### Attenuation

A physical signal does not remain perfectly unchanged as it travels through a medium.

**Attenuation** is the reduction in signal strength as a signal travels through a transmission medium.

The basic relationship is:

```
Distance increases
        ↓
Signal strength decreases
```

In copper, electrical resistance and other transmission-line characteristics contribute to signal loss. 

In fiber, optical power is lost through physical mechanisms such as *absorption* and *scattering*.

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Absorption|Terminology]]

![[Cybersecurity journey/1. Networking/Terminology#𝑨 - Scattering|Terminology]]

In wireless communication, electromagnetic energy spreads through the environment and is affected by distance and propagation conditions.

The mechanisms differ, but the engineering problem is similar:

```
Longer transmission distance
        ↓
More signal loss
```

### The 100-Meter Ethernet Limit

A standard structured twisted-pair Ethernet channel is commonly specified to a maximum length of approximately:

```
100 meters
```

A typical structured cabling channel can be thought of as:

```
90 m permanent link
        +
10 m patch cords
        =
100 m channel
```

This does not mean the signal suddenly disappears at exactly 100 meters. It means the applicable Ethernet and cabling standards define performance requirements that must be met within the specified channel length.

As distance increases:

```
Distance
    ↓
Attenuation + Other Physical Impairments
    ↓
Reduced Signal Quality
    ↓
More Difficult Signal Detection
    ↓
Potential Transmission Errors
```

The 100-meter value is therefore an engineering performance limit, not a magical physical wall.

### Why Not Simply Increase Transmit Power?

If a signal becomes weaker with distance, it may seem logical to simply transmit a stronger signal.

That is not a universal solution.

Increasing transmit power does not eliminate:

```
Attenuation
Crosstalk
External Interference
Receiver Limitations
Physical Medium Constraints
```

<mark style="background:#ff4d4f">Increasing power can also create additional interference or violate the specifications of the physical-layer system.</mark>

The real objective is:

```
Make the desired signal sufficiently distinguishable
from unwanted energy at the receiver.
```

### Noise

**Noise** is <u>unwanted energy</u> or variation that interferes with the desired signal.

The basic model is:

```
Desired Signal
      +
Unwanted Noise
      ↓
Received Signal
```

The receiver wants to recover the intended signal, but noise makes that task more difficult.

Possible sources of noise include:

```
Electrical equipment
Power systems
Other communication systems
Electronic components
Thermal effects
Radio-frequency energy
Environmental electromagnetic energy
```

The exact sources depend on the transmission medium.

The important principle is:

```
The receiver does not operate in a perfectly silent environment.
```

### Interference

**Interference** refers to unwanted energy from other signals or external sources that affects communication.

A copper Ethernet cable may operate near electrical equipment or other cables. A wireless network may operate near:

```
Other Wi-Fi networks
Bluetooth devices
Other radio transmitters
Electronic equipment
```

The desired transmission must therefore be distinguished from other energy present in the physical environment.

![[Cybersecurity journey/1. Networking/Q&A#❔ - What is the difference between noise and interference ?|Q&A]]

### Signal-to-Noise Ratio

A central measurement is **[[Signal-to-Noise Ratio (SNR)]]**.

SNR describes the relationship between the desired signal and the noise.

Conceptually:

```
SNR = Signal Strength / Noise Strength
```

It is commonly expressed in decibels:

```
SNR (dB)
```

A higher SNR generally means the desired signal is much stronger relative to the noise.

A lower SNR means the noise is closer in strength to the desired signal.

Conceptually:

```
High SNR

Signal:
████████████████

Noise:
██
```

Compared with:

```
Low SNR

Signal:
████████

Noise:
██████
```

The first situation gives the receiver more separation between the desired signal and unwanted energy.

### Why Higher SNR Is Better

Think about speaking to another person.

In a quiet room:

```
Your voice
    ↓
Very little background noise
    ↓
Easy to understand
```

In a loud environment:

```
Your voice
    ↓
Strong background noise
    ↓
Harder to understand
```

The speaker has not necessarily changed. The relationship between the desired signal and background noise has changed.

Networking systems face the same fundamental problem.

A high SNR generally provides more margin for the receiver to correctly distinguish the intended transmission.

A low SNR makes signal detection more difficult.

```
Higher SNR
    ↓
Better separation between signal and noise
    ↓
Generally better signal quality
```

### Attenuation and SNR

Attenuation and SNR are closely related.

Imagine:

```
Signal = 100 units
Noise  = 1 unit
```

The desired signal is much stronger than the noise.

Now suppose the signal is attenuated:

```
Signal = 10 units
Noise  = 1 unit
```

The noise did not necessarily increase. The desired signal became weaker.

Therefore, the relationship between the signal and noise became worse.

Conceptually:

```
Distance increases
        ↓
Signal attenuates
        ↓
Signal-to-noise relationship can decrease
        ↓
Receiver has less margin
```

This is one reason distance matters even when the physical medium remains the same.

### Crosstalk

**Crosstalk** occurs when a signal traveling through one communication channel unintentionally couples into another channel.

This is particularly important in twisted-pair copper cabling.

A typical Ethernet cable contains multiple pairs:

```
Pair 1
Pair 2
Pair 3
Pair 4
```

Signals traveling through one pair generate electromagnetic fields. Because the pairs are physically close to each other, some of that energy can couple into neighboring pairs.

Conceptually:

```
Pair A's Signal ───────────────────────→
        )))))))))))))))
Pair B's Signal ───────────────────────→
```

The unwanted energy from Pair A has affected Pair B, and the other way around. That is crosstalk.

### Why Twisted Pairs Reduce Crosstalk

The conductors are not simply arranged as parallel wires. They are twisted together.

```
Conductor A
╲╱╲╱╲╱╲╱╲╱

Conductor B
╱╲╱╲╱╲╱╲╱╲
```

Twisting changes the physical relationship between the conductors and surrounding electromagnetic fields along the cable.

This helps reduce unwanted coupling and improves signal integrity.

The twisting is therefore an important part of the cable's engineering.

### Differential Signaling and *Noise Rejection*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Noise rejection|Definitions]]

Twisted-pair Ethernet commonly uses differential signaling.

The receiver is interested in the difference between the two conductors in a pair.

If external interference affects both conductors similarly, much of that common interference can be rejected.

```
Desired signal on pair
        +
Common external noise
        ↓
Receiver compares the conductors
        ↓
Much of the common noise can be rejected
```

The cable geometry and signaling method work together to improve resistance to interference.

### Cable Category and Crosstalk

As Ethernet technologies increase in speed, physical-layer requirements become more demanding.

<u>Higher data rates generally require tighter signal-integrity requirements.</u>

Cabling categories therefore specify electrical performance characteristics. For example:

```
Cat5e
Cat6
Cat6a
Cat8
```

These categories are not simply labels meaning:

```
Higher category = Faster Internet
```

They describe physical and electrical characteristics of the cabling.

Crosstalk performance is one important factor.

As signaling frequencies and performance requirements increase, controlling unwanted coupling becomes increasingly important.

### NEXT

NEXT means

```
Near-End Crosstalk
```

It describes unwanted coupling between pairs measured at the end of the cable near the transmitting source.

A related concept is FEXT which means

```
Far-End Crosstalk
```

At this stage, you do not need to memorize every testing procedure. You should understand:

```
NEXT
→ Crosstalk observed at the near end

FEXT
→ Crosstalk observed at the far end
```

**Signal Quality Is a System Problem**.

### What Happens When Signal Quality Gets Worse?

As the signal becomes weaker relative to noise and interference, the receiver has less margin for correctly distinguishing the intended signal.

Eventually, the physical-layer system may no longer be able to reliably recover the transmitted information.

Conceptually:

```
Good Signal Quality
        ↓
Reliable Reception
```

versus:

```
Poor Signal Quality
        ↓
More Difficult Reception
        ↓
Potential Errors
        ↓
Potential Retransmissions / Reduced Performance
```

The exact behavior depends on the physical-layer technology.

### Why Physical Layer Knowledge Matters

When a user says:

```
"The network is slow."
```

there are many possible explanations.

The problem could be:

```
Application behavior
Transport behavior
Routing
Congestion
Packet loss
Wireless interference
Cable problems
Signal attenuation
Physical-layer errors
```

Without understanding the physical layer, it is easy to overlook a physical cause.

A strong network engineer considers the entire communication path instead of immediately assuming that the problem is DNS, routing, or the Internet.

### Key Takeaways

1. Attenuation is signal loss over distance
2. Longer physical paths generally produce more attenuation
3. Ethernet twisted-pair channels are commonly limited to 100 meters under the relevant structured-cabling model
4. The 100-meter limit is a performance specification, not a point where the signal suddenly disappears
5. Noise is unwanted energy that interferes with the desired signal
6. Interference can originate from external sources or other signals
7. Crosstalk is unwanted coupling between communication channels
8. Twisting the conductors helps control electromagnetic coupling
9. Differential signaling helps reject common-mode interference
10. SNR describes the relationship between signal and noise
11. Higher SNR generally means better signal quality
12. As signal strength decreases relative to noise,signal recovery becomes more difficult

### Final Mental Model

Keep this model in mind:

```
Transmitter
    ↓
Desired Signal
    ↓
Physical Medium
    ↓
Attenuation
    +
Noise
    +
Interference
    +
Crosstalk
    ↓
Received Signal
    ↓
SNR / Signal Quality
    ↓
Receiver
    ↓
Recovered Data
```

The key question is no longer simply:

```
"Can the signal travel?"
```

It is:

```
"Can the receiver still distinguish the intended signal
from everything else after it has traveled through the medium?"
```

That question leads directly into the engineering problem of signal quality and, in the next part, into the physical limits that affect network latency and performance.

## Lesson 2.2 — Signals, Noise, and Attenuation — Part 3

### Latency as a Physical Problem

So far, we have looked at the quality of a signal.

Now we need to look at something different:

```
How long does it take for the signal to get there?
```

This is the problem of **latency**.

Latency is the time required for information to travel from one point to another.

In networking, latency is often discussed in terms of

```
Milliseconds (ms)
```

For example:

```
10 ms
30 ms
80 ms
150 ms
```

A smaller latency generally means that the communication takes less time to travel between the endpoints.

The important point is that latency is not purely a software problem.

A significant portion of latency comes from physics.

![[Cybersecurity journey/1. Networking/Q&A#❔ - Does low latency always mean less time from A to B?|Q&A]]

### The Speed of Light

Electromagnetic signals propagate extremely quickly.

In a vacuum (where there is no friction), the speed of light is approximately:

```
299,792,458 meters per second
```

This is usually approximated as:

```
3 × 10^8 m/s
```

That sounds effectively instantaneous.

It is not.

The distances involved in modern networks are l<u>arge enough that propagation time becomes measurable.</u>

A signal traveling across a continent or an ocean cannot arrive before the signal physically has time to traverse that distance.

This creates a fundamental networking constraint.

```
Longer physical distance
        ↓
Longer propagation time
```

### Propagation Delay

The time required for a signal to physically travel through a medium is called **propagation delay**.

A simplified equation is:

```
Propagation Delay = Distance / Propagation Speed
```

For example, if a signal must travel a distance of:

```
1,000,000 meters
```

and its propagation speed is approximately:

```
200,000,000 m/s
```

then:

```
Delay = 1,000,000 / 200,000,000
Delay = 0.005 seconds
Delay = 5 ms
```

This is only the propagation component.

Real networks contain additional sources of delay.

### Signals Do Not Travel at Exactly the Speed of Light in Vacuum

The common statement:

```
"Network signals travel at the speed of light."
```

is useful as an approximation, but it is not physically exact.

The propagation speed depends on the medium.

For example:

```
Vacuum
≈ 3 × 10^8 m/s
Fiber
< 3 × 10^8 m/s
Copper
< 3 × 10^8 m/s
```

The refractive properties of fiber and the electrical properties of transmission media affect propagation speed.

This means that the physical path matters.

The signal cannot simply move at the maximum possible speed regardless of what carries it.

![[Pasted image 20260913123646.png]]

### Fiber and Long-Distance Networking

Fiber optic cables are extremely important for long-distance networking.

They can carry signals across:

```
Buildings
Cities
Countries
Oceans
```

But fiber does not eliminate propagation delay.

A *transatlantic fiber* cable may span thousands of kilometers.

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Transatlantic fiber|Definitions]]

The signal therefore needs a finite amount of time to travel from one side of the ocean to the other.

This is true even if the network has:

```
Extremely high bandwidth
Modern routers
High-performance switches
Fast processors
```

None of these can make the physical distance disappear.

### Algeria → USA Example

Consider communication between Algeria and a destination in the United States.

The exact *Round-Trip Time (RTT)* depends on the endpoints and routing path, but an RTT around:

```
45 ms
```

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Round-Trip Time (RTT)|Definitions]]

is a useful conceptual example for understanding the physical constraint.

The important lesson is not that every Algeria → USA connection will always be exactly 45 ms.

The important lesson is:

```
The endpoints are geographically separated.
The signal must physically travel between them.
That distance creates unavoidable propagation delay.
```

Even a perfectly engineered network cannot make the signal arrive before it has physically had time to travel the required path.

### Why There Is No Perfect Solution

Suppose engineers build a better router.

It may process packets faster.

Suppose engineers improve the fiber.

It may carry much more data.

Suppose engineers improve the software.

Applications may become more efficient.

None of these changes can completely remove propagation delay caused by physical distance.

You cannot configure a router to violate the speed at which the signal propagates through the medium.

This gives us an important principle:

```
Some networking problems are caused by configuration.
Some networking problems are caused by technology.
Some networking problems are caused by physics.
```

Latency caused by physical distance belongs primarily to the third category.

### RTT — Round-Trip Time

When you use a tool such as [[ping]], you commonly observe a value called **round-trip time**, or:

```
RTT
```

RTT represents the time required for a packet to travel to the destination and for the corresponding response to return.

Conceptually:

```
Your Computer
     | Request
     ↓
  Network
     ↓
Destination
     | Response
     ↓
  Network
     ↓
Your Computer
```

The measurement covers the <u>round trip</u>.

Therefore:

```
RTT ≈ Outbound Travel + Return Travel
```

The exact RTT also includes other sources of delay.

### RTT Is Not Just Distance

It is important not to make the mistake:

```
RTT = Physical Distance / Speed
```

That is too simplistic.

A real network path can include:

```
Propagation delay
Transmission delay
Queuing delay
Processing delay
Serialization
Routing
Switching
Congestion
Network equipment
```

The actual observed RTT is therefore a combination of several components.

A useful conceptual model is:

```
RTT
≈
Propagation
+
Transmission
+
Processing
+
Queuing
+
Return-path delays
```

<mark style="background:#fff88f">The exact behavior depends on the network.</mark>

### Propagation Delay vs Transmission Delay

These two concepts are easy to confuse.

**Propagation delay** is about how long the signal takes to physically travel across the medium.

**Transmission delay** is about how long it takes to place the packet's bits onto the link.

They are different.

Consider a packet being transmitted onto a link.

```
Packet
████████████████████

Link
──────────────────────────────→

First bit
    ↓
starts traveling

Last bit
    ↓
finishes entering the link
```

The first bit can already be traveling through the medium while the remaining bits are still being transmitted.

This means:

```
Transmission delay
≠
Propagation delay
```

### Transmission Delay

Transmission delay can be approximated as:

```
Transmission Delay = Packet Size / Link Rate
```

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Link rate|Definitions]]

For example, suppose a packet is:

```
1,000 bytes
```

That is:

```
8,000 bits
```

If the link rate is:

```
100 Mbps
```

then:

```
Transmission Delay
= 8,000 / 100,000,000
= 0.00008 seconds
= 0.08 ms
```

This is the time required to push those bits onto the link.

It does not describe how long the first bit takes to physically travel to the destination.

### Propagation vs Transmission

Consider two separate questions:

```
Question 1:
How long does it take to put all the bits onto the link?
→ Transmission delay

Question 2:
How long does it take the signal to physically travel
from one end of the link to the other?
→ Propagation delay
```

This distinction is fundamental.

A link can have:

```
High bandwidth
+
High propagation delay
```

at the same time.

That brings us to bandwidth.

### Bandwidth

Bandwidth describes the capacity of a communication link.

For a network link, it is commonly expressed in:

```
bits per second
```

Examples include:

```
100 Mbps
1 Gbps
10 Gbps
100 Gbps
```

Bandwidth answers a question such as:

```
How much data can this link carry per unit of time?
```

A useful analogy is a pipe.

Imagine a water pipe.

A larger pipe can carry more water at once.

In networking:

```
Larger pipe
    ↓
Higher bandwidth
```

But the distance between the source and destination is a different property.

### Bandwidth vs Latency

This is one of the most important distinctions in networking.

**Bandwidth** is about capacity. **Latency** is about time.

They are not the same thing.

Consider two links:

```
Link A
Bandwidth: 1 Gbps
Latency:   100 ms

Link B
Bandwidth: 100 Mbps
Latency:   10 ms
```

Link A has ten times the bandwidth.

But Link B has much lower latency.

Therefore:

```
Higher bandwidth
does not automatically mean
lower latency.
```

And:

```
Lower latency
does not automatically mean
higher bandwidth.
```

They describe different properties of the network.

### Why High Bandwidth Does Not Fix Latency

Suppose an application is communicating across a very long-distance fiber connection.

Engineers increase the link capacity:

```
10 Gbps
        ↓
100 Gbps
```

The amount of data that can be carried per second has increased dramatically.

But the geographic distance has not changed.

The signal still has to travel the same physical route.

Therefore:

```
Bandwidth ↑
Latency caused by distance ≠ automatically ↓
```

### Why Latency Matters to Applications

Latency affects how quickly applications can exchange information.

For example:

```
Interactive SSH
Online gaming
Voice communication
Video conferencing
Web applications
Remote desktops
Database applications
```

can all be sensitive to latency.

The effect differs by application.

A large file transfer may tolerate high latency if it has enough bandwidth and uses an efficient transport.

An interactive application may feel sluggish even when the link has enormous bandwidth.

### A 10-Gbps Link Can Still Feel Slow

Imagine a network connection with:

```
Bandwidth: 10 Gbps
RTT:       150 ms
```

The link can carry an enormous amount of data.

But an interactive application may still experience noticeable delay when it waits for responses across the network.

This demonstrates why the phrase:

```
"The connection is fast."
```

is <u>technically</u> incomplete.

A network engineer should ask:

```
Fast in what sense?

Bandwidth?
Latency?
Throughput?
Packet loss?
Application response time?
```

### Bandwidth and Latency Work Together

Bandwidth and latency are separate properties, but they can interact.

Imagine downloading a large file.

With high bandwidth:

```
More data can be transmitted per second.
```

With high latency:

```
Feedback and acknowledgements take longer
to travel between endpoints.
```

Transport protocols such as TCP therefore have to account for both.

This is one reason long-distance high-bandwidth networks can require careful tuning.

### Bandwidth-Delay Product

A useful concept for understanding the interaction between bandwidth and latency is the *Bandwidth-Delay Product (BDP).*

![[Cybersecurity journey/1. Networking/Definitions#🧠 - Bandwidth-Delay Product (BDP)|Definitions]]

A simplified form is:

```
BDP = Bandwidth × RTT
```

For example:

```
Bandwidth = 100 Mbps
RTT       = 100 ms
```

Convert the values:

```
100 Mbps = 100,000,000 bits/s

100 ms = 0.1 s
```

Then:

```
BDP
= 100,000,000 × 0.1
= 10,000,000 bits
```

Convert to bytes:

```
10,000,000 / 8
= 1,250,000 bytes
```

Approximately:

```
1.25 MB
```

This means that at 100 Mbps with a 100 ms RTT, roughly 1.25 MB of data can correspond to one round-trip's worth of link capacity.

![[Pasted image 20260914201519.png]]

### Why BDP Matters

BDP becomes especially important on:

```
Long-distance links
High-bandwidth links
High-latency links
```

A link can have enormous capacity, but <mark style="background:#fff88f">protocols need enough data in flight to keep that capacity utilized.</mark>

Conceptually:

```
High Bandwidth
      +
High RTT
      ↓
Large Bandwidth-Delay Product
      ↓
More data may need to be in flight
      ↓
Protocol behavior becomes important
```

This is one reason performance engineering cannot be reduced to looking at a single bandwidth number.

### The Physical Limit

Return to the fundamental problem:

```
Two endpoints are separated by physical distance.
```

No configuration can make the distance zero.

No routing protocol can make electromagnetic propagation instantaneous.

No amount of bandwidth can completely eliminate propagation delay.

Engineers can improve:

```
Routing
Processing
Queuing
Equipment
Protocol behavior
```

They can also choose better physical paths.

But the fundamental propagation component remains.

```
Distance
    ↓
Propagation time
    ↓
Physical lower bound on latency
```

### Geography vs Network Path

Consider:

```
Host A
   ↓
Router
   ↓
Provider Network
   ↓
International Link
   ↓
Provider Network
   ↓
Host B
```

The actual path may not be a straight line between the two endpoints.

It may contain:

```
Multiple routers
Multiple links
Different carriers
Undersea cables
Peering locations
Exchange points
```

Therefore:

```
Geographic distance
        ≠
Exact network-path distance
```

But geography still imposes a physical lower bound.

### A Useful Engineering Mental Model

When examining latency, think in layers of cause:

```
Physical Distance
        ↓
Propagation Delay
+
Packet Size / Link Rate
        ↓
Transmission Delay
+
Routers / Switches
        ↓
Processing Delay
+
Busy Links
        ↓
Queuing Delay
+
Actual Network Route
        ↓
Path-Dependent Delay
=
Observed RTT
```

This model helps prevent simplistic conclusions.

### Key Takeaways

1. Latency is the time required for communication to travel
2. Propagation delay is caused by the physical travel of a signal through a medium
3. Signals cannot propagate instantaneously
4. Greater physical distance generally creates greater propagation delay
5. An Algeria → USA RTT around 45 ms is a useful example of physical distance creating measurable latency
6. The exact RTT depends on the endpoints, route, and other sources of delay
7. RTT measures a round trip, not just one-way travel
8. Transmission delay is the time required to place the packet's bits onto the link
9. Propagation delay is the time required for the signal to travel through the medium
10. Bandwidth describes link capacity
11. Throughput describes the actual achieved data rate
12. Bandwidth and latency are separate properties
13. High bandwidth does not automatically mean low latency
14. The Bandwidth-Delay Product describes how much data can be in flight during a given RTT
### Final Mental Model

Keep these two dimensions separate:

```
CAPACITY
Bandwidth
    ↓
How much data can be carried per second

TIME
Latency
    ↓
How long communication takes to travel
```

Then add the physical constraint:

```
Distance
    ↓
Propagation Delay
    ↓
Latency
```

And remember:

```
High Bandwidth
        +
High Latency
```

is completely possible.

A network can be capable of moving enormous amounts of data while still taking a noticeable amount of time for information to travel between distant endpoints.

That distinction is essential for understanding real-world network performance.

In the next part, we will turn these concepts into practice using `ping`, RTT measurements, geographic distance, and network paths.

### Lesson Checkpoint

Before moving on, you should be able to explain this statement in your own words:

```
A network can carry enormous amounts of data per second
and still have noticeable latency because bandwidth and
propagation delay are different physical properties.
```

You should also be able to explain:

```
Why does a signal weaken?
Why does noise matter?
Why does twisting copper pairs help?
What does SNR tell us?
Why does distance create latency?
Why does ping report RTT instead of pure propagation delay?
Why can two geographically similar destinations have different RTTs?
Why can increasing bandwidth fail to reduce latency?
```

If you can answer those questions without memorizing the wording, you have the physical foundation needed for the next networking topics.

Now you're ready for [[Lesson 2.2 - Lab]].