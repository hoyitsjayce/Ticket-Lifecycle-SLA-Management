# Help Desk Ticket Lifecycle & SLA Management

Freshservice · Active Directory · Windows Server

Hi,

In this hands-on project I simulate a full IT help desk ticket lifecycle, 
priority triage, SLA enforcement, and tiered escalation, and resolve a subset 
of tickets against real Active Directory infrastructure.

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

<img width="452" height="168" alt="priority matrix" src="https://github.com/user-attachments/assets/b0269c73-9444-4013-9088-8b0fcb711ae0" />

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

<img width="452" height="168" alt="priority matrix" src="https://github.com/user-attachments/assets/548d0c56-3615-4d7b-97bc-bd6470acdbb4" />


**SLA targets configured:**

| Priority | Respond Within | Resolve Within | Operational Hours |
|---|---|---|---|
| Urgent | 1 hr | 4 hrs | Business Hours |
| High | 4 hrs | 8 hrs | Business Hours |
| Medium | 8 hrs | 24 hrs | Business Hours |
| Low | 24 hrs | 72 hrs | Business Hours |

<img width="1918" height="842" alt="Screenshot 2026-08-29 190013" src="https://github.com/user-attachments/assets/583be9c2-2117-41ac-a270-3b5232ad3c03" />

**Escalation rules:** response escalation fires 30 minutes before the SLA 
deadline; resolution escalation fires 1 hour before deadline — both route to 
Tier 2 Support.

<img width="1919" height="849" alt="Screenshot 2026-08-29 193549" src="https://github.com/user-attachments/assets/6af28377-e47a-4ac1-98b9-dc7c94a55d48" />


## Phase 2 — Ticket Queue

9 tickets were logged, each assigned priority via the matrix above rather than 
by guesswork:

<img width="1917" height="848" alt="Screenshot 2026-08-29 202954" src="https://github.com/user-attachments/assets/f1c50fad-89b0-47fa-a6d4-8175c7db73a4" />


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

<img width="1917" height="882" alt="Triage 2" src="https://github.com/user-attachments/assets/2f70da32-35ed-46dd-944f-d0148a55ea7c" />

<img width="1919" height="883" alt="Triage 1" src="https://github.com/user-attachments/assets/a1ab40fa-9bfc-423d-b1b4-a47aef9075ba" />

## Phase 4 — Resolution Against Live Infrastructure

Three tickets were resolved against a real Windows Server Active Directory 
domain controller (ADDC01) rather than closed with a placeholder note.

**Domain controller health check**

```powershell
Get-Service NTDS, DNS, Netlogon, Kdc
```

<img width="1021" height="679" alt="Get-Service" src="https://github.com/user-attachments/assets/c8f913bd-9a5b-4090-9cf4-94b74b9dbb12" />


```powershell
nslookup jayce.local
```

<img width="1021" height="684" alt="nslookup" src="https://github.com/user-attachments/assets/a797efa3-2b17-4d45-a9bf-9ffe1d3d292b" />

**Conclusion:** all four services confirmed Running, DNS resolved correctly — 
no actual outage. Diagnostics ruled out server-side failure, pointing to a 
transient client-side issue instead.

**Account lockout**

Configured a domain account lockout policy (Default Domain Policy → Account 
Lockout Policy, threshold: 5 attempts), triggered a real lockout against a 
domain account, confirmed in Active Directory Users and Computers.

<img width="1022" height="767" alt="accountlockout" src="https://github.com/user-attachments/assets/59d3a0fc-87b6-4bf8-8c01-95b1712f55be" />

Resolved via "Unlock account," verified with a successful login.

<img width="1013" height="768" alt="successfullogin" src="https://github.com/user-attachments/assets/8d815edf-2f64-447f-b989-18407ce5433f" />

**Print Spooler failure**

Stopped the Print Spooler service, reproducing a genuine Windows error:

<img width="1020" height="723" alt="printererror" src="https://github.com/user-attachments/assets/6208c1bb-022e-4df8-b4db-cf6db8d65acc" />

Restarted the service via `services.msc`, confirmed print functionality restored.

<img width="802" height="589" alt="printspoolerrunning" src="https://github.com/user-attachments/assets/3630e424-0fca-4d17-be64-2e889ea89655" />

## Resolution Notes

#INC-1
Diagnostic findings: Logged into ADDC01 (domain controller) and verified core AD servicesL NTDS (Active Directory Domain Services), DNS Server, Netlogon, and Kdc (Kerberos Key Distribution Center) through Get-Service. Ran nslookup against the domain (jayce.local) and confirmed correct resolution to the DC's IP (192.168.10.7) ruling out DNS as a cause.

Conclusion: No actual domain controller outage found. All authentication-related services were healthy and DNS resolution was functioning correctly. Likely cause was a client-side issue, local network hiccup, or isolated user error rather than an infrastructure failure.

Resolution: Advised reporting users to retry login. Recommend monitoring for recurrence and if the issue repeats, investigate client-side rather than server side, since server health is confirmed. 

#INC-9
Diagnostic findings: Confirmed account lockout in Active Directory Users and Computers (ADUC). User's domain account showed "locked out" status under the Account tab.

Resolution: Unlocked the account via ADUC. Verified fix by logging in from the affected endpoint with correct credentials.

Root cause: Repeated failed authentication attempts (likely due to an expired/forgotten password) triggered the domain's account lockout policy.

Preventive note: If this becomes a recurring issue for this user, consider reviewing for a stale cached credential on a mobile device or secondary machine that's repeatedly submitting the old password in background. 

#INC-5
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
infrastructure. Platform migration to osTicket in progress for continued, 
trial-independent lab work.
