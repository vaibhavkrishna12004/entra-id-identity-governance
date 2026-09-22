# Joiner–Mover–Leaver (JML) Identity Lifecycle
When I built this Entra environment, I kept coming back to one idea: every control I set up only makes sense as part of an identity's journey through the organization. A user joins, changes roles, and eventually leaves — and access has to keep pace at every step. That journey is the Joiner–Mover–Leaver model, and this is how I mapped each stage to what I actually built.

## Joiner

When someone joins, they need an identity and the right baseline access straight away enough to do their job, nothing more.
When I provisioned my users, I set their department and job title attributes deliberately, because that attribute data is what does the work later. A new IT hire lands in the `IT-Team-Dynamic` group automatically — I don't add them, the dynamic rule does, based on their department. They inherit the correct access the moment their account exists. And because I built the Conditional Access MFA policy across all users, they're covered by baseline security from their very first sign-in.
The piece I couldn't build on a trial tenant is SCIM provisioning in a real setup that would push the new identity straight into downstream apps like Slack or Salesforce at the moment of creation, so the joiner is fully set up everywhere without manual account-making.

## Mover

This is the stage organizations quietly get wrong. When someone changes role, their old access is supposed to fall away and new access replace it. When it doesn't, you get privilege creep people slowly accumulating access they no longer need until their account is a security liability.
What I built handles the core of this automatically: change a user's department attribute and they drop out of their old dynamic group and into the new one old access gone, new access granted, no manual step. But dynamic groups only catch attribute-driven access. That's exactly why I set up Access Reviews — the periodic recertification that forces someone to look at group membership and confirm it's still justified, catching the residual access that a role change leaves behind.
In a fuller setup, PIM closes the last gap here any elevated admin role someone picked up for their old position is time-bound and expires, rather than following them into the new role.

## Leave
When someone leaves, every door has to close, fast. A dormant but still enabled account is one of the most common ways breaches happen.
I made this real rather than theoretical I disabled a departing user's account (Aisha Khan in Sales). The moment an account is disabled in Entra, authentication is blocked everywhere that identity connects. That's the power of centralizing identity: one action, and access is revoked across every connected app at once. Removing them from their groups strips group-based access in the same stroke, and Access Reviews act as the backstop that surfaces any account that *should* have been offboarded but slipped through.
The real-world extension is SCIM again automated deprovisioning that deletes or disables the user's downstream app accounts the instant they're disabled in Entra, so nothing is left behind in a system someone forgot about.

## Why this framing matters to me
Working through JML changed how I see the individual controls. Dynamic groups, Conditional Access, PIM, access reviews on their own they're just features. Strung along the lifecycle of a real person joining, moving, and leaving, they become a system. Most access-related breaches trace back to a broken point in this chain — usually a leaver who never lost access, or a mover who kept piling up privilege. Building identity around JML, with automation doing the routine work and detective controls catching what slips, is what separates a governed environment from a directory full of accounts nobody is watching.
