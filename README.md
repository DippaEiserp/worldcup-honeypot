# worldcup-honeypot

Cybersecurity research project deploying global honeypots to capture attacker behavior during FIFA World Cup 2026.

A network of honeypots deployed across AWS during the FIFA World Cup 2026 to study how attackers exploit major global events.

## What this is

Big sporting events bring big cybercrime. During the 2022 World Cup, CISA reported a sharp uptick in sports-themed phishing campaigns. The 2026 tournament, co-hosted by the US, Mexico, and Canada, is the largest in history...and almost certainly the largest target :) 

This project tries to answer some questions with real data instead of guesses:

- Who is actually attacking infrastructure during the tournament?
- Where are they coming from geographically?
- What kinds of lures work: fake ticket portals, fake streaming, credential stuffing?
- Do attacks spike on match days, or do they run constant?

To find out, I'm deploying a custom honeypot stack on AWS across several regions. Each instance runs Cowrie and Dionaea — open source honeypots that emulate SSH and SMB services — dressed with World Cup themed internal details to give attackers something interesting to interact with after they break in. The honeypots only respond to attempted unauthorized access. All captured data is anonymized before publication.

## Architecture

```
            attackers (Phase 2)
            simulated attacks (Phase 1)
                |
                v
    +------------------------------+
    |  Honeypots                   |
    |  Cowrie + Dionaea            |
    |  (Local VMs / AWS Phase 2)   |
    +-------------+----------------+
                  |
                  v
            +------------+
            | Log files  |
            +-----+------+
                  |
                  v
            +------------+
            | PostgreSQL |
            +-----+------+
                  |
                  v
            +------------+
            |  Dashboard |
            |  PHP + JS  |
            +------------+
```

## Stack

- Honeypots: Cowrie (SSH) and Dionaea (SMB/FTP/HTTP)
- Infrastructure: VirtualBox locally (Phase 1), AWS EC2 free tier (Phase 2, optional)
- Backend: PHP, PostgreSQL
- Frontend: vanilla JavaScript, HTML, CSS
- Data pipeline: Python for log parsing and normalization
- Attack simulation tools (Phase 1): Hydra, nmap, Metasploit

## Timeline

Two-phase build matching the tournament:

- Week 1 (June 15-22): local lab setup. VirtualBox VMs running Cowrie. Initial data pipeline.
- Week 2 (June 23-29): add Dionaea. Design custom World Cup themed environment inside honeypots. Build PostgreSQL schema.
- Week 3 (June 30-July 6): dashboard MVP. Run simulated attacks against the local stack to populate test data.
- Week 4 (July 7-13): public dashboard launch with live data (either local-via-ngrok or AWS free tier deployment depending on Phase 1 results).
- Week 5 (July 14-19): final tournament week, peak data collection, polish.
- Post-tournament (late July - August): full analysis writeup, published on Medium and LinkedIn.

## Why I'm building this

I'm an IT major with a cybersecurity minor. I competed on the blue team at MWCCDC this spring defending Windows servers against an active red team, which got me interested in the other side — what attackers actually do when nobody's watching. I'll be interning at TuningSQL.com in August working with PHP and PostgreSQL, so this project also doubles as a way to get comfortable with that stack before I start.

## A note on this repo

Code is publicly visible but not licensed for reuse. If you find something interesting and want to build on it or talk about it, message me.

Contact: hoangdang020805@gmail.com

All rights reserved.