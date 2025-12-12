# Address Resolution Protocol (ARP)

>  Definition The **Address Resolution Protocol (ARP)** is a vital network protocol that translates logical **IP addresses** (like IPv4) into physical **MAC (Media Access Control) addresses** on a local network (LAN), allowing devices to find and communicate with each other.

---



# How ARP Works (The Process)

### 1. Data Arrival

A device wants to send data to another device on the same LAN, knowing its **IP address** but _not_ its **MAC address**.

### 2. ARP Request (Broadcast)

The source device sends an **ARP Request** as a **broadcast message** to all devices on the network, asking:

"Who has IP address `[Target IP]`? Tell me your MAC address."

### 3. Device Response

All devices get the request, but **only the device with the matching IP** responds with its specific MAC address in an **ARP Reply (unicast)**.

### 4. Cache & Forward

The original device receives the MAC address, stores it in its **ARP cache**, and sends the data directly to the target's MAC address.