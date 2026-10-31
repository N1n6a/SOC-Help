# The Core SOC Vocabulary

Before we get into other important details, let's speak the language. 
You certainly don't want to hear words such as 'Triage' or 'False Positive' on your first day and not know anything about them.


## 1. People & Places
### SOC (Security Operations Center)

The team, or room, that watches an organization's systems 24/7, looking for attacks (Yes, they're always on the lookout).
Think of them as security cameras, except that these are humans who actually understand what they're seeing.
As a SOC Analyst, we typically have 3 tiers (Some organizations may use 2, or 4).

### Tier 1 Analyst
The first line of defense, and most likely your first job if you're pursuing a SOC career.
You look at incoming alerts and decide whether it's nothing, if it's a problem, and if it needs escalation or not.
Your job is mostly deciding whether alerts lit something important up or not, and writing reports and documenting them.
The rest of the tiers mostly depend on you!!

### Tier 2 Analyst
This is where things start getting tougher.
The Tier 1's report that said it needs escalation reaches here. You dig deeper, investigate, and help contain the problem.

### Tier 3 Analyst
This is the senior Analyst level.
They handle the hardest incidents, hunt threats, analyze malware, and improve detection rules.
While you're busy receiving silly alerts at Tier 1, Tier 3 Analysts look at everything you don't.

### Incident Responder
This guy takes over when an incident is confirmed and follows the response plan to contain, remove, and recover while still preserving evidence.

### Threat Hunter
This dude doesn't wait for alerts. He goes looking for attackers who are hiding quietly that didn't trigger any alert.


---

## 2. The Tools
### SIEM (Security Information and Event Management)
Collects logs from everywhere into one place, then raises alerts when something looks wrong. This is the SOC's main screen.

### EDR (Endpoint Detection and Response)
Watches individual computers (endpoints) closely (processes, files, commands) to detect, investigate, and respond to threats, including isolating amachine from the network.

### SOAR (Security Orchestration, Automation, and Response)
Automates the boring repeated work.
E.x.: When alert *X* is raised, do *Y* automatically.

### IDS (Intrusion Detection System)
Watches the network traffic and tells you about suspicious stuff.

### IPS (Intrusion Prevention System)
Basically the same as an IDS, but it can also block the traffic.

### Firewall
Allows or blocks network connections based on rules.

### Sandbox
A safe, isolated box where you run a suspicious file to see what it does without risking the real system.

---
## 3. The "What Happened" Words

### Event 
Something that *happened*
### Log
The record of what happened
### Alert
What the system flared up for you to look at.
### Detection
The rule that decides when to raise an alert
### Baseline 
What normal looks like
### IOC (Indicator of Compromise)
Evidence that something might have been compromised.
It could be a malicious IP, domain, or file hash.
Contrast it with IOA: IoC = footprint, IoA = behavior
### IOA (Indicator of Attack)
A sign that an attack is happening right now
### TTP (Tactics, Techniques, and Procedures)
How attackers operate (Their goal, method, and exact steps)
Tactics = Goal, Techniques = Method, Procedures = Exact Steps

---
## 4. The Verdict Words

### True Positive (TP)
The alert fired, and there ***really*** was a threat. The system was right.

### False Positive (FP)
The alert fired, but it was harmless.
E.x.: IT runs an authorized vulnerability scan, and the IDS flags it as a port scan.

### True Negative (TN)
No alert, and nothing bad happened.

### False Negative (FN)
No alert, but something bad ***did*** happen.
This is the scariest, because nobody knows the attacker is already inside.
This is the job of threat unting and detection improvement.

> A way to remember it is : Second word is what system said (Positive = alert, Negative = no alert), and the first word is whether the system was right or wrong.

On top of these, you'll also see three words describing the activity itself:   
**Benign** : Harmless normal activity   
**Suspicious** : Weird, but can't prove it's bad yet.  
**Malicious** : Confirmed. 

---
## 5. The Attack Words

### Threat 
Anything that could cause harm (Attacker, malware, etc.)

### Vulnerability
Weakness that can be abused (Unpatched system, weak password, etc.)

### Exploit
The actual method or code used to abuse a vulnerability

### Risk
Likelihood that a threat exploits a vulnerability, and the impact if it does.

> Think of a house. The ***vulnerability*** is an unlocked window. The ***threat*** is a burglar. The ***exploit*** is them climbing through it. The ***risk*** is how likely that is, and how much they could steal.

### Malware
Any software made to cause harm (viruses, ransomware, trojans, worms, etc.)

### Phishing
Tricking someone, usually by email, into clicking a link, opening a file, or giving up their password.

### Payload
The part of an attack that actually does the damage, like the malware that gets downloaded and runs.

### C2 (Command And Control)
The attacker's remote server that infected machines talk to receive orders

### Persistence
Techniques attackers use to keep access even after reboots, logouts, or password changes.

### Privilege Escalation
Gaining higher perimissions than you started with.
E.x.: Starting from an employee, and escalating to a manager.

### Lateral Movement
Jumping from an infected machine to another on the same network

### Exfiltration 
Stealing data and sending it out

### Threat Actor
The person/group/organization behind an attack

### APT (Advanced Persistent Threat)
A skilled, well-funded group often tied to governments, that stays hidden for a long time.

### Zero-Day
A vulnerability unknown to the vendor, so no patches exist it for it yet.
Zero-Day Exploit is when the attacker finds it before the organization.

### Brute-Force
Basically trying all possible attemptsat breaking a password using a wordlist. Some wordlists can reach millions of passwrods!

### Ransomware
A malware that encrypts the device and asks for an amount of money to decrypt them.
Remember: Not just because you paid the ransom means the malware is gone!
---
## 6. The Response Words

### Triage
The first quick look at an alert to decide how serious it is and who should handle it.
Like an ER Nurse sorting patients before any doctor sees them

### Escalation
Passing the alert to someone more experienced or higher up, along with your notes.

### Containment
Stopping the damage from spreading (Isolating the machine, blocking the IP, etc.)

### Eradication
Removing the threat completely (Deleting the malware, closing the hole, patching the system, etc.)

### Recovery 
Getting everything back to normal and making sure the system is safe.

### Playbook
Step-by-step instructions for handling one specific type of alert (like suspicious PowerShell)
This is mostly the decision flow for a scenario.

### Runbook
Similar to Playbook, but usually more technical and focused on exact tasks or commands.
This, different from the Playbook, is more like the exact steps for a single task.
### Ticket
The record of an alert or case where you write your notes, findings, and verdict.

### Incident
A confirmed security problem.

> An alert isn't necessarily an Incident.
> An alert shows something might be bad. 
> An incident ***confirms*** something is bad.
> Most alerts (that turn out to be FP) never become incidents.

## 7. Processes

### SLA (Service Level Agreement)
A promise about response time.
As an example : Critical alerts must be looked at within 15 minutes. Miss it, and people get unhappy. Say bye-bye to that salary raise.

### Shift Handover
SOCs run all day and night, they never stop.
You can't let the next shift start from zero without knowing what you did, so you write notes for them: what's open, what's suspicious, what's waiting on someone.
Your documentation skills matter a lot here.