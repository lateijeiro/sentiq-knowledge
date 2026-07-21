# Sentiq B2B Pricing Strategy

Version: 1.2

Status: Current implemented package catalog + pricing strategy

---

# Purpose

This document defines the pricing strategy for Sentiq B2B.

The objective is to establish a sustainable commercial model for sports organizations while maintaining accessibility for clubs that want to improve athlete development.

---

# Pricing Philosophy

Sentiq should not be positioned as simple software licensing.

The value is not only access to a platform.

The value comes from:

- A development methodology.
- Better athlete understanding.
- Better coach-athlete conversations.
- A structured development process.

---

# Commercial Principle

The pricing model should balance:

- Accessibility for clubs.
- Sustainable business growth.
- Value generated for organizations.
- Scalability.

---

# B2B Customer Model

The main customer is the organization.

Examples:

- Rugby, hockey, football and basketball clubs.
- Sports academies.
- Training programs.
- Youth development organizations.
- Organizations from any other sport.

---

# Pricing Variables

Pricing can consider:

---

## Number Of Athletes

Organizations with more athletes generate more value and require more capacity.

---

## Number Of Coaches

More coaches increase the scope of adoption.

---

## Number Of Categories

Clubs with multiple categories require broader implementation.

---

## Level Of Support

Some organizations may require:

- Onboarding.
- Training.
- Follow-up.
- Strategic support.

---

# Current Package Catalog

The implemented Sentiq Org catalog has three monthly packages in USD.

Package differences currently define organization capacity and overage rates. They do not, by themselves, prove feature-level entitlement differences.

---

## Starter

Base price:

USD 199 / month / organization

Included capacity:

- 150 athletes
- Unlimited roster structure
- No commercial cap on coach/staff memberships
- No commercial cap on club admins

Athlete overage:

- USD 2 per additional player

Growth becomes more cost-effective at 250 athletes, when Starter plus overage reaches USD 399.

---

## Growth

Base price:

USD 399 / month / organization

Included capacity:

- 400 athletes
- Unlimited roster structure
- No commercial cap on coach/staff memberships
- No commercial cap on club admins

Athlete overage:

- USD 1.60 per additional player

Performance becomes more cost-effective at 650 athletes, when Growth plus overage reaches USD 799.

---

## Performance

Base price:

USD 799 / month / organization

Included capacity:

- 1000 athletes
- Unlimited roster structure
- No commercial cap on coach/staff memberships
- No commercial cap on club admins

Athlete overage:

- USD 1.20 per additional player

From athlete 1001 onward, the product keeps the overage estimate visible and marks the organization as requiring a Custom plan.

---

## Custom

Organizations can also use manual/custom billing and limits.

In this mode:

- Base price can be defined manually.
- Athlete capacity can be defined manually.
- Athlete overage can be defined manually.
- The displayed plan name is `Custom`.

This supports pilots, founding-club agreements and negotiated institutional contracts without creating another fixed package.

---

# Current Billing Behavior

Package billing is currently configured as:

- Monthly billing cycle
- USD currency
- Manual payment source
- Base price plus athlete overage
- Period estimate based on current organization usage
- Upgrade recommendation when overage reaches the next package price
- Custom-required warning when Performance exceeds 1000 athletes
- Invoice generation and PDF download
- Invoice states: pending, paid, void

The data model allows annual cycles and Stripe as possible values, but the implemented package builder currently uses monthly/manual billing. They should not be presented as active automated payment flows.

---

# Existing Organization Migration

Existing organizations assigned to Starter, Growth or Performance must have their stored package snapshot updated when this catalog is deployed.

The backend includes a dry-run migration:

`npm run migrate:org-packages-v2`

After reviewing the listed organizations, apply it with:

`npm run migrate:org-packages-v2 -- --apply`

This sets roster, staff and club-admin commercial limits and overage rates to `null`, while preserving those schema fields for possible future reuse.

---

# Capacity And Usage Rules

Commercial capacity usage includes:

- Active players and pending player invitations

Pending invitations count toward capacity to prevent organizations from bypassing limits by issuing invitations before acceptance.

Rosters, coach/staff memberships and club admins remain available as operational metrics but have no package cap or overage.

When athlete usage exceeds the included capacity, the estimate adds the corresponding player overage. If that projected total reaches the next fixed package price, Sentiq recommends upgrading rather than charging a less favorable total.

---

# Current Commercial Philosophy

The objective of the catalog is not only to maximize revenue.

It should help Sentiq:

- Validate willingness to pay.
- Validate adoption at different organization sizes.
- Create successful customer cases.
- Understand perceived value.
- Maintain predictable capacity and support expectations.

---

# Clubes Fundadores And Pilots

Founding clubs and pilots may receive custom conditions such as:

- Preferential pricing.
- Extended pilot period.
- Custom limits.
- Direct product feedback channel.

These conditions should be represented as a Custom agreement rather than changing the fixed package definitions for every customer.

---

# Pricing Should Reward Commitment

Organizations that commit to:

- More athletes.
- Longer relationships.
- Strategic collaboration.

can receive negotiated conditions through a Custom agreement.

---

# B2C Relationship

B2C and B2B have different pricing logic.

The organization pays for:

- Athlete-development methodology.
- Organization-wide adoption.
- Coach enablement.
- Capacity and operational visibility.
- Institutional value.

B2C pricing should be maintained in the product/Pro documentation rather than treated as part of the Sentiq Org package catalog.

---

# Revenue Expansion Opportunities

Future opportunities:

---

## Additional Services

Examples:

- Training programs.
- Development workshops.
- Coach education.

---

## Strategic Partnerships

Examples:

- Sponsors.
- Foundations.
- Sports organizations.

---

## Institutional Programs

Examples:

- Athlete development initiatives.
- Social impact programs.

---

# Commercial Risks

---

# Pricing Too Low

Risk:

The product may be perceived as low value.

---

# Pricing Too High

Risk:

Clubs may not adopt before value is proven.

---

# Complex Pricing

Risk:

Sales conversations become harder.

The initial model should remain simple.

---

# Pricing Validation

Important questions:

- Are clubs willing to pay?
- Does adoption justify the investment?
- What value do clubs perceive?
- Which features create the most value?

---

# Success Metrics

Measure:

## Commercial

- Number of paying clubs.
- Monthly recurring revenue.
- Average revenue per club.

---

## Adoption

- Athletes per club.
- Active coaches.
- Usage frequency.

---

## Retention

- Club renewal.
- Expansion within organizations.

---

# Long-Term Vision

Sentiq pricing should evolve as the platform proves its impact.

The objective is to build a sustainable business while helping more organizations improve athlete development.

---

# Final Principle

Sentiq should not compete on being the cheapest tool.

It should compete on creating the most valuable athlete development experience.