# Enterprise Identity & Access Management - Microsoft Entra ID

I built a working enterprise identity environment in Microsoft Entra ID to understand how organizations actually control access at scale not from a tutorial, but by standing up a real tenant and configuring each governance control myself. This repo documents what I built, the decisions behind it, and what I learned where things broke.

The environment simulates a company, NimbusCorp, and covers the full identity lifecycle: provisioning users, granting access through groups, enforcing security policy, managing privileged access, reviewing access periodically, and deprovisioning leavers.

## What I built

### 1. The directory - users and structure

I started by provisioning six users across five departments (Finance, HR, IT, Developers, Sales), each with department and job-title attributes set. This attribute data isn't fake, it's what later drives automatic group membership. Getting the foundation right mattered because everything downstream keys off it.

### 2. Group-based access - assigned and dynamic

I created two kinds of groups deliberately, to show the difference:

- **Finance-Team (assigned):** members added manually. Simple, but doesn't scale someone has to remember to add and remove people.
- **IT-Team-Dynamic (dynamic):** membership driven by a rule `user.department -eq "IT"`. I added no one manually; Entra pulled in the IT user automatically because their attribute matched. This is how access scales without human error: change a person's department and their group access follows.

Access is always granted to the group, never the individual so permissions scale with the organization instead of becoming a manual mess.

### 3. Conditional Access - enforcing MFA

I built a policy requiring multi-factor authentication for all users across all cloud apps. I ran it in report-only mode (Security Defaults and Conditional Access can't both enforce at once on this tenant), which still demonstrates the policy design. I deliberately excluded my own admin account from the enforced version locking yourself out of your own tenant is a classic first-timer mistake, and I wanted the config to reflect that awareness.

### 4. Privileged Identity Management (PIM) - just-in-time admin

Standing admin access is a liability: if an always-on admin account is compromised, so is the tenant. I configured PIM to make a user *eligible* for a privileged role (Application Administrator) rather than permanently assigned they activate it only when needed, and it expires.

**An honest note:** the eligible assignment itself failed with a persistent "role not found" error - a known Entra backend bug on trial tenants where PIM doesn't fully onboard directory roles. I confirmed it wasn't my configuration (the setup flow completed correctly) and documented it rather than pretending it worked. Configuring PIM correctly and recognizing a platform bug is itself the skill.

### 5. Access Reviews - catching privilege creep

I set up a one-time access review of the Finance-Team group, with myself as reviewer, to demonstrate recertification the periodic "does this person still need this access?" check that catches access role changes leave behind. This is the detective control that stops permissions quietly accumulating over time.

### 6. SAML SSO - federated sign-in

I added the Microsoft Entra SAML Toolkit as an enterprise app, assigned a user, and configured SAML single sign-on (Entity ID, Reply URL, sign-on URL). This demonstrates federating an external application to Entra as the identity provider one login, centrally controlled, instead of per-app credentials.

### 7. The Leaver step - deprovisioning

To make the lifecycle real rather than theoretical, I disabled a departing user's account (Aisha Khan, Sales). Disabling in Entra immediately blocks authentication everywhere that identity connects — a single point of deprovisioning. This is the step organizations most often get wrong, and dormant enabled accounts are a common breach vector.

## The identity lifecycle (JML)

Everything above maps to the Joiner–Mover–Leaver model that underpins identity governance:

- **Joiner:** provisioned with least-privilege access via dynamic groups; MFA enforced from first sign-in.
- **Mover:** changing a user's department attribute automatically shifts their group access; access reviews catch what role changes miss.
- **Leaver:** disabling the account revokes access everywhere at once.

Full write-up in JML-lifecycle.md

## What I learned

- Attribute-driven (dynamic) group membership is the difference between identity that scales and identity that relies on someone remembering to click. It clicked when I watched a user populate a group with no manual step.
- Least privilege isn't one setting it's the thread running through group design, Conditional Access, PIM, and access reviews together.
- Real platforms have bugs. PIM's role-onboarding failure taught me that documenting a blocker honestly, after confirming it isn't your own error, is part of the job not something to hide.
- Locking down a tenant while keeping yourself in it (the admin exclusion) is a real operational concern, not a footnote.

## Environment

- **Platform:** Microsoft Entra ID (P2 trial for PIM, dynamic groups, and access reviews)
- **Organization:** NimbusCorp (simulated), 6 users across 5 departments

## Screenshots

Configuration evidence for each component is in screenshots

## Component status

| # | Component | Status |
|---|-----------|--------|
| 1 | Users & directory structure | Done |
| 2 | Group-based access (assigned) | Done |
| 3 | Dynamic groups | Done |
| 4 | Conditional Access | Done (report-only) |
| 5 | SAML SSO | Done |
| 6 | Privileged Identity Management | Configured (blocked by Entra backend bug, documented) |
| 7 | Access Reviews | Done |
| 8 | Joiner-Mover-Leaver lifecycle | Done |
