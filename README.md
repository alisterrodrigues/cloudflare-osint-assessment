# Cloudflare OSINT Security Assessment

<p align="center">
  <img src="https://img.shields.io/badge/Methodology-Passive_Reconnaissance-informational?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Target-Cloudflare_Inc.-orange?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Tools-WHOIS%20%7C%20BuiltWith%20%7C%20LinkedIn%20%7C%20HackerOne%20%7C%20CVE_Details-blue?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Framework-MITRE_ATT%26CK_TA0043-red?style=for-the-badge" />
</p>

---

## What This Is

A structured Open-Source Intelligence (OSINT) assessment of Cloudflare, Inc. — conducted using entirely passive techniques with no active scanning, no system interaction, and no automated scraping. All information was gathered through publicly accessible interfaces.

The assessment maps what an informed adversary could learn about Cloudflare before ever touching their systems: network infrastructure, technology stack, personnel profiles, hiring patterns, and historical vulnerability trends. It then translates those findings into concrete security implications and defensive recommendations.

This is the type of work that precedes a full external penetration test — the reconnaissance phase that determines where to look, what to probe, and who to target.

---

## Scope & Methodology

The assessment was structured across four phases, following the MITRE ATT&CK Reconnaissance tactic (TA0043):

| Phase | Objective | Tools Used |
|---|---|---|
| **Domain & Network Recon** | Map infrastructure, nameservers, IP ranges, mail architecture | WHO.IS, ViewDNS.info |
| **Technology Stack Profiling** | Identify web frameworks, CDN, analytics, payment, email systems | BuiltWith |
| **Personnel Intelligence** | Profile leadership, map workforce distribution, identify talent pipelines | LinkedIn |
| **Vulnerability Assessment** | Analyze bug bounty history, CVE trends, disclosed vulnerability patterns | HackerOne, CVE Details |

**Constraint:** Strictly passive throughout. No authentication, no direct system requests, no automated tooling beyond standard public interfaces.

---

## Key Findings

### Infrastructure
- Cloudflare operates its own domain registrar (IANA ID 1910) with domain protections through 2033
- Five dedicated nameservers in the `162.159.x.x` range handle all DNS resolution
- Mail infrastructure routes through `mailstream.mxrecord.io` — a Cloudflare-affiliated but externally hosted service — creating a third-party dependency in a critical communications channel
- Traceroute analysis revealed AWS infrastructure at hop 2, confirming a hybrid cloud architecture combining proprietary and third-party cloud services

### Technology Stack
- **Frontend:** jQuery (2014) + React (2020) dual-track codebase; Gatsby JS (2021) and Astro (2025) indicate active modernization
- **CDN:** Multi-provider strategy — Cloudflare's own CDN plus Amazon CloudFront
- **Analytics:** 20+ active integrations including Google Analytics 4, Cloudflare Insights, Adobe DTM, 6sense, Demandbase
- **Marketing automation:** Salesforce, Marketo, SalesLoft — all active as of April 2025
- **Payment:** Stripe (Nov 2020) as primary processor; Visa acceptance layer
- **Email security:** SPF + DMARC + DMARC Reject policies implemented; Zendesk and Mandrill for transactional mail
- **Deprecated:** PHP, Express.js, jQuery Waypoints — removed between 2023–2024

### Personnel Profile
- 5,318 LinkedIn-associated employees; 2,634 in the US (California and Texas as primary hubs); 584 in the UK; 342 in Portugal
- Technical-dominant workforce: 1,332 in IT, 1,141 in Engineering
- Top talent pipelines: University of Texas at Austin (86), Instituto Superior Técnico (56), UC Berkeley (56)
- Leadership transition in March 2025: John Graham-Cumming moved from CTO to Board Member
- Security leadership includes AI/ML-driven SecOps profiles with Zero Trust and cloud security expertise

### Vulnerability Patterns (2020–2025)
- **10 privilege escalation CVEs** — the single most common vulnerability class across the period
- **9 input validation CVEs** — consistent presence year-over-year
- **6 denial of service CVEs** — linked to large-scale infrastructure operations
- **Zero SQL injection or XSS CVEs** — suggests effective preventive controls against web application attack vectors
- **Highest-severity (CVSS ≥ 9.0):** CVE-2022-3320 (Zero Trust policy bypass), CVE-2021-3907 (OctoRPKI RCE via path traversal), CVE-2014-125026 (LZ4 memory corruption)
- **Most recent (2025):** CVE-2025-0651 — improper privilege management in WARP Windows client (CVSS 6.1, symlink attack vector)

### Bug Bounty Program (HackerOne)
- **$323,825 total paid** across 294 resolved reports
- **163 reports in the last 90 days** (as of April 2025), $13,725 paid in that window
- **57 assets in scope** — comprehensive coverage across the product portfolio
- Critical submissions concentrated in HTTP request smuggling, authorization bypass, and email routing flaws
- Single researcher ("albertspedersen") responsible for multiple critical findings — repeat success patterns suggest specific product areas warrant continued focus

---

## Risk Summary

| Risk Area | Severity | Basis |
|---|---|---|
| Supply chain — third-party script integrations | High | 20+ active third-party JS integrations on main domain; historical precedent for this attack class |
| HTTP request smuggling | High | Multiple critical disclosures; recurring pattern in Transform Rules and Origin Rules |
| Privilege escalation — client software | Medium | 10 CVEs 2020–2025; CVE-2025-0651 in WARP confirms ongoing exposure |
| Email infrastructure — third-party dependency | Medium | Externally hosted MX + historical critical vuln in Email Routing feature |
| Executive OSINT surface | Low–Medium | Location + role + transition data publicly aggregatable for spear-phishing |
| Legacy code coexistence | Low | jQuery still active; manageable with dependency pinning + SCA tooling |

---

## Defensive Takeaways

This assessment demonstrates a point that applies to any organization, not just Cloudflare:

**An adversary conducting this exact analysis — using only free, public tools — can build a targeting profile in hours.** The same profile takes most organizations months to detect as a threat.

The defensive response is not to remove all public information (that's neither realistic nor necessary) but to conduct this analysis on yourself first, understand what your exposure looks like to an informed outsider, and address the highest-leverage gaps before they're found by someone with less constructive intentions.

Specific mitigations applicable to findings in this assessment:
- Stateful inspection over stateless packet filtering
- Content Security Policy + Subresource Integrity for third-party scripts
- Software Composition Analysis (SCA) tooling for legacy dependency tracking
- Context-aware privilege models to address the escalation pattern
- Personnel guidance for executive information hygiene on public profiles

---

## Report

The full technical report is available in the `report/` directory.

> **[→ Read the Full OSINT Assessment Report](./report/)**

Covers complete methodology, all figures and data tables, section-by-section analysis, vulnerability deep dives, and detailed recommendations.

---

## Tools & Technologies

| Tool | Purpose |
|---|---|
| **WHO.IS** | WHOIS/RDAP domain registration analysis |
| **ViewDNS.info** | DNS record enumeration, traceroute analysis |
| **BuiltWith** | Technology stack profiling |
| **LinkedIn** | Personnel intelligence, workforce distribution |
| **HackerOne** | Bug bounty program analysis, disclosed vulnerability review |
| **CVE Details** | Historical CVE trend analysis by type and CVSS score |
| **MITRE ATT&CK** | Analytical framework — Reconnaissance tactic (TA0043) |

---
---

*Developed by Alister A. Rodrigues. All research conducted using publicly available information through passive means only. No Cloudflare systems were accessed, scanned, or interacted with during this assessment. This work is intended for defensive security research and education.*
