>Definition **CIDR (Classless Inter-Domain Routing)** is the modern method used to assign and allocate IP addresses, replacing the rigid Class A, B, and C system.

## 1. The Core Idea: Flexible Boundaries

CIDR eliminates the fixed boundaries of Class A, B, and C. Instead of the network portion being locked to the first 8, 16, or 24 bits, CIDR allows the network administrator to **choose exactly where the boundary is**.

### Fixed vs. Flexible

- **Classful:** The boundary is **fixed** by the first number (e.g., if it starts with `192`, the boundary is always after the third octet).
    
- **CIDR:** The boundary is **flexible** and can be set anywhere to custom-fit the size of the network needed.
    

---

## 2. The Notation: The Slash ( / )

The key feature of CIDR is the slash notation (often called the **CIDR block**).

- **Format:** `IP Address / Prefix Length`
    
- **Example:** `192.168.10.0 / 24`
    

### The Prefix Length

The number after the slash is the most important part. It tells you exactly how many bits (from the left) are used for the **Network ID**.

- A `/24` means the first **24 bits** are the Network ID.
    
- A `/26` means the first **26 bits** are the Network ID.