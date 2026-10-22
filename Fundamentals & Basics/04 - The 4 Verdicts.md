# The Four Verdicts

This is probably one of the most important, if not *the* most important, in being an Analyst.
This is where you decide an alert : Was the system right, or did it misfire? 

How do we even decide it?
Think of it like this : If the system raised an alert, then it's Positive. Was the system right?? Then it's True.

So... something bad that happened, an alert fired? It's a True Positive (TP).
What if nothing bad happened, but an alert fired?? That'd be a False Positive. (FP)
Now, if something bad *really* happened, but no alert was raised? False Negative (FN) (Mostly worked on by Tier 2 & Tier 3 Analysts, sometimes even SOC Engineers!).
Lastly, if nothing bad happened and no alert was fired, it doesn't really matter and it's a True Negative. (TN)

Now that you've understood that (Probably, XD), let's do some light practice.

### TP

> 10 failed logins in <120 seconds
> Successful login at 01:58:12PM
> Source: 203.0.113.50
After running that IP through an [IP Checker](https://whatismyipaddress.com/ip-lookup), it turns out it's one of those flagged IPs.
That already gives away a lot: A flagged IP, and a successful login in under 11 attempts, under two minutes. 
That is a ***True Positive***. The attacker is already inside, and it's time to escalate! This is no longer our job!

### FP

> 10 failed logins in <120 seconds
> USER: Alice

This could also flag a brute force attempt, but there's a clue: The user was Alice.
You check the logs. She just came back from a vacation, forgot her password, and reset it. There is no attack, just simple confusion.

As a Tier 1 Analyst, you'll meet lots of these. A big part of your work is to figure it out quickly but carefully, whether an alert is an FP or TP.
Try not to confuse "Alert looks like an FP" with "I proved the alert is an FP". Always check the IP, time, user, events, processes (If possible), before closing anything. 
And don't go harsh on yourself if you accidentally flagged a TP as FP or vice versa, you're still human. You're prone to making mistakes. :*

### TN

No alert, and nothing bad happened. This means that you won't even see this as an alert, since there's nothing to raise!

### FN

This one is the scary guy. No alert, yet something really happened.
You can't really investigate an alert that never fired. Usually, it's discovered through a threat hunter finding it manually (These guys live in the system for the sole purpose of finding threats), a different alert firing later in the attack (Maybe a file being uploaded to an external server), a user reporting something strange (Remote control outside of IT, strange patterns, random files opening, etc.), or an outside party telling the company (Maybe a customer or the police).

This could also be a new malware that has no signature recognizes, so the antivirus stays silent. This is an FN. (Remember the flaw of signature-based detection in the file 02?)

---
So, which is worse? 
Generally, both. In specific scenarios.
An FN is obviously, at first glance, more dangerous. You have an attacker in your network and you didn't even get a clue on if he's here or not!
BUT! Too many FPs could cause an FN? 
How? By drowning analysts in unnecessary noise. They'll eventually click close on a real attack, thinking it's just another FP.
That's why teams constantly tune their detection rules; To cut FPs withought opening the door to FNs.

---
Now, let's talk about the benign True Positive.
What is it? Well, the system did its job and the alert was correct, but the activity was allowed.

> Port scan detected from 198.51.100.2

You check, and it turns out that IP is the security team's. They were the ones running the port scan, and it turns out it wasn't really an attack.

This is called a Benign True Positive (BTP). Not every team uses this label, and the label doesn't really matter. Use whatever you're comfortable with if you know what you're talking about :)

###### Activity Description 
- ***Benign*** : Harmless, normal.
- ***Suspicious*** : Odd, but you can't prove it's bad. It needs a deeper dig or escalation, depending on the alert itself.
- ***Malicious*** : Confirmed bad.

Take this example.
The SIEM alerts on a Word PowerShell chain. You find that the IT built a legitimate macro that does this for reports. This is a Benign True Positive. You can also consider it an FP.

But what about a ransomware sample encrypting files on a laptop, but the antivirus doesn't have a signature for it? This is a Malicious False Negative. It's confirmed malware.
This isn't fully used, and you can simply use the 4 verdicts; Way easier, and way more profound.

As an example, a full verdict would sound like : Alert is a True Positive, and the activity is malicious.
Or maybe : False Positive with a benign activity.