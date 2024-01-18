# SDN

### Key Features
1. **Network Topology Awareness**: The controller requires knowledge of all host addresses in the network. Lack of this information leads to packet drops.
2. **Static Network Support**: The network to be managed should not involve adding or removing switches.
3. **Traffic Management**: Manages TCP/UDP traffic, primarily focusing on routing new incoming packets to switches.
4. **Path Search**: Implements a lowest-cost path search for routing packets.
5. **Flow Control**: Blocks flows on links with excessive active flows by setting a limit. Links between switches and hosts are exempt from this limitation.
6. **Rule Management**: Inactive flows are deleted from links upon rule timeout expiry, thus freeing up the link in the virtual graph.

### Primary Objective
- Generates TCP/UDP connections between client and server, reaching the flow limit on a link, which is then blocked. New flows are routed via alternative paths.
- Connections between switches and hosts are excluded from these operations.

### Secondary Objective
- Once flows on the primary path complete, routing rules are removed post-timeout, reactivating the link.
- Demonstrates link reactivation by generating new TCP/UDP flow.

### Automated Testing
- Initial `pingall` for MAC address discovery and ARP table population.
- Creation of two servers (UDP h2 and TCP h3) and establishment of three connections:
  - h5 to h2 (UDP, 100 seconds)
  - h6 to h3 (TCP, 100 seconds, with ACK response flow)
  - h4 to h2 (UDP, 100 seconds)
- Observing the active flow limit on link s2-s3 and routing behavior.
- A sleep action for 20 seconds followed by a new UDP stream from h4 to h2, demonstrating reactivation of the s2-s3 link.
- Limited bandwidth for clearer InMon mininet dashboard (Sflow) graphs.
