# Sentiq Campaigns

Version: 1.0

Status: Current (implementation complete; grant behavior can be environment-flagged)

---

# Purpose

Campaigns distribute Sentiq Pro (or related benefits) through shared partner, club or event codes.

They are a B2C acquisition and activation tool operated from the platform admin side.

They are **not** athlete-to-athlete referrals.

---

# What A Campaign Is

A campaign is an admin-defined offer with:

- Name and lifecycle status
- Start / end dates
- Generated shareable code
- Benefit definition (typically Pro access)
- Capacity limit
- Usage / participant tracking

One code is shared by many recipients until capacity is reached.

---

# What A Campaign Is Not

- Not a personal referral link owned by an athlete
- Not a Red follow invitation
- Not an organization roster invitation (those are separate Org flows)
- Not Early Adopter first-100 enrollment

See [referrals.md](./referrals.md) for the distinction table.

---

# Product Surfaces

## Admin (Current)

Sentiq Org super-admin:

- Create / edit / activate campaigns
- Configure capacity and benefit
- Export QR / link
- View participants and stats

## Athlete Redemption (Current)

Public campaign landing:

- Validate code
- Login / register if needed
- Join campaign and receive benefit when eligible

## Athlete App (Current)

If the athlete has campaign-origin Pro:

- Home can show `CampaignProHomeCard`
- Access is treated as effective Pro

The mobile app does not currently act as the primary code-entry surface; redemption is centered on the campaign web flow.

---

# Backend Concepts

Key models / services:

- `Campaign`
- `CampaignUser`
- Benefits / entitlement grant pipeline
- Feature flag such as `CAMPAIGN_BENEFITS_ENABLED` (grants may be disabled even if admin CRUD exists)

Behavior includes:

- Atomic capacity reservation
- Idempotent joins
- Handling athletes who already have Pro
- Rollback safety on failures

---

# Typical Use Cases

- Partner launches
- Tournament / camp promotions
- Club pilots with limited Pro seats
- Influencer or community drops with capacity caps

---

# Athlete Experience Principles

- Clear benefit explanation before join
- No false scarcity theater beyond real capacity
- Respect existing Pro sources (do not confuse access origin)
- After grant, athlete should land in productive product usage, not only a paywall

---

# Relationship With Monetization

Campaigns can:

- Accelerate activation with temporary or defined Pro access
- Create conversion opportunities after the grant ends
- Support partner-funded distribution

Campaigns should not:

- Replace a healthy free core experience
- Be the only reason the product feels valuable
- Be mis-labeled as referral growth in KPIs

---

# Metrics

- Campaigns created / active
- Joins vs capacity
- Activation of campaign users (first evaluation / objective)
- Retention of campaign cohort vs organic
- Conversion to paid after benefit ends
- Already-Pro skip / overlap rates

---

# Operational Flags

Treat environment flags as ops configuration:

- Campaign benefit granting may be on or off
- Admin management can remain available while grants are paused

Document production state in ops notes when relevant; keep this Knowledge base focused on product behavior.

---

# Related Documents

- [user-acquisition.md](./user-acquisition.md)
- [referrals.md](./referrals.md)
- [growth-loops.md](./growth-loops.md)
- [free-plan.md](./free-plan.md)
- [../03-product/premium.md](../03-product/premium.md)

---

# Final Principle

Campaigns help the right athletes discover Sentiq through trusted partners.

They distribute access.

They do not replace product value or personal advocacy.
