## Alert & Detection Fundamentals

This is the foundation of SOC work. Whether a Tier 1, Tier 2, or Tier 3, it's fundamental and they all fall if this falls. 
This is the pillar holding everything upright.

You'll spend lots of time looking at something that says "something happened, could be bad. Figure out what the hell this could be".
To do that, and to correctly understand and escalate it properly, you need to understand the chain of investigation :
Activity → Event → Log → Detection → Alert → Investigation

This chain is broken down below ↓ :D

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

> 2026-10-05 23:08:01 [INFO] [AuthService] User authentication successful for UID: 4092
> 2026-10-05 23:08:03 [WARN] [DBPool] High connection latency detected: 420ms (Threshold: 200ms)
> 2026-10-05 23:08:05 [ERROR] [PaymentGateway] Transaction failed: Timeout awaiting response from provider

If you got to know enough, you can skip these next lines and go straight to the next heading; Alert.

How do Linux logs differ from Windows logs?
Both have a different structure of logs.
Linux has plain-text files or binary journald logs, and it has a numeric UID as an identifier (0 for root, 1000 for users, etc.).

> Oct  5 23:12:01 server sudo: pam_unix(sudo:session): session opened for user root(uid=0) by alice(uid=1000) 
> Oct 05 23:14:02 web-01 systemd[1]: Started Apache HTTP Server.

Windows has a very structured XML/binary .evtx files with predefined fields. It also doesn't have a simple UID, it has an SID; Security Identifier.
Just a notice: When you open the Windows Event Manager and you see the Event ID, that's not the log's SID; The Event ID tells you what happened (4624 → Successful windows logon, 4625 → Failed logon. etc.), whereas the SID shows who did it.
![Windows Event Manager](https://www.freecodecamp.org/news/content/images/2021/10/ss-10.png)

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
Establishing a baseline is important and its one of the easiest mitigation techniques for detecting anomalies. That's what we'll talk about next.


### Baseline

Your baseline is the understanding of what's normal. It's the fundamental of Anomaly Detection.
Without a baseline, it's harder to determine what unusual would look like.
For example, a server normally communicates with 192.0.2.152. One night it suddenly starts communicating with 203.0.113.211.
From your normal baseline, being that the server communicates with a specific IP (In this case, 192.0.2.152), you'd feel something wrong is going on when the server started communicating with a different IP.
If you didn't have that baseline, then it wouldn't have looked strange!

### Indicators
#### Indicator of Compromise (IOC)

This is a piece of information you can use to identify or investigate the activity.
It is an evidence or clue associated with compromise. Just because you see on doesn't automatically mean the device is already compromised.

Some indicator that you'll see and use are:
- IP addresses: An incoming message from a malicious IP can already indicate more than enough whether that email is safe to open or not. You can check for malicious IP Addresses on any site, but preferably [VirusTotal](https://www.virustotal.com/gui/home/upload). It has lots of uses, and will be mentioned again.
- Domains & URLs: Some phishing campaigns make exact copies of the phished website. Pass the Domain or URL through VirusTotal (Above) to make sure it's safe.
- File hashes: File hashes are unique. If you want to make sure your file is legit, compare the hash value of your downloaded file (generated locally) to an official checksum published on a trusted source (Like VirusTotal, or the vendor.)
- Filename: A name can give away whether the file is compromised or not. This is usually done in phishing, when people are sent files and are asked to open it. These could also be done with an injected file that looks legit but doesn't come from a trusted source. Once you open it, the malware/attacker is free to move in your device/network.
- Username/Email address: Just like filename, a misspelled email or username could spoil the attack before it even reaches its destination target.
- Process: As in a previous example, WINWORD.EXE spawning powershell.exe already shows the attack.

#### Indicator of Attack (IOA)

Different from a compromise, but completes it.
An IOA means that something happened/ is happening, such as PowerShell running out of nowhere for a split second with a long command that downloads a payload and executes it. 
You don't necessarily have to know the exact malware, but you have to know it indicates an attack has happened.


### TTP

As intimidating as this sounds, it isn't.
It simply describes how attackers operate.

TTP = **Tactics** + **Techniques** + **Procedures**
Tactic is the attacker's onjective. Technique is the method used to achieve it. Procedure is the actual implementation.

An easier way to remember this is asking yourself 3 questions : What do they want? How are they achieving it? What exactly are they doing?
As much as attackers can change their IP, domain, hash, or anything else, their behavioral patterns can remain recognizable.

--- 

Now that we've gone through all this, you may want to practice it a bit.

``` markdown
ALERT: Suspicious PowerShell Activity

Host: WS-102
User: John Doe
Parent Process: WINWORD.EXE
Child Process: POWERSHELL.EXE

Command:
powershell -enc [base64 data]

Destination:
203.0.113.231
```

When you analyze this, you'll simply find the ***event*** as PowerShell execution.
The ***log*** is that the endpoint recorded the process creation and command line.
Your ***detection*** rule is an identified suspicious PowerShell behavior.
Your ***indicator*** is everything that happened.
Its ***potential IOA*** shows suspicious behavior (Word → PowerShell → Encoded command)
Could be a ***possible IOC*** if the destination IP turns out to be a known malicious infrastructure.
Using ***TTP***, you could map the observed behavior to an ATT&CK technique involving PowerShell.

Now comes the most important part: Connecting the dots.
What document was opened? Was there even any document? What happened after the document was opened? Where did it download from? If there was a process born after it, what was it? Did it create persistence? Did it showed up in the firewall?