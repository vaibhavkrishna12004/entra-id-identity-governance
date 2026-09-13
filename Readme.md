# Enterprise Identity & Access Management — Microsoft Entra ID

A hands-on identity governance project built in Microsoft Entra ID, simulating how a real organization (NimbusCorp) manages who has access to what. Covers user lifecycle, group-based access, conditional access, SSO, privileged access, and access reviews.

## Objective

Design and document an end-to-end enterprise identity environment that mirrors real-world IAM practice — least privilege, group-based access control, and identity governance.

## Environment

- **Platform:** Microsoft Entra ID (Azure)
- **Organization:** NimbusCorp (simulated)
- **Directory:** 6 users across 5 departments (Finance, HR, IT, Developers, Sales)

## Components

| # | Component | Status |
|---|-----------|--------|
| 1 | Users & Directory Structure | Done |
| 2 | Group-Based Access (assigned) | Done |
| 3 | Dynamic Groups | Planned (requires Entra ID P2) |
| 4 | Conditional Access Policies | Planned (requires Entra ID P2) |
| 5 | SAML SSO Integration | In progress |
| 6 | Privileged Identity Management (PIM) | Planned (requires Entra ID P2) |
| 7 | Access Reviews | Planned (requires Entra ID P2) |
| 8 | Joiner-Mover-Leaver (JML) Lifecycle | Planned |

## Progress

### Users & Directory Structure
Created a directory of 6 users spanning 5 departments, each with job title and department attributes set — the foundation for group-based and attribute-based access control.

### Group-Based Access
Created a security group (Finance-Team) with assigned membership, demonstrating how access is granted to groups rather than individuals so that permissions scale with the organization.

## Roadmap

The remaining components (dynamic groups, conditional access, PIM, access reviews) build on Entra ID P2 features and will be added as the project progresses.

## Screenshots

See the `/screenshots` folder for configuration evidence
