# Case Study: Operational Take-down of a Targeted Phishing Infrastructure Cluster

**Author:** White Fang  
**Role:** Threat Intelligence & Incident Response Specialist  
**Date:** May 28, 2026  
**Status:** Mitigated / Infrastructure Suspended  

---

## Executive Summary
In May 2026, a highly targeted credential-harvesting campaign was detected impersonating critical government transportation and law enforcement frameworks in Azerbaijan—specifically the Ministry of Internal Affairs (**DİN**) and State Traffic Police (**DYP**). The threat actors deployed a localized web application titled `DİN | Yay Cərimə Portalı 2026` designed to defraud citizens and exfiltrate banking credentials.

This case study documents the end-to-end lifecycle of the investigation: from initial passive OSINT discovery and infrastructure fingerprinting to the **secure execution of an international take-down operation** via upstream registrars and national regulatory authorities (CERT).

---

## Technical Analysis & Infrastructure Footprint

### 1. Host Profile (Network Intelligence)
Perimeter routing analysis revealed that the threat actor bypassed standard Content Delivery Network (CDN) proxy masking (e.g., Cloudflare), exposing their origin server directly to the public internet:

*   **Origin IPv4 Address:** `209.74.85.172`
*   **Assigned CIDR Network:** `209.74.64.0/19` (NAMEC-4)
*   **Autonomous System (ASN):** `AS22612` — Namecheap, Inc.
*   **Regional Internet Registry (RIR):** ARIN
*   **Geolocation:** United States (US)

### 2. Adversary Server Fingerprint
Active infrastructure scanning on Port **443/TCP** provided deep insights into the threat actor's operating environment:
*   **Operating System Platform:** `AlmaLinux` (RHEL-based Enterprise Linux)
*   **Web Server Daemon:** `Apache/2.4.63`
*   **Cryptographic Layer:** `OpenSSL/3.5.1`
*   **Backend Application Environment:** `PHP/8.3.29`
*   **HTTP Protocol Status:** `200 OK` (Confirmed live payload delivery)

### 3. JARM TLS Fingerprint
Advanced Transport Layer Security (TLS) client-hello probing yielded the following JARM signature:
```text
27d40d40d00040d00042d43d000000b5ce48eb9aaa95d750e8df42b900e12b
Analyst Note: Querying threat intelligence databases for this specific JARM configuration returned zero global overlaps. This isolates the configuration profile, indicating a custom-built, freshly provisioned standalone server environment rather than a automated script-kiddie deployment.

Adversary OpSec & Infrastructure Rotation (Reverse IP Mapping)
Through historical DNS and Reverse IP pivoting, a malicious cluster of five distinct domains was mapped to the exact same origin IP (209.74.85.172). All domains were activated within the DNS layer simultaneously on 2026-05-20:

asancerime[.]online (Mimicking ASAN state payment gateway)
cerime-dyp[.]online (Targeting traffic fine queries)
dyp-odenis[.]online (Using localized language terms for "payment")
dypodenis[.]online (Syntax variation for scaling attack surface)
dypodenisi[.]online (Grammatical linguistic variation)

Defensive Bypass Tactic (Dormancy)
The Let's Encrypt SSL certificate (Serial: 6069ec23e41875d0fc7d503825da39826fc) was issued on May 6, 2026. However, the operational Smishing (SMS phishing) campaign and active domain routing did not begin until May 20, 2026.

This 14-day dormancy period is a highly calculated adversary maneuver designed to deceive proactive threat hunting algorithms and automated filters that flag "Newly Registered Domains" (NRDs) during their first week of existence.

Incident Response & Coordinated Take-down Timeline
To ensure operational security (OpSec) and prevent adversarial counter-reconnaissance, the entire mitigation phase was executed from an isolated environment using Tails OS, routed through the Tor network, utilizing zero-knowledge encrypted comms (ProtonMail).

+------------------+      +----------------+      +--------------------+
|  Tails Live OS   | ---> |  Tor Network   | ---> |  ProtonMail Client |
+------------------+      +----------------+      +--------------------+
                                                             |
                                            +----------------+----------------+
                                            |                                 |
                                            v                                 v
                                  [Namecheap Abuse]                     [CERT.az Team]
                             (Upstream Host Termination)          (National Boundary Defense)
Milestone 1: Upstream Hosting Abuse Escalation
A comprehensive abuse dispatch was submitted to Namecheap, Inc., citing direct violations of their Acceptable Use Policy (AUP) concerning active financial fraud and state-level impersonation. The ticket contained structural server headers, JARM signatures, and the mapped five-domain cluster to justify an immediate host-level isolation.

Milestone 2: National Cyber Authorities Engagement
Simultaneously, a high-priority technical brief was routed to the National Computer Emergency Response Team (CERT.az). The objective was to facilitate immediate domestic internet service provider (ISP) DNS-sinkholing and border gateway protocol (BGP) mitigation to isolate the threat vector from regional consumers before upstream hosting processing was finalized.

Key Takeaways & Analytical Conclusions
Linguistic Optimization: Threat actors are heavily investing in highly accurate, localized vocabulary to craft persuasive smishing vectors, moving away from generic global templates.

Infrastructure Staging: The use of mid-tier enterprise stacks (AlmaLinux, patched PHP 8.3) combined with intentional certificate aging proves that contemporary fraud networks are adopting classic Advanced Persistent Threat (APT) operational staging concepts.

Proactive Takedowns work: Combining automated infrastructure fingerprinting (Reverse IP + JARM) with immediate, secure multi-channel escalation reduces the standard Threat Window from weeks to hours, effectively neutralizing the adversary's financial return on investment (ROI).

Disclaimer: All indicators, domain names, and technical records artifacts within this repository have been defanged ([.]) for safe reference and educational analysis.
