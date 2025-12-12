


 Definition **Classful IP addressing** was an early method (1981–1993) for assigning IP addresses and dividing the IPv4 address space. It was later replaced by **CIDR (Classless Inter-Domain Routing)**, which uses prefixes instead of fixed classes.

In this method, IP addresses were divided into **five classes (A, B, C, D, E)**, identified by their first few bits and designed for specific purposes.

## Classes Breakdown

### 1. Unicast Communication (Classes A, B, C)

Used for standard network communication across different scales:

- **Class A:** Large networks.
    
- **Class B:** Medium networks.
    
- **Class C:** Small networks.
    

### 2. Specialized Use (Classes D, E)

- **Class D:** Reserved for **multicasting**.
    
- **Class E:** Reserved for **experimental use**.

---

## (i) Class A

**Usage:** Assigned to networks with a **large number of hosts**.

### Structure

- **Network ID:** 8 bits long.
    
- **Host ID:** 24 bits long.
    
- **Bit Configuration:**
    
    - The higher-order bit in a Class A address is **always set to `0`**.
        
    - The remaining **7 bits** in the first octet are used to determine the Network ID.
        
    - The 24-bit Host ID is used to determine the host in the network.
        

### Specifications

- **Total Hosts:** Class A has $2^{24} - 2$ host IDs.
    
- **Default Subnet Mask:** `255.0.0.0`.
    

---

## (ii) Class B

**Usage:** IP addresses belonging to Class B are assigned to networks that range from **medium to large-sized networks**.

### Structure

- **Network ID:** 16 bits long.
    
- **Host ID:** 16 bits long.
    
- **Bit Configuration:**
    
    - The higher-order bits of the first octet are **always set to `10`**.
        
    - The remaining **14 bits** are used to determine the network.
        

### Specifications

- **Range:** `128 - 191`.
    
- **Default Subnet Mask:** `255.255.0.0`.
    
- **Total Networks:** $2^{14} = 16,384$ network addresses.
    
- **Total Hosts:** $2^{16} - 2 = 65,534$ host addresses.
    

---

## (iii) Class C

**Usage:** IP addresses belonging to Class C are assigned to **small-sized networks**.

### Structure

- **Network ID:** 24 bits long.
    
- **Host ID:** 8 bits long.
    
- **Bit Configuration:**
    
    - The three higher-order bits are **always set to `110`**.
        
    - The remaining **21 bits** are used to determine the Network ID.
        

### Specifications

- **Range:** `192 - 223`.
    
- **Default Subnet Mask:** `255.255.255.0`.
    
- **Total Networks:** $2^{21} = 2,097,152$ network addresses.
    
- **Total Hosts:** $2^8 - 2 = 254$ host addresses.
    

---

## (iv) Class D

**Usage:** IP addresses belonging to Class D are **reserved for multicasting**.

### Specifications

- **Range:** `224 - 239`.
    
- **Bit Configuration:** The higher-order bits of the first octet are **always set to `1110`**.
    
- **Subnet:** Does not possess any subnet.
    

---

## (v) Class E

**Usage:** IP addresses belonging to Class E are **reserved for experimental and research purposes**.

### Specifications

- **Range:** `240.0.0.0 - 255.255.255.255`.
    
- **Bit Configuration:** The higher-order bits of the first octet are **always set to `1111`**.