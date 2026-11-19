# 🔐 MFA Lab — Microsoft Entra ID & M365 Admin Center

---

## 📄 Summary

In this lab, I configured Multi-Factor Authentication (MFA) for a Microsoft 365 tenant using the **Microsoft Entra ID admin center**. Starting from the M365 Admin Center, I enabled per-user MFA for licensed accounts and then created a **Conditional Access policy** to enforce MFA across all cloud applications — the enterprise-standard approach used in real-world IT environments.

The lab was performed on a live Microsoft 365 Business Premium trial tenant (**AMTECHLab**) with a mix of licensed and unlicensed users. I acted as the Global Administrator to configure identity security settings, then verified the end-user MFA experience by signing in as a standard user.

This lab reflects skills directly relevant to roles in **IT Support, Cloud Administration, and Cybersecurity**, including identity management, Zero Trust security principles, and Microsoft 365 administration.

---

## 🎯 Lab Objectives

- Navigate from the Microsoft 365 Admin Center to Microsoft Entra ID
- Enable per-user MFA for licensed users in the tenant
- Create and configure a Conditional Access policy to require MFA on all cloud apps
- Apply a break-glass admin exclusion to prevent tenant lockout
- Register MFA as an end user using the Microsoft Authenticator app
- Verify the MFA sign-in challenge is enforced on subsequent logins

---

## 📋 Lab Environment

| Detail | Info |
|--------|------|
| **Tenant** | AMTECHLab.onmicrosoft.com |
| **Tools Used** | Microsoft 365 Admin Center, Microsoft Entra ID |
| **Difficulty** | Beginner – Intermediate |
| **Est. Time** | 30–45 minutes |
| **License Required** | M365 Business Premium (for Conditional Access) |

### 👥 Users

| Name | Username | License |
|------|----------|---------|
| admin *(Global Admin)* | admin@AMTECHLab.onmicrosoft.com | M365 Business Premium |
| Alice Adams | alice@AMTECHLab.onmicrosoft.com | M365 Business Premium |
| Carol Chen | carol@AMTECHLab.onmicrosoft.com | M365 Business Premium |
| Bob Baker | bob@AMTECHLab.onmicrosoft.com | Unlicensed |
| Jamie Randle | jamie.randle@AMTECHLab.onmicrosoft.com | Unlicensed |
| Pablo Marquez | pablo.marquez@AMTECHLab.onmicrosoft.com | Unlicensed |
| Tracy Dalhousie | tracy.dalhousie@AMTECHLab.onmicrosoft.com | Unlicensed |

---

## 🧪 Steps

### Phase 1 — Access Microsoft Entra ID

1. Sign in to the **M365 Admin Center** at [https://admin.microsoft.com](https://admin.microsoft.com) using `admin@AMTECHLab.onmicrosoft.com`.
2. In the left nav, go to **Show all → Admin centers → Microsoft Entra**.
3. This opens the Entra admin center at [https://entra.microsoft.com](https://entra.microsoft.com).

<img width="622" height="73" alt="1 log-into-entra" src="https://github.com/user-attachments/assets/791e1cf8-6c14-4d52-b97b-c6c86f640576" />

**Verify it's on:**

1. Go back to `Identity → Properties → Manage security defaults`
2. Confirm the toggle shows `Enabled`
3. If it still shows disabled, refresh the page and check again — changes can take a moment to save

> ✅ **Expected result:** Security defaults is toggled to Enabled. All users will now be required to register for MFA within 14 days.


<img width="431" height="369" alt="2 manage-security-defaults" src="https://github.com/user-attachments/assets/40412e6d-985f-417e-bfbf-a54df052442b" />

---

### Phase 2 — Enable Per-User MFA

> Per-user MFA works on any M365 plan and requires no additional license.

1. In Entra, navigate to **Identity → Users → All users**.
2. At the top of the page, click **Per-user MFA**.
3. Select **Alice Adams** and **Carol Chen**.
4. In the right panel, click **Enable**.
5. Confirm the prompt — both users will show status: **Enabled**.


<img width="862" height="671" alt="3 per-user-mfa" src="https://github.com/user-attachments/assets/b643b66d-bbba-487d-9a90-15bb715b9c50" />


---

### Phase 3 — Create a Conditional Access Policy *(Enterprise Best Practice)*

> Requires **Microsoft Entra ID P1** — included with M365 Business Premium.

1. In Entra, go to **Protection → Conditional Access → Policies**.
2. Click **+ New policy** and name it:
   ```
   Require MFA – All Users
   ```
3. **Assignments → Users:**
   - Include: **Select users and groups** → choose the **M365 Business Premium** licensed users:
     - `alice@AMTECHLab.onmicrosoft.com`
     - `carol@AMTECHLab.onmicrosoft.com`
   - Exclude: `admin@AMTECHLab.onmicrosoft.com` *(break-glass account)*

   > ⚠️ **Always exclude at least one admin account** to avoid locking yourself out of the tenant.

   > 💡 **Why not all users?** Unlicensed accounts (Bob, Jamie, Pablo, Tracy, support) have no access to M365 cloud apps, so enforcing MFA on them adds no security value. Scoping the policy to licensed users keeps things clean and follows the **principle of least privilege**.

4. **Target resources:** Select **All cloud apps**.

5. **Grant:** Select **Require multifactor authentication** → Grant access.

6. **Enable policy:** Set to **On** (or use **Report-only** first to audit before enforcing).

7. Click **Create**.


<img width="608" height="909" alt="5 mfa-policypng" src="https://github.com/user-attachments/assets/bbac2d21-e250-46a8-abf5-b691f35324f3" />

---

### Phase 4 — Test MFA as an End User

1. Open a **private/incognito browser window**.
2. Go to [https://myapps.microsoft.com](https://myapps.microsoft.com).
3. Sign in as **Alice Adams** (`alice@AMTECHLab.onmicrosoft.com`).
4. Alice is prompted to **register an MFA method** — select Microsoft Authenticator.
5. Complete the registration wizard by scanning the QR code in the app.
6. Sign out, then sign back in — confirm that the **MFA challenge appears** before access is granted. ✅

<img width="441" height="529" alt="6 alice-mfa-sign-in" src="https://github.com/user-attachments/assets/ed5e7b6d-b2b1-4e77-997e-215d2e216574" />

<img width="318" height="241" alt="7 alice-sign-in-mfa-success" src="https://github.com/user-attachments/assets/ffc52826-9292-4e75-a883-7f6dc880b398" />

7. Confirm the log data on Entra ID

<img width="1202" height="69" alt="8 entra-alice-log-in" src="https://github.com/user-attachments/assets/df0c2f4b-e70a-47e5-b9e8-2743cfe13fca" />


---

## 🔒 Security Considerations

### Break-Glass Accounts
When creating a Conditional Access policy that applies to all users, it is critical to **exclude at least one emergency admin account**. Without this exclusion, a misconfigured policy could lock all administrators out of the tenant — with no way to recover without contacting Microsoft Support. Break-glass accounts should be:
- Excluded from MFA Conditional Access policies
- Stored securely with credentials in a physical safe or sealed envelope
- Monitored with alerts for any sign-in activity

### Per-User MFA vs. Conditional Access
Per-user MFA (Phase 2) is a legacy method. While it works, it applies a blanket setting with no flexibility. **Conditional Access is the recommended enterprise approach** because it allows MFA to be scoped by user group, location, device compliance, sign-in risk, and application — giving administrators far more granular control.

### Report-Only Mode
Before enforcing a Conditional Access policy in a production environment, it is best practice to enable it in **Report-only mode** first. This allows administrators to review the sign-in logs and see which users *would* have been affected — without actually blocking or challenging anyone. This prevents unexpected disruptions.

### MFA Method Strength
Not all MFA methods are equally secure:

| Method | Security Level |
|--------|---------------|
| SMS / Phone call | ⚠️ Low — vulnerable to SIM swapping |
| Microsoft Authenticator (push) | ✅ Medium — convenient and widely used |
| Microsoft Authenticator (number matching) | ✅ High — mitigates MFA fatigue attacks |
| FIDO2 Security Key (e.g. YubiKey) | ✅ Highest — phishing-resistant |

For this lab, **Microsoft Authenticator with push notifications** was used. In a production environment, number matching or FIDO2 keys would be preferred.

### Scoping MFA to Licensed Users Only
The Conditional Access policy targets only the three **M365 Business Premium** licensed accounts — admin, Alice, and Carol. Unlicensed users (Bob, Jamie, Pablo, Tracy, support) have no access to M365 cloud apps, so applying MFA to them provides no security benefit and creates unnecessary noise in sign-in logs.

This scoping also reflects the **principle of least privilege**: apply controls where they're needed, not broadly for the sake of it. In a larger organization, this would typically be managed by assigning users to a security group (e.g. `Licensed-M365-Users`) and targeting that group in the policy — making it easy to automatically include new licensed users as they're onboarded.

---

## 💡 What I Learned

**Navigating the Microsoft Admin Ecosystem**
I gained hands-on experience navigating between the Microsoft 365 Admin Center and Microsoft Entra ID. These are two distinct portals that work together, and understanding the relationship between them is key to managing a Microsoft 365 environment.

**Two Approaches to MFA**
I learned the difference between per-user MFA (a simpler, legacy toggle) and Conditional Access policies (the enterprise standard). Conditional Access is significantly more powerful — it lets you apply MFA only under certain conditions, such as when a user signs in from an untrusted location or an unmanaged device.

**Why Break-Glass Accounts Matter**
One of the most important real-world lessons was the importance of excluding a break-glass admin account from MFA policies. Creating a policy that locks out all administrators is a common and costly mistake — this lab reinforced why that exclusion is non-negotiable.

**The End-User MFA Experience**
By testing the sign-in flow as Alice Adams, I understood what the registration and challenge process looks like from a user's perspective. This is valuable for Help Desk and IT Support roles where end-user guidance and troubleshooting are common tasks.

**Report-Only Mode as a Safety Net**
I learned that Conditional Access policies can be deployed in Report-only mode before enforcement. This is a best practice that allows admins to assess impact without disrupting users — a technique that translates directly to change management in enterprise environments.

---

## ✅ Completion Checklist

- [x] Accessed Microsoft Entra ID from the M365 Admin Center
- [x] Enabled per-user MFA for Alice Adams and Carol Chen
- [x] Created a Conditional Access policy requiring MFA for all users
- [x] Excluded the admin break-glass account from the CA policy
- [x] Verified MFA registration prompt appears for a standard user
- [x] Confirmed MFA challenge appears on subsequent sign-in

--


