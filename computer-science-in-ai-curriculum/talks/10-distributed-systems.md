# Distributed Systems

**Backbone Course #10** · **Duration:** 50 minutes

## The One-Sentence Pitch
Once a system spans multiple machines, the hard part stops being "how do I write correct code" and becomes "how do I stay correct when any machine, at any moment, might silently fail."

## Audience & Prerequisites
This talk is for learners comfortable with basic programming and networking concepts (a single client talking to a single server); no prior distributed-systems experience is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: why "just add more machines" is hard |
| 0:05–0:15 | Partial failure and the missing global clock |
| 0:15–0:26 | Consensus: getting machines to agree |
| 0:26–0:37 | Replication and consistency models |
| 0:37–0:46 | The CAP theorem |
| 0:46–0:50 | Key takeaway and close |

### Hook: why "just add more machines" is hard
Open with a deceptively simple scenario: two friends are trying to agree over a bad phone connection whether to meet at 5pm or 6pm, and one message gets dropped. Now one friend thinks they agreed on 5pm, the other doesn't know if their reply arrived, and neither can be fully sure what the other believes — despite both being perfectly rational and honest. This is distributed systems in miniature: the challenge isn't logic, it's uncertainty introduced by an unreliable channel between independent parties. Scale that up to thousands of servers coordinating a bank's ledger or a social network's feed, and you get the central theme of this talk — distributed systems are hard not because the code is complicated, but because the world the code runs in is unreliable in ways a single machine never has to think about.

### Partial failure and the missing global clock
On one machine, either the program is running or it has crashed — there's no in-between, and you always know which. Across multiple machines, you get partial failure: some machines are up, some are down, some are just slow, and from the outside these three situations can look identical — a server that hasn't responded in 10 seconds might be dead, or might just be busy, and you often cannot tell which without waiting arbitrarily long. Compounding this, there is no shared global clock — each machine has its own clock that drifts slightly, so "which event happened first" across two different machines is not a question you can answer just by comparing timestamps. These two facts together — you can't reliably detect failure, and you can't reliably order events — are the root cause of nearly every hard problem in distributed systems, including everything covered next.

### Consensus: getting machines to agree
Consensus is the problem of getting multiple machines to agree on a single value or a single ordering of events, even when some of them might crash or messages might be delayed — for example, multiple database replicas need to agree on which transaction happened first. This sounds simple until you realize any protocol has to work correctly even if a message is lost, delayed, or a machine dies at the worst possible moment mid-protocol. Real, battle-tested algorithms exist to solve this rigorously — Paxos and its more readable descendant Raft are the two most widely referenced — and the pattern they share is roughly: propose a value, get a majority of machines to acknowledge it, and only consider it committed once a majority agrees, so the system tolerates any minority of failures without ever committing two conflicting answers. You don't need to derive Paxos or Raft to use this idea productively — what matters is recognizing that "getting distributed machines to agree on one thing" is a solved, named problem with proven algorithms, not something you should improvise.

### Replication and consistency models
Replication means keeping copies of the same data on multiple machines — for durability (a copy survives if one machine dies) and for performance (readers can be served from whichever copy is closest). The moment you have copies, you have to decide how tightly they stay in sync, and that choice is called a consistency model. Strong consistency means every reader, everywhere, always sees the most recent write immediately — simple to reason about, but it typically requires waiting for replicas to coordinate, which costs latency and can stall entirely if a replica is unreachable. Eventual consistency means replicas are allowed to briefly disagree, but will converge to the same value if writes stop — this is much faster and more available, but it means a user can occasionally read stale data, which is the real tradeoff: correctness-under-pressure versus speed-and-availability, and different applications (a bank balance versus a "likes" counter) land on opposite sides of that tradeoff on purpose.

### The CAP theorem
The CAP theorem gives a name to the tradeoff just described: in the presence of a network partition (some machines can't talk to others), a distributed system must choose between Consistency (every node sees the same data) and Availability (every request gets a response) — you cannot fully guarantee both at the same time, hence "pick two" as the informal slogan, though partition tolerance is really a fact of life you must design around rather than a knob you turn off. Concretely: if the network splits into two groups of servers that can't reach each other, you either refuse to answer some requests until the split heals (favoring consistency) or you let both sides keep answering and risk them disagreeing (favoring availability). This isn't a flaw in any particular system's engineering — it's a mathematical fact about any system with replicated data, which is exactly why it's taught as a theorem rather than a rule of thumb, and why real systems explicitly advertise which side of the tradeoff they've chosen.

### Key takeaway and close
Bring it back to the phone-call analogy from the hook: every technique covered today — consensus protocols, replication strategies, the CAP tradeoff — is a structured way of coping with the same underlying uncertainty, "I can't always tell what the other party knows or whether my message got through." Modern infrastructure (databases, cloud storage, distributed caches) all sit on top of these ideas, usually invisibly, but the moment something behaves strangely under load or during an outage, it's almost always one of these tradeoffs showing itself.

## Key Takeaway
Distributed systems are hard because of partial failure and the lack of a shared clock, not because the code is complex — and consensus, replication, and the CAP theorem are three different lenses on the same unavoidable tradeoff between agreement and availability.

## Go Deeper
- [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.824 — Distributed Computer Systems Engineering](https://ocw.mit.edu/courses/6-824-distributed-computer-systems-engineering-spring-2006/pages/syllabus/)
