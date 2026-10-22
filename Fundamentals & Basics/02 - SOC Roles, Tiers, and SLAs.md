# SOC Roles, Tiers, and SLAs

Usually and most probably all of the time, SOC isn't one big room of people doing the same thing. Each one has their own role in the team with their specific job, and alerts get passed along like a relay race.

### Tiers

Imagine a SOC gets thousands of alerts a day.
Most are noise, some are small problems, and a few are real attacks. If your most experienced analyst had to look at every single on of them, they'd burn out by lunch!
That's why SOCs split the work into tiers.
Imagine it as a hospital. The nurse checks everyone, the doctor takes the serious cases, and the specialist handles the complicated ones.
Now what are all those tiers and roles??

#### SOC Tier 1 (Triage Analyst)

He's the first line of defense and most probably where you'll start.
In this job, you have to monitor the SIEM dashboard and incoming alerts, triage (Quickly decide if an alert is harmless, suspicious, or serious), close the obvious False Positives (You'll learn verdicts in the fourth file), follow playbooks step by step, and escalate anything you can't close to Tier 2.

Here, you'll mostly use SIEM, ticketing system, and email security tools.
You'll have to pay attention to detail, document clearly, and stay calm when 40 alerts are waiting.

Not to trouble you, but it's not the easy job just because it's the first tier. Closing a real attack as a False Postive is how breaches happen. The most important skill here is knowing when you're not sure, and escalating instead of guessing.

#### Tier 2 (Incident Analyst)

This is where TIer 1's escalation goes, and where the real investigation starts.

In this tier, you'll dig deeper into logs, the host, and the network traffic. You will also have to confirm whether the alert is a True Positive or a False Positive. Also, add working out the scope to the list; *which machines? Which users?? HOW FAR DID IT SPREAD?!?!*
And after it, the hard part : Containment. Yeah, and then you escalate it to Tier 3 when it's too complex or serious, and it has already spread beyond containment.

SIEM queries, EDR, packet captures, sandboxes, and threat intel platforms are what is mostly used here.

#### Tier 3 (Senior Analyst/Threat Hunter)

This is the most experienced on the team.

These guys are the ones that handle the most complex incidents. They hunt for threat that never triggered an alert (False Negatives), analyze malwarea and do deeper forensics, write and improve detection rules based on what they learn, and they also help train the lower tiers.

They're not always the top of the chain, but most of the time that's where it ends. 

#### Other Roles

Not every SOC has every role, and small teams combine several into one person. But these names will sure come up.

###### SOC Manager

He runs the team; Schedules shifts, tracks performance, and talks to the rest of the company.

###### Incident Responder

Takes over something is confirmed bad. Contains, removes, and recovers.

###### Threat Hunter

Goes looking for hidden attackers instead of waiting for alerts.

###### Detection Engineer

Builds and tunes the detection rules and SIEM content, so alerts are better and quieter.

###### Threat Intelligence Analyst

Researches attackers and gives the SOC fresh IOCs and TTPs to watch for.

###### Forensic Analyst

Studies disks, memory, and logs after an incident to find out exactly what happened.

###### Malware Analyst

Takes apart malicious files to understand what they do.


### Escalation

A good escalation note answers these questions, step by step:

- **What** is the alert? (Name, ID, time)
- **Which** host, user, and IP are involved?
- **What** did I **check**? (Logs, user's recnet activity, etc.)
- **What** did I **find**? (Facts, not guesses)
- **Why** am I escalating? (Can't confirm, looks malicious, needs containment, etc.)
- **What** do I **recommend**? (Isolate host? Reset password?)

If Tier 2 has to redo your whole investigation, the escalation wasn't good.

### SLAs

Service Level Agreement. In a SOC, it's a promise about response time.
The faster an attacker is stopped, the less damage they do, so SLAs get tighter as severity goes up.

| Severity | Respond Within |
| :--- | ---: |
| Critical | 15 mins        |
| High     | 1 h            |
| Medium   | 4 h            |
| Low      | 24 h           |

Examples for the severities :
Critical : A ransomware activity on a server
High : Confirmed malware on a workstation
Medium : Suspicious login from a new country
Low : Policy violation, port scan from inside

An SLA has two usual parts :
- Response Time (How fast someone starts working on it)
- Resolution Time (How fast it should be finished)

Any alert not handled in time are SLA breaches. Breaches get reported, and a lot of them means the team is understaffed or buried in noise.

### Metrics

**MTTD** (Mean Time To Detect) : How long between the attack starting and the SOC noticing it.
**MTTA** (Mean Time To Acknowledge/Answer) : How long between the alert firing and an analyst picking it up
**MTTR** (Mean Time To Respond/Resolve) : How long until the problem is handled.

### Shift Handover

Attackers really don't care what time it is. SOCs have to work 24/7 in shifts. When your shift ends, the next analyst needs to know:

Which tickets are still open, and what's the next step on each.
What's suspicious but not confirmed.
What's waiting on someone else (A user's reply, a Tier 2 answer, etc.)
Any ongoing incident and its current status.

A bad handover is a disaster. That'd mean the next analyst starts from zero, and the SLA clock keeps ticking. (Good luck with getting that 'pay raise' XDDD)
