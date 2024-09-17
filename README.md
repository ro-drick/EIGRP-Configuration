# Packet Tracer Lab: EIGRP Configuration and Unequal-Cost Load Balancing

This lab focuses on configuring Enhanced Interior Gateway Routing Protocol (EIGRP) for a network of routers. You will also configure unequal-cost load balancing and set appropriate passive interfaces.

### **Objectives:**
1. Set up hostnames, IP addresses, and enable interfaces on all routers.
2. Configure loopback interfaces on each router.
3. Enable EIGRP with appropriate settings, including passive interfaces and disabling auto-summary.
4. Set up unequal-cost load balancing on **R1** for traffic directed to the 192.168.4.0/24 network.

---
<img src="https://github.com/ro-drick/EIGRP-Configuration/blob/main/rip-eigrp.PNG">
## **Step-by-Step Instructions**

### 1. Configure Hostnames and IP Addresses on Each Device

- Assign the appropriate hostnames to each router.
  
#### On **R1**:
```bash
R1# conf t
R1(config)# hostname R1
R1(config)# interface Gig0/0
R1(config-if)# ip address 10.0.12.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface FastEthernet1/0
R1(config-if)# ip address 10.0.13.1 255.255.255.252
R1(config-if)# no shutdown
R1(config-if)# exit
```

#### On **R2**:
```bash
R2# conf t
R2(config)# hostname R2
R2(config)# interface Gig0/0
R2(config-if)# ip address 10.0.12.2 255.255.255.252
R2(config-if)# no shutdown
R2(config-if)# exit
R2(config)# interface FastEthernet1/0
R2(config-if)# ip address 10.0.24.1 255.255.255.252
R2(config-if)# no shutdown
R2(config-if)# exit
```

#### On **R3**:
```bash
R3# conf t
R3(config)# hostname R3
R3(config)# interface FastEthernet1/0
R3(config-if)# ip address 10.0.13.2 255.255.255.252
R3(config-if)# no shutdown
R3(config-if)# exit
R3(config)# interface FastEthernet2/0
R3(config-if)# ip address 10.0.34.1 255.255.255.252
R3(config-if)# no shutdown
R3(config-if)# exit
```

#### On **R4**:
```bash
R4# conf t
R4(config)# hostname R4
R4(config)# interface FastEthernet2/0
R4(config-if)# ip address 10.0.34.2 255.255.255.252
R4(config-if)# no shutdown
R4(config-if)# exit
R4(config)# interface FastEthernet1/0
R4(config-if)# ip address 10.0.24.2 255.255.255.252
R4(config-if)# no shutdown
R4(config-if)# exit
R4(config)# interface Gig0/0
R4(config-if)# ip address 192.168.4.254 255.255.255.0
R4(config-if)# no shutdown
R4(config-if)# exit
```

---

### 2. Configure Loopback Interfaces on Each Router

#### On **R1**:
```bash
R1# conf t
R1(config)# interface loopback 0
R1(config-if)# ip address 1.1.1.1 255.255.255.255
R1(config-if)# exit
```

#### On **R2**:
```bash
R2# conf t
R2(config)# interface loopback 0
R2(config-if)# ip address 2.2.2.2 255.255.255.255
R2(config-if)# exit
```

#### On **R3**:
```bash
R3# conf t
R3(config)# interface loopback 0
R3(config-if)# ip address 3.3.3.3 255.255.255.255
R3(config-if)# exit
```

#### On **R4**:
```bash
R4# conf t
R4(config)# interface loopback 0
R4(config-if)# ip address 4.4.4.4 255.255.255.255
R4(config-if)# exit
```

---

### 3. Configure EIGRP on Each Router

- Configure **EIGRP** with Autonomous System (AS) 100 and disable auto-summary. Make the loopback interfaces passive.

#### On **R1**:
```bash
R1# conf t
R1(config)# router eigrp 100
R1(config-router)# no auto-summary
R1(config-router)# network 10.0.12.0 0.0.0.3
R1(config-router)# network 10.0.13.0 0.0.0.3
R1(config-router)# network 1.1.1.1 0.0.0.0
R1(config-router)# passive-interface loopback 0
R1(config-router)# exit
```

#### On **R2**:
```bash
R2# conf t
R2(config)# router eigrp 100
R2(config-router)# no auto-summary
R2(config-router)# network 10.0.12.0 0.0.0.3
R2(config-router)# network 10.0.24.0 0.0.0.3
R2(config-router)# network 2.2.2.2 0.0.0.0
R2(config-router)# passive-interface loopback 0
R2(config-router)# exit
```

#### On **R3**:
```bash
R3# conf t
R3(config)# router eigrp 100
R3(config-router)# no auto-summary
R3(config-router)# network 10.0.13.0 0.0.0.3
R3(config-router)# network 10.0.34.0 0.0.0.3
R3(config-router)# network 3.3.3.3 0.0.0.0
R3(config-router)# passive-interface loopback 0
R3(config-router)# exit
```

#### On **R4**:
```bash
R4# conf t
R4(config)# router eigrp 100
R4(config-router)# no auto-summary
R4(config-router)# network 10.0.24.0 0.0.0.3
R4(config-router)# network 10.0.34.0 0.0.0.3
R4(config-router)# network 192.168.4.0 0.0.0.255
R4(config-router)# network 4.4.4.4 0.0.0.0
R4(config-router)# passive-interface loopback 0
R4(config-router)# exit
```

---

### 4. Configure Unequal-Cost Load Balancing on R1

To configure **unequal-cost load balancing** on **R1** when sending traffic to the 192.168.4.0/24 network, use the following steps.

#### On **R1**:
1. Set the **variance** command in EIGRP to allow for unequal-cost paths:
    ```bash
    R1# conf t
    R1(config)# router eigrp 100
    R1(config-router)# variance 2
    R1(config-router)# exit
    ```

2. Verify the unequal-cost load balancing by checking the EIGRP routing table:
    ```bash
    R1# show ip route eigrp
    ```

---

## **Verification Steps:**

1. **Ping the loopback addresses:** Ensure each router can ping the loopback interfaces of all other routers:
    ```bash
    R1# ping 2.2.2.2
    R1# ping 3.3.3.3
    R1# ping 4.4.4.4
    ```

2. **Test PC connectivity:** Verify that **PC1** can ping **192.168.4.254** and beyond:
    ```bash
    PC1> ping 192.168.4.254
    ```

3. **Check EIGRP neighbors:** Ensure all routers have established neighbor relationships:
    ```bash
    R1# show ip eigrp neighbors
    ```

4. **Check routing tables:** Verify that unequal-cost load balancing is in effect:
    ```bash
    R1# show ip route
    ```

---

## **Conclusion**

In this lab, I configured IP addresses, loopback interfaces, and EIGRP routing on four routers. Additionally, I applied unequal-cost load balancing on **R1** to optimize traffic distribution. The lab reinforced key concepts such as EIGRP configuration, passive interfaces, and routing optimization using variance for load balancing.

## Acknowledgements


Special thanks to **Jeremy's IT Lab** for providing valuable resources and tutorials that greatly contributed to the completion of this exercise. His in-depth explanations and practical demonstrations have been instrumental in enhancing my understanding of Cisco networking concepts and the effective use of Packet Tracer.

For more information and additional resources, visit [Jeremy's IT Lab](https://jeremysitlab.com/) and check out his YouTube for the full course, [Jeremy's IT Lab Free CCNA 200-301 | Complete Course](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ)
