
# 🌐 Module 1 — Identity Fabric (The Foundation of the Multi-Cloud Control Plane)  
**Identity • SAML • SCIM • Federation • Zero Trust • Workforce Identity**

<p align="center">
  <img src="https://github.com/S3curethecloud/stc-assets/blob/main/logos/stc-shield.png" width="140" alt="STC Identity Fabric"/>
</p>

---

# ⭐ Introduction: Identity Is the New Perimeter

The Guru stands on the same California beach, hands clasped behind his back, watching the tide roll in.

> *“Before we talk about Zero Trust, before we talk about compute, networking, or Terraform…  
we speak of Identity.  
For identity is the passport, the citizenship, the key, and the fingerprint of your entire cloud civilization.”*

In a multi-cloud world, **Identity Fabric is the glue** that binds AWS, Azure, and GCP into one coherent system.

You will build:

- A unified identity strategy  
- Central authentication  
- Automated provisioning  
- Cloud-to-cloud workforce governance  
- Federated login across providers  
- The root of Zero Trust  

**Identity Fabric is the heartbeat of the Control Plane.**

---

# 🧱 Section 1 — What Is the Identity Fabric?

The **Identity Fabric** is the unified identity architecture that operates across all clouds.

It answers:

✔ Who are you?  
✔ What can you access?  
✔ Who decides your permissions?  
✔ How does your identity flow between AWS, Azure, and GCP?  
✔ How do we automate lifecycle changes?

It includes:

- Identity Source  
- Authentication Protocols (SAML, OIDC)  
- User Lifecycle (SCIM)  
- Groups → Roles → Permissions  
- Federation Trusts  
- Zero Trust Policies  
- Logs + Audit Trails  

Identity Fabric = **the backbone of multi-cloud access.**

---

# 🧭 Section 2 — Identity Architecture (High-Level)

All enterprise architectures use a **central identity provider** (IdP):

- Microsoft Entra ID  
- Okta  
- PingIdentity  

This IdP becomes the **Source of Truth**.

It federates outward:

```
Enterprise IdP → AWS IAM Identity Center  
               → Azure RBAC / Entra  
               → GCP IAM Workforce Federation
```

Each cloud trusts the IdP for:

- Authentication  
- Claims  
- Group membership  
- SCIM lifecycle replication  

**Identity is centralized — access is distributed.**

---

# 🔑 Section 3 — AWS IAM Identity Center (Successor to AWS SSO)

AWS’s modern workforce identity service:

### **AWS IAM Identity Center**

Supports:

- SAML 2.0  
- SCIM (user lifecycle automation)  
- Permission Sets  
- Multi-account assignments  

Important distinction:

- **IAM** = machine identity  
- **IAM Identity Center** = human identity  

**Human users ALWAYS authenticate through Identity Center.**

---

# 🔄 Section 4 — Migrating AWS Identity Source → Entra ID  
### *(One of the most valuable real-world enterprise skills)*

The Guru bends down and draws a key in the sand.

> *“If your kingdom changes its passport authority,  
make sure every citizen’s name matches exactly.”*

Identity migration requires **precision**.

---

## ✅ STEP 1 — Prepare AWS Usernames  
Usernames in AWS **must match** Entra email addresses:

```
john.smith@company.com
```

If mismatched, fix via:

- AWS Console  
- AWS CLI (`identitystore UpdateUser`)  

---

## ✅ STEP 2 — Change AWS Identity Source

In **IAM Identity Center → Settings → Identity Source**:

- Click **Change**  
- Select **External Identity Provider**  
- Download **AWS SAML Service Provider Metadata** (XML)

This file contains your AWS SP configuration.

---

## ✅ STEP 3 — Create Entra Enterprise Application

Entra Admin Center:

1. Enterprise Applications  
2. New Application  
3. Search for: **AWS IAM Identity Center**  
4. Open → **Single Sign-On → SAML**  
5. Upload AWS SP metadata  

Entra automatically configures:

- EntityID  
- ACS URL  
- SAML endpoints  

---

## ✅ STEP 4 — Configure Claims (**CRITICAL STEP**)

In **Attributes & Claims**:

```
NameID → user.mail  
Format → EmailAddress
```

This MUST match AWS username.  
**If this is wrong → authentication fails.**

---

## ✅ STEP 5 — Download Entra IdP Metadata

Download the **Federation Metadata XML**.  
This file contains:

- IdP certificate  
- SAML endpoints  
- Signing keys  

---

## ✅ STEP 6 — Upload Entra Metadata Back to AWS

Return to AWS IAM Identity Center:

- Upload the metadata file  
- Confirm the Identity Source switch  

🛑 **Login will not work until users/groups are assigned in Entra.**

---

# 👥 Section 5 — Assign Users & Groups in Entra

Entra controls **who can authenticate**.

Assign:

- Users  
- Groups  

To the AWS IAM Identity Center enterprise application.

If not assigned → access blocked.

---

# 🔁 Section 6 — SCIM Provisioning (Automatic User Lifecycle)

**SCIM = System for Cross-domain Identity Management**

It keeps AWS Identity Center synchronized with Entra.

### SCIM Steps:

### In AWS
- Enable **Automatic Provisioning**  
- Copy:
  - **SCIM endpoint URL**  
  - **SCIM secret token**

### In Entra
Set:

```
Provisioning Mode → Automatic
Tenant URL → <SCIM endpoint>
Secret Token → <SCIM token>
```

Run **Test Connection**  
Save  
Turn on provisioning  

Sync cycle: **~40 minutes**

SCIM ensures:

- User created in Entra → appears in AWS  
- User disabled → removed in AWS  
- Group membership changes → update AWS roles  

---

# 🧿 Section 7 — Identity in Zero Trust

Identity drives Zero Trust across clouds.

### **AWS**
- Permission Sets  
- IAM policies  
- Session time limits  
- MFA  
- CloudTrail logging  

### **Azure**
- Conditional Access  
- Identity Protection  
- MFA challenges  
- Privileged Identity Management  

### **GCP**
- Workforce Federation  
- IAM Bindings  
- Context-aware Access  

Identity is the *first and strongest* Zero Trust control.

---

# 🔍 Section 8 — Troubleshooting Identity Federation

### ❌ User cannot sign in
- Wrong NameID claim  
- User not assigned  
- Identity Source not saved  
- Browser cached old session  

### ❌ User missing in AWS Identity Center
- SCIM cycle not completed  
- Group not assigned  
- SCIM token expired  

### ❌ User signs in but sees no accounts
- No permission sets assigned  

The Guru nods:

> *“Identity is fragile when misconfigured,  
unbreakable when aligned.”*

---

# 🧠 Summary — Identity Fabric Is the Heartbeat of the Control Plane

The Guru closes Module 1:

> **“Identity is the one force that travels across clouds,  
through networks, into workloads, over permissions.  
Control identity…  
and you control the cloud itself.”**

You have now awakened the foundation of the Multi-Cloud Control Plane.  
Module 2 awaits.

---

# END OF FILE
