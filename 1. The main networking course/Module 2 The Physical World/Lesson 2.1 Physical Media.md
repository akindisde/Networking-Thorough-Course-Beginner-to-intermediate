## Module 2 — The Physical World

## Lesson 2.1 — Physical Media (Part 1)

### Introduction

In Module 1, we studied how data moves through networks using protocols, packets, frames, and the layered networking model.

We learned that an application can generate data, the Transport layer can encapsulate that data into segments, the Network layer can place those segments into packets, and the Link layer can place those packets into frames.

But eventually, all of that information must become something physical.

A computer cannot transmit an abstract TCP segment through the air or down a cable.

At the Physical layer, the information represented by bits must be converted into physical signals that can travel through a transmission medium.

Those signals may be:

- Electrical signals traveling through copper.
- Pulses of light traveling through optical fiber.
- Radio waves traveling through free space.

The Physical layer is therefore where the logical world of networking meets the physical world.

This is where networking becomes constrained by physics.

Signals weaken with distance.

Electrical interference can corrupt signals.

Light behaves differently depending on the fiber design.

Radio transmissions spread through space and compete with other transmissions.

The physical medium therefore directly influences:

- Maximum transmission distance.
- Available bandwidth.
- Signal quality.
- Latency.
- Error rates.
- Installation requirements.
- Cost.
- Security.
- Network architecture.

A network engineer who understands only IP addresses and routing tables but does not understand physical media will eventually encounter problems they cannot properly diagnose.

The purpose of this lesson is to understand what physically carries the bits.

### Learning Objectives

After completing this lesson, you should be able to:

- Explain the role of the Physical layer.
- Describe how bits are represented as physical signals.
- Explain how twisted-pair copper carries Ethernet signals.
- Describe the structure of an Ethernet twisted-pair cable.
- Explain why Ethernet copper cables contain four twisted pairs.
- Distinguish between Cat5e, Cat6, Cat6a, and Cat8.
- Explain the significance of the 100-meter Ethernet channel limit.
- Understand why copper transmission distance is limited.
- Identify the basic components inside an Ethernet cable.
- Understand the relationship between cable specifications and Ethernet speeds.

### The Physical Layer

The Physical layer is the lowest layer of the networking stack.

Its purpose is to transmit raw information across a physical medium.

At this layer, the network is concerned with questions such as:

- What physical signal represents a binary 1?
- What physical signal represents a binary 0?
- How quickly can signals change?
- How far can a signal travel?
- What frequency range is used?
- How much interference can the signal tolerate?
- What type of connector is required?
- What type of cable or radio system is used?

The Physical layer does not understand:

- IP addresses.
- TCP ports.
- HTTP requests.
- DNS names.
- MAC addresses.

Those concepts belong to higher layers.

The Physical layer deals with the actual transmission mechanism.

A simplified view is:

```text
Application Data

↓

Bits

↓

Physical Encoding

↓

Physical Signal

↓

Transmission Medium

↓

Physical Signal

↓

Recovered Bits
````

For copper Ethernet, the physical signal is electrical.

For fiber Ethernet, the physical signal is optical.

For Wi-Fi, the physical signal is electromagnetic radiation in the radio-frequency spectrum.

### Bits Are Not Literally "Electricity"

A common beginner misconception is that a network cable carries literal 1s and 0s as electrical objects.

It does not.

A bit is a logical abstraction.

The physical layer uses a defined signaling scheme to represent information using measurable physical properties.

For example, depending on the technology, information may be represented using changes in:

- Voltage.
    
- Current.
    
- Signal timing.
    
- Phase.
    
- Frequency.
    
- Amplitude.
    
- Light intensity.
    

The receiver observes the incoming signal and interprets it according to the rules defined by the physical-layer technology.

The important idea is:

```text
Logical Data

↓

Encoded Into Signals

↓

Transmitted Physically

↓

Detected By Receiver

↓

Decoded Back Into Data
```

The physical layer therefore acts as the bridge between digital information and physical phenomena.

### Copper Ethernet

The most common wired medium for Local Area Networks is **twisted-pair copper**.

It is used extensively for:

- Desktop computers.
    
- Laptops with wired connections.
    
- IP phones.
    
- Network printers.
    
- Security cameras.
    
- Wireless access points.
    
- Ethernet switches.
    
- Servers.
    

A typical Ethernet cable looks simple from the outside.

```text
+--------------------------------------+
| Protective Outer Jacket              |
|                                      |
|   Twisted Pair 1                     |
|   Twisted Pair 2                     |
|   Twisted Pair 3                     |
|   Twisted Pair 4                     |
|                                      |
+--------------------------------------+
```

Inside the outer jacket are eight individual copper conductors.

These eight conductors are arranged into four pairs.

```text
8 Conductors

↓

4 Pairs

↓

2 Conductors per Pair
```

Each pair consists of two insulated copper wires twisted around each other.

### Why Are the Wires Twisted?

The twisting is not decorative.

It is an important engineering technique used to reduce electromagnetic interference.

Every electrical signal traveling through a conductor generates an electromagnetic field.

Nearby electrical signals can interfere with one another.

This is known as **crosstalk**.

Twisting the conductors helps reduce the effect of external interference and limits interference between pairs.

The two conductors in a pair are exposed to similar electromagnetic conditions.

Because the signaling system compares the relationship between the conductors, much of the unwanted interference can be rejected.

This technique is closely related to **differential signaling**.

### Differential Signaling

Modern Ethernet over twisted-pair copper uses differential signaling.

Instead of interpreting a signal based solely on the voltage of one wire relative to ground, the receiver examines the voltage difference between the two conductors in a pair.

Conceptually:

```text
Wire A:  +V

Wire B:  -V

Difference = Signal
```

If external interference affects both wires similarly, the receiver can reject much of that common interference.

For example:

```text
Original:

Wire A = +1
Wire B = -1

Difference = 2
```

Suppose external interference adds the same disturbance to both:

```text
Wire A = +1 + Noise
Wire B = -1 + Noise
```

The difference between the two wires can still be recovered.

This is one reason twisted-pair differential signaling is effective in electrically noisy environments.

The precise signaling schemes used by Ethernet are more sophisticated than this simplified example, but the underlying principle is important.

### The Four Twisted Pairs

A standard Ethernet twisted-pair cable contains:

```text
Pair 1
├── Conductor
└── Conductor

Pair 2
├── Conductor
└── Conductor

Pair 3
├── Conductor
└── Conductor

Pair 4
├── Conductor
└── Conductor
```

Total:

```text
4 pairs × 2 conductors = 8 conductors
```

The four pairs are individually twisted.

The number of twists per unit length can differ between pairs.

This helps reduce crosstalk between adjacent pairs.

Some higher-performance cable designs also use additional internal separators or improved shielding to reduce interference.

### Why Does Ethernet Need Four Pairs?

Different Ethernet standards use the available pairs in different ways.

Older Ethernet technologies such as 10BASE-T and 100BASE-TX traditionally used two pairs for data transmission.

Gigabit Ethernet over twisted pair, such as 1000BASE-T, uses all four pairs simultaneously.

Conceptually:

```text
10/100 Mbps Ethernet

Pair 1 → Transmit
Pair 2 → Receive

Other pairs → Not used for data
```

Whereas:

```text
1000BASE-T

Pair 1 → Data
Pair 2 → Data
Pair 3 → Data
Pair 4 → Data
```

Modern Ethernet technologies use increasingly sophisticated signaling techniques to transmit data across the available pairs.

The important lesson is that the physical cable contains more conductors than are necessarily required by every Ethernet standard.

### The Ethernet Cable Is More Than "A Wire"

A network cable is an engineered transmission system.

Its performance depends on many factors:

- Conductor quality.
    
- Conductor diameter.
    
- Pair twisting.
    
- Insulation.
    
- Crosstalk characteristics.
    
- Shielding.
    
- Connector quality.
    
- Installation practices.
    
- Cable length.
    

Two cables may look nearly identical externally while having significantly different electrical performance.

This is why Ethernet cables are categorized according to specific technical standards.

### Cable Categories

Common Ethernet cable categories include:

- Cat5e
    
- Cat6
    
- Cat6a
    
- Cat8
    

The term "Cat" stands for **Category**.

The category describes the cable's electrical performance characteristics and frequency capability.

A higher category generally provides better performance characteristics, but the category alone does not automatically determine the speed of the network.

The Ethernet standard, transceiver hardware, cable length, installation quality, and environmental conditions all matter.

### Cat5e

**Category 5e (Cat5e)** is one of the most widely deployed twisted-pair Ethernet cable categories.

Cat5e is commonly used for:

- 100 Mbps Ethernet.
    
- 1 Gbps Ethernet.
    
- Many standard residential and office installations.
    

Cat5e supports frequencies up to approximately:

```text
100 MHz
```

For standard Ethernet deployments, Cat5e can support **1000BASE-T** at distances up to the standard copper Ethernet channel limit when the installation meets the required specifications.

This makes Cat5e sufficient for many ordinary office and home networks.

### Cat6

**Category 6 (Cat6)** improves electrical performance compared with Cat5e.

Cat6 supports frequencies up to approximately:

```text
250 MHz
```

It generally provides better crosstalk performance and improved signal characteristics.

Cat6 is commonly used for:

- 1 Gbps Ethernet.
    
- Some 10 Gbps Ethernet deployments over shorter distances.
    

The important distinction is that Cat6 does not mean "10 Gbps over 100 meters in every situation."

10 Gbps Ethernet over Cat6 may be supported for shorter channel lengths depending on installation conditions and crosstalk performance.

For a full 100-meter 10 Gbps deployment, Cat6a is the more appropriate choice.

### Cat6a

**Category 6A (Cat6a)** is designed for higher-performance Ethernet applications.

Cat6a supports frequencies up to approximately:

```text
500 MHz
```

It is commonly associated with:

```text
10GBASE-T
```

Cat6a is designed to support 10 Gbps Ethernet over the standard 100-meter channel length when properly installed.

Cat6a typically provides improved resistance to alien crosstalk compared with Cat6.

This makes it particularly useful in:

- Enterprise networks.
    
- Data centers.
    
- High-density cable installations.
    
- New building infrastructure.
    

### Cat8

**Category 8 (Cat8)** is designed for very high-speed copper Ethernet applications.

Cat8 supports frequencies up to approximately:

```text
2000 MHz
```

It is associated with Ethernet technologies such as:

```text
25GBASE-T
40GBASE-T
```

Cat8 is primarily intended for specialized high-performance environments, particularly data centers.

It is not generally necessary for ordinary home or office networks.

The cable category should therefore be selected based on actual network requirements rather than simply choosing the highest number available.

### Comparing Cable Categories

A simplified comparison:

|Category|Approx. Frequency|Typical Use|
|---|--:|---|
|Cat5e|100 MHz|1 Gbps Ethernet|
|Cat6|250 MHz|1 Gbps, some shorter-distance 10 Gbps|
|Cat6a|500 MHz|10 Gbps up to 100 m|
|Cat8|2000 MHz|25/40 Gbps specialized copper deployments|

These values should not be interpreted as a simple rule that "higher category always means higher network speed."

The actual speed depends on the entire physical link.

For example:

```text
Network Speed

=

NIC Capability

+

Switch Port Capability

+

Ethernet Standard

+

Cable Category

+

Cable Length

+

Installation Quality
```

The weakest component can limit the overall result.

### Cable Category vs Ethernet Standard

This distinction is essential.

A cable category describes the performance characteristics of the physical cable.

An Ethernet standard defines how Ethernet communication operates over the physical medium.

For example:

```text
Cat6a
```

is a cable category.

```text
10GBASE-T
```

is an Ethernet technology.

They are related, but they are not the same thing.

A network engineer must understand both.

### The 100-Meter Limit

One of the most important facts about copper Ethernet is the standard maximum channel length.

For conventional twisted-pair Ethernet, the commonly referenced maximum channel length is:

```text
100 meters
```

This typically consists of:

```text
90 meters permanent link

+

10 meters patch cords

=

100 meters total channel
```

This is an important distinction.

The 100-meter value is not simply a rule that says:

> "No Ethernet cable may ever be longer than 100 meters."

It is a specification for a compliant Ethernet channel under the relevant standards.

The actual installation consists of:

- Horizontal cable.
    
- Patch panels.
    
- Patch cords.
    
- Connectors.
    
- Wall outlets.
    

All of these contribute to the total channel.

### Why Is Copper Limited to 100 Meters?

The limit exists because electrical signals degrade as they travel.

Several physical effects contribute.

#### Attenuation

As a signal travels through copper, some of its energy is lost.

The signal becomes weaker with distance.

This phenomenon is called **attenuation**.

Conceptually:

```text
Strong Signal

████████████

↓

Cable

↓

Weaker Signal

██████
```

At some point, the receiver can no longer reliably distinguish the intended signal from noise.

### Electromagnetic Interference

Copper cables are susceptible to electromagnetic interference from the surrounding environment.

Potential sources include:

- Electric motors.
    
- Power cables.
    
- Fluorescent lighting.
    
- Radio transmitters.
    
- Industrial machinery.
    

The longer a cable runs, the greater the opportunity for interference to affect the signal.

Twisting and differential signaling help mitigate these effects, but they cannot eliminate them completely.

### Crosstalk

Crosstalk occurs when signals traveling through one pair interfere with signals traveling through another pair.

Because multiple pairs are located inside the same cable, careful engineering is required to minimize this interference.

Crosstalk becomes more significant as:

- Transmission frequencies increase.
    
- Cable lengths increase.
    
- More cables are bundled together.
    

This is one reason higher-category cables have stricter electrical performance requirements.

### Signal Integrity

The receiver must correctly interpret the signal sent by the transmitter.

As distance increases:

```text
Signal Quality

↓

Attenuation

+

Noise

+

Crosstalk

↓

Reduced Signal Integrity
```

Eventually, the receiver may not be able to reliably distinguish the transmitted symbols.

The result can be:

- Bit errors.
    
- Frame errors.
    
- Retransmissions.
    
- Reduced performance.
    
- Loss of link.
    

The 100-meter channel specification exists to ensure that compliant Ethernet links maintain sufficient signal quality for reliable operation.

### What Happens Beyond 100 Meters?

Suppose a switch is located in one building and a computer is located 250 meters away.

Running a single copper Ethernet cable directly between them is not a standard solution.

Instead, the network should use an appropriate intermediate technology.

For example:

```text
Switch

↓

100 m Copper

↓

Intermediate Switch

↓

100 m Copper

↓

Intermediate Switch

↓

50 m Copper

↓

Endpoint
```

Alternatively, fiber optic cable may be used.

```text
Switch

↓

Fiber

↓

Remote Switch

↓

Copper

↓

Endpoint
```

The choice depends on:

- Distance.
    
- Required bandwidth.
    
- Environment.
    
- Budget.
    
- Security requirements.
    
- Existing infrastructure.
    

### Why Fiber Is Different

Copper transmits electrical signals.

Fiber transmits light.

Because fiber does not carry electrical current in the same way copper does, it has fundamentally different characteristics.

Fiber is generally:

- Less affected by electromagnetic interference.
    
- Capable of much longer distances.
    
- Capable of very high bandwidth.
    
- Electrically isolated between endpoints.
    

These advantages make fiber ideal for:

- Building-to-building connections.
    
- Data center interconnects.
    
- ISP networks.
    
- Long-distance backbone links.
    

Fiber will be covered in detail in Part 2.

### Physical Layer Failures

When a network connection fails, the problem may exist below the IP layer.

For example:

```text
Application
     ↑
Transport
     ↑
Network
     ↑
Link
     ↑
Physical  ← Fault
```

If the cable is damaged, the higher layers cannot communicate.

A network engineer may observe:

```text
No Link Light
```

or:

```text
Interface Down
```

before seeing any IP-related errors.

This is why physical-layer troubleshooting is fundamental.

A technician should never assume that every network problem is caused by:

- IP configuration.
    
- DNS.
    
- Routing.
    
- Firewalls.
    

Sometimes the problem is simply:

```text
Broken Cable
```

### Physical Layer Troubleshooting

When troubleshooting a wired Ethernet connection, begin with the physical layer.

Check:

1. Is the cable connected?
    
2. Is the connector properly seated?
    
3. Are the switch and endpoint interfaces operational?
    
4. Are link LEDs active?
    
5. Is the cable damaged?
    
6. Is the cable category appropriate?
    
7. Is the cable longer than the supported channel length?
    
8. Are there excessive bends or physical stresses?
    
9. Is the cable routed near strong sources of electromagnetic interference?
    
10. Is the cable correctly terminated?
    

Only after confirming the physical layer should you move upward through the networking stack.

### Key Takeaways

- The Physical layer converts logical bits into physical signals.
    
- Copper Ethernet uses electrical signaling over twisted-pair cables.
    
- A standard Ethernet twisted-pair cable contains eight conductors arranged into four twisted pairs.
    
- Twisting reduces electromagnetic interference and crosstalk.
    
- Differential signaling allows the receiver to detect the intended signal while rejecting much common-mode interference.
    
- Cat5e, Cat6, Cat6a, and Cat8 describe different levels of cable performance and frequency capability.
    
- Cat5e is commonly sufficient for 1 Gbps Ethernet.
    
- Cat6 provides higher performance and can support some 10 Gbps deployments over shorter distances.
    
- Cat6a is designed for 10 Gbps Ethernet over the standard 100-meter channel.
    
- Cat8 is intended for specialized high-speed copper applications such as certain data center deployments.
    
- The standard Ethernet copper channel limit is commonly 100 meters, consisting of up to 90 meters of permanent link plus up to 10 meters of patch cords.
    
- Copper signals are limited by attenuation, electromagnetic interference, crosstalk, and signal integrity.
    
- A cable category and an Ethernet standard are different concepts.
    
- Physical-layer problems must be considered before assuming a problem exists at the IP, routing, or application layers.
    

### Practical Questions

1. Why does Ethernet use twisted pairs instead of eight straight parallel wires?
    
2. What is the purpose of differential signaling?
    
3. How many individual conductors exist inside a standard twisted-pair Ethernet cable?
    
4. How many twisted pairs exist inside the cable?
    
5. What is the difference between Cat6 and Cat6a?
    
6. Why is Cat6a generally preferred over Cat6 for a full-length 10 Gbps copper deployment?
    
7. What does the 100-meter Ethernet channel limit actually include?
    
8. What physical phenomena cause copper signals to degrade over distance?
    
9. If two computers are 250 meters apart, why should you not simply install one 250-meter copper Ethernet cable between them?
    
10. Why should physical-layer troubleshooting usually begin before troubleshooting IP addressing or routing?
    

### Preview

In Part 2, we will move from electrical signaling to optical and radio-based transmission.

We will examine how fiber optic cables transmit information using light, why **single-mode fiber** can carry signals over extremely long distances while **multimode fiber** is optimized for shorter links, and how **Wavelength Division Multiplexing (WDM)** allows multiple optical signals to share the same fiber.

We will then examine wireless radio as a fundamentally different physical medium, comparing the 2.4 GHz and 5 GHz bands and exploring why the broadcast nature of radio communication makes wireless security and encryption essential.

## Lesson 2.1 — Physical Media (Part 2)

### Fiber Optic Communication

Copper is only one way to move information through a network.

Instead of using electrical signals, fiber optic networking uses **light**.

A fiber optic cable contains extremely thin strands of glass or plastic designed to guide light from one endpoint to another.

The basic concept is:

```text
Electrical Data

↓

Optical Transmitter

↓

Light Pulses

↓

Optical Fiber

↓

Optical Receiver

↓

Electrical Data
````

The networking equipment converts electrical information into optical signals at the transmitting end.

The light travels through the fiber.

At the receiving end, an optical receiver detects the incoming light and converts it back into electrical information that the network equipment can process.

### Why Use Light?

Using light instead of electrical signaling provides several important advantages.

Fiber is generally:

- Resistant to electromagnetic interference.
    
- Capable of very high bandwidth.
    
- Capable of much longer distances than ordinary copper Ethernet.
    
- Electrically non-conductive.
    
- Useful for connections between buildings and across large geographic areas.
    

Copper carries electrical energy.

Fiber carries light.

This difference has major consequences for network engineering.

### Basic Fiber Structure

A simplified fiber optic cable contains several layers.

```text
+---------------------------------------+
| Outer Jacket                          |
|                                       |
|  Protective Layer                     |
|                                       |
|    Cladding                           |
|    ┌─────────────────────────────┐    |
|    │ Core                        │    |
|    │                             │    |
|    │      Light                 │    |
|    │        → → → → →           │    |
|    │                             │    |
|    └─────────────────────────────┘    |
|                                       |
+---------------------------------------+
```

The most important components are:

- Core
    
- Cladding
    
- Coating
    
- Protective jacket
    

### The Core

The **core** is the central region through which the light travels.

The core is made from glass or another suitable optical material.

Its diameter depends on the type of fiber.

This distinction becomes important when comparing single-mode and multimode fiber.

### The Cladding

Surrounding the core is the **cladding**.

The cladding has a different refractive index from the core.

This allows light to remain guided within the fiber.

The fundamental optical principle involved is **total internal reflection**.

### Total Internal Reflection

Light normally changes direction when it moves between materials with different refractive indices.

Under the appropriate conditions, however, light hitting the boundary between the core and cladding is reflected back into the core.

This allows the optical signal to travel along the fiber.

Conceptually:

```text
Cladding
────────────────────────────────

      ↘       ↗       ↘
       ↘     ↗         ↘
        ↘   ↗           ↘
         ↘ ↗             ↘
          ↓
        CORE

────────────────────────────────

Cladding
```

The actual behavior of modern optical fiber is more accurately described using electromagnetic wave propagation, but total internal reflection is a useful introductory model.

### Single-Mode Fiber

**Single-mode fiber (SMF)** uses a very small core.

A typical single-mode fiber core is approximately:

```text
8–10 micrometers
```

A common specification is:

```text
9 µm
```

The small core allows light to propagate primarily through a single mode.

This greatly reduces **modal dispersion**.

As a result, single-mode fiber is capable of supporting extremely long distances and very high data rates.

### Why Single-Mode Goes So Far

One of the major limitations of multimode fiber is modal dispersion.

Different light paths can arrive at different times.

In single-mode fiber, the small core dramatically restricts the number of propagation modes.

This produces much less modal dispersion.

The result is:

```text
Less Modal Dispersion

↓

Cleaner Signal Over Distance

↓

Longer Reach
```

Single-mode fiber is therefore widely used for:

- ISP backbone networks.
    
- Metropolitan networks.
    
- Long-distance telecommunications.
    
- Data center interconnects.
    
- Building-to-building connections.
    
- Submarine fiber systems.
    

With appropriate optical equipment, single-mode systems can operate over distances of tens or even hundreds of kilometers without electrical regeneration.

The exact achievable distance depends heavily on:

- Fiber type.
    
- Optical transceiver.
    
- Wavelength.
    
- Transmit power.
    
- Receiver sensitivity.
    
- Splice losses.
    
- Connector losses.
    
- Dispersion.
    
- Optical budget.
    

Therefore, "100+ km" should be understood as a possible system capability, not a universal physical limit of every single-mode link.

### Multimode Fiber

**Multimode fiber (MMF)** has a larger core than single-mode fiber.

Common multimode core diameters include:

```text
50 µm
62.5 µm
```

The larger core allows multiple modes of light to propagate through the fiber.

Conceptually:

```text
Single-Mode

→────────────────────────→


Multimode

↗───────────────↘
→────────────────→
↘───────────────↗
```

The different paths can arrive at slightly different times.

This creates **modal dispersion**.

As the distance increases, the optical pulses can spread out and become harder to distinguish.

Therefore, multimode fiber is generally used for shorter distances.

### Typical Multimode Distance

The exact maximum distance depends on:

- Fiber grade.
    
- Optical wavelength.
    
- Ethernet standard.
    
- Transceiver type.
    
- Required data rate.
    

For example, some common multimode Ethernet implementations can reach approximately:

```text
550 meters
```

at certain data rates using appropriate OM3/OM4-class fiber and optics.

The 550-meter figure should therefore not be memorized as "the maximum distance of multimode fiber."

It is an example of a particular fiber/transceiver combination.

### Single-Mode vs Multimode

|Property|Single-Mode|Multimode|
|---|---|---|
|Core|Very small|Larger|
|Typical Core|~9 µm|50 or 62.5 µm|
|Propagation|Primarily one mode|Multiple modes|
|Modal Dispersion|Very low|Higher|
|Typical Reach|Very long|Shorter|
|Common Use|WAN, backbone, long-distance|LAN, data center, building links|
|Optical Equipment|Often more specialized|Often optimized for shorter links|

The key concept is not simply:

```text
Single-mode = long
Multimode = short
```

The deeper explanation is:

```text
Core Geometry

↓

Propagation Modes

↓

Dispersion

↓

Reach
```

### Fiber Does Not Mean Unlimited Bandwidth

Fiber is often described as having "unlimited bandwidth."

This is an oversimplification.

Fiber has enormous potential bandwidth, but the actual network capacity depends on the complete optical system.

Factors include:

- Optical transceiver capability.
    
- Wavelength.
    
- Modulation.
    
- Fiber type.
    
- Dispersion.
    
- Optical power.
    
- Receiver sensitivity.
    
- Network architecture.
    

A fiber strand does not automatically provide a specific Ethernet speed.

For example, the same physical fiber infrastructure may support different speeds depending on the optics connected to it.

### Wavelength

Optical networking commonly uses specific wavelengths of light rather than arbitrary visible colors.

Common telecommunications windows include regions around:

```text
850 nm
1310 nm
1550 nm
```

The wavelength affects:

- Fiber attenuation.
    
- Dispersion.
    
- Optical component design.
    
- Maximum reach.
    
- Transceiver cost.
    

Multimode Ethernet frequently uses approximately 850 nm optics.

Single-mode systems commonly use 1310 nm or 1550 nm regions depending on the application.

### Wavelength Division Multiplexing

One of the most powerful characteristics of fiber is that a single strand can carry multiple optical signals simultaneously using different wavelengths.

This is called **Wavelength Division Multiplexing (WDM)**.

The basic idea is:

```text
λ1 ────────────────→
λ2 ────────────────→
λ3 ────────────────→
λ4 ────────────────→

        One Fiber
```

Each wavelength represents an independent optical channel.

At the receiving end, optical components separate the wavelengths.

Conceptually:

```text
Multiple Signals

λ1 ─┐
λ2 ─┼──→ WDM Mux → One Fiber → WDM Demux ─┬→ λ1
λ3 ─┤                                      ├→ λ2
λ4 ─┘                                      ├→ λ3
                                           └→ λ4
```

This allows network operators to dramatically increase the capacity of existing fiber infrastructure without installing additional physical fibers.

### CWDM and DWDM

Two important WDM technologies are:

**CWDM — Coarse Wavelength Division Multiplexing**

and

**DWDM — Dense Wavelength Division Multiplexing**

CWDM uses wavelengths spaced farther apart.

DWDM uses wavelengths much more closely spaced together.

DWDM can therefore support a much larger number of optical channels on the same fiber.

This technology is extensively used in telecommunications and high-capacity backbone networks.

### Fiber Pairs

A fiber Ethernet connection may use:

```text
Fiber A → Transmit

Fiber B → Receive
```

This is a common duplex configuration.

Some optical technologies can transmit and receive using different wavelengths over the same physical fiber.

This is often called **BiDi** or bidirectional optics.

For example:

```text
Fiber

→ λ1

← λ2
```

The two directions share the same physical strand while using different wavelengths.

### Fiber Connectors

Common fiber connector types include:

- LC
    
- SC
    
- ST
    
- MPO/MTP
    

The connector is only the physical interface.

It does not determine whether the fiber is single-mode or multimode by itself.

For example, LC connectors are widely used with both single-mode and multimode systems.

### Fiber Splicing

Long fiber links often contain splices.

A splice joins two optical fibers together.

The two major categories are:

- Mechanical splicing.
    
- Fusion splicing.
    

**Fusion splicing** uses heat to permanently join the fibers.

A properly executed fusion splice can have very low optical loss.

Splicing becomes particularly important in:

- Long-distance networks.
    
- ISP infrastructure.
    
- Outside-plant fiber.
    
- Backbone deployments.
    

### Fiber Security

Fiber is often considered more secure against passive interception than copper.

The reason is straightforward.

Copper carries electrical signals that can often be monitored by making electrical contact with the conductor.

Fiber carries light inside glass.

An attacker cannot simply attach a conventional electrical probe to the fiber and read the signal.

However, fiber is **not impossible to tap**.

Specialized optical tapping techniques can extract a portion of the optical signal.

Depending on the technique, tapping may also introduce detectable optical loss or other changes.

Therefore:

```text
Fiber

≠

Perfectly Secure
```

The correct security conclusion is:

```text
Fiber is more resistant to passive interception

but

Fiber does not eliminate the need for encryption.
```

### Wireless Communication

The third major physical medium is wireless radio.

Instead of using a physical cable, wireless networking uses electromagnetic radiation to transmit information through free space.

Wi-Fi is the most familiar example.

The simplified process is:

```text
Digital Data

↓

Radio Transmitter

↓

Electromagnetic Signal

↓

Free Space

↓

Radio Receiver

↓

Digital Data
```

Wireless networking therefore eliminates the physical cable between the communicating devices.

That provides mobility and flexibility.

It also introduces unique technical and security challenges.

### Wireless Is a Shared Medium

A copper cable physically confines most of its signal to the cable.

A fiber cable confines light within the optical medium.

Radio is fundamentally different.

A radio transmission propagates through space.

Other devices within the appropriate reception area may be capable of receiving the signal.

This makes wireless communication a **shared medium**.

Conceptually:

```text
              Laptop
                 ↑
                 |
                 |
Phone ←────── Access Point ──────→ Tablet
                 |
                 |
              IoT Device
```

The access point's transmission is not physically confined to a single cable between two devices.

Its radio signal propagates through the surrounding environment.

### The Broadcast Nature of Wireless

Wireless transmissions can potentially be received by multiple stations within radio range.

This does not mean every device automatically understands or accepts every transmission.

Authentication, encryption, channel access rules, and protocol behavior determine what devices can actually use the communication.

But from a physical-layer security perspective, the important point is:

```text
Radio signals leave the intended device

↓

They propagate through physical space

↓

Other receivers may detect them
```

This is why wireless networks require strong security controls.

### Why Encryption Is Mandatory

Suppose a wireless network transmits sensitive information without encryption.

An attacker nearby may be able to capture the radio transmissions.

Even if the attacker cannot actively join the network, they may still collect the wireless frames.

Encryption protects the confidentiality of the information contained in those frames.

Modern Wi-Fi security mechanisms include:

- WPA2.
    
- WPA3.
    

Older systems such as WEP are obsolete and should not be used.

The critical principle is:

```text
Wireless visibility

+

Radio reception

↓

Potential interception

↓

Encryption required
```

Encryption should not be treated as an optional feature for serious wireless networks.

### Wireless Is Not "Less Secure" Simply Because It Is Wireless

The correct engineering statement is more precise.

Wireless has a different physical security model.

With a wired network, an attacker typically needs some form of physical access to the network infrastructure or cable.

With wireless, the transmission itself propagates through an environment that may be accessible to unintended receivers.

This increases the importance of:

- Strong encryption.
    
- Strong authentication.
    
- Secure credentials.
    
- Proper access-point configuration.
    
- Radio-frequency planning.
    
- Monitoring.
    

### 2.4 GHz vs 5 GHz

Wi-Fi has historically operated across several frequency bands.

Two important bands are:

```text
2.4 GHz
5 GHz
```

They have different propagation characteristics and practical trade-offs.

### 2.4 GHz

The 2.4 GHz band generally provides better propagation through walls and longer practical range than 5 GHz under similar conditions.

Advantages include:

- Better penetration through obstacles.
    
- Longer practical range.
    
- Broad device compatibility.
    

Disadvantages include:

- Less available spectrum.
    
- More congestion.
    
- Greater interference from other devices.
    
- Fewer non-overlapping channels in many regulatory domains.
    

Devices such as:

- Bluetooth equipment.
    
- Microwave ovens.
    
- Wireless peripherals.
    

can also operate in or around the 2.4 GHz spectrum and contribute to interference.

### 5 GHz

The 5 GHz band generally provides:

- More available spectrum.
    
- More channels.
    
- Higher potential throughput.
    
- Less congestion in many environments.
    

However, higher frequency radio signals generally experience greater attenuation through walls and other obstacles than lower-frequency signals.

Therefore, 5 GHz often provides:

```text
Higher Potential Throughput

but

Shorter Practical Range
```

The actual result depends heavily on:

- Transmit power.
    
- Antenna characteristics.
    
- Building materials.
    
- Channel width.
    
- Interference.
    
- Regulatory restrictions.
    
- Client capabilities.
    

### Comparing 2.4 GHz and 5 GHz

|Characteristic|2.4 GHz|5 GHz|
|---|---|---|
|Range|Generally longer|Generally shorter|
|Wall penetration|Generally better|Generally worse|
|Available spectrum|More limited|More available|
|Interference|Often higher|Often lower|
|Potential throughput|Lower|Higher|
|Common use|Coverage and compatibility|Higher-performance WLAN|

This is a general comparison rather than an absolute rule.

Modern Wi-Fi systems may also operate in additional bands such as 6 GHz, which will be covered when we study wireless networking in greater depth.

### Frequency Does Not Equal Speed

A common misconception is:

> "5 GHz is twice as fast as 2.4 GHz."

This is incorrect.

The frequency is not directly the network speed.

Actual Wi-Fi throughput depends on factors such as:

- Channel width.
    
- Modulation.
    
- Coding rate.
    
- Number of spatial streams.
    
- Signal-to-noise ratio.
    
- Wi-Fi generation.
    
- Client capabilities.
    
- Access-point capabilities.
    
- Interference.
    

A 5 GHz network can often provide higher throughput because it generally has more usable spectrum and can support wider channels, but frequency alone does not determine speed.

### Signal Attenuation in Wireless

Wireless signals also weaken over distance.

This is called attenuation.

A simplified model is:

```text
Access Point

██████████

↓

Distance

↓

██████

↓

Further Distance

↓

██
```

Obstacles can increase attenuation significantly.

Examples include:

- Concrete.
    
- Brick.
    
- Metal.
    
- Glass with certain coatings.
    
- Furniture.
    
- Human bodies.
    

The resulting signal strength influences whether a wireless client can maintain a reliable connection.

### Signal-to-Noise Ratio

One of the most important concepts in wireless networking is **Signal-to-Noise Ratio (SNR)**.

SNR describes the strength of the desired signal relative to background noise.

Conceptually:

```text
Strong Signal
+
Low Noise
=
High SNR
```

Whereas:

```text
Weak Signal
+
High Noise
=
Low SNR
```

Higher SNR generally allows the system to use more efficient modulation and coding schemes.

As SNR decreases, Wi-Fi may need to reduce its data rate to maintain reliability.

This can produce an important real-world effect:

```text
Distance Increases

↓

Signal Quality Decreases

↓

Lower Modulation/Coding

↓

Lower Throughput
```

A client can therefore remain connected while experiencing dramatically lower performance.

### Copper vs Fiber vs Wireless

We can now compare the three media.

|Property|Copper|Fiber|Wireless|
|---|---|---|---|
|Signal|Electrical|Optical|Radio|
|Physical cable|Yes|Yes|No|
|EMI resistance|Limited|Excellent|Not applicable in same sense|
|Typical reach|Short/medium|Medium to extremely long|Environment-dependent|
|Shared medium|Usually no in modern switched Ethernet|Usually point-to-point|Yes|
|Passive interception|Possible|Difficult but possible|Radio can be received within range|
|Encryption|Depends on protocol/use|Still required|Essential|
|Mobility|Low|Low|High|

There is no universally "best" medium.

The correct choice depends on the network requirements.

### Choosing the Appropriate Medium

A network engineer should consider:

```text
Distance

+

Bandwidth

+

Environment

+

Interference

+

Security

+

Cost

+

Installation Requirements

+

Future Expansion
```

For example:

A desktop computer inside an office may use:

```text
Twisted-Pair Copper
```

A building-to-building backbone may use:

```text
Single-Mode Fiber
```

A mobile laptop may use:

```text
Wi-Fi
```

An ISP's long-distance backbone may use:

```text
Single-Mode Fiber + WDM
```

The physical medium should be selected based on engineering requirements rather than convenience alone.

### Key Takeaways

- Fiber optic networks transmit information using light rather than electrical signals.
    
- Fiber consists of a core surrounded by cladding and protective layers.
    
- The core and cladding have different optical properties that allow the signal to remain guided through the fiber.
    
- Single-mode fiber has a very small core and supports extremely long-distance communication.
    
- Multimode fiber has a larger core and allows multiple propagation modes, resulting in greater modal dispersion and shorter typical reach.
    
- A 550-meter reach is a common example for certain multimode Ethernet implementations, not a universal maximum for all multimode systems.
    
- WDM allows multiple optical wavelengths to share the same fiber.
    
- CWDM and DWDM provide different levels of wavelength multiplexing density.
    
- Fiber is resistant to electromagnetic interference and is difficult to passively tap compared with copper, but it is not impossible to tap.
    
- Wireless networks transmit information through radio waves.
    
- Wireless is inherently a shared medium because radio signals propagate through physical space.
    
- Wireless encryption is essential because unintended receivers may be able to detect radio transmissions.
    
- 2.4 GHz generally provides better range and penetration, while 5 GHz generally provides more spectrum and higher potential throughput.
    
- Frequency alone does not determine wireless speed.
    
- Signal quality, interference, channel width, modulation, coding, and device capabilities all influence Wi-Fi performance.
    
- The correct physical medium depends on distance, bandwidth, environment, security, cost, and deployment requirements.
    

### Practical Questions

1. What physical phenomenon carries information through an optical fiber?
    
2. What are the core and cladding, and why are they both necessary?
    
3. Why does single-mode fiber generally support longer distances than multimode fiber?
    
4. What is modal dispersion?
    
5. Why is the 550-meter figure for multimode fiber not a universal maximum?
    
6. What problem does WDM solve?
    
7. What is the difference between CWDM and DWDM?
    
8. Why is fiber generally more resistant to electromagnetic interference than copper?
    
9. Why does fiber still require encryption even though it is difficult to tap?
    
10. Why is wireless considered a shared medium?
    
11. Why can a nearby attacker potentially detect Wi-Fi transmissions without being physically connected to the network?
    
12. Why does 5 GHz generally provide higher potential throughput than 2.4 GHz?
    
13. Why does 2.4 GHz generally provide better range than 5 GHz?
    
14. Why is it incorrect to say that "5 GHz is twice as fast as 2.4 GHz"?
    
15. What is SNR, and why does it affect Wi-Fi throughput?
    

### Mental Model

Remember the three physical media using this model:

```text
COPPER

Electrical Signal
        ↓
Shorter Reach
        ↓
EMI / Crosstalk Considerations


FIBER

Light
        ↓
Very Long Reach
        ↓
High Bandwidth
        ↓
WDM Can Multiply Capacity


WIRELESS

Radio Waves
        ↓
Shared Physical Space
        ↓
Mobility
        ↓
Interference + Interception Risk
        ↓
Encryption Essential
```

The Physical layer determines what can physically happen to the signal before any higher-layer protocol can do anything about it.

## Lesson 2.1 — Physical Media (Part 3)

### Coaxial Cable

Twisted-pair copper and fiber optic cable are not the only physical media used in networking.

Another important medium is **coaxial cable**, commonly called **coax**.

Coaxial cable has been used extensively in:

- Cable television networks.
- Broadband Internet access.
- DOCSIS networks.
- CCTV systems.
- Radio-frequency systems.
- Antenna systems.

Unlike twisted-pair Ethernet, coaxial cable has a very different internal structure.

A simplified cross-section looks like this:

```text
        Outer Jacket
    ┌───────────────────┐
    │   Shield          │
    │ ┌───────────────┐ │
    │ │ Dielectric    │ │
    │ │   ┌───────┐   │ │
    │ │   │ Core  │   │ │
    │ │   └───────┘   │ │
    │ └───────────────┘ │
    └───────────────────┘
````

The main components are:

- Central conductor.
    
- Dielectric insulator.
    
- Metallic shield.
    
- Outer jacket.
    

### The Central Conductor

The central conductor carries the electrical signal.

It is typically made from copper or another conductive material.

Unlike twisted-pair Ethernet, which uses multiple individually insulated conductors arranged into pairs, coaxial cable has a central conductor surrounded by insulating and shielding layers.

### The Dielectric

The central conductor is surrounded by a dielectric material.

The dielectric electrically separates the center conductor from the outer shield.

It also contributes to the electrical characteristics of the cable.

### The Shield

Around the dielectric is a conductive shield.

The shield can consist of:

- Braided metal.
    
- Foil.
    
- A combination of foil and braid.
    

The shield helps contain the electromagnetic field and protects the signal from external interference.

This is one of the fundamental differences between coaxial cable and ordinary unshielded twisted-pair cable.

### The Outer Jacket

The outer jacket protects the internal components from the physical environment.

Depending on the cable's intended installation, the jacket may be designed to resist:

- Moisture.
    
- Physical abrasion.
    
- Temperature changes.
    
- Chemicals.
    
- Fire.
    

The exact jacket construction depends on the cable type and installation environment.

### Coaxial Cable as a Transmission Medium

Coaxial cable carries electrical signals.

Therefore, like twisted-pair copper, it is affected by physical phenomena such as:

- Attenuation.
    
- Noise.
    
- Electromagnetic interference.
    
- Signal reflections.
    
- Cable length.
    

The shielding provides significant protection against external electromagnetic interference, but coaxial cable is not immune to signal degradation.

### Cable Television Networks

One of the most important real-world uses of coaxial cable is **cable television infrastructure**.

A simplified cable network may look like:

```text
Provider Network
       |
       |
   Distribution
       |
       |
   Coaxial Plant
       |
       +--------- Home A
       |
       +--------- Home B
       |
       +--------- Home C
```

This is fundamentally different from the dedicated point-to-point Ethernet link between a computer and switch.

Multiple customers may share portions of the same physical infrastructure.

### Shared Medium

A **shared medium** is a medium in which multiple devices or subscribers use the same physical transmission resource.

This concept is important for understanding cable networks.

Consider:

```text
                Shared Coax
                    |
       +------------+------------+
       |            |            |
     Home A       Home B       Home C
```

The physical infrastructure is shared.

This creates different engineering requirements from a dedicated point-to-point link.

The network must coordinate how information is transmitted and delivered to the appropriate subscriber.

### DOCSIS

**DOCSIS** stands for:

```text
Data Over Cable Service Interface Specification
```

It is a family of specifications used to deliver data services over cable television infrastructure.

DOCSIS allows cable operators to use existing coaxial access networks to provide broadband Internet service.

A simplified architecture is:

```text
Internet
   |
   |
Cable Operator Network
   |
   |
CMTS
   |
   |
Coaxial Access Network
   |
   |
Cable Modem
   |
   |
Customer Network
```

The customer typically has a **cable modem** or gateway.

The cable modem communicates with the provider's infrastructure over the coaxial access network.

### CMTS

A traditional DOCSIS architecture uses a device called a **Cable Modem Termination System**, or **CMTS**.

The CMTS exists within the provider's network and communicates with cable modems across the access network.

Conceptually:

```text
Cable Modem
     ⇅
Coaxial Network
     ⇅
    CMTS
     ⇅
Provider Network
     ⇅
  Internet
```

The CMTS handles the provider-side communication with customer cable modems.

Modern architectures can also use newer distributed architectures, but the fundamental idea remains the same: customer equipment communicates with provider infrastructure across a shared cable access network.

### Why Shared Media Matter

The shared nature of cable networks has important implications.

A network engineer needs to understand:

- Multiple customers may use the same physical infrastructure.
    
- The medium has finite capacity.
    
- Traffic must be coordinated.
    
- The provider must manage the available spectrum.
    
- The network must separate customers logically.
    
- Physical infrastructure can affect many customers simultaneously.
    

This is another example of why the physical layer influences higher-layer network behavior.

### Shared Does Not Mean "Everyone Gets Everyone Else's Data"

A common misconception is:

> If multiple customers share the same coaxial network, every customer can automatically read every other customer's Internet traffic.

That is not a correct description of modern cable networks.

The physical medium may be shared, but network systems use protocol mechanisms, addressing, encryption, provisioning, and traffic isolation to prevent customers from simply receiving and interpreting one another's private communications.

The important distinction is:

```text
Shared Physical Infrastructure

≠

Shared Access to Private Data
```

This distinction will become increasingly important as we study switching, VLANs, wireless networks, and network security.

### Coaxial vs Twisted Pair

Both are copper-based media, but their construction is very different.

```text
Twisted Pair:

Pair A → twisted conductors
Pair B → twisted conductors
Pair C → twisted conductors
Pair D → twisted conductors


Coax:

Central conductor
       ↓
Dielectric
       ↓
Metallic shield
       ↓
Outer jacket
```

Twisted-pair Ethernet is heavily associated with local Ethernet access.

Coaxial cable is heavily associated with RF distribution and cable broadband.

### Physical Security of Coax

Coaxial cable can be physically accessed and tapped.

Because it carries electrical signals, an attacker with physical access to the cable or associated infrastructure may potentially attempt to intercept or manipulate the signal.

This means physical security matters.

Examples include:

- Securing cable distribution points.
    
- Protecting exposed cable.
    
- Restricting access to network cabinets.
    
- Protecting provider infrastructure.
    
- Monitoring unauthorized physical modifications.
    

As with other physical media, encryption provides an additional layer of protection.

### Passive Tapping

A **passive tap** attempts to observe communication without actively disrupting or modifying the signal.

The basic concept is:

```text
Normal Signal

Device A ─────────────── Device B
                  |
                  |
                Tap
                  |
                  ↓
             Monitoring
```

The goal is to observe the communication while minimizing changes to the original signal.

Copper-based media can be susceptible to physical interception because the signal is electrical.

However, whether a particular tapping technique works depends on the cable, signaling system, equipment, and physical access available to the attacker.

### Physical Access Is a Security Boundary

This leads to an important networking security principle:

> Physical access can undermine logical security.

Imagine an organization has:

- Strong passwords.
    
- Firewalls.
    
- Network segmentation.
    
- Authentication.
    
- Encryption.
    

An attacker who gains unauthorized physical access to network infrastructure may still be able to create serious problems.

They could potentially:

- Disconnect cables.
    
- Replace equipment.
    
- Insert unauthorized devices.
    
- Attempt signal interception.
    
- Damage infrastructure.
    
- Introduce rogue networking equipment.
    

Network security therefore begins below the application layer.

It begins with the physical environment.

### Comparing Physical Interception

The four media introduced in this lesson can now be compared from a physical-security perspective.

|Medium|Physical Signal|Passive Interception|
|---|---|---|
|Twisted-pair copper|Electrical|Possible with physical access|
|Coaxial|Electrical/RF|Possible with physical access|
|Fiber|Optical|Difficult but possible with specialized equipment|
|Wireless|Radio|Signals can be received within range|

None of these should be interpreted as "secure" or "insecure" by themselves.

Security depends on:

- Physical access.
    
- Signal propagation.
    
- Encryption.
    
- Authentication.
    
- Network architecture.
    
- Monitoring.
    

### Ethernet Cable Construction

Return to twisted-pair Ethernet.

A standard Ethernet twisted-pair cable contains:

```text
8 conductors

4 twisted pairs

2 conductors per pair
```

The conductors are individually insulated.

The insulation allows the conductors to remain electrically separated.

The four pairs are then twisted at controlled rates.

The complete assembly is placed inside an outer jacket.

### Cable Anatomy

A simplified representation:

```text
+---------------------------------------+
|              Outer Jacket             |
|                                       |
|  Pair 1       Pair 2                  |
|   ↻ ↻          ↻ ↻                    |
|                                       |
|  Pair 3       Pair 4                  |
|   ↻ ↻          ↻ ↻                    |
|                                       |
+---------------------------------------+
```

The twisting is carefully engineered.

It helps reduce:

- External interference.
    
- Pair-to-pair crosstalk.
    

Different pairs may have different twist rates.

This helps reduce the likelihood that repeated electrical patterns from one pair will strongly couple into another pair.

### Shielded vs Unshielded Cable

Twisted-pair Ethernet cable can also be constructed with different levels of shielding.

Common terminology includes:

- UTP — Unshielded Twisted Pair.
    
- FTP — Foiled Twisted Pair.
    
- STP — Shielded Twisted Pair.
    

Terminology varies between standards and manufacturers, so cable markings should be interpreted carefully.

The important concept is:

```text
UTP

No overall metallic shield


Shielded Cable

Additional conductive shielding

↓

Greater protection against external interference
```

Shielding does not automatically make a cable better for every installation.

Proper grounding and installation are important.

Poorly implemented shielding can create problems rather than solving them.

### The Ethernet Connector

The connector commonly associated with twisted-pair Ethernet is often called an **RJ-45 connector**.

Strictly speaking, the modular connector used for Ethernet is generally an **8P8C connector**.

The distinction is useful for a networking professional.

```text
8P8C

8 Positions
8 Contacts
```

The connector provides eight contacts corresponding to the eight conductors in the cable.

### T-568B

The conductors must be terminated in a defined order.

One commonly used wiring arrangement is **T-568B**.

The pin order is:

```text
Pin 1 — White/Orange
Pin 2 — Orange
Pin 3 — White/Green
Pin 4 — Blue
Pin 5 — White/Blue
Pin 6 — Green
Pin 7 — White/Brown
Pin 8 — Brown
```

Memorize this order.

It is useful when:

- Building patch cables.
    
- Troubleshooting cable faults.
    
- Testing wall outlets.
    
- Examining patch panels.
    
- Diagnosing incorrect terminations.
    

### T-568B Visualized

Viewed from the connector side with the contacts oriented appropriately:

```text
Pin:     1          2       3          4
         |          |       |          |
      W/Orange   Orange   W/Green     Blue

Pin:     5          6       7          8
         |          |       |          |
       W/Blue     Green   W/Brown     Brown
```

The actual physical orientation depends on how the connector is being held, so when terminating a cable, always establish the correct reference orientation before applying the pin sequence.

### T-568A

Another recognized wiring arrangement is **T-568A**.

Its order is:

```text
Pin 1 — White/Green
Pin 2 — Green
Pin 3 — White/Orange
Pin 4 — Blue
Pin 5 — White/Blue
Pin 6 — Orange
Pin 7 — White/Brown
Pin 8 — Brown
```

The difference between T-568A and T-568B is primarily the interchange of the green and orange pairs.

The blue and brown pairs remain in the same positions.

### T-568A vs T-568B

```text
T-568A

1 W/Green
2 Green
3 W/Orange
4 Blue
5 W/Blue
6 Orange
7 W/Brown
8 Brown


T-568B

1 W/Orange
2 Orange
3 W/Green
4 Blue
5 W/Blue
6 Green
7 W/Brown
8 Brown
```

Both are standardized wiring arrangements.

The important thing is consistency.

### Straight-Through Cable

If both ends use the same wiring standard:

```text
T-568B ─────────────── T-568B
```

or:

```text
T-568A ─────────────── T-568A
```

the corresponding conductors connect to the same pins.

This is commonly called a **straight-through cable**.

Conceptually:

```text
Pin 1 ───────── Pin 1
Pin 2 ───────── Pin 2
Pin 3 ───────── Pin 3
...
Pin 8 ───────── Pin 8
```

### Crossover Cable

If one end uses T-568A and the other uses T-568B:

```text
T-568A ─────────────── T-568B
```

the transmit and receive pairs are crossed.

Historically, this was important when directly connecting similar Ethernet devices.

For example:

```text
Computer ↔ Computer

Switch ↔ Switch
```

Modern Ethernet equipment commonly supports **Auto-MDI/MDI-X**, which allows interfaces to automatically detect and compensate for the required pair configuration.

As a result, manually building crossover cables is far less important in modern networks than it once was.

However, understanding the concept remains important for troubleshooting and understanding Ethernet's physical layer.

### Why the Pin Order Matters

Suppose the cable is terminated incorrectly.

The cable might physically look fine.

The connector may click perfectly into the port.

But the electrical connections may be wrong.

Possible results include:

```text
No Link

Intermittent Link

Reduced Speed

Packet Errors

Link Negotiation Problems
```

This is why cable testing is a fundamental networking skill.

### Cable Tester

A basic Ethernet cable tester can verify whether the conductors are connected correctly.

A typical tester has:

```text
Main Unit

+

Remote Unit
```

The cable is connected between them.

The tester sends signals through the conductors and checks the resulting connections.

A correctly wired cable may produce:

```text
1 → 1
2 → 2
3 → 3
4 → 4
5 → 5
6 → 6
7 → 7
8 → 8
```

A faulty cable might show something like:

```text
1 → 1
2 → 2
3 → 6
6 → 3
```

or:

```text
1 → 1
2 → 2
3 → Open
4 → 4
...
```

This lets the technician identify wiring problems without relying solely on whether a network connection works.

### Open Circuit

An **open circuit** means that a conductor does not provide a continuous electrical path.

Possible causes include:

- Broken conductor.
    
- Poor termination.
    
- Damaged connector.
    
- Incorrect crimping.
    

The tester may report:

```text
Pin 3 → Open
```

This indicates that the expected electrical path is not continuous.

### Short Circuit

A **short circuit** occurs when conductors that should remain electrically separate become connected.

This can occur because of:

- Damaged insulation.
    
- Incorrect termination.
    
- Connector problems.
    
- Physical cable damage.
    

The result can prevent the Ethernet interface from operating correctly.

### Miswire

A cable can also have all conductors connected while still being wired incorrectly.

For example:

```text
Expected:

1 → 1
2 → 2
3 → 3
4 → 4


Actual:

1 → 1
2 → 2
3 → 6
6 → 3
```

This is not an open circuit.

The conductors are connected.

They are simply connected to the wrong pins.

A proper cable tester can identify this type of fault.

### Split Pair

A particularly important wiring problem is a **split pair**.

The cable may appear to have the correct pin numbers at both ends, but the conductors are not actually paired correctly.

The electrical continuity test may pass while the cable's transmission characteristics are degraded.

This matters because Ethernet relies on carefully constructed twisted pairs.

For example, suppose the intended pairs are:

```text
Pair A:
1 + 2

Pair B:
3 + 6
```

If conductors are rearranged so that the continuity appears correct but the actual physical pairs are incorrect, the cable may experience severe crosstalk.

This is why professional cable certification is more sophisticated than simply checking continuity.

### Continuity vs Certification

A basic cable tester answers a relatively simple question:

> Are the conductors connected correctly?

A cable certifier answers much more demanding questions about whether the installed link meets the performance requirements of a particular cabling category.

Certification testing may measure characteristics such as:

- Insertion loss.
    
- Return loss.
    
- NEXT.
    
- FEXT.
    
- Propagation delay.
    
- Delay skew.
    
- Resistance.
    
- Length.
    

This distinction matters in professional network installations.

A cable can have perfect continuity and still fail to meet the performance requirements of its category.

### Physical Cable Path

The physical cable itself is only part of the network infrastructure.

A typical office Ethernet path might look like:

```text
Computer
   |
   |
Patch Cable
   |
   |
Wall Outlet
   |
   |
Horizontal Cable
   |
   |
Patch Panel
   |
   |
Patch Cable
   |
   |
Switch
```

This is why network engineers need to understand physical infrastructure.

A problem may occur anywhere along the path.

### Cable Tracing

When a cable is not labeled, technicians may need to trace it physically.

The process may involve:

1. Identifying the endpoint.
    
2. Following the visible cable.
    
3. Locating wall outlets.
    
4. Identifying patch panels.
    
5. Finding the corresponding switch port.
    
6. Documenting the physical path.
    

A basic physical network diagram might look like:

```text
PC-01
  |
  | Patch Cable
  |
Wall Jack A-12
  |
  | Horizontal Cable
  |
Patch Panel A
  |
  | Patch Cable
  |
Switch-01
  |
Port 18
```

This information becomes extremely valuable during troubleshooting.

### Physical Security During Cable Tracing

Tracing cables is not only a technical task.

It is also a security task.

While following a cable, ask:

- Where can someone physically access this cable?
    
- Does it pass through public areas?
    
- Is it exposed?
    
- Is the network cabinet locked?
    
- Are patch panels accessible?
    
- Are unused switch ports secured?
    
- Are cables labeled?
    
- Could someone easily insert an unauthorized device?
    

A physical network can have strong logical security and still be poorly protected physically.

### Lab Preparation

For the practical portion of this lesson, you will work with a physical Ethernet cable if hardware is available.

You will need:

```text
Ethernet Patch Cable

+

Cable Tester
```

If a cable tester is not available, you can instead trace a network cable through a room or lab environment.

The goal is not merely to identify a cable.

The goal is to connect what you see physically with the networking concepts studied so far.

### Lab Objective

By the end of the lab, you should be able to:

- Identify the physical medium.
    
- Identify the cable category.
    
- Identify the eight conductors.
    
- Identify the four twisted pairs.
    
- Recognize T-568B termination.
    
- Test continuity where equipment is available.
    
- Identify wiring faults.
    
- Trace a cable path where testing equipment is unavailable.
    
- Identify physical security concerns.
    

### Part 3 Summary

Coaxial cable is an important copper-based physical medium used extensively in cable television and DOCSIS broadband networks.

Unlike a dedicated point-to-point Ethernet cable, portions of a cable access network may be shared among multiple subscribers.

The shared nature of the medium affects how the network is designed, managed, and secured.

Twisted-pair Ethernet cable has a different construction.

It contains eight individual conductors arranged into four twisted pairs.

The twisting helps reduce interference and crosstalk, while differential signaling allows Ethernet receivers to recover the intended signal from the electrical relationship between conductors.

Ethernet cables are terminated according to defined wiring arrangements such as T-568A and T-568B.

T-568B uses this order:

```text
1 — White/Orange
2 — Orange
3 — White/Green
4 — Blue
5 — White/Blue
6 — Green
7 — White/Brown
8 — Brown
```

A cable with the same standard on both ends is generally a straight-through cable.

T-568A on one end and T-568B on the other produces a crossover arrangement.

Modern Ethernet equipment usually handles crossover requirements automatically through Auto-MDI/MDI-X, but understanding the wiring remains important for physical-layer troubleshooting.

A cable tester can identify continuity problems, shorts, opens, and wiring faults.

However, continuity testing alone does not prove that a cable meets its full performance specification.

Professional certification testing examines additional electrical characteristics such as insertion loss, return loss, and crosstalk.

The key lesson is that the physical network is an engineered system.

A network connection can fail because of a broken conductor, poor termination, excessive length, interference, crosstalk, damaged connectors, or other physical-layer problems long before IP addressing or routing becomes relevant.

### Knowledge Check

1. What are the four major components of a coaxial cable?
    
2. Why is coaxial cable commonly used in cable TV and DOCSIS networks?
    
3. What does it mean for a physical medium to be shared?
    
4. What does DOCSIS stand for?
    
5. What is the purpose of the shield in coaxial cable?
    
6. How many conductors are inside a standard twisted-pair Ethernet cable?
    
7. How many twisted pairs are present?
    
8. What is the T-568B pin order?
    
9. What is the difference between T-568A and T-568B?
    
10. What is a straight-through cable?
    
11. What is a crossover cable?
    
12. Why are crossover cables less important in modern Ethernet networks?
    
13. What is an open circuit?
    
14. What is a short circuit?
    
15. What is a split pair?
    
16. Why can a cable pass a basic continuity test and still fail to perform properly?
    
17. What is the difference between cable testing and cable certification?
    
18. Why is physical access considered a security concern?
    
19. Why does a shared physical medium not necessarily mean that every user can read every other user's traffic?
    
20. If a workstation has no network connectivity, why should the physical cable and termination be checked before investigating DNS or routing?

## Lesson 2.1 — Physical Media (Part 4)

### Lab — Physical Media Investigation

This lab turns the concepts from the lesson into practical networking work.

The objective is to stop thinking about a network cable as an abstract connection between two devices and start treating it as an actual physical transmission system.

You will examine the cable, identify its construction, determine how it is terminated, test it if equipment is available, and trace the physical path of a network connection when testing equipment is not available.

### Lab Objectives

By the end of this lab, you should be able to :

- Identify twisted-pair Ethernet cable.
- Identify the cable category from its markings.
- Identify the eight conductors.
- Identify the four twisted pairs.
- Identify T-568B termination.
- Explain why the conductors are twisted.
- Test cable continuity.
- Identify open circuits.
- Identify short circuits.
- Identify miswired conductors.
- Understand the difference between continuity testing and cable certification.
- Trace a physical network cable through a room.
- Identify physical security concerns.
- Connect physical-layer problems to network connectivity failures.

### Required Equipment

If hardware is available:

```text
1 × Ethernet patch cable

1 × Ethernet cable tester

Optional:
1 × Ethernet switch
1 × Computer
RJ-45/8P8C connectors
Crimping tool
Cable stripper
````

If a cable tester is not available:

```text
1 × Ethernet cable
A visible network installation
A wall outlet
A patch panel and/or switch if accessible
```

Do not disconnect or modify production network infrastructure without authorization.

### Part A — Inspect the Cable

Begin with a visual inspection.

Do not connect anything yet.

Examine the cable from end to end.

Look for:

- Cable category markings.
    
- Manufacturer markings.
    
- Damage.
    
- Cuts.
    
- Crushed sections.
    
- Excessive bends.
    
- Damaged connectors.
    
- Missing connector clips.
    
- Exposed conductors.
    
- Unusual kinks.
    

Look for markings such as:

```text
CAT5E
CAT6
CAT6A
CAT8
```

The category marking gives you information about the cable's intended electrical performance.

Record what you find.

```text
Cable category:

Cable length:

Connector type:

Visible damage:

Cable markings:
```

### Part B — Identify the Cable Construction

If the cable can be safely opened or you have a cutaway cable available, examine its internal construction.

You should find:

```text
8 individual conductors

↓

4 twisted pairs

↓

Outer protective jacket
```

Identify each pair.

The four pairs are typically associated with the following color families:

```text
Orange
Green
Blue
Brown
```

Each pair contains:

```text
Solid-color conductor

+

White/color-striped conductor
```

For example:

```text
White/Orange + Orange

White/Green + Green

White/Blue + Blue

White/Brown + Brown
```

Do not assume that the physical arrangement inside the cable determines the pin numbering.

Pin numbering depends on how the conductors are terminated at the connector.

### Part C — Identify T-568B

Inspect one connector.

Determine whether it follows the T-568B arrangement.

The T-568B sequence is:

```text
Pin 1 — White/Orange
Pin 2 — Orange
Pin 3 — White/Green
Pin 4 — Blue
Pin 5 — White/Blue
Pin 6 — Green
Pin 7 — White/Brown
Pin 8 — Brown
```

Record the observed order.

```text
Pin 1:
Pin 2:
Pin 3:
Pin 4:
Pin 5:
Pin 6:
Pin 7:
Pin 8:
```

Compare your observation against the standard sequence.

### Part D — Understand the Pairing

Do not only memorize the pin colors.

Identify which pins belong to each twisted pair.

For T-568B:

```text
Orange pair:

Pin 1 — White/Orange
Pin 2 — Orange


Green pair:

Pin 3 — White/Green
Pin 6 — Green


Blue pair:

Pin 4 — Blue
Pin 5 — White/Blue


Brown pair:

Pin 7 — White/Brown
Pin 8 — Brown
```

Notice something important.

The green pair does not occupy adjacent pin numbers.

It occupies:

```text
3 + 6
```

This is one reason that simply looking at pin numbers without understanding the physical pair structure can lead to mistakes.

### Part E — Understand Why Pairing Matters

Ethernet does not merely need eight connected conductors.

It needs the correct conductors to remain physically paired.

The physical pairing affects:

- Differential signaling.
    
- Crosstalk.
    
- Signal integrity.
    
- High-frequency performance.
    

Consider two cables.

Cable A:

```text
Correct conductors

+

Correct physical pairs
```

Cable B:

```text
Correct continuity

+

Incorrect physical pairs
```

Both may appear electrically connected.

But Cable B can have significantly worse transmission characteristics.

This is the idea behind a **split pair**.

The lesson is:

```text
Continuity

≠

Correct transmission performance
```

### Part F — Test Continuity

If you have a cable tester, connect the cable to the tester.

Follow the tester's instructions.

A correctly wired straight-through cable should produce a corresponding sequence:

```text
1 → 1
2 → 2
3 → 3
4 → 4
5 → 5
6 → 6
7 → 7
8 → 8
```

Record the result.

```text
Tester result:

1 →
2 →
3 →
4 →
5 →
6 →
7 →
8 →
```

### Part G — Diagnose a Fault

A cable tester may reveal several types of problems.

#### Open Circuit

Example:

```text
1 → 1
2 → 2
3 → Open
4 → 4
5 → 5
6 → 6
7 → 7
8 → 8
```

The third conductor does not provide a continuous electrical path.

Possible causes include:

- Broken conductor.
    
- Poor termination.
    
- Damaged connector.
    
- Failed crimp.
    

#### Miswire

Example:

```text
1 → 1
2 → 2
3 → 6
4 → 4
5 → 5
6 → 3
7 → 7
8 → 8
```

The conductors are connected, but the pin assignments do not match.

#### Short Circuit

A short occurs when conductors that should remain electrically isolated become connected.

The exact tester output depends on the equipment.

Possible causes include:

- Damaged insulation.
    
- Incorrect termination.
    
- Connector damage.
    
- Physical cable damage.
    

### Part H — Identify a Split Pair

A more advanced cable fault is a split pair.

A basic continuity test may indicate that all eight pins are connected correctly.

However, the physical pair relationships may be wrong.

The cable can therefore pass a simple wiremap test while failing to maintain the expected electrical characteristics.

This is important because Ethernet depends on the physical properties of the twisted pairs, not just electrical continuity.

A professional cable certifier can detect characteristics that a simple continuity tester cannot.

### Part I — Cable Certification

A basic tester generally answers:

```text
Are the conductors connected correctly?
```

A professional certifier asks a much more demanding question:

```text
Does this installed link meet the performance requirements
of its specified cabling category?
```

Certification can involve measurements such as:

```text
Insertion Loss

Return Loss

NEXT

FEXT

Propagation Delay

Delay Skew

Resistance

Length
```

This distinction is important in professional network engineering.

A cable can pass continuity testing while still failing certification.

### Part J — Trace a Cable Path

If you do not have a cable tester, perform a physical cable trace.

Choose a visible Ethernet connection in a room or lab.

Start at an endpoint such as:

```text
Computer
```

or:

```text
Access Point
```

Follow the cable.

Document every physical transition.

For example:

```text
Computer

↓

Patch Cable

↓

Wall Outlet

↓

Horizontal Cable

↓

Patch Panel

↓

Patch Cable

↓

Switch

↓

Switch Port
```

Record what you can identify.

```text
Endpoint:

Wall jack:

Patch panel:

Patch panel port:

Switch:

Switch port:

Cable category:

Approximate cable path:
```

### Part K — Draw the Physical Path

Create a physical topology diagram.

Example:

```text
PC-01
  |
  | Patch Cable
  |
Wall Jack A-12
  |
  | Horizontal Cable
  |
Patch Panel A
  |
  | Patch Cable
  |
Switch-01
  |
Port 18
```

This is different from a logical network diagram.

A logical diagram might show:

```text
PC-01

10.10.10.25
     |
     |
Switch-01
     |
     |
Router-01
```

The logical diagram describes network relationships.

The physical diagram describes where the equipment and cables actually exist.

A network engineer needs both.

### Part L — Identify the Physical Security Boundary

While tracing the cable, examine the physical environment.

Ask:

```text
Can someone access this cable?

Can someone unplug it?

Can someone insert a device?

Is the wall outlet publicly accessible?

Is the patch panel locked?

Is the network cabinet locked?

Are unused switch ports exposed?

Does the cable pass through an unsecured area?

Could someone physically damage the cable?
```

Document anything relevant.

```text
Physical security observations:

1.

2.

3.

4.
```

### Part M — Think Like a Troubleshooter

Imagine a workstation suddenly loses network connectivity.

You arrive at the workstation.

The user says:

> "The Internet isn't working."

Do not immediately assume the problem is DNS.

Do not immediately change the IP configuration.

Do not immediately restart the router.

Start at the physical layer.

A reasonable initial investigation is:

```text
Endpoint powered on?
        ↓
Ethernet cable connected?
        ↓
Connector seated?
        ↓
Cable visibly damaged?
        ↓
Link light present?
        ↓
Switch port active?
        ↓
Correct physical path?
        ↓
Cable test passes?
        ↓
Then investigate higher layers
```

This follows a fundamental troubleshooting principle:

```text
Physical

↓

Link

↓

Network

↓

Transport

↓

Application
```

You should understand the lower layers before assuming the problem exists at a higher layer.

### Part N — Failure Scenario

Imagine this situation:

```text
PC
 |
 | Cat6 cable
 |
Wall Outlet
 |
 | Horizontal cable
 |
Patch Panel
 |
 | Patch cable
 |
Switch
```

The user reports:

```text
"No network connection."
```

You check the PC.

The network interface reports:

```text
Link Down
```

What does this tell you?

It strongly suggests that you should investigate the physical/link connection before spending time troubleshooting:

```text
DNS

Default Gateway

Routing

HTTP

Internet Applications
```

Potential physical causes include:

```text
Damaged cable

Disconnected cable

Bad connector

Bad wall jack

Bad patch-panel connection

Bad switch port

Incorrect termination

Physical cable failure
```

The lesson is not that every "link down" problem is caused by a cable.

The lesson is that the observed symptom should guide you toward the appropriate layer.

### Part O — Failure Scenario: Link Up but Slow

Now consider a different situation.

The workstation has:

```text
Link Up
```

but the connection performs poorly.

You might see:

```text
Low throughput

Packet errors

Interface errors

Link negotiation at an unexpected speed
```

Now the physical layer still deserves investigation.

Possible causes include:

- Damaged cable.
    
- Poor termination.
    
- Excessive cable length.
    
- Crosstalk.
    
- Interference.
    
- Poor-quality cabling.
    
- Incorrect cabling category.
    
- Faulty network interface.
    
- Faulty switch port.
    

This demonstrates an important principle:

```text
Physical-layer problems do not always produce "Link Down."
```

A degraded physical link can remain operational while producing poor performance.

### Part P — Connect Physical Media to Network Performance

The physical layer can influence everything above it.

Consider this chain:

```text
Physical degradation

↓

Signal integrity problems

↓

Frame errors

↓

Retransmissions / recovery

↓

Reduced effective throughput

↓

Poor application performance
```

The user may experience:

```text
"Downloads are slow."
```

The actual root cause may be:

```text
A damaged Ethernet cable.
```

This is why good network troubleshooting requires understanding the complete stack.

### Part Q — Compare the Three Media

Complete the following table from your understanding of the lesson.

|Property|Twisted Pair|Fiber|Coax|
|---|---|---|---|
|Physical signal||||
|Main conductor/media||||
|Typical applications||||
|EMI characteristics||||
|Typical distance||||
|Shared medium possible?||||
|Passive interception possible?||||
|Common security consideration||||

Then add wireless:

|Property|Wireless|
|---|---|
|Physical signal||
|Transmission medium||
|Typical applications||
|Shared medium?||
|Passive interception possibility||
|Main security requirement||

### Part R — Engineering Decision Exercise

You are designing three network connections.

#### Scenario 1

A workstation is located 20 meters from an Ethernet switch inside an office.

Requirements:

```text
Reliable wired connection
Standard enterprise Ethernet
Low cost
```

Which medium would you choose?

Explain why.

#### Scenario 2

Two buildings are separated by several kilometers.

Requirements:

```text
High bandwidth
Long distance
Resistance to electromagnetic interference
Reliable backbone connection
```

Which medium would you choose?

Explain why.

#### Scenario 3

Employees need network access while moving around an office.

Requirements:

```text
Mobility
No physical cable to each client
Multiple users
```

Which medium would you choose?

Explain why.

### Part S — Security Exercise

Consider the following situations.

#### Situation 1

An attacker gains physical access to an exposed copper Ethernet cable.

What security risks exist?

#### Situation 2

An attacker is standing outside a building within range of its Wi-Fi network.

What makes wireless fundamentally different from a physically confined cable?

#### Situation 3

An organization uses fiber for its backbone.

Does that eliminate the need for encryption?

Explain.

### Part T — Technician Checklist

When dealing with physical connectivity problems, use this checklist.

```text
[ ] Endpoint powered on

[ ] Network interface enabled

[ ] Cable connected

[ ] Connector seated correctly

[ ] Cable physically intact

[ ] Correct cable category

[ ] Cable length within specification

[ ] Correct termination

[ ] Link light present

[ ] Switch port operational

[ ] Cable tester result checked

[ ] Physical path documented

[ ] Patch panel checked

[ ] Wall outlet checked

[ ] Physical security concerns considered
```

Do not blindly replace components.

Use evidence.

### Part U — Documentation Exercise

Create a short record for the cable you inspected.

```text
Cable Investigation Record

Cable category:
Cable length:
Cable type:
Connector:
Termination:
T-568A or T-568B:
Continuity result:
Detected faults:
Physical condition:
Physical path:
Connected devices:
Switch port:
Security observations:
```

If you could not determine a field, write:

```text
Unknown
```

Do not guess.

Professional network documentation should distinguish observed facts from assumptions.

### Part V — Questions to Answer Before Moving On

Answer these without looking back at the lesson.

1. What physically carries information through twisted-pair Ethernet?
    
2. Why are Ethernet conductors twisted?
    
3. Why does Ethernet use differential signaling?
    
4. How many conductors are inside a standard twisted-pair Ethernet cable?
    
5. How many twisted pairs are present?
    
6. What is the T-568B pin order?
    
7. Which pins belong to the green pair in T-568B?
    
8. What is the difference between a straight-through and crossover cable?
    
9. Why is Auto-MDI/MDI-X important?
    
10. What is an open circuit?
    
11. What is a short circuit?
    
12. What is a split pair?
    
13. Why can a cable pass continuity testing but still fail performance requirements?
    
14. What is the difference between a cable tester and a cable certifier?
    
15. What physical medium does DOCSIS use for its traditional access network?
    
16. What does it mean for a network medium to be shared?
    
17. Why does shared physical infrastructure not automatically mean that users can read each other's private traffic?
    
18. Why is physical access a security concern?
    
19. Why can wireless signals be detected without physical access to a cable?
    
20. Why should encryption still be used over fiber?
    

### Instructor Demonstration

The instructor should demonstrate the physical concepts rather than only explaining them verbally.

A useful demonstration sequence is:

```text
1. Show an intact Ethernet cable.

2. Show the outer jacket.

3. Show the eight conductors.

4. Separate the four twisted pairs.

5. Explain the purpose of the twisting.

6. Show a T-568B connector.

7. Identify all eight pins.

8. Identify the four physical pairs.

9. Test a correctly terminated cable.

10. Test a deliberately faulty cable if available.

11. Demonstrate an open circuit.

12. Demonstrate a miswire.

13. Explain why continuity does not equal certification.

14. Trace a cable from endpoint to switch.

15. Identify the physical security boundaries.
```

The student should be able to see the relationship between the abstract networking model and the physical infrastructure carrying the communication.

### Final Mental Model

At the beginning of this lesson, it is easy to think of networking as:

```text
Computer ───────── Network ───────── Computer
```

A network engineer needs a much more detailed mental model.

For copper Ethernet:

```text
Application Data
       ↓
Transport Data
       ↓
Network Packet
       ↓
Ethernet Frame
       ↓
Physical Encoding
       ↓
Electrical Signal
       ↓
Twisted Copper Pairs
       ↓
Connector
       ↓
Patch Cable
       ↓
Patch Panel
       ↓
Horizontal Cable
       ↓
Wall Outlet
       ↓
Patch Cable
       ↓
Switch Port
```

For fiber:

```text
Data
 ↓
Electrical Signal
 ↓
Optical Transmitter
 ↓
Light
 ↓
Fiber Core
 ↓
Optical Receiver
 ↓
Electrical Signal
```

For wireless:

```text
Data
 ↓
Radio Transmitter
 ↓
Electromagnetic Wave
 ↓
Free Space
 ↓
Radio Receiver
 ↓
Data
```

For coaxial access networks:

```text
Data
 ↓
Electrical/RF Signal
 ↓
Coaxial Cable
 ↓
Shared Access Network
 ↓
Provider Infrastructure
```

The networking stack ultimately depends on physical reality.

Packets cannot travel without signals.

Signals cannot travel without a medium.

The medium has limits.

Those limits influence bandwidth, distance, reliability, architecture, troubleshooting, and security.

### Lesson 2.1 Completion Criteria

You are ready to move forward when you can explain, without memorization alone:

```text
Why copper uses twisted pairs.

Why fiber uses a core and cladding.

Why single-mode travels farther than multimode.

Why WDM increases fiber capacity.

Why wireless is a shared medium.

Why wireless requires encryption.

Why coaxial networks can be shared.

Why Ethernet copper has a 100-meter channel limit.

Why T-568B has a specific pin order.

Why continuity does not prove cable performance.

Why physical security is part of network security.
```

The central lesson is simple:

```text
Networking is not only software.

The bits have to physically travel somewhere.
```

Everything built on top of networking protocols ultimately depends on the physical medium carrying the signal.