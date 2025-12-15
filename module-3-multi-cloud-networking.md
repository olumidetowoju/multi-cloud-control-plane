# M3.0 — Networking Foundations (Zero Trust)

**Module 3 — Networking | StageCloud Control Plane GURU Mode — Mental Models Before Mechanics**

---

## Why Networking Comes After Identity

In **Module 2**, we answered **WHO** is requesting access.

In **Module 3**, we answer **WHERE** that access is allowed to go.

> Identity without networking is permission without boundaries. Networking without identity is walls without guards.

Zero Trust networking exists to **contain blast radius**, not to replace identity.

---

### 🌊 The “Third Cloud” Analogy (Networking)

**The GURU speaks:**

> “Identity is the passport at the city gate. But a passport does not stop a thief from walking into every building.
>
> Networking is the city design itself: streets that dead-end, neighborhoods separated by walls, checkpoints between districts, and tunnels that only trusted citizens can use.
>
> Zero Trust networking is not about faster roads. It is about fewer places to go.”

**In this analogy:**
- **VPC / VNet** = the city
- **Subnets** = neighborhoods
- **Route tables** = road maps
- **Security Groups / NSGs** = checkpoints
- **Private Endpoints** = guarded tunnels

---

## Core Zero Trust Networking Principles

### 1. Assume Breach
Design every network as if an attacker is already inside.

**Implication:**
- Flat networks are forbidden
- East–west traffic is untrusted
- Internal traffic is inspected and restricted

### 2. Deny by Default
No packet should move unless explicitly allowed.

**Implication:**
- No `0.0.0.0/0` inbound rules
- No wide-open east–west traffic
- Every allow rule must have a reason

### 3. Least-Privilege Routing
Routes are permissions.  
If traffic can reach a destination, it will eventually be abused.

**Implication:**
- Route tables are scoped per subnet
- Management traffic is isolated
- Data tiers are unreachable from public zones

### 4. Egress Is More Dangerous Than Ingress
Most breaches succeed not by entry, but by exfiltration.

**Implication:**
- Control outbound traffic
- Centralize internet egress
- Prefer private endpoints over public APIs

---

## AWS Networking Mental Model (Pre-Lab)

**AWS Primitives**
- VPC — security boundary
- Subnets — trust zones
- Security Groups — stateful, identity-aware firewalls
- NACLs — stateless guardrails
- VPC Endpoints — private service access

**Zero Trust Interpretation**
- Security Groups = identity gates
- NACLs = blast-radius brakes
- VPC Endpoints = internet bypass

---

## Azure Networking Mental Model (Pre-Lab)

**Azure Primitives**
- VNet — isolation boundary
- Subnets — trust tiers
- NSGs — stateless enforcement
- UDRs — traffic steering
- Private Endpoints — private service access

**Zero Trust Interpretation**
- NSGs = policy enforcement plane
- UDRs = intentional traffic paths
- Private Link = zero-public-exposure

---

## East-West Traffic: The Silent Killer

Most security teams focus on **north–south** traffic. Attackers love **east–west**.

**Zero Trust demands:**
- Tier-to-tier isolation
- Explicit service-to-service rules
- No subnet-wide trust

### Micro-Segmentation (Conceptual)
Micro-segmentation is not about thousands of rules. It is about **meaningful boundaries**.

**Patterns:**
- Management plane isolated
- Application tiers isolated
- Data tier isolated
- Shared services accessed via explicit paths

---

## Identity-Aware Networking (Preview)

In mature Zero Trust environments:
- Identity posture influences network access
- Device compliance affects reachable routes
- Risk scores gate network paths

**Examples (not implemented yet):**
- AWS Verified Access
- Azure Global Secure Access

> These concepts bridge **Module 2 → Module 3**.

---

## What This Module Will Not Do

- ❌ No workloads
- ❌ No compute instances
- ❌ No applications
- ❌ No open ports

Those belong to **Module 4 — Compute**.

---

## How This Prepares You for Lab 04

After this chapter, you should be able to answer:
- Why flat networks are dangerous
- Where segmentation boundaries belong
- How AWS and Azure differ philosophically
- Why private endpoints matter

**Only then do we build.**

---

### Interview Soundbite (Use This)

> “Zero Trust networking is not about perimeter firewalls. It is about designing networks so that even trusted identities have nowhere dangerous to go.”

---

## Next
Proceed to:

👉 **[Lab 04 — Zero Trust Networking (Hands-On Live Deployment)]**

*Where theory becomes enforcement.*
