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