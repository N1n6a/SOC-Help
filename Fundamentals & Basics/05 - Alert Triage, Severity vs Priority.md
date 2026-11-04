# Alert Triage, Severity vs Priority

A SOC doesn't get one alert a day; It gets hundreds, sometimes thousands!
You can't investigate every single one in depth, so you need a way to decide what to look at first.

### Alert Triage

Triage is the first quick look at an alert to decide: is this real, how bad is it, and what happens next?

The word itself comes from hospitals. In an ER, a nurse doesn't treat patients in the order they arrived; Someone with a paper cut waits, and someone with chest pain goes first.

Triage is more of trying to make a good enough decision rather than a full investigation. The full digging comes later, usually at Tier 2.

### Triage Steps
Every analyst has their own habits , but a solid routine looks like this. Follow it in order :

1. #### Read the alert properly
What rule fired? What did it detect? Don't skim, because the alert name alone can be misleading!

2. #### Collect the basics
Answer the 5 W's from the log:
- ***Who*** : Which user/account?
- ***Which*** : Which host/device?
- ***What*** : What action happened?
- ***Where*** : Which source/destination IP?
- ***When*** : What exact time (and which time zone?)

3. #### Check the context
Is this normal for this user or host? (Use the baseline here.)
Is this host a laptop or a database server? (db can mean a leak.)
Has this alert fired for the same user/IP before?

4. #### Check the indicators
Look up IPs, domains, and file hashes.
Remember, an IoC match is a clue, not a final answer.

5. #### Decide the verdict
TP, FP, or "I'm not sure yet" (This means suspicious, so you keep digging or escalate). 
See the 4 Verdicts

6. #### Set the priority 
How urgent is this? (This is explained more below ↓)

7. #### Take action
Close it, escalate it (If needed), or start containment (If needed), depending on what you found.

8. #### Document everything
Write down what you checked, what you fonund, and why you decided what you decided.
If it isn't written, it didn't happen.

### Severity 
It's just how bad the activity could be, technically if it's real.
This is usually set by the detection rule or the security tool, and it normally uses the following levels.
Severity means "how bad this type of activity is?"

| Level    | What is means          |
| :---     |                   ---: | 
| Low      | Minor or informational |
| Medium   | Worth checking         |
| High     | Likely real danger     |
| Critical | Serious and active     |

### Priority 
Priority is how urgently you should handle it, and it comes from **severity** + **context**
It means "What should I work on first?"

Priority is changed by context:
Is it confirmed? (A verified TP is more urgent than a 'maybe') 
Is it spreading? (Is it on one machine or many?)
Is it exposed? (Does the machine face the internet??)
Who's involved? (An intern's device, or maybe a CEO's account?)
Is it a test VM or the DC?
Is data at risk?

And even though Severity and Priority are connected, they're not one and the same thing.

| Alert | Severity | Context | Priority |
| :---  | :---:    | :---:   | ---:     |
| Malware Detected | High | On an isolated test VM with no network access | Low | 
| Unusual Login Time | Medium | On the CEO's account, from a new country | High |
| 15 Failed Logins | Medium | On a DC, then a success | Critical |
| Port scan | Low | From the security team's scheduled scanner | Very Low (benign TP) |

Notice how the high-severity alert has a low priority, and how a medium-severity jumped to a high priority?
This is exactly why you can't just sort your queue by severity.

So, how do you decide it? By using a priority matrix. SOCs use their own versions, but here's a simple one :

| | Low-Value Asset | Medium-Value Asset | High-Value Asset | 
| :--- | :---: | :---: | ---: |
| Low Severity | Low | Low | Medium |
| Medium Severity | Low | Medium | High | 
| High Severity | Medium | High | Critical |

---

But when do I escalate?

Well, you don't have to be 100% sure to escalate! You cna pass it up when:

- You've confirmed (Or strongly suspect) a TP.
- The priority is high or critical
- It involves a critical asset or privileged account 
- You see signs of spreading (Lateral Movement) or data theft
- You're stuck, or it's beyond your tier (Don't be shy to ask for help, it is still part of a job and not a failure!)

Make sure when you escalate to include what happened, what you checked, what you found, and what you think should happen next. 
A vague "This looks weird" isn't an escalation!!


### Alert Triage

If analysts get too many low-quality alerts, they start closing the without really looking
That's called alert fatigue, and it's how real attacks get missed.
Good triage and tuning detection rules keep the queue healthy.