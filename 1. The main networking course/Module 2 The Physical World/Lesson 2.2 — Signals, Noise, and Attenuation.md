## Lesson 2.2 — Signals, Noise, and Attenuation — Part 1

### From Bits to Physical Signals

In networking, we often talk about data as bits:

```
1011010010110100
```

But a physical network cannot literally send an abstract `1` or `0 through a cable or through the air.

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

The physical signal depends on the medium being used.

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

### Differential Signaling

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

It is part of the electrical engineering of the cable.

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
   |
Cable
   |
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

Those are not necessarily identical.

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

That means networking is constrained by physical signal quality.

The protocols we study later can provide mechanisms for detection, retransmission, error handling, congestion control, and recovery.

But those mechanisms operate on top of a physical system that has fundamental limitations.

### Key Takeaways

```
Bits are logical representations.

Bits must be encoded into physical signals
before they can travel through a network.

Copper carries electrical signals.

Fiber carries optical signals.

Wireless uses electromagnetic radio signals.

The physical layer connects logical networking
to the real physical world.

Twisted-pair Ethernet uses pairs of conductors
and differential signaling.

The receiver must recover the intended signal
from an imperfect physical environment.

Distance, noise, interference, and the properties
of the medium all affect signal quality.
```

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

In copper, electrical resistance and other transmission-line characteristics contribute to signal loss. In fiber, optical power is lost through physical mechanisms such as absorption and scattering. In wireless communication, electromagnetic energy spreads through the environment and is affected by distance and propagation conditions.

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

Increasing power can also create additional interference or violate the specifications of the physical-layer system.

The real objective is:

```
Make the desired signal sufficiently distinguishable
from unwanted energy at the receiver.
```

### Noise

**Noise** is unwanted energy or variation that interferes with the desired signal.

The basic model is:

```
Desired Signal
      +
Unwanted Noise
      ↓
Received Signal
```

The receiver wants to recover the intended signal, but noise makes that task more difficult.

Possible sources include:

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

### Signal-to-Noise Ratio

A central measurement is **Signal-to-Noise Ratio**, commonly abbreviated:

```
SNR
```

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
Pair A

Signal ───────────────────────→


        )))))))))))))))


Pair B

Signal ───────────────────────→
```

The unwanted energy from Pair A has affected Pair B.

That is crosstalk.

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

### Differential Signaling and Noise Rejection

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

### Crosstalk Between Pairs

The four pairs in an Ethernet cable are physically close to each other.

This creates the possibility of interaction between them.

The cable is engineered to control that interaction. Different pairs can use different twist rates, changing the physical relationship between pairs and helping reduce predictable coupling.

The result is improved transmission performance.

A useful mental model is:

```
Multiple Electrical Signals
        ↓
Same Physical Cable
        ↓
Potential Electromagnetic Coupling
        ↓
Cable Design + Twisting + Differential Signaling
        ↓
Reduced Interference
```

### Cable Category and Crosstalk

As Ethernet technologies increase in speed, physical-layer requirements become more demanding.

Higher data rates generally require tighter signal-integrity requirements.

Cabling categories therefore specify electrical performance characteristics.

For example:

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

One important measurement is:

```
NEXT
```

NEXT means:

```
Near-End Crosstalk
```

It describes unwanted coupling between pairs measured at the end of the cable near the transmitting source.

A related concept is:

```
FEXT
```

which means:

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

### Signal Quality Is a System Problem

A receiver does not simply receive the original signal unchanged.

A more realistic model is:

```
Transmitted Signal
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
Receiver
        ↓
Recovered Information
```

The receiver has to determine what was actually transmitted despite these physical effects.

This is the fundamental challenge of physical-layer communication.

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

### Practical Example — A Long Copper Cable

Imagine an Ethernet link using a copper cable that approaches the maximum supported channel length.

The transmitter sends a signal.

As the signal travels:

```
Signal
 ↓
Attenuation
 ↓
Weaker Signal
```

At the same time, the cable may experience:

```
External Noise
+
Crosstalk
+
Other Electrical Effects
```

The receiver therefore sees a signal that is both weaker and potentially contaminated.

The receiver must still determine what was transmitted.

This is why Ethernet cabling specifications define strict physical requirements.

### Practical Example — Damaged Cable

Imagine a cable has been physically damaged.

The cable might still maintain some level of electrical connectivity.

However, the physical characteristics of the cable may have changed.

For example:

```
Damaged Geometry
        ↓
Changed Electrical Characteristics
        ↓
Potentially Increased Signal Loss
        ↓
Potentially Increased Crosstalk
        ↓
Reduced Signal Quality
```

A simple continuity test might not reveal every possible physical-performance problem.

This is why professional cable certification involves much more than checking whether every conductor has continuity.

### Practical Example — Poor Cable Installation

Imagine several Ethernet cables are installed incorrectly.

They may be:

```
Overly bent
Compressed
Poorly terminated
Installed near sources of interference
Incorrectly paired
Excessively long
```

Even if the network initially appears to work, the physical layer may have reduced performance margins.

A network engineer therefore needs to understand:

```
"Link works"

does not necessarily mean:

"The physical installation is optimal."
```

Physical-layer quality can affect reliability and performance even when a connection remains operational.

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

```
Attenuation is signal loss over distance.

Longer physical paths generally produce more attenuation.

Ethernet twisted-pair channels are commonly limited to
100 meters under the relevant structured-cabling model.

The 100-meter limit is a performance specification,
not a point where the signal suddenly disappears.

Noise is unwanted energy that interferes with the desired signal.

Interference can originate from external sources or other signals.

Crosstalk is unwanted coupling between communication channels.

Twisting the conductors helps control electromagnetic coupling.

Differential signaling helps reject common-mode interference.

SNR describes the relationship between signal and noise.

Higher SNR generally means better signal quality.

As signal strength decreases relative to noise,
signal recovery becomes more difficult.
```

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

In networking, latency is often discussed in terms of:

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

### The Speed of Light

Electromagnetic signals propagate extremely quickly.

In a vacuum, the speed of light is approximately:

```
299,792,458 meters per second
```

This is usually approximated as:

```
3 × 10^8 m/s
```

That sounds effectively instantaneous.

It is not.

The distances involved in modern networks are large enough that propagation time becomes measurable.

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

A transatlantic fiber cable may span thousands of kilometers.

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

The exact RTT depends on the endpoints and routing path, but an RTT around:

```
45 ms
```

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

When you use a tool such as:

```
ping
```

you commonly observe a value called **round-trip time**, or:

```
RTT
```

RTT represents the time required for a packet to travel to the destination and for the corresponding response to return.

Conceptually:

```
Your Computer
     |
     | Request
     ↓
  Network
     |
     ↓
Destination
     |
     | Response
     ↓
  Network
     |
     ↓
Your Computer
```

The measurement covers the round trip.

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

The exact behavior depends on the network.

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

**Bandwidth** is about capacity.

**Latency** is about time.

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

### The Pipe Analogy

Imagine two pipes.

Pipe A:

```
Very wide
Very long
```

Pipe B:

```
Narrow
Very short
```

Pipe A can carry a large amount of water.

But water may take a long time to travel through the entire pipe.

Pipe B carries less water at a time, but the water may reach the other end quickly.

Networking behaves similarly.

```
Bandwidth
=
How much can move through the link

Latency
=
How long it takes to get there
```

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

This is one of the most important lessons in network performance engineering.

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

is technically incomplete.

A network engineer should ask:

```
Fast in what sense?

Bandwidth?
Latency?
Throughput?
Packet loss?
Application response time?
```

### Bandwidth and Throughput Are Not the Same

Bandwidth is the theoretical or configured capacity of a link.

Throughput is the actual rate of successfully delivered data.

For example:

```
Link capacity:
1 Gbps

Actual throughput:
700 Mbps
```

The link has 1 Gbps of bandwidth, but the application may only achieve 700 Mbps of useful throughput.

Factors that can reduce throughput include:

```
Congestion
Packet loss
Protocol overhead
Receiver limitations
Sender limitations
TCP behavior
Latency
Wireless interference
Physical-layer problems
```

Therefore:

```
Bandwidth = capacity

Throughput = achieved data rate
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

A useful concept for understanding the interaction between bandwidth and latency is the:

```
Bandwidth-Delay Product
```

or:

```
BDP
```

A simplified form is:

```
BDP = Bandwidth × RTT
```

It represents approximately how much data can be "in flight" during one round-trip time.

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

### Why BDP Matters

BDP becomes especially important on:

```
Long-distance links
High-bandwidth links
High-latency links
```

A link can have enormous capacity, but protocols need enough data in flight to keep that capacity utilized.

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

### Why Geography Matters

If you run:

```
ping 192.0.2.1
```

and receive:

```
time=2 ms
```

that destination is likely relatively close in network terms.

If another destination produces:

```
time=120 ms
```

there may be substantial geographic or routing distance involved.

However, geography alone does not determine RTT.

The route matters too.

Two destinations at similar geographic distances can have very different RTTs because their traffic may follow different paths.

### Geography vs Network Path

Consider:

```
Host A
   |
   ↓
Router
   |
   ↓
Provider Network
   |
   ↓
International Link
   |
   ↓
Provider Network
   |
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

```
Latency is the time required for communication to travel.

Propagation delay is caused by the physical travel
of a signal through a medium.

Signals cannot propagate instantaneously.

Greater physical distance generally creates greater
propagation delay.

An Algeria → USA RTT around 45 ms is a useful example
of physical distance creating measurable latency.

The exact RTT depends on the endpoints, route,
and other sources of delay.

RTT measures a round trip, not just one-way travel.

Transmission delay is the time required to place
the packet's bits onto the link.

Propagation delay is the time required for the signal
to travel through the medium.

Bandwidth describes link capacity.

Throughput describes the actual achieved data rate.

Bandwidth and latency are separate properties.

High bandwidth does not automatically mean low latency.

The Bandwidth-Delay Product describes how much data
can be in flight during a given RTT.
```

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

## Lesson 2.2 — Signals, Noise, and Attenuation — Part 4

### Lab — Measuring Latency With Ping

The final part of this lesson turns the physical concepts into an observation you can perform on a real network.

The objective is not simply to run `ping`.

The objective is to connect an observed RTT value to the physical and network properties that produced it.

You should be able to look at a result such as:

```
64 bytes from 203.0.113.10:
time=12.4 ms
```

and understand that the number represents an actual measurement of communication time across a real network path.

The central question for this lab is:

```
How does distance affect RTT?
```

You will compare destinations at different geographic distances and look for a relationship between:

```
Geography
Network path
Propagation
RTT
```

### Learning Objectives

By the end of this lab, you should be able to:

```
Use ping to measure RTT.

Explain what an RTT measurement represents.

Compare RTT values between destinations.

Relate larger physical distances to propagation delay.

Explain why RTT is not determined by geography alone.

Distinguish propagation delay from other sources of latency.

Recognize that a measured RTT is an observation of
an entire network path, not a pure measurement of distance.
```

### What Ping Measures

The `ping` utility sends an ICMP Echo Request toward a destination.

A reachable destination can respond with an ICMP Echo Reply.

Conceptually:

```
Your Computer
      |
      | ICMP Echo Request
      ↓
   Network
      |
      ↓
 Destination
      |
      | ICMP Echo Reply
      ↓
   Network
      |
      ↓
Your Computer
```

The elapsed time between sending the request and receiving the reply is measured.

This gives us:

```
RTT = Round-Trip Time
```

A simplified representation is:

```
Request
   ↓
Destination
   ↓
Reply
   ↓
Local Host
```

The measured value includes the time associated with the complete round trip.

### What RTT Does Not Tell You

A ping result does not directly tell you:

```
The exact physical distance
The exact cable length
The exact propagation delay
The exact route
The exact amount of queuing
```

It gives you an observed RTT.

That RTT is produced by several factors.

A useful model is:

```
Observed RTT
    =
Propagation
+
Transmission
+
Processing
+
Queuing
+
Return-path effects
```

Therefore, treat ping as a measurement of network behavior, not as a geographic-distance meter.

### Preparing the Lab

You need a system with a command-line interface.

On Linux or macOS:

```
ping <destination>
```

On Windows:

```
ping <destination>
```

The exact output format varies by operating system.

For example, you may see something similar to:

```
64 bytes from 203.0.113.10: icmp_seq=1 ttl=54 time=18.7 ms
64 bytes from 203.0.113.10: icmp_seq=2 ttl=54 time=19.2 ms
64 bytes from 203.0.113.10: icmp_seq=3 ttl=54 time=18.9 ms
```

The important field for this lab is:

```
time=18.7 ms
```

That is the measured RTT for that individual exchange.

### Important Note About Destinations

Do not assume that a hostname or IP address tells you the exact physical location of the responding system.

A destination may be:

```
Anycasted
Behind a CDN
Hosted in a different location
Using a cloud provider
Reached through an unexpected route
```

For this lab, the goal is to understand the relationship between distance and RTT, not to claim that every measured value maps directly to a specific geographic location.

Use destinations whose approximate geographic locations are known when possible.

### Establishing a Baseline

Start by pinging a destination that is relatively close to you.

Run:

```
ping <nearby-destination>
```

Allow enough requests to obtain several measurements.

Record:

```
Destination
Approximate location
Minimum RTT
Average RTT
Maximum RTT
Packet loss
```

A simple table can be used:

|Destination|Approx. Location|Min RTT|Avg RTT|Max RTT|Loss|
|---|---|---|---|---|---|
|A|Nearby|||||

The exact fields available depend on your operating system.

### Test a More Distant Destination

Now choose a destination that is significantly farther away geographically.

Run:

```
ping <distant-destination>
```

Again, record several measurements.

Add the result to your table:

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Destination|Approx. Location|Min RTT|Avg RTT|Max RTT|Loss|
|A|Nearby|||||
|B|Distant|||||

Now compare the results.

Ask:

```
Which destination has the lower RTT?

Which destination has the higher RTT?

Is the farther destination also higher in RTT?

How consistent are the measurements?
```

### Add an International Destination

Now test a destination in another country or on another continent.

For example, if your testing environment permits it:

```
ping <international-destination>
```

Record the result.

Your table might now look like:

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Destination|Approx. Location|Min RTT|Avg RTT|Max RTT|Loss|
|A|Nearby|||||
|B|Same country / region|||||
|C|International|||||

Do not focus on the exact numbers.

Focus on the relationship between them.

### Expected Pattern

A common observation is:

```
Nearby destination
        ↓
Lower RTT

Farther destination
        ↓
Higher RTT
```

This makes intuitive sense.

The signal has to travel through a physical network path.

Greater distance generally means greater propagation time.

But the relationship will not be perfectly proportional.

You may see something like:

```
Destination A
Distance: relatively short
RTT:      8 ms

Destination B
Distance: much longer
RTT:      25 ms

Destination C
Distance: extremely long
RTT:      90 ms
```

The exact values are not important.

The physical principle is:

```
More distance
        ↓
More propagation time
        ↓
Potentially higher RTT
```

### Why RTT Is Not Exactly Proportional to Distance

Suppose Destination A is geographically closer than Destination B.

You might expect:

```
A → lower RTT
B → higher RTT
```

Often that is true.

But consider two different routes:

```
Destination A

Your Computer
     ↓
Router
     ↓
Provider
     ↓
Destination
```

and:

```
Destination B

Your Computer
     ↓
Router
     ↓
Provider
     ↓
International Carrier
     ↓
Another Provider
     ↓
Multiple Routers
     ↓
Destination
```

The second path may have much more network equipment and a longer physical route.

Therefore:

```
Geographic distance
        ≠
Exact network-path distance
```

A packet rarely travels in a perfectly straight geographic line.

### The Route Matters

Imagine two destinations that are geographically similar distances from you.

They can still have different RTTs.

Why?

Because their network paths can differ.

One route may be:

```
Short physical path
Few routers
Low congestion
```

while another may be:

```
Longer physical path
More routers
More queuing
More congestion
```

The measured RTT can therefore differ substantially.

This is why a network engineer does not look at one ping result and immediately conclude:

```
"This destination is exactly X kilometers away."
```

That conclusion is not justified by the measurement alone.

### Propagation Delay in the Lab

The physical component of the RTT can be represented conceptually as:

```
Your Computer
      |
      | Propagation
      ↓
Destination
      |
      | Propagation
      ↓
Your Computer
```

Because the request and response travel in opposite directions, the round-trip measurement includes propagation in both directions.

A simplified model is:

```
RTT propagation component
≈
Outbound propagation
+
Return propagation
```

The actual RTT contains additional components.

### Processing Delay

Routers and hosts need to process packets.

Conceptually:

```
Packet arrives
      ↓
Device examines packet
      ↓
Device processes packet
      ↓
Device forwards / responds
```

This takes time.

Modern networking equipment can process traffic extremely quickly, but the delay is not necessarily zero.

Across a multi-hop path, many devices can contribute some amount of processing delay.

### Queuing Delay

Another important source is queuing.

Imagine a router's outgoing interface is busy.

Packets may have to wait:

```
Packet
   ↓
Router Queue
   ↓
Waiting
   ↓
Transmission
```

This creates additional delay.

Unlike propagation delay, queuing delay can change significantly over time.

For example:

```
Low traffic
    ↓
Small queue
    ↓
Low queuing delay
```

versus:

```
Heavy traffic
    ↓
Large queue
    ↓
Higher queuing delay
```

This is one reason repeated ping measurements may not all be identical.

### Why Ping Results Vary

You may see:

```
18 ms
19 ms
18 ms
20 ms
18 ms
```

rather than exactly:

```
18 ms
18 ms
18 ms
18 ms
18 ms
```

That variation is normal.

The network is dynamic.

Small changes can occur because of:

```
Queuing
Scheduling
Processing
Traffic conditions
Wireless conditions
Routing behavior
Measurement granularity
```

The physical propagation time does not suddenly change dramatically every second, but other components of RTT can.

### Packet Loss

Ping can also report packet loss.

For example:

```
10 packets transmitted
10 packets received
0% packet loss
```

or:

```
10 packets transmitted
9 packets received
10% packet loss
```

Packet loss is not the same thing as latency.

A destination can have:

```
Low RTT + Packet Loss
```

or:

```
High RTT + No Packet Loss
```

These are different network conditions.

Do not interpret:

```
High RTT = Packet Loss
```

or:

```
Packet Loss = High RTT
```

as universal rules.

They are separate measurements, although network congestion and physical problems can sometimes influence both.

### Comparing the Results

After testing several destinations, answer these questions:

```
1. Which destination had the lowest average RTT?

2. Which had the highest average RTT?

3. Which destination was geographically closest?

4. Which destination was geographically farthest?

5. Did geographic distance generally correlate with RTT?

6. Were there any exceptions?

7. What could explain the exceptions?

8. Did RTT vary between individual packets?

9. Was any packet loss observed?

10. What part of RTT is fundamentally caused by physical distance?
```

The important answer to the final question is:

```
Propagation delay.
```

### Exercise — Build a Latency Table

Create a table containing at least three destinations.

Use:

```
Destination
Approximate geographic location
Minimum RTT
Average RTT
Maximum RTT
Packet loss
```

Example:

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|Destination|Location|Min|Avg|Max|Loss|
|A|Nearby|||||
|B|Regional|||||
|C|International|||||

Then write a short observation:

```
The nearby destination had ______ RTT.

The farther destination had ______ RTT.

The international destination had ______ RTT.

Overall, the results suggest that ______.
```

### Exercise — Explain the Difference

Answer the following without looking at the previous sections.

#### Question 1

What is propagation delay?

#### Question 2

Why does physical distance affect latency?

#### Question 3

Why can a 10-Gbps link still have high latency?

#### Question 4

Why is RTT not exactly equal to propagation delay?

#### Question 5

Why can two destinations at similar geographic distances have different RTTs?

#### Question 6

What is the difference between bandwidth and latency?

#### Question 7

What does ping measure?

#### Question 8

Why can two consecutive ping measurements have different RTT values?

### Exercise — Predict Before Measuring

Before running ping, make predictions.

Choose three destinations:

```
Destination A → Nearby
Destination B → Regional
Destination C → International
```

Predict:

```
A RTT: ______
B RTT: ______
C RTT: ______
```

Then run the tests.

Compare:

```
Prediction
    vs.
Measurement
```

If your prediction is wrong, do not simply discard it.

Ask why.

This is how networking becomes engineering rather than memorization.

### Exercise — Bandwidth vs Latency

Consider these two links:

```
Link A
Bandwidth: 100 Mbps
RTT:       10 ms

Link B
Bandwidth: 10 Gbps
RTT:       100 ms
```

Answer:

```
Which link has greater bandwidth?

Which link has lower latency?

Which link has greater capacity?

Which link might feel more responsive for an interactive application?

Which link can potentially transfer more data per second?
```

The important lesson is:

```
Bandwidth and latency answer different questions.
```

### Exercise — Explain the Algeria → USA Example

Use the earlier conceptual example:

```
Algeria → USA
RTT ≈ 45 ms
```

Explain why an RTT of this general magnitude can exist even when:

```
The network equipment is extremely fast.
The fiber carries very high bandwidth.
The computers are powerful.
```

Your explanation should include:

```
Geographic distance
Physical propagation
Network path
Round-trip measurement
```

Do not state that 45 ms is a universal value.

The exact RTT depends on the actual endpoints and path.

### Lab Discussion

After completing the measurements, discuss the following:

```
Why did the nearby destination generally have lower RTT?

Why did the farther destination generally have higher RTT?

Why were the results not perfectly proportional to distance?

What role does the network route play?

What role can queuing play?

Which component of latency is imposed by physics?

Can bandwidth eliminate propagation delay?

Can a router configuration make the speed of light faster?
```

The final question has a deliberately simple answer:

```
No.
```

A network engineer can optimize the path, but cannot configure physics.

### Connecting the Lab to the Physical Layer

The entire lesson can now be connected together.

A physical signal travels through a medium.

The medium has physical properties.

The signal can weaken.

The signal can encounter noise and interference.

The signal also requires finite time to travel.

Therefore:

```
Physical Medium
      ↓
Signal Characteristics
      ↓
Attenuation / Noise / Interference
      ↓
Signal Quality
```

and:

```
Physical Distance
      ↓
Propagation Delay
      ↓
Latency
```

These are separate but related physical constraints.

### Two Different Questions

When troubleshooting a network, ask two different questions.

First:

```
Can the receiver reliably understand the signal?
```

This relates to:

```
Attenuation
Noise
Interference
Crosstalk
SNR
Signal integrity
```

Second:

```
How long does communication take?
```

This relates to:

```
Propagation delay
Transmission delay
Processing delay
Queuing delay
Network path
RTT
```

A network can have excellent signal quality but high latency.

A network can also have low latency but poor signal quality.

These are not the same problem.

### Complete Lesson Mental Model

The entire lesson can now be represented as:

```
DATA
  ↓
Physical-Layer Encoding
  ↓
SIGNAL
  ↓
Physical Medium
  ↓
┌──────────────────────────────┐
│ Physical Effects             │
│                              │
│ Attenuation                  │
│ Noise                        │
│ Interference                 │
│ Crosstalk                    │
│ Propagation Delay            │
└──────────────────────────────┘
  ↓
RECEIVED SIGNAL
  ↓
Signal Quality / SNR
  ↓
Receiver
  ↓
Recovered Data
```

There are two major physical questions:

```
QUALITY

Can the receiver distinguish
the intended signal?

        ↓

SNR
Attenuation
Noise
Interference
Crosstalk
```

and:

```
TIME

How long does communication take?

        ↓

Propagation
Transmission
Processing
Queuing
Network Path
```

### Final Takeaways

```
Signals weaken as they travel through physical media.

Attenuation reduces signal strength.

Noise and interference make signal recovery more difficult.

Crosstalk is unwanted coupling between communication channels.

SNR describes the relationship between desired signal
and noise.

Higher SNR generally gives the receiver more signal margin.

Physical distance creates propagation delay.

Propagation delay cannot be completely eliminated by
faster processors, higher bandwidth, or better software.

RTT measures a round trip between endpoints.

RTT includes more than propagation delay.

Network paths are not necessarily geographically straight.

Geographic distance generally influences RTT,
but it does not uniquely determine RTT.

Bandwidth describes capacity.

Throughput describes achieved data rate.

Latency describes time.

High bandwidth and high latency can exist simultaneously.
```

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

Why can two geographically similar destinations have
different RTTs?

Why can increasing bandwidth fail to reduce latency?
```

If you can answer those questions without memorizing the wording, you have the physical foundation needed for the next networking topics.