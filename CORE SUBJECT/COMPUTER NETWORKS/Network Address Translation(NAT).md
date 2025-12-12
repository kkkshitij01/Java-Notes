
Here is the structured explanation of **NAT (Network Address Translation)**. This format is designed for you to memorize and reproduce in an exam or interview.

---

### **1. Definition**

**Network Address Translation (NAT)** is a method used by routers to remap one IP address space into another by modifying network address information in the IP header of packets while they are in transit.

- **Simple Terms:** It is the process of changing the **Private IP address** (used inside your local network) to a **Public IP address** (used on the internet) so that devices can communicate with the outside world.
    

---

### **2. Why do we need NAT? (The Purpose)**

There are two main reasons:

1. **Conservation of IPv4 Addresses:** There are not enough Public IPv4 addresses for every device in the world (phone, laptop, watch, fridge). NAT allows an entire building (hundreds of devices) to share just **one** Public IP.
    
2. **Security (Hiding Internal Structure):** It hides the internal private IP addresses from the outside world. Hackers on the internet see only the router’s Public IP, not the specific IP of your laptop.