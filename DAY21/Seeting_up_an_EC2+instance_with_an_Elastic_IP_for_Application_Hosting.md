#  Elastic IP in AWS

##  Overview

An **Elastic IP (EIP)** is a static public IPv4 address provided by **:contentReference[oaicite:0]{index=0} (AWS)**.  

It is designed for dynamic cloud computing and allows you to maintain a fixed public IP address for your EC2 instance.

Unlike automatically assigned public IP addresses, an Elastic IP does **not change** when you stop and start your instance.

---

##  Key Features of Elastic IP

### 1. Static Public IP Address
- Remains the same even after stopping and starting the EC2 instance.
- Provides consistency for applications that require a fixed public endpoint.
- Useful for production environments and DNS configurations.

---

### 2. Behavior When Instance Is Stopped
- The Elastic IP **remains associated** with the instance.
- When the instance starts again, it uses the **same Elastic IP address**.
- The IP does not change.

---

### 3. Behavior When Instance Is Terminated
- If the EC2 instance is terminated:
  - The Elastic IP does **not automatically disappear**.
  - It becomes **unassociated**.
  - It remains **allocated to your AWS account**.
- You must manually release it if you no longer need it.

---

##  Pricing Considerations

AWS may charge for:
- Elastic IPs that are allocated but **not associated** with a running instance.
- Unused Elastic IP addresses.

To avoid unnecessary costs:
- Release Elastic IPs that are no longer needed.

---

##  Why Use Elastic IP?

Elastic IPs are useful for:

- Hosting production applications.
- Maintaining a fixed IP for DNS records.
- Disaster recovery (remapping the IP to another instance quickly).
- High availability setups.

---



---


