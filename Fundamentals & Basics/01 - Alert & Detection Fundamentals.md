## Alert & Detection Fundamentals

This is the foundation of SOC work. Whether a Tier 1, Tier 2, or Tier 3, it's fundamental and they all fall if this falls. 
This is the pillar holding everything upright.

You'll spend lots of time looking at something that says "something happened, could be bad. Figure out what the hell this could be".
To do that, and to correctly understand and escalate it properly, you need to understand the chain of investigation :
Activity → Event → Log → Detection → Alert → Investigation

This chain is broken down below \/ :D

### Event 

Simply, something that happened! Could be anything from a successful login to a reported intrusion.
Could be someone logging in, a USB device connected, a firewall allowing a connection, a new process starting, etc.
Basically, any action happening is an event.

Keep in mind that not _every_ event is a security event!!!

> User a1b2c3 executed whoami in cmd.exe
> User 1a2b3c executed ( :loop \n start notepad \n goto loop) in cmd.exe

The first event doesn't always have to be malicious; user a1b2c3 may just be curious!
However, user 1a2b3c isn't running a normal code. That code infinitely loops through thousands if not millions of separate windows of Notepad in seconds, until the RAM & computer resources are exhausted. (Yes, it's a real command and I hold no responsibility if you use it against yourself or somebody else. It's for educational purposes only.)



### Log

Consider it a diary of the system.
Logs are the recorded representaions of events.

It holds a record of *Who* did the Event, on *Which* device, *What* action was done, *What* the results of the action was, *Where* it was done (usually IP, rarely and probably never location), and *When* it was done down to the hour, minute, and second.

If you confuse Logs for Events: Events is something happening. Logs are the records of what happened.


### Alert 

An alert is generated whenever your security system decides something deserves attention (And sometimes, in uncommon cases, not deserve any but still sounds up. Especially on Windows.)

If, for example, your detection system detects five failed logins followed by a sixth successful login, it might flag it as a "Possible Brute-Force Attack". (Keep in mind: Not every failed login attempt followed by a successful attempt is a successful brute-force. Make use of the IP, the time, user, and other given information to determine whether it actually was a brute-force or a user who just forgot their password.)

The alert isn't the attack itself. It's the system's way of telling you something unusual happened here. It doesn't prove anything.
You'll understand why **Alert** was added here instead of after **Detection** like in the chain at the beginning of this page.


### Detection

A detection is the logic used to identify suspicious activity. 
This is where we define the logic/rules for the machine to follow for flagging a suspicious pattern.
Example of a detection rule:
> IF
>    user failed logins >= 10
>    within 5 minutes
> THEN
>    generate brute-force alert
A brute-force alert is raised when a user fails logging in for 10 times in under 300 seconds.
This doesn't necessarily mean there really was a brute-force attempt. The user could have mistakenly mistyped. Make sure who was the person behind it.

Another detection rule (Surprisingly common but a reliable way hackers still use):
> IF
>    parent_process = WINWORD.EXE
> AND
>    child_process = powershell.exe
> THEN
>    alert
This raises an alert if the parent process WINWORD.EXE (Microsoft Word) spawns a child process powershell.exe. This is a perfect example of how a successful phishing attempt.
An attached Microsoft Word file, when opened, spawns a powershell execution, thus infecting the said device.
For this attack, look at [https://attack.mitre.org/techniques/T1059/]


### Signature

A recognizable pattern associated with a known activity.
Antiviruses use a database they match to possible malwares.
If they recognize a partifular malware file by its hash or characteristics, it'll flag it.

However, signature-based detection has an obvious flaw: What if we haven't seen a recognizable pattern before??

#### Behavioral Detection

Instead of asking if the malware signature exists, why not ask if it looks suspicious? 
Even if we don't know the malware itself, its behavioral claim may look suspicious. That's how we decide on something even if we don't know what it is, but we know what it does.

#### Threshold Detection

What if you put a threshold instead of behaviorally detecting? And when that threshold is passed, it raises an alert.
Whether it be more than a specific amount of failed logins, or DNS requests, it'll raise an alert!
It's only downside comes in our previous example: malware detection. We can't detect malware with a threshold, and this wasn't made for that, so don't bother using it XD

#### Correlation Detection

Now think with me about combining multiple events in a continuous serie. That's what correlation is. 
An event may not look suspicious, but multiple events back to back may look questionable. Our previous WINWORD.EXE example is perfect for this scenario.
WINWORD.EXE alone may not look suspicious at all, but once powershell becomes its child process it all makes sense.

#### Anomaly Detection

Imagine: You always log in computer at 09:00AM. One day, you log in at 12:30PM. 
This could trigger an anomaly detection.
Anomaly Detection follows the principle that normal behavior = baseline. Any deviation from it would be an anomaly.
Establishing a baseline is important and its one of the easiest mitigation techniques for detecting anomalies.