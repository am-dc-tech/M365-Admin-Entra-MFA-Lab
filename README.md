# 🔐 MFA Lab — Microsoft Entra ID & M365 Admin Center

> **Hands-on lab** demonstrating how to enable and enforce Multi-Factor Authentication (MFA) for users in a Microsoft 365 tenant using Microsoft Entra ID (formerly Azure Active Directory).

---

## 📋 Lab Overview

| Detail | Info |
|--------|------|
| **Tenant** | AMTECHLab.onmicrosoft.com |
| **Tools Used** | Microsoft 365 Admin Center, Microsoft Entra ID |
| **Difficulty** | Beginner – Intermediate |
| **Est. Time** | 30–45 minutes |
| **License Required** | M365 Business Premium (for Conditional Access) |

---

## 🎯 Objectives

- Enable per-user MFA through Microsoft Entra ID
- Create a Conditional Access policy to enforce MFA on all cloud apps
- Register MFA as an end user and verify the sign-in challenge
- Apply break-glass admin exclusions to avoid tenant lockout

---

## 👥 Lab Users

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

## 🧪 Lab Steps

### Phase 1 — Access Microsoft Entra ID

1. Sign in to the **M365 Admin Center** at [https://admin.microsoft.com](https://admin.microsoft.com) using `admin@AMTECHLab.onmicrosoft.com`.
2. In the left nav, go to **Show all → Admin centers → Microsoft Entra**.
3. This opens the Entra admin center at [https://entra.microsoft.com](https://entra.microsoft.com).

---

### Phase 2 — Enable Per-User MFA

> Per-user MFA works on any M365 plan and requires no extra license.

1. In Entra, navigate to **Identity → Users → All users**.
2. At the top of the page, click **Per-user MFA**.
3. Select **Alice Adams** and **Carol Chen**.
4. In the right panel, click **Enable**.
5. Confirm the prompt — both users will show status: **Enabled**.

---

### Phase 3 — Create a Conditional Access Policy *(Best Practice)*

> Requires **Microsoft Entra ID P1** — included in M365 Business Premium.

1. In Entra, go to **Protection → Conditional Access → Policies**.
2. Click **+ New policy** and name it:
   ```
   Require MFA – All Users
   ```
3. **Assignments → Users:**
   - Include: **All users**
   - Exclude: `admin@AMTECHLab.onmicrosoft.com` *(break-glass account)*

   > ⚠️ **Always exclude at least one admin account** to avoid locking yourself out of the tenant.

4. **Target resources:** Select **All cloud apps**.

5. **Grant:** Select **Require multifactor authentication** → Grant access.

6. **Enable policy:** Set to **On** (or use **Report-only** first to audit before enforcing).

7. Click **Create**.

---

### Phase 4 — Test MFA as an End User

1. Open a **private/incognito browser window**.
2. Go to [https://myapps.microsoft.com](https://myapps.microsoft.com).
3. Sign in as **Alice Adams** (`alice@AMTECHLab.onmicrosoft.com`).
4. Alice will be prompted to **register an MFA method** (Authenticator app recommended).
5. Complete the registration wizard.
6. Sign out, then sign back in — confirm that the **MFA challenge appears** before access is granted.

---

## ✅ Completion Checklist

- [ ] Accessed Microsoft Entra ID from the M365 Admin Center
- [ ] Enabled per-user MFA for Alice Adams and Carol Chen
- [ ] Created a Conditional Access policy requiring MFA for all users
- [ ] Excluded the admin break-glass account from the CA policy
- [ ] Verified MFA registration prompt appears for a standard user
- [ ] Confirmed MFA challenge appears on subsequent sign-in

---

## 🛠️ Skills Demonstrated

`Microsoft Entra ID` `Multi-Factor Authentication` `Conditional Access` `M365 Admin Center` `Identity & Access Management` `Zero Trust Security` `Break-Glass Accounts` `Microsoft Authenticator`

---

## 📌 Notes

- **Per-user MFA** (Phase 2) is a legacy method but useful for quick enablement without Conditional Access.
- **Conditional Access** (Phase 3) is the enterprise standard and offers more granular control (location, device compliance, risk-based, etc.).
- Unlicensed users (Bob, Jamie, Pablo, Tracy) cannot be targeted by Conditional Access policies that require Entra P1 — assign licenses first if needed.

---

## 📂 Related Labs

| Lab | Description |
|-----|-------------|
| *(Coming soon)* SSPR Lab | Configure Self-Service Password Reset in Entra |
| *(Coming soon)* Identity Protection | Set up risk-based Conditional Access |

---

*Lab environment: Microsoft 365 Business Premium trial tenant — AMTECHLab*
