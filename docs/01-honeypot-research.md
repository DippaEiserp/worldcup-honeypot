# Honeypot Stack Research

## The Question

Which honeypot framework best fits this project's goals: a portfolio cybersecurity research piece deployed during the FIFA World Cup 2026, showcasing real-world attack patterns while staying within a $10/month budget?

## Options Considered

### T-Pot
- Source: github.com/telekom-security/tpotce
- Maintainer: Deutsche Telekom Security
- A meta-honeypot that runs 20+ honeypots in a single Docker stack
- Pros: Mature, multiple attack surfaces, built-in dashboard
- Cons: Requires 8GB RAM. Not eligible for AWS free tier. ~$60/month per region. Deal-breaker for $10/month budget.

### Cowrie (standalone)
- Source: github.com/cowrie/cowrie
- Fake SSH/Telnet server
- Pros: Lightweight, fits t2.micro free tier, captures attacker commands in detail
- Cons: SSH attacks only

### Dionaea (standalone)
- Source: github.com/DinoTools/dionaea
- Fake SMB, FTP, HTTP, MSSQL
- Pros: Lightweight, catches malware uploads
- Cons: Narrower than T-Pot

### Custom lightweight stack
- Cowrie + Dionaea + custom internal World Cup themed environment

## Decision

Custom lightweight stack: Cowrie + Dionaea on AWS t2.micro instances.

Reasoning:
- Fits $10/month budget. Likely $0 on free tier.
- Covers the most common attack vectors: SSH brute force, SMB exploitation, malware delivery
- Internal World Cup themed environment (hostnames, decoy files, fake admin paths) gives attackers something interesting to interact with once they break in
- All deployment is on owned AWS infrastructure. No public-facing applications. Attackers find the honeypots through standard internet scanning, not deception.

## Legal/ethical framing

Standard honeypot deployment on owned infrastructure is well-established practice in security research. Cowrie and Dionaea are taught in university security programs and used by enterprise SOC teams.

The honeypots only respond to attempted unauthorized access. They do not lure normal users with deceptive content. Captured attacker data is anonymized before any public publication.

## Trade-offs accepted

- Less attack surface than T-Pot (no ICS, no MSSQL)
- Have to build the dashboard ourselves vs using T-Pot's Kibana
- More manual log parsing work in the pipeline

## Deployment Strategy

Two-phase approach to manage cost and risk:

### Phase 1: Local Lab (Weeks 1-3)
- VirtualBox VMs on local hardware
- Cowrie and Dionaea running on Ubuntu Server
- Attacks simulated using offensive tools (Hydra, nmap, Metasploit) against the local honeypots
- Dashboard built with this controlled data
- $0 cost, full iteration speed

### Phase 2: AWS Free Tier Deployment (Weeks 4+)
- Once local stack is stable, deploy a single t2.micro instance to AWS us-east-1
- Capture real-world opportunistic attacks from internet scanners and botnets
- Stay within free tier limits to maintain $0 cost
- Budget alerts configured at $5, $8, $10 as safety net

### Decision rationale

- Local-first eliminates financial risk during the learning/building phase
- Demonstrates more thorough engineering process (built and tested locally, then deployed)
- AWS deployment becomes a feature addition rather than the foundation
- Phase 2 is optional — if Phase 1 results are strong enough, project can ship without cloud deployment