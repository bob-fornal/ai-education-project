# Computer & Network Security

**Backbone Course #15** · **Duration:** 50 minutes

## The One-Sentence Pitch
Security is not a checklist of fixes — it's a mindset of asking "what could go wrong and who benefits from it going wrong," and then layering defenses because no single one will ever be perfect.

## Audience & Prerequisites
This talk is for learners comfortable with basic programming and web concepts (client, server, requests); no prior security experience is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: security is a mindset, not a feature |
| 0:05–0:15 | The CIA triad and threat modeling |
| 0:15–0:27 | Common vulnerability classes by example |
| 0:27–0:37 | Authentication vs. authorization |
| 0:37–0:46 | Defense-in-depth |
| 0:46–0:50 | Key takeaway and close |

### Hook: security is a mindset, not a feature
Open by asking the room: if you built a login form today, what could go wrong with it, beyond "the password is wrong"? Most beginners will only think about the intended use case — a legitimate user typing their real password — and never about the person deliberately trying to break it: someone submitting a username with code embedded in it, or trying ten thousand passwords in a minute, or intercepting the request over an open Wi-Fi network. Security engineering means routinely asking "how would someone misuse this on purpose," a question ordinary feature development never has to ask, because ordinary users don't try to break things. That single mental shift — designing for an adversary, not just a user — is the theme underneath everything in this talk.

### The CIA triad and threat modeling
The CIA triad names the three properties security aims to protect: Confidentiality (only authorized people can read the data), Integrity (only authorized people can change the data, and you'd notice if someone else did), and Availability (the system keeps working for legitimate users when they need it). Almost every security concept maps onto protecting one of these three — encrypting a database backup protects confidentiality, a checksum on a downloaded file protects integrity, and defending against a flood of fake traffic protects availability. Threat modeling is the practice of systematically asking, for a given system, who would want to attack it, what would they gain, and which of the three properties they'd target — it's a way of thinking, done early and repeatedly, not a one-time checklist you complete and file away. The value of threat modeling is that it turns "security" from a vague worry into a concrete, prioritized list of specific risks worth specific defenses.

### Common vulnerability classes by example
A buffer overflow happens when a program writes more data into a fixed-size block of memory than that block was allocated to hold — imagine being handed a small box and told to "put your groceries in it," but instead cramming in twice as much, so the extra groceries spill into whatever's sitting next to the box; in memory, that spillover can silently overwrite other data or even code the program was about to execute, which an attacker can exploit deliberately. Injection attacks (like SQL injection) happen when a program takes user-provided data and mistakenly treats part of it as executable code or commands instead of pure data — for example, a web form that inserts a user's input directly into a database command, so a user who types `'; DROP TABLE users; --` instead of a name can trick the database into executing that as an instruction rather than storing it as text. Both vulnerability classes share the same root cause: a boundary the program assumed would never be crossed (this input is only data, this input will always fit) turns out to be enforceable by an attacker rather than guaranteed by the system, which is exactly the kind of assumption threat modeling is meant to surface before it ships.

### Authentication vs. authorization
Authentication answers "who are you" — proving identity, typically via a password, a fingerprint, or a security key — and it happens once, at login. Authorization answers a completely different question, "what are you allowed to do now that we know who you are" — a logged-in user might be authenticated but still not authorized to view another user's private data or delete someone else's account. People conflate these constantly, and the mistake has real consequences: a system that checks authentication carefully but forgets to check authorization on every sensitive action is exactly how a logged-in but low-privilege user ends up able to access data or perform actions they were never supposed to reach, simply by guessing a URL or an ID that the system never re-checked permissions for. The discipline to build in from day one: authentication happens once at the door, but authorization has to be checked again, explicitly, at every single sensitive action — never assume that being logged in implies being allowed.

### Defense-in-depth
Defense-in-depth is the operating philosophy that no single security control is ever perfect, so systems should be protected by multiple independent layers, such that a failure in any one layer doesn't mean total compromise. A concrete stack: a firewall limits what network traffic can even reach a server, input validation limits what data a request can contain, authorization checks limit what an authenticated request can do, and encryption limits what's readable even if data is somehow intercepted or stolen — an attacker who breaks through one layer still has to break through the others before causing real damage. This mirrors physical security intuitions people already have — a house with a locked door, an alarm system, and a safe for valuables doesn't rely on any single one of those being unbreakable, it relies on the combination making a successful break-in unlikely and, if it happens anyway, limited in damage. The practical lesson: never design a system assuming one control (like "we validate all input" or "we have a firewall") is sufficient on its own — assume it will eventually fail, and ask what the next layer catches when it does.

### Key takeaway and close
Bring it back to the opening login-form question: a security-minded engineer doesn't just ask "does this work for a legitimate user," they ask which of confidentiality, integrity, and availability an attacker could target, whether a boundary like buffer size or code-versus-data is actually enforced, whether every sensitive action re-checks authorization rather than just authentication, and whether a single point of failure could compromise everything. That combination of adversarial thinking and layered, redundant defenses is what security engineering actually is — not a list of fixes to memorize, but a lens to apply to every design decision.

## Key Takeaway
Security means designing for an adversary rather than just a legitimate user, and because no single defense is ever perfect, real systems layer authentication, authorization, validation, and encryption so that one failure doesn't become a total compromise.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.858 — Computer Systems Security](https://ocw.mit.edu/courses/6-858-computer-systems-security-fall-2014/pages/syllabus/)
- Harvard PLL: [CS50's Introduction to Cybersecurity](https://pll.harvard.edu/course/cs50s-introduction-cybersecurity)
