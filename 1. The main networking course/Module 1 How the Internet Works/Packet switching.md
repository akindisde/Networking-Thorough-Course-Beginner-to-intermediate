**Packet switching** is a digital networking method that breaks messages into small, manageable blocks of data called **packets**. Each packet is individually addressed, buffered, and routed independently across a shared network using intermediate switches or routers, before being reassembled into the original message at the destination.

## 1. How Packet Switching Works

Instead of reserving a dedicated end-to-end circuit, packet switching handles data in discrete, encapsulated units through a store-and-forward mechanism:

**1.1. Segmentation & Encapsulation :**Source Node.

The sending host breaks large data streams into smaller payloads and appends Layer 3 and Layer 4 headers containing source/destination IP addresses, port numbers, sequence identifiers, and error-checking checksums.

**2.2. Store-and-Forward Routing :**Intermediate Routers.

Each router along the path receives a packet, temporarily stores it in an ingress buffer memory, verifies the header checksum, consults its routing table, and forwards the packet toward the optimal egress interface.

**3.3. Dynamic Pathing & Multiplexing :**Transit Network.

Packets belonging to the same application stream can travel across different physical links depending on real-time topology changes and network congestion, dynamically sharing infrastructure via statistical multiplexing.

**4.4. Reassembly & Decapsulation :**Destination Node.

The destination host receives the individual packets, utilizes sequence numbers to reorder packets delivered out of sequence, strips off the network headers, and passes the reconstructed payload to the application layer.

## 2. Two Primary Operational Modes

Packet-switched networks implement data delivery using one of two core architectural models:

### A. Connectionless (Datagram) Packet Switching

- **Mechanism:** Every packet is treated as an independent unit (a datagram) containing complete destination addressing metadata.
    
- **Path Selection:** Routers evaluate each packet independently; consecutive packets from the same communication session can follow entirely different physical paths.
    
- **Characteristics:** Eliminates connection setup delay and provides high resilience against router failures (traffic dynamically reroutes around dead links), but packets can arrive out of order.
    
- **Primary Example:** The IPv4/IPv6 Internet protocol suite.
    

### B. Connection-Oriented (Virtual Circuit) Packet Switching

- **Mechanism:** A logical path known as a Virtual Circuit (VC) is established between the endpoints before data transfer commences.
    
- **Path Selection:** All packets in a session follow the pre-established logical path using short Virtual Circuit Identifiers (VCIs) or labels rather than evaluating full Layer 3 destination addresses at every hop.
    
- **Characteristics:** Ensures sequential packet delivery with reduced lookup overhead per hop, but requires explicit signaling setup and teardown phases.
    
- **Primary Examples:** MPLS (Multiprotocol Label Switching), Frame Relay, and ATM (Asynchronous Transfer Mode).
    

## 3. The Four Sources of Packet Latency

The total latency ($D_{\text{total}}$) incurred by a packet passing through a network hop is determined by four additive delay factors:

$$D_{\text{total}} = D_{\text{proc}} + D_{\text{queue}} + D_{\text{trans}} + D_{\text{prop}}$$

- **Processing Delay ($D_{\text{proc}}$):** Time required by the router CPU to inspect packet headers, verify bit integrity, and determine the egress interface.
    
- **Queuing Delay ($D_{\text{queue}}$):** Time the packet spends waiting in memory buffers before being transmitted onto the outbound link (fluctuates with network congestion).
    
- **Transmission Delay ($D_{\text{trans}}$):** Time needed to push all bits of a packet onto the physical medium, calculated as $D_{\text{trans}} = \frac{L}{R}$ (where $L$ is packet size in bits and $R$ is link rate in bits per second).
    
- **Propagation Delay ($D_{\text{prop}}$):** Time required for a signal bit to physically travel across the physical medium distance, calculated as $D_{\text{prop}} = \frac{d}{s}$ (where $d$ is distance and $s$ is propagation velocity through copper, fiber, or air).
    

## 4. Advantages and Disadvantages

> **Efficiency Advantage:** Statistical multiplexing allows packet switching to saturate link capacity on demand, making it vastly more resource-efficient than circuit switching for bursty data traffic.

- **Advantages:**
    
    - **Bandwidth Efficiency:** Shared links are utilized continuously; idle users release capacity instantly for active transmissions.
        
    - **High Fault Tolerance:** Adaptive routing protocols allow surviving routers to route around dead links or hardware failures mid-stream.
        
    - **Flexible Data Rates:** Nodes with vastly different connection speeds can communicate seamlessly through intermediate switch buffering.
        
- **Disadvantages:**
    
    - **Variable Latency & Jitter:** Unpredictable queuing delays during congestion spikes complicate real-time traffic (requiring Quality of Service mechanisms like DiffServ).
        
    - **Protocol Overhead:** Every packet carries header bytes, reducing the net payload throughput ratio compared to raw streams.
        
    - **Buffer Overflow Loss:** When ingress traffic exceeds egress link capacity for sustained periods, router queues overflow and drop packets.