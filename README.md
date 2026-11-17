Microsoft 365 Onboarding Lab
A step-by-step GitHub lab for onboarding a new organization to Microsoft 365 — covering user provisioning, security configuration, shared mailbox setup, and runbook documentation.
---
Summary
This lab walks you through setting up a Microsoft 365 tenant for a small organization. You will provision user accounts, assign licenses, enforce multi-factor authentication via Security Defaults, configure a shared mailbox in Exchange Online, verify resources with PowerShell, and document every step in this repository as a versioned runbook.
Duration: 60–90 minutes  
Level: Beginner – Intermediate  
Tools: Microsoft 365 Admin Center · Exchange Online · PowerShell · GitHub
---
Scenario
Contoso Startup has just purchased 10 Microsoft 365 Business Standard licenses. As the IT administrator you must:
Onboard five employees with licensed accounts
Enforce baseline security (MFA) before distributing credentials
Configure a shared `support@` mailbox for the team
Document and commit the entire process to this GitHub repository so it is repeatable
The CTO wants the runbook pushed before end of day.
---
Lab Steps
Step 1 — Fork the Repository
Fork this repo to your own GitHub account and clone it locally:
```bash
git clone https://github.com/YOUR\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\_USERNAME/contoso-m365-onboarding.git
cd contoso-m365-onboarding
```
Create a feature branch before making any changes:
```bash
git checkout -b onboarding/contoso-setup
```
Step 2 — Sign in to the Microsoft 365 Admin Center
Navigate to admin.microsoft.com with your Global Administrator account.
Confirm your licenses are visible:  
Billing → Your products — you should see 10 × Microsoft 365 Business Standard.
Step 3 — Create User Accounts
Go to Users → Active users → Add a user and create five accounts:
Display Name	Username
Alice Adams	alice@yourdomain.onmicrosoft.com
Bob Baker	bob@yourdomain.onmicrosoft.com
Carol Chen	carol@yourdomain.onmicrosoft.com
Dave Diaz	dave@yourdomain.onmicrosoft.com
Eve Evans	eve@yourdomain.onmicrosoft.com
Assign each user a Microsoft 365 Business Standard license during creation.
> ⚠️ \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*Important:\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\* Store temporary passwords in a local `secrets.md` file. This file is listed in `.gitignore` and must \\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*never\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\* be committed to the repository.
Step 4 — Enable Security Defaults (MFA)
In the Admin Center, go to Azure Active Directory → Properties
Select Manage Security Defaults at the bottom of the page
Toggle Security defaults to Enabled
Save the change
This enforces MFA for all users on their next sign-in. Record this change in `docs/security-setup.md`.
Step 5 — Create a Shared Mailbox
Go to Teams & groups → Shared mailboxes → Add a shared mailbox
Set the name to `Support` and address to `support@yourdomain.onmicrosoft.com`
After creation, open the mailbox and add all five users as Members
Members can now send and receive email on behalf of the Support mailbox.
Step 6 — Verify via PowerShell
Open PowerShell and connect to Exchange Online:
```powershell
Install-Module -Name ExchangeOnlineManagement -Force
Connect-ExchangeOnline -UserPrincipalName admin@yourdomain.onmicrosoft.com

# List all shared mailboxes
Get-Mailbox -RecipientTypeDetails SharedMailbox | Select DisplayName, PrimarySmtpAddress

# Confirm members
Get-MailboxPermission -Identity support@yourdomain.onmicrosoft.com
```
Paste the output into `docs/verification-log.md` before committing.
Step 7 — Document & Commit Your Runbook
Update this `README.md` with the date completed and your name, then stage and commit:
```bash
git add README.md docs/ .gitignore
git commit -m "feat: complete M365 initial onboarding for Contoso"
git push origin onboarding/contoso-setup
```
Step 8 — Open a Pull Request
On GitHub, open a Pull Request from `onboarding/contoso-setup` into `main`.
Title: `feat: M365 initial onboarding — Contoso`
Description: Summarize what was configured (users, licenses, MFA, shared mailbox)
Request a peer review before merging
---
Security Considerations
Area	Guidance
Never commit secrets	Passwords, tenant IDs, and tokens must stay out of Git. Add `secrets.md` and `\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\*.env` to `.gitignore` before the first commit.
Least privilege	Assign only the licenses and roles each user needs. Avoid giving everyone Global Admin.
Enable MFA first	Security Defaults must be enabled before credentials are distributed — even trial tenants get compromised.
Audit log retention	Enable the Unified Audit Log in Microsoft Purview so all admin actions are retained and reviewable.
Shared mailbox access	Only add members who genuinely need access. Review membership quarterly and remove leavers promptly.
Branch protection	Protect `main` with a required PR review before merging to prevent unauthorized runbook changes.
---
What I Learned
How to navigate the Microsoft 365 Admin Center and provision licensed user accounts from scratch
How Security Defaults enforce MFA tenant-wide without requiring Entra ID P1/P2 licenses
How to create and configure shared mailboxes in Exchange Online and add delegate members
How to use PowerShell with `Connect-ExchangeOnline` to verify provisioned resources programmatically
Why documentation as code matters — storing runbooks in GitHub makes onboarding repeatable, reviewable, and auditable
How to use `.gitignore` to prevent sensitive files from being accidentally committed
How pull requests model a change-approval workflow, reducing configuration errors in production
---
Repository Structure
```
contoso-m365-onboarding/
├── README.md                  # This file — main lab guide
├── .gitignore                 # Excludes secrets and local env files
├── docs/
│   ├── security-setup.md      # Record of security configuration changes
│   └── verification-log.md    # PowerShell output confirming provisioned resources
└── secrets.md                 # ⛔ LOCAL ONLY — never committed
```
---
Prerequisites
Microsoft 365 trial or sandbox tenant with Global Administrator access
PowerShell 5.1+ with the `ExchangeOnlineManagement` module
A GitHub account
---
License
MIT — free to use, adapt, and share for learning purposes.
---
Completed by: `YOUR NAME` — November 2026
