# Help Desk Ticket Lifecycle & SLA Management

Freshservice · Active Directory · Windows Server

Hi,

In this hands-on project I simulate a full IT help desk ticket lifecycle, 
priority triage, SLA enforcement, and tiered escalation, and resolve a subset 
of tickets against real Active Directory infrastructure.

## Table of Contents
1. [Project Overview](#project-overview)
2. [How Triage & SLA Enforcement Works](#how-triage--sla-enforcement-works)
3. [What Was Built](#what-was-built)
4. [Phase 1 — Priority Matrix & SLA Policy](#phase-1--priority-matrix--sla-policy)
5. [Phase 2 — Ticket Queue](#phase-2--ticket-queue)
6. [Phase 3 — Triage](#phase-3--triage)
7. [Phase 4 — Resolution Against Live Infrastructure](#phase-4--resolution-against-live-infrastructure)
8. [SLA Report](#sla-report)
9. [Resolution Notes](#resolution-notes)
10. [Key Concepts Demonstrated](#key-concepts-demonstrated)
11. [Tools & Technologies](#tools--technologies)
12. [Project Status](#project-status)

## Project Overview

The goal of this project was to simulate what it looks like when an IT help 
desk enforces consistent prioritization and escalation standards, rather than 
leaving triage decisions to individual technician judgment.

| Component | Detail |
|---|---|
| Ticketing Platform | Freshservice (Enterprise trial) |
| Domain | jayce.local |
| Domain Controller | Windows Server 2022 (on-prem, ADDC01) |
| Priority Model | Impact × Urgency matrix → Low/Medium/High/Urgent |

## How Triage & SLA Enforcement Works

1. Ticket arrives → Impact and Urgency assessed
   ↓
2. Priority assigned via matrix
   ↓
3. SLA clock starts based on priority
   ↓
4. Triage decision made: Tier 1 scope, or escalate?
   ↓
5. If threshold breached, automatic escalation notifies Tier 2
   ↓
6. Ticket resolved, documented, closed

**Key terms:**

| Term | Meaning |
|---|---|
| Impact | How many people are affected — single user, department, or org-wide |
| Urgency | How severely it's affecting them — workaround exists, degraded, or fully blocked |
| SLA | The target time to respond to and resolve a ticket based on priority |
| Tier 1 / Tier 2 | First-line support vs. escalated support requiring deeper access or specialized skill |

## What Was Built

- A **4-tier SLA policy** with response and resolution targets, business-hours aware
- **Escalation rules** auto-notifying Tier 2 Support on threshold breach
- A **9-ticket queue** spanning Hardware, Software, Network, and Onboarding categories
- **Triage documentation** on 5 tickets with Tier 1 vs Tier 2 reasoning
- **3 tickets resolved against real lab infrastructure**

## Phase 1 — Priority Matrix & SLA Policy

Before logging any tickets, a priority matrix and SLA policy were defined to 
remove guesswork from triage.

<img width="452" height="168" alt="output-onlinepngtools" src="https://github.com/user-attachments/assets/0f57ce48-fc70-48a3-901e-fa209f436044" />

**SLA targets configured:**

| Priority | Respond Within | Resolve Within | Operational Hours |
|---|---|---|---|
| Urgent | 1 hr | 4 hrs | Business Hours |
| High | 4 hrs | 8 hrs | Business Hours |
| Medium | 8 hrs | 24 hrs | Business Hours |
| Low | 24 hrs | 72 hrs | Business Hours |

<img width="1918" height="842" alt="Screenshot 2026-08-29 190013" src="https://github.com/user-attachments/assets/21e479a1-f2c4-4d8d-b94f-2a37b619c9f2" />

**Escalation rules:** response escalation fires 30 minutes before the SLA 
deadline; resolution escalation fires 1 hour before deadline and both route to 
Tier 2 Support.

<img width="1919" height="849" alt="Screenshot 2026-08-29 193549" src="https://github.com/user-attachments/assets/cd87f76e-8b1c-4461-949a-42bf9a79f54a" />


## Phase 2 — Ticket Queue

9 tickets were logged, each assigned priority via the matrix above rather than 
by guesswork:

<img width="1917" height="848" alt="Screenshot 2026-08-29 202954" src="https://github.com/user-attachments/assets/c8b463e0-6550-4df4-b865-dcae27733d0a" />


| Priority | Count | Examples |
|---|---|---|
| Urgent | 2 | Domain controller unreachable, Ransomware alert |
| High | 2 | VPN client failure, MFA lockout |
| Medium | 2 | New hire onboarding, Shared drive access |
| Low | 3 | Printer offline, software install request, distribution list |

## Phase 3 — Triage

Each ticket was evaluated for Tier 1 vs Tier 2 routing, with reasoning 
documented at the time of the decision:

| Ticket | Priority | Decision | Reasoning |
|---|---|---|---|
| Domain controller unreachable | Urgent | → Tier 2 | Org-wide impact, requires server-level access beyond Tier 1 scope |
| Ransomware alert | Urgent | → Tier 2 | Security incident policy mandates immediate escalation |
| Printer offline | Low | Tier 1 | Standard connectivity issue, within first-line scope |
| New hire laptop | Medium | Tier 1 | Standard onboarding task |
| VPN client failure | High | Tier 1 (time-boxed) | Attempting standard troubleshooting first; escalate within the hour if unresolved |

<img width="1917" height="882" alt="Triage 2" src="https://github.com/user-attachments/assets/04e4d886-bf2d-4973-abfc-719676f865dd" />

<img width="1919" height="883" alt="Triage 1" src="https://github.com/user-attachments/assets/72c831f7-f980-4be6-a9a1-32bc62b3aa50" />

<img width="1915" height="879" alt="Triage 4" src="https://github.com/user-attachments/assets/11c2e42c-34b9-47d3-82d2-a001e6c72165" />

<img width="1918" height="883" alt="Triage 5" src="https://github.com/user-attachments/assets/d958e46d-c87f-45ae-9cb2-db70239277dd" />

<img width="1918" height="953" alt="Triage 3" src="https://github.com/user-attachments/assets/6be77d12-06d8-40ff-83da-8ecd1270ab1e" />

## Phase 4 — Resolution Against Live Infrastructure

Three tickets were resolved against a real Windows Server Active Directory 
domain controller (ADDC01) rather than closed with a placeholder note.

**INC-1 "Domain controller unreachable, no one can log in" (Domain Controller Health Check)**

```powershell
Get-Service NTDS, DNS, Netlogon, Kdc
```

<img width="1021" height="679" alt="Get-Service" src="https://github.com/user-attachments/assets/c8f913bd-9a5b-4090-9cf4-94b74b9dbb12" />


```powershell
nslookup jayce.local
```

<img width="1021" height="684" alt="nslookup" src="https://github.com/user-attachments/assets/a797efa3-2b17-4d45-a9bf-9ffe1d3d292b" />

**Conclusion:** all four services confirmed Running, DNS resolved correctly. 
No actual outage. Diagnostics ruled out server-side failure, pointing to a 
transient client-side issue instead.

**INC-9 "Multi-factor authentication not sending codes, user locked out of all systems." (Account lockout)**

Configured a domain account lockout policy (Default Domain Policy → Account 
Lockout Policy, threshold: 5 attempts)

<img width="1021" height="680" alt="grouppolicy" src="https://github.com/user-attachments/assets/16062ab8-6407-4711-8be3-41c215bdbdd0" />

Triggered a real lockout against a domain account, confirmed in Active Directory Users and Computers.

<img width="1022" height="767" alt="accountlockout" src="https://github.com/user-attachments/assets/b8bb1df6-b2a1-4697-a478-95a7034a63ff" />

Resolved via "Unlock account," verified with a successful login.
<img width="803" height="591" alt="unlockaccount" src="https://github.com/user-attachments/assets/1327767c-a0eb-4cc1-8889-6b2127e48dc7" />

Confirmed Successful Login
<img width="1013" height="768" alt="successfullogin" src="https://github.com/user-attachments/assets/b6ddad25-add7-49d6-a79f-56a0130c24a2" />

**Inc-5 "Printer offline in accounting" (Print Spooler failure)**

Stopped the Print Spooler service, reproducing a genuine Windows error:

<img width="1020" height="723" alt="printererror" src="https://github.com/user-attachments/assets/6208c1bb-022e-4df8-b4db-cf6db8d65acc" />

Restarted the service via `services.msc`, confirmed print functionality restored.

<img width="802" height="589" alt="printspoolerrunning" src="https://github.com/user-attachments/assets/f4697f7a-9bda-46b1-9168-0dde578b4fb9" />

## SLA Report

| Ticket | Priority | SLA Target (Resolve) | Outcome | SLA Status |
|---|---|---|---|---|
| #INC-1 | Urgent | 1 hr | Resolved — no outage found | Breached |
| #INC-9 | High | 8 hrs | Resolved — account unlocked | Breached |
| #INC-5 | Low | 72 hrs | Resolved — spooler restarted | Breached |

**SLA Breach Rate:** 3 of 3 tracked tickets breached (100%)

**Root cause:** Project work was paused for several days mid-cycle after 
initial ticket creation. All three tickets were correctly triaged and prioritized before the delay occurred. The breach reflects elapsed project time, not misdiagnosis.

## Resolution Notes

**#INC-1**
Diagnostic findings: Logged into ADDC01 (domain controller) and verified core AD services NTDS (Active Directory Domain Services), DNS Server, Netlogon, and Kdc (Kerberos Key Distribution Center) through Get-Service. Ran nslookup against the domain (jayce.local) and confirmed correct resolution to the DC's IP (192.168.10.7) ruling out DNS as a cause.

Conclusion: No actual domain controller outage found. All authentication-related services were healthy and DNS resolution was functioning correctly. Likely cause was a client-side issue, local network hiccup, or isolated user error rather than an infrastructure failure.

Resolution: Advised reporting users to retry login. Recommend monitoring for recurrence and if the issue repeats, investigate client-side rather than server side, since server health is confirmed. 

**#INC-9**
Diagnostic findings: Confirmed account lockout in Active Directory Users and Computers (ADUC). User's domain account showed "locked out" status under the Account tab.

Resolution: Unlocked the account via ADUC. Verified fix by logging in from the affected endpoint with correct credentials.

Root cause: Repeated failed authentication attempts (likely due to an expired/forgotten password) triggered the domain's account lockout policy.

Preventive note: If this becomes a recurring issue for this user, consider reviewing for a stale cached credential on a mobile device or secondary machine that's repeatedly submitting the old password in background. 

**#INC-5**
Diagnostic findings: Investigated reported printer offline issue. Attempted to access printer functions on the affected endpoint and received the error: "Windows can't open Add Printer. The local print spooler service is not running." Confirmed via services that the Print Spooler service was stopped.

Root Cause: Print Spooler service had stopped, which disables all printing functionality system-wide regardless of the printer hardware's status.

Resolution: Restarted the Print Spooler service via Services console. Confirmed printing functionality restored.

Preventive note: If this recurs frequently on the same machine, consider setting the Print Spooler service's recovery options to automatically restart on failure.

## Key Concepts Demonstrated

- Impact × Urgency priority matrix — removing subjectivity from triage
- SLA policy design — response/resolution targets, business-hours-aware clocks
- Tiered escalation — policy-based vs access-based reasons to route beyond Tier 1
- Real AD domain controller diagnostics (`Get-Service`, `nslookup`)
- Account lockout policy configuration and resolution via ADUC
- Windows service administration (Print Spooler) as root-cause diagnosis

## Tools & Technologies

Freshservice · Windows Server 2022 · Active Directory Domain 
Services · Group Policy Management · PowerShell

## Project Status

✅ Complete — Priority matrix, SLA policy, 9-ticket queue, and triage 
documentation finished in Freshservice. 3 tickets resolved against live AD 
infrastructure.
