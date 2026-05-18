# KIC Platform Lead — Technical Assessment

## Context

KIC is a health and fitness platform operating across a mobile app, Shopify ecommerce, Mindbody studio bookings. These systems currently hold separate customer records with no unified identity layer. This assessment asks you to design and partially build a solution to that problem.

The core challenge is **identity resolution**: the same person appears differently across every system. They may check out as a guest on Shopify, book a class under a different email in Mindbody, browse anonymously on mobile, and click a Facebook ad — all without a shared key. Your job is to design a system that stitches these signals together into a single canonical profile.

---

## The Assessment

### Part 1 — Architecture (~1 hour)

Fill in `ARCHITECTURE.md`. Your response should cover the data model, integration points, identity resolution strategy, failure modes, rollout approach, and a worked example of the "We Miss You" campaign use case described inside the file.

### Part 2 — Build (~2–3 hours, use as a guide not a hard stop)

Build a Next.js application in this repo. The folder structure and dependencies are provided. What you build, how you model the data, and the decisions you make are entirely up to you.

**The brief:** build a service that ingests webhook events from multiple sources (Shopify `order.created` and Mindbody `booking.created`), resolves each event to a canonical customer profile using whatever identity signals are present in the payload, stores the events, and exposes a simple internal-tool frontend where a KIC team member can look up a customer by any known signal (email, phone, device ID, or platform ID) and see their combined activity timeline.

Each webhook payload carries multiple identity signals — see `AGENTS.md` for payload shapes and the signal inventory. The payloads are intentionally designed so that the same real customer appears with different emails across systems, sometimes with no email at all, and can only be linked via overlapping signals (e.g. same phone number, or same device ID seen in two separate events).

Mock payload shapes and resolution rules are in `AGENTS.md`.

### Bonus (optional)

- Normalise signals before matching: lowercase + trim emails, E.164 phone numbers
- Ensure duplicate webhook delivery does not create duplicate records — document how
- Add a filter on the frontend to view only orders or only bookings
- Record merge provenance: when two profiles are unified, store which signal triggered the merge and when, so it can be reviewed or reversed

---

## Setup

```bash
cp .env.example .env
npm install
npm run dev
```

The Prisma schema (`prisma/schema.prisma`) has a minimal starting point — extending the data model is part of the exercise. Once you have your models, run:

```bash
npx prisma migrate dev --name <descriptive-name>
```

`prisma studio` (`npm run db:studio`) is available once the database is migrated.

---

## Submission

Push to your own GitHub repo and share the link. Fill in `NOTES.md` with your assumptions, tradeoffs, and what you'd do differently with more time.

---

## AI Tools

You are welcome to use AI tools (Claude, Codex, Cursor, Copilot, etc.). We care about the decisions you make and how you reason about the problem, not whether you typed every line yourself.

---

## What We're Looking For

Clear architectural thinking, clean pragmatic code, good error handling, and honest reasoning. We are not looking for perfection.
