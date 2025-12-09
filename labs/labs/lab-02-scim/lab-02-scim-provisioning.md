# 🧪 LAB 02 — LIVE DEPLOYMENT  
# **SCIM Provisioning From Entra ID → AWS IAM Identity Center**  
Automatic user + group lifecycle sync — real enterprise setup


---

# ⭐ WHY THIS LAB IS REQUIRED

Your identity source is now **External (Microsoft Entra ID)**.  
This changes everything:

- AWS internal directory becomes **inactive**  
- Users must come from **Entra via SCIM**  
- SAML login cannot succeed until AWS **knows who the users are**  
- SCIM is the **only** mechanism that creates user objects in AWS Identity Store  

SCIM synchronizes:

- Users  
- Display names  
- Emails  
- Groups  
- Group membership  
- Updates  
- Deletions  

This is real enterprise identity lifecycle automation.

---

# 🚀 LET’S BEGIN LAB 02 — LIVE

# ⭐ STEP 1 — ENABLE SCIM IN AWS IAM IDENTITY CENTER

Navigate:

```
AWS Console → IAM Identity Center → Settings → Automatic provisioning
```

You will see a button:

👉 **Enable**

Click **Enable**.

AWS will display:

- **SCIM endpoint (Tenant URL)**  
- **SCIM Access Token (Secret Token)**  
- **Token expiration timer**

Copy BOTH values:

Example:

```
SCIM Endpoint:
https://scim.us-east-1.amazonaws.com/d-9066178bc5/scim/v2/

Access Token:
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ...
```

You will paste these into Entra.

---

# ⭐ STEP 2 — OPEN THE AWS IAM IDENTITY CENTER ENTERPRISE APP IN ENTRA

Navigate:

```
Azure Portal → Entra ID → Enterprise Applications → AWS IAM Identity Center
```

Select:

👉 **Provisioning**

You will see:

`Provisioning Mode: Manual`

---

# ⭐ STEP 3 — SWITCH PROVISIONING MODE TO AUTOMATIC

Click:

👉 **Edit Provisioning**

Set:

```
Provisioning Mode → Automatic
```

A new form appears.

---

# ⭐ STEP 4 — ENTER AWS SCIM DETAILS INTO ENTRA

Under **Admin Credentials**:

| Field | Value |
|-------|--------|
| Tenant URL | *AWS SCIM Endpoint* |
| Secret Token | *AWS SCIM Token* |

Example:

```
Tenant URL:
https://scim.us-east-1.amazonaws.com/d-9066178bc5/scim/v2/
```

Click:

👉 **Test Connection**

### ⭐ Expected Result:

```
Connection successful
```

If it fails, common causes:

- Token expired  
- Token missing characters  
- Tenant URL missing trailing slash  
- Wrong region  

If needed → click **Regenerate Token** in AWS.

---

# ⭐ STEP 5 — SAVE THE CONFIGURATION

Click:

👉 **Save**

Entra reloads the provisioning settings.

---

# ⭐ STEP 6 — DEFINE WHAT GETS SYNCED

Scroll to **Settings** → Choose:

```
Scope → Sync only assigned users and groups
```

This prevents syncing your entire organization (VERY important).

---

# ⭐ STEP 7 — TURN ON PROVISIONING

At the top:

```
Provisioning Status → On
```

Click:

👉 **Save**

Entra will begin provisioning on a 40-minute cycle unless forced manually.

---

# ⭐ STEP 8 — FORCE A MANUAL SYNC (TO SEE RESULTS NOW)

Click:

👉 **Restart provisioning**  
or  
👉 **Start provisioning** (on first setup)

This triggers an immediate SCIM push.

---

# ⭐ STEP 9 — VERIFY USERS APPEAR IN AWS (LIVE)

Navigate:

```
AWS Console → IAM Identity Center → Users
```

You should now see your Entra users:

Example:

```
ola.omoniyi@yourtenant.onmicrosoft.com
Status: Enabled
Provisioned by: SCIM
```

🎉 **This is the moment SAML login becomes possible.**

---

# ⭐ STEP 10 — ASSIGN PERMISSION SETS IN AWS

Even though the user exists, AWS will show no access until a **Permission Set** is assigned.

Navigate:

```
IAM Identity Center → AWS Accounts
```

Choose your AWS account → Click:

👉 **Assign Users**

Select your SCIM-provisioned user.

Assign:

- **AdministratorAccess** (testing)  
or  
- **ReadOnlyAccess**  

Click **Submit**.

---

# ⭐ STEP 11 — TEST FEDERATED LOGIN (LIVE)

Open your AWS SSO URL:

```
https://d-9066178bc5.awsapps.com/start
```

Expected flow:

1. Redirect → **Entra login**  
2. Enter email + password  
3. MFA challenge (if required)  
4. Redirect → AWS SSO  
5. See **AWS Accounts page**  
6. Click “Management Console” → Logged in as Entra user  

🔥 This proves your **SAML + SCIM end-to-end federation** is working.

---

# 🧪 TROUBLESHOOTING (LIVE LOGIC)

| Issue | Fix |
|-------|------|
| ❌ User not found | SCIM not started / NameID mismatch |
| ❌ Access denied | No assigned Permission Set |
| ❌ “Code isn’t right” | SCIM username mismatch |
| ❌ Redirect loop | Wrong URL or metadata mismatch |
| ❌ Test connection failed | Wrong token or wrong SCIM endpoint |

---

# 🎉 LAB 02 SUCCESS CHECKLIST

| Task | Status |
|------|--------|
| SCIM enabled in AWS | ✔ |
| SCIM URL + Token copied | ✔ |
| Entra Provisioning → Automatic | ✔ |
| SCIM connection successful | ✔ |
| Assigned Entra users | ✔ |
| Forced provisioning run | ✔ |
| User appears in AWS | ✔ |
| Permission set assigned | ✔ |
| SAML login successful | ✔ |

---

# END OF LAB FILE
