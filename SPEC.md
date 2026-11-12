# Archer Product Specification

**Status:** MVP scope confirmed; detailed behavior remains open where marked below  
**Product:** Archer, a freelance marketplace

## Product summary

Archer helps clients find independent talent and helps freelancers discover work, submit proposals, and talk with clients about a project. The first release is web-first. Mobile is a later client for the same product and API.

## Accounts and roles

- Archer has three account roles: **client**, **freelancer**, and **admin**.
- Each account has exactly one role.
- Clients and freelancers can use the marketplace workflows below.
- Admin product capabilities are not yet defined.

## Confirmed MVP features

### Accounts and profiles

- Users can register and sign in.
- Client and freelancer accounts have role-specific profiles.
- Users can view and update their own profile.
- Public profiles can be viewed by other users.

### Jobs

- Clients can create and publish a job posting.
- People can browse and search open job postings.
- Job details show the scope, engagement type, budget, and currency.
- A client can close a job they own.

### Proposals

- A freelancer can submit a proposal to an open job.
- A proposal includes an introductory message and may include a bid.
- Freelancers can view and withdraw their own pending proposals.
- Clients can review proposals on their own jobs and reject a pending proposal.
- Submitting a proposal creates a conversation between that freelancer and the job's client.
- Hiring or accepting a proposal is not part of the confirmed MVP.

### Messaging

- A client and a freelancer can exchange messages in the conversation created by a proposal.
- Only conversation participants can view or send messages in that conversation.
- Users can view their own conversations.

### Language

- The web interface supports English and Burmese (Myanmar).
- Users can switch languages, and Archer remembers their choice on that browser.
- User-authored job, profile, proposal, and message content remains in the language it was written; Archer does not automatically translate it.

## Currency and payments

- Job budgets and proposal bids can be represented in **USD** or **MMK**.
- The selected currency is shown with each amount; users are not shown converted estimates.
- Archer does not convert between currencies or process payments in the MVP.
- Amount precision and display rules, including fractional MMK values, remain to be decided.

## Deferred and out of scope

- Mobile app workflows are deferred until the web-first MVP is established. Mobile will use the Archer API.
- Payment processing, escrow, payouts, and stored balances are out of scope.
- Currency conversion and live exchange rates are out of scope.
- Hiring/contracts, milestones, reviews, and disputes are not in the confirmed MVP.
- Admin workflows are not yet defined.

## Product decisions still open

1. Which profile fields are required for clients and freelancers?
2. Which job fields are required, and can clients edit or reopen a job after publishing or closing it?
3. Can freelancers edit or withdraw a proposal after it has been reviewed? What proposal states should users see?
4. Can clients accept a proposal, and if so, what does acceptance mean without contracts or payments?
5. Can users start conversations outside a proposal, and are attachments or read indicators needed?
6. Should amounts allow fractional USD or MMK values? How should each currency be formatted?
7. What admin tasks are needed, and how should admin access be provisioned and protected?
8. Which regions should the first release support?
9. Which confirmed workflows should mobile support, and should it launch on iOS, Android, or both?
