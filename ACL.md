# ACCESS CONTROL LISTS

## What is an ACL (Access Control List) in AWS?
An Access Control List (ACL) in AWS is a security feature that acts as a firewall for controlling network traffic at the subnet level within a Virtual Private Cloud (VPC).

## How ACLs Work
- Network ACLs operate at the **subnet level**, applying rules to all traffic entering or leaving a subnet.
- They use **stateless rules**, meaning both incoming and outgoing traffic must be explicitly allowed.
- Each rule defines whether to allow or deny traffic based on:
  - **Protocol** (TCP, UDP, ICMP, etc.)
  - **Port Range** (80 for HTTP, 443 for HTTPS, etc.)
  - **Source IP** (where the traffic comes from)
  - **Destination IP** (where the traffic is going)
  - **Rule Number** (determines rule priority, lower numbers have higher priority)

## Key Features of ACLs
✅ **Subnet-Level Security:** Unlike Security Groups (which control instance-level access), ACLs manage entire subnets.
✅ **Stateless:** Responses to requests must be explicitly allowed (separate inbound and outbound rules).
✅ **Numbered Rules:** AWS evaluates rules in order, starting from the lowest rule number.
✅ **Default ACL Behavior:**
   - The default VPC ACL allows all inbound and outbound traffic.
   - Newly created ACLs deny all traffic until rules are added.

## Example Use Case
Let’s say you have an AWS VPC with two subnets:
- **Public Subnet** (for web servers)
- **Private Subnet** (for databases)


### Example - ACL Rules for Public Subnet

| Rule #  | Type   | Protocol | Port Range | Source            | Allow/Deny |
|---------|--------|----------|------------|-------------------|------------|
| 100     | HTTP   | TCP      | 80         | 0.0.0.0/0 (anyone) | ALLOW      |
| 110     | HTTPS  | TCP      | 443        | 0.0.0.0/0 (anyone) | ALLOW      |
| 120     | SSH    | TCP      | 22         | 192.168.1.0/24 (only internal access) | ALLOW |
| Default | -      | -        | -          | -                 | DENY       |




### 🔹 This setup allows:
- Public access to web servers (**HTTP & HTTPS**).
- **SSH access** from a specific internal range.
- **Blocks all other incoming traffic**.

## Difference Between ACLs and Security Groups
| Feature            | Network ACL (NACL) | Security Group |
|-------------------|------------------|---------------|
| **Level of control** | Subnet-level | Instance-level |
| **Rules type** | Allow & Deny | Allow only |
| **Stateful?** | No (stateless) | Yes (stateful) |
| **Evaluation Order** | Processed in numerical order | All rules are checked |
| **Use Case** | Subnet-wide traffic filtering | Instance-specific security |

## When to Use ACLs in AWS?
✔️ To create a **first layer of security** at the subnet level.
✔️ When you need **broad traffic filtering** for an entire subnet.
✔️ To **block unwanted IPs** at the network level (e.g., block traffic from specific countries).
✔️ To add an **extra security layer** along with Security Groups.

## Conclusion
📌 A **Network ACL (NACL)** is a **subnet-level security mechanism** that filters both inbound and outbound traffic, helping control who can enter and leave a subnet.
📌 **Security Groups and NACLs work together** to ensure a secure AWS environment.

