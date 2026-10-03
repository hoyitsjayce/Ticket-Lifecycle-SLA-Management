# Help Desk Ticket Lifecycle & SLA Management

Freshservice (ITSM) · Active Directory · Windows Server

Hi,

In this hands-on project I simulate a full IT help desk ticket lifecycle — 
priority triage, SLA enforcement, and tiered escalation — and resolve a subset 
of tickets against real Active Directory infrastructure rather than hypothetical fixes.

## Table of Contents
1. Project Overview
2. Environment
3. How Triage & SLA Enforcement Works
4. What Was Built
5. Phase 1 — Priority Matrix & SLA Policy
6. Phase 2 — Ticket Queue
7. Phase 3 — Triage
8. Phase 4 — Resolution Against Live Infrastructure
9. Errors & Troubleshooting
10. Key Concepts Demonstrated
11. Tools & Technologies
12. Project Status

## Project Overview

The goal of this project was to simulate what it looks like when an IT help 
desk enforces consistent prioritization and escalation standards, rather than 
leaving triage decisions to individual technician judgment.

A 9-ticket queue was built spanning the full priority spectrum. Each ticket's 
priority was derived from an Impact × Urgency matrix rather than assigned 
arbitrarily. Tickets requiring escalation were routed to Tier 2 with documented 
reasoning, and a subset of tickets were resolved against a real Active Directory 
domain controller rather than closed with placeholder notes.

| Component | Detail |
|---|---|
| Ticketing Platform | Freshservice (Enterprise trial) |
| Domain | jayce.local |
| Domain Controller | Windows Server 2022 (on-prem, ADDC01) |
| Priority Model | Impact × Urgency matrix → Low/Medium/High/Urgent |
| SLA Enforcement | Tiered response/resolve targets, business-hours aware, auto-escalation |

## How Triage & SLA Enforcement Works

1. Ticket arrives → Impact and Urgency assessed
   ↓
2. Priority assigned via matrix (Single User/Department/Org-wide × 
   Workaround Exists/Degraded/Fully Blocked)
   ↓
3. SLA clock starts based on priority (response + resolution targets)
   ↓
4. Triage decision made: Tier 1 scope, or escalate (policy-based or 
   access-based)?
   ↓
5. If SLA threshold is breached, automatic escalation notifies Tier 2
   ↓
6. Ticket resolved, documented, and closed

**Priority Matrix:**

| Impact ↓ / Urgency → | Workaround Exists | Degraded, No Workaround | Fully Blocked |
|---|---|---|---|
| Single User | Low | Medium | High |
| Department/Team | Medium | High | Urgent |
| Org-wide/Company | High | Urgent | Urgent |

## What Was Built

A complete ticket lifecycle environment including:

- A **4-tier SLA policy** (Urgent/High/Medium/Low) with response and 
  resolution targets, business-hours aware
- **Escalation rules** auto-notifying Tier 2 Support if response (30 min) or 
  resolution (1 hr) thresholds are breached
- A **9-ticket queue** spanning Hardware, Software, Network, and Onboarding 
  categories across all four priority levels
- **Triage documentation** on 5 tickets — reasoning for each Tier 1 vs Tier 2 
  routing decision
- **3 tickets resolved against real lab infrastructure** (not placeholder fixes)

## Folder Structure
