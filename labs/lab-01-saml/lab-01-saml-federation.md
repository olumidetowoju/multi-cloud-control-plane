
# 🧪 Lab 01 — SAML Federation Between AWS IAM Identity Center and Microsoft Entra ID  
**Hands-On: Identity Source Migration + SAML Authentication Trust**

<p align="center">
  <img src="https://github.com/S3curethecloud/stc-assets/blob/main/logos/stc-shield.png" width="140" alt="STC Lab Logo"/>
</p>

---

# ⭐ Objective

By the end of this lab, you will:

- Switch AWS IAM Identity Center’s identity source → **External IdP (Entra ID)**  
- Establish a full **SAML 2.0 trust** between AWS & Entra  
- Configure **claims mapping** (NameID → `user.mail`)  
- Validate authentication from **Entra → AWS**  
- Understand the identity flow end-to-end  

> **This is the foundational skill of any multi-cloud identity architect.**

---

# ⭐ Prerequisites

### **You need:**

### **AWS**
- AWS Organization (recommended)  
- IAM Identity Center enabled  
- At least one internal directory user for testing  

### **Microsoft Entra ID**
- Cloud Application Administrator or Global Admin  
- Ability to create an Enterprise App  
- Users with valid email addresses  

---

# ⭐ What You Will Build

A complete identity federation flow:

```
+------------------------+        SAML (Auth)
|   Microsoft Entra ID   | -------------------------+
|  (Identity Provider)   |                          |
+------------------------+                          |
                                                    |  TRUST
+------------------------+                          |
| AWS IAM Identity Center| <------------------------+
|   (Service Provider)   |
+------------------------+
```

Authentication will flow **from Entra → AWS** using SAML.

---

# 🔥 STEP 1 — Prepare Existing AWS IAM Identity Center Users  
Ensures a smooth **NameID match**.

### **1.1 Navigate to Users**
AWS Console → IAM Identity Center → **Users**

### **1.2 Check usernames**
Each username **must exactly** equal the Entra email:

```
AWS username: john.smith@yourcompany.com  
Entra email: john.smith@yourcompany.com
```

If mismatched, fix using AWS Console **or** AWS CLI:

```bash
aws identitystore update-user \
  --identity-store-id d-xxxxxxxxxx \
  --user-id <UserID> \
  --user-name "john.smith@yourcompany.com"
```

---

# 🔥 STEP 2 — Begin Identity Source Migration in AWS

### **2.1 Go to Identity Source**
IAM Identity Center → **Settings → Identity Source**

### **2.2 Click Change**
Choose:

```
External Identity Provider (SAML)
```

### **2.3 Download AWS SAML Metadata**
Click:

```
Download metadata file
```

You’ll upload this file into Entra next.

> 📌 **This XML file contains your AWS SP configuration — keep it safe.**

---

# 🔥 STEP 3 — Create AWS IAM Identity Center Enterprise App in Entra ID

### **3.1 Go to Entra Admin Center**
Navigation:

```
Identity → Applications → Enterprise Applications → New Application
```

### **3.2 Search for:**
```
AWS IAM Identity Center
```

Select → **Create**

### **3.3 Configure SAML**
Inside the application:

```
Single Sign-On → SAML
```

### **3.4 Upload AWS Metadata**
Upload the file from Step 2.

This auto-populates:

- Identifier (EntityID)  
- Reply URLs  
- Sign-on URLs  
- Certificate binding  

---

# 🔥 STEP 4 — Configure SAML Claims (**CRITICAL STEP**)

Go to:

```
Attributes & Claims → Edit
```

Set:

| Field | Value |
|-------|--------|
| NameID Format | Email Address |
| Source Attribute | `user.mail` |

> ⚠️ **The NameID MUST match AWS username exactly — this is the #1 cause of failure.**

---

# 🔥 STEP 5 — Download Entra Federation Metadata

Inside the SAML blade:

```
Federation Metadata XML → Download
```

This file will be uploaded back to AWS.

---

# 🔥 STEP 6 — Upload Entra Metadata into AWS

Return to:

```
IAM Identity Center → Settings → Identity Source
```

Upload the Entra metadata XML.

AWS warns you:

- Internal directory users may lose passwords  
- Federation becomes mandatory  

Click:

```
Change Identity Source
```

AWS is now federated with Entra. 🎉

---

# 🔥 STEP 7 — Assign Users & Groups in Entra (Access Control)

**If a user is not assigned here, they cannot authenticate.**

### **7.1 Go to Enterprise Application**
```
Enterprise Applications → AWS IAM Identity Center → Users and Groups
```

### **7.2 Add Users / Groups**

Click → **Add user/group**

Assign:

- Yourself  
- Test users  
- Identity groups  

📌 **No assignment → No login.**

---

# 🔥 STEP 8 — Test SAML Login

Visit your AWS SSO URL:

```
https://your-aws-sso-portal.awsapps.com/start
```

Expected flow:

1. Redirect to **Entra login**  
2. Successful authentication  
3. Redirect to **AWS SSO portal**  
4. Accounts & Permission Sets visible  

If this works → **Federation is complete.**

---

# 🔎 Troubleshooting Guide

### ❌ “User not found”
Fix → NameID = `user.mail` & AWS username must match Entra email exactly.

### ❌ User appears but has no AWS access
Fix → Assign Permission Sets in IAM Identity Center.

### ❌ Redirect loop
Fix → Wrong Reply URL or browser cache issues.

### ❌ User missing in AWS Identity Center
Fix → SCIM not yet configured (Lab 02).

---

# ⭐ Lab Completion Summary

You have successfully:

✔ Migrated AWS Identity Center → **External Entra ID provider**  
✔ Built a full **SAML federation trust**  
✔ Configured **correct NameID claim: user.mail**  
✔ Created the Entra Enterprise App  
✔ Uploaded SP & IdP metadata files correctly  
✔ Assigned users & validated access  
✔ Logged into AWS with Entra identity  

> 🎓 **You now possess enterprise-level federation skills used by real cloud architects.**

---

# END OF LAB FILE
