#️⃣ LAB 02 — SCIM Provisioning from Azure Entra ID → AWS IAM Identity Center  
**StageCloud Academy — Updated 2025 Edition**

> **Prerequisite:** Lab 01 (SAML Federation) must be completed successfully.

---

## ⭐ 1. Overview

In this lab, you will implement **SCIM automatic user provisioning**  
from **Azure Entra ID → AWS IAM Identity Center**.

You will:

- Exchange SAML metadata between Azure & AWS  
- Connect Azure SCIM provisioning to AWS  
- Provision multiple users automatically  
- Assign permission sets  
- Validate federated login  

This is the **final step** in building your **Identity Fabric**.

---

## ⭐ 2. Prerequisites

### From Lab 01:
- AWS Identity Center set to **External IdP (Entra)**  
- NameID → `user.mail` configured  
- SAML federation fully functional  
- Azure Enterprise App **AWS IAM Identity Center** created  

### AWS Requirements:
- IAM Identity Center enabled  
- AWS Organizations root access  
- SCIM provisioning **enabled**

### Azure Requirements:
- Azure Entra ID  
- AWS Enterprise Application created  
- Ability to assign users/groups to app  

---

## ⭐ 3. Part A — SAML Metadata Exchange (MANDATORY)

> **SCIM will NOT work unless SAML metadata is exchanged properly.**

### Step 3.1 — Download AWS SAML Metadata  
AWS Console → IAM Identity Center →  
**Settings → Identity Source → Change / View**

Click **Download metadata file**

Save as:

```
aws-idc-metadata.xml
```

---

### Step 3.2 — Upload AWS Metadata into Azure  
Azure Portal → Entra ID → Enterprise Apps →  
**AWS IAM Identity Center → Single Sign-On → SAML**

Click:

```
Upload metadata file
```

Azure auto-populates:

- Identifier  
- Reply URLs  
- Logout URLs  

---

### Step 3.3 — Configure NameID Claim  
Azure → **Attributes & Claims → Edit**

Set:

| Setting | Value |
|--------|--------|
| NameID Format | EmailAddress |
| Source Attribute | `user.mail` |

> **#1 cause of federation failure:** Incorrect NameID.

---

### Step 3.4 — Download Azure Federation Metadata

Click:

```
Download Federation Metadata XML
```

Save as:

```
azure-idp-metadata.xml
```

---

### Step 3.5 — Upload Azure Metadata Back Into AWS  
AWS → IAM Identity Center → Identity Source

Upload:

```
azure-idp-metadata.xml
```

Click **Save**.

> **SAML Federation is now complete.**

---

## ⭐ 4. Part B — Enable SCIM Provisioning in AWS

AWS Console → IAM Identity Center →  
**Settings → Automatic Provisioning**

Click:

```
Enable
```

AWS displays:

- **SCIM Tenant URL**  
- **SCIM Access Token**

Example:

```
https://scim.us-east-1.amazonaws.com/d-9a675141db/scim/v2/
Token: eyJhbGciOiJ...
```

> Copy both values — you will paste them into Azure.

---

## ⭐ 5. Part C — Configure SCIM in Azure

Azure Portal → Entra ID → Enterprise Apps →  
**AWS IAM Identity Center → Provisioning**

Under **Admin Credentials**:

| Field | Value |
|-------|--------|
| Tenant URL | (AWS SCIM URL) |
| Secret Token | (AWS SCIM Access Token) |

Click:

✔ **Test Connection**  
✔ **Save**  
✔ **Start Provisioning**

---

## ⭐ 6. Part D — Create Azure Users (Updated UI)

Azure Portal → Entra ID → **Users → New User**

### **User 1 — Primary Architect**
```
Username: olumide.towoju@gmail.com
First name: Olumide
Last name: Towoju
Display name: Olumide Towoju
Password: Auto-generated
```


⚠ **SCIM requires FIRST + LAST NAME**  
Missing → AWS returns:

```
400 BadRequest — "The attribute name is required"
```

---

## ⭐ 7. Part E — Assign Users to AWS Enterprise App

Azure Portal → Entra ID → Enterprise Apps →  
**AWS IAM Identity Center → Users and Groups → Add user**

Assign:

- `olumide.towoju@gmail.com`  

> Only ASSIGNED users enter SCIM provisioning scope.

---

## ⭐ 8. Part F — Trigger SCIM Provisioning

Azure → Enterprise Apps → AWS IAM Identity Center →  
**Provisioning → Restart Provisioning**

Check **Provisioning Logs**:

Expected per user:

- Import user → Success  
- Determine if in scope → Success  
- Match user → Success  
- Perform Action → **Create — SUCCESS**  

This means the user has been created in AWS Identity Store.

---

## ⭐ 9. Part G — Verify Users in AWS

AWS Console → IAM Identity Center → **Users**

Expected:

### **User: Olumide Towoju**
```
Username: olumide.towoju@gmail.com
Status: Enabled
Created by: SCIM
```

> SCIM provisioning is now fully operational.

---

## ⭐ 10. Part H — Assign Permission Sets in AWS

AWS Console → IAM Identity Center →  
**AWS Accounts → Select Account → Assign Users**

Assign:

- Olumide Towoju  

Choose Permission Set:

✔ `AdministratorAccess` (for testing)

AWS shows:

```
We reprovisioned your AWS account successfully and applied the updated permission set.
```

---

## ⭐ 11. Part I — Validation (Live Authentication Test)

Open the AWS SSO portal:

```
https://d-9a675141db.awsapps.com/start
```

---

### **Login Test: Architect**
```
Username: olumide.towoju@gmail.com
Password: (reset on first login)
```

Expected:

- MFA required  
- Access Portal loads  
- Assigned AWS account visible  
- Console access successful  

---

### **Login Test: Architect**
```
Username: olumide.towoju@gmail.com
Password: (reset on first login)
```

Expected:

- Password reset  
- MFA registration  
- AWS Portal loads  
- AWS Console login works  

---

## ⭐ 12. Screenshots to Capture (Binder Documentation)

### Azure:
- Provisioning logs (SUCCESS)  
- App assignments  

### AWS:
- Identity Center → Users  
- Permission Set assignments  
- Access Portal screen  
- Console header showing identity  

---

## ⭐ 13. Lab 02 — Completion Statement

You have successfully deployed:

✔ SAML federation (Lab 01)  
✔ SCIM provisioning  
✔ Multi-user identity sync  
✔ AWS permission assignment  
✔ Complete authentication flow  
✔ MFA enforcement  
✔ Multi-user SSO to AWS Console  

🎉 **Your Identity Fabric is now fully operational across Entra → AWS.**

---

# ✅ **END OF LAB 02**
