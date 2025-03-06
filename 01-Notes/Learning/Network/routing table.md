### **1. Longest Prefix Match**

- The **longest prefix match** rule states that when a router looks up a destination IP address in its routing table, it selects the route with the **most specific network prefix** (i.e., the longest subnet mask).
- This ensures that packets are forwarded along the most precise and efficient path.

---

### **2. Real-World Example**

#### **Scenario**

Imagine you have a router with the following routing table:

|Destination Network|Subnet Mask|Next Hop|Outgoing Interface|
|---|---|---|---|
|192.168.1.0|255.255.255.0|Directly Connected|Ethernet0|
|192.168.1.0|255.255.255.128|192.168.1.1|Ethernet1|
|192.168.1.128|255.255.255.192|192.168.1.2|Ethernet2|
|0.0.0.0|0.0.0.0|192.168.1.254|Ethernet3|

- **Destination Networks** :
    - `192.168.1.0/24`: A larger subnet covering all IPs from `192.168.1.0` to `192.168.1.255`.
    - `192.168.1.0/25`: A smaller subnet covering IPs from `192.168.1.0` to `192.168.1.127`.
    - `192.168.1.128/26`: An even smaller subnet covering IPs from `192.168.1.128` to `192.168.1.191`.
    - `0.0.0.0/0`: The default route, which matches any IP address not covered by other routes.

#### **Packet to Route**

A packet arrives at the router with the **destination IP address** : `192.168.1.150`.

---

### **3. Step-by-Step Process**

#### **Step 1: Router Examines the Destination IP**

The router examines the destination IP (`192.168.1.150`) and compares it against all entries in its routing table.

#### **Step 2: Compare Against Each Route**

The router performs a bitwise AND operation between the destination IP and each subnet mask to determine if the IP falls within the network range.

1. **Route 1: `192.168.1.0/24`**
    
    - Subnet Mask: `255.255.255.0`
    - Network Range: `192.168.1.0` to `192.168.1.255`
    - Result: `192.168.1.150` matches this route.
    - Prefix Length: `/24` (24 bits).
2. **Route 2: `192.168.1.0/25`**
    
    - Subnet Mask: `255.255.255.128`
    - Network Range: `192.168.1.0` to `192.168.1.127`
    - Result: `192.168.1.150` does **not** match this route.
3. **Route 3: `192.168.1.128/26`**
    
    - Subnet Mask: `255.255.255.192`
    - Network Range: `192.168.1.128` to `192.168.1.191`
    - Result: `192.168.1.150` matches this route.
    - Prefix Length: `/26` (26 bits).
4. **Route 4: `0.0.0.0/0`**
    
    - Subnet Mask: `0.0.0.0`
    - Network Range: Any IP address.
    - Result: `192.168.1.150` matches this route.
    - Prefix Length: `/0` (0 bits).

#### **Step 3: Apply Longest Prefix Match**

- The router selects the route with the **longest prefix length** (most specific match).
- Matches:
    - `192.168.1.0/24` (`/24`)
    - `192.168.1.128/26` (`/26`)
    - `0.0.0.0/0` (`/0`)
- The **longest prefix match** is `192.168.1.128/26` (`/26`).

#### **Step 4: Forward the Packet**

- The router forwards the packet to the next hop specified in the selected route:
    - Next Hop: `192.168.1.2`
    - Outgoing Interface: `Ethernet2`

---

### **4. Why Longest Prefix Match Matters**

- Without the longest prefix match rule, the router might choose a less specific route (e.g., `192.168.1.0/24`), which could lead to inefficient or incorrect routing.
- The longest prefix match ensures that the router always selects the most precise route available.

---

### **5. Another Example**

Let’s consider another packet with the destination IP: `192.168.1.50`.

1. **Route 1: `192.168.1.0/24`**
    
    - Matches (`/24`).
2. **Route 2: `192.168.1.0/25`**
    
    - Matches (`/25`).
3. **Route 3: `192.168.1.128/26`**
    
    - Does not match.
4. **Route 4: `0.0.0.0/0`**
    
    - Matches (`/0`).

- The **longest prefix match** is `192.168.1.0/25` (`/25`).
- The router forwards the packet to the next hop: `192.168.1.1` via `Ethernet1`.

---

### **6. Final Answer**

In a real-world scenario, the router uses the **longest prefix match** rule to select the most specific route for a packet. For example:

- A packet destined for `192.168.1.150` matches both `192.168.1.0/24` and `192.168.1.128/26`, but the router chooses `192.168.1.128/26` because it has the longest prefix (`/26`).
- Similarly, a packet destined for `192.168.1.50` matches both `192.168.1.0/24` and `192.168.1.0/25`, but the router chooses `192.168.1.0/25` because it is more specific.

This ensures efficient and accurate routing of packets across networks.