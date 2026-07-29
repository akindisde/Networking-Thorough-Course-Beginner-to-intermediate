**Circuit switching** is a network implementation where a dedicated, continuous physical or logical channel is reserved end-to-end between two nodes before communication begins. This channel remains exclusively allocated to the session until the call or transfer terminates, regardless of whether data is actively being transmitted.

## 1. The Three Phases of a Circuit-Switched Connection

Unlike connectionless packet routing, circuit switching requires a strict lifecycle for every session:

**1.1. Circuit Establishment :**Control Signalling & Resource Reservation.

A dedicated path is requested through intermediate switches. Signals reserve specific time slots, frequencies, or physical channels along the entire route. If any link is unavailable, the connection request is blocked.

**2.2. Data Transfer :**Exclusive Channel Payload Transmission.

Data streams continuously across the established path. Because resources are dedicated, packets do not require intermediate routing headers, leading to constant transmission speed and zero queuing delay.

**3.3. Circuit Teardown :**Resource Release & Channel Unbinding.

When the connection ends, a tear-down signal propagates through the switches to release the reserved bandwidth, time slots, and physical routes back to the shared pool.

## 2. Channel Multiplexing Techniques

To avoid assigning an entire physical wire to a single connection, circuit-switched networks split transmission media into multiple independent channels using multiplexing:

- **Frequency Division Multiplexing (FDM):** The total available bandwidth is divided into distinct frequency channels (used in legacy telephone systems and radio/cable TV).
    
- **Time Division Multiplexing (TDM):** The full frequency spectrum is assigned to a single connection for a brief, repeating time slot (used in T1/E1 and ISDN lines).
    
- **Wavelength Division Multiplexing (WDM):** Optical fiber signals are split into separate light wavelengths (colors) to carry independent data streams simultaneously over optical transport networks.
    

## 3. Circuit Switching vs. Packet Switching

|**Metric / Attribute**|**Circuit Switching**|**Packet Switching**|
|---|---|---|
|**Path Allocation**|Dedicated end-to-end path established before transfer|Dynamic routing on a per-packet basis|
|**Bandwidth Utilization**|Static (Unused capacity is wasted during silences)|High (Statistical multiplexing shares bandwidth)|
|**Latency & Jitter**|Fixed / Deterministic (No queuing delays)|Variable (Subject to buffer queuing and jitter)|
|**Header Overhead**|Minimal (Routing parameters used during setup only)|High (Every individual packet carries IP/MAC headers)|
|**Fault Tolerance**|Low (If an intermediate node fails, the circuit drops)|High (Packets automatically reroute around dead nodes)|
|**Primary Applications**|Legacy PSTN, GSM voice calls, SONET/SDH, OTN|Ethernet, IPv4/IPv6 Internet, MPLS networks|

## 4. Key Advantages and Disadvantages

> **Trade-off:** Circuit switching trades resource efficiency for predictable performance and guaranteed Quality of Service (QoS).

- **Advantages:**
    
    - **Guaranteed Bandwidth:** Perfect for real-time voice and synchronous communications.
        
    - **Zero Packet Reordering:** Data arrives in the exact sequence it was transmitted.
        
    - **No Network Congestion (Mid-Session):** Congestion can only happen during setup; once established, throughput is guaranteed.
        
- **Disadvantages:**
    
    - **Inefficient Resource Use:** Bandwidth remains locked even when no data is actively flowing (e.g., pauses in speech).
        
    - **High Setup Overhead:** Establishing a circuit takes time before any payload data can be transferred.
        
    - **Call Blocking:** New connections are rejected outright if all intermediate channels are occupied.