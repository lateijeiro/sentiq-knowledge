# Sentiq B2B Pricing Strategy

Version: 1.1

Status: Current package catalog + pricing strategy

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

- 2 rosters
- 40 players
- 6 coach/staff memberships
- 2 club admins

Overage rates:

- USD 20 per additional roster
- USD 2 per additional player
- USD 6 per additional coach/staff membership
- USD 12 per additional club admin

---

## Growth

Base price:

USD 449 / month / organization

Included capacity:

- 6 rosters
- 140 players
- 18 coach/staff memberships
- 4 club admins

Overage rates:

- USD 15 per additional roster
- USD 1.60 per additional player
- USD 5 per additional coach/staff membership
- USD 10 per additional club admin

---

## Performance

Base price:

USD 999 / month / organization

Included capacity:

- 20 rosters
- 500 players
- 60 coach/staff memberships
- 8 club admins

Overage rates:

- USD 12 per additional roster
- USD 1.20 per additional player
- USD 4 per additional coach/staff membership
- USD 8 per additional club admin

---

## Custom

Organizations can also use manual/custom billing and limits.

In this mode:

- Base price can be defined manually.
- Capacity limits can be defined manually.
- Overage rates can be defined manually.
- The displayed plan name is `Personalizado`.

This supports pilots, founding-club agreements and negotiated institutional contracts without creating another fixed package.

---

# Current Billing Behavior

Package billing is currently configured as:

- Monthly billing cycle
- USD currency
- Manual payment source
- Base price plus usage overages
- Period estimate based on current organization usage
- Invoice generation and PDF download
- Invoice states: pending, paid, void

The data model allows annual cycles and Stripe as possible values, but the implemented package builder currently uses monthly/manual billing. They should not be presented as active automated payment flows.

---

# Capacity And Usage Rules

Usage includes:

- Rosters
- Active players and pending player invitations
- Active coach/staff memberships and pending invitations
- Active club admins and pending invitations

Pending invitations count toward capacity to prevent organizations from bypassing limits by issuing invitations before acceptance.

When usage exceeds the included capacity, the estimate adds the corresponding unit overage.

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