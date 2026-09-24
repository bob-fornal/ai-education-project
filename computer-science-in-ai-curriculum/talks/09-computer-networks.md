# Computer Networks

**Backbone Course #9** · **Duration:** 50 minutes

## The One-Sentence Pitch
Every time you load a web page, a stack of independent, layered protocols cooperates to turn "get me this URL" into billions of bits routed correctly across networks that have never met each other.

## Audience & Prerequisites
This talk is for learners who have written basic programs and used the internet daily but have never looked under the hood; no prior networking knowledge is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: the internet is a network of networks |
| 0:05–0:15 | The layered model |
| 0:15–0:25 | IP addressing and routing |
| 0:25–0:35 | TCP vs. UDP |
| 0:35–0:47 | Walkthrough: what happens when you type a URL |
| 0:47–0:50 | Key takeaway and close |

### Hook: the internet is a network of networks
Open by asking: when you type a URL, your laptop has to reach a server it may have never contacted, on a network it doesn't own, possibly on the other side of the planet, in under a second. No single company or machine controls the whole path — the internet is really thousands of independently operated networks that agree to speak common protocols to each other. That word, "protocol," is the theme of the whole talk: an agreed-upon set of rules that lets two machines that know nothing about each other's internals still cooperate correctly. Everything else in this talk is really the story of a handful of protocols stacked on top of each other, each solving one narrow piece of the problem.

### The layered model
Networking is organized into layers — commonly described as application, transport, network, and link — where each layer solves one problem and simply trusts the layer below it to have solved its own. The link layer moves bits between two directly connected devices (like your laptop and your Wi-Fi router); the network layer (IP) gets a packet from any source to any destination across multiple networks; the transport layer (TCP/UDP) adds delivery guarantees on top of that; and the application layer (HTTP, DNS, etc.) is where the actual meaningful data — a web page, an email — lives. The key insight is why layering exists at all: it lets each layer be built, replaced, or debugged independently, so a web developer never has to think about Wi-Fi signal encoding, and a router never has to know or care what a JPEG is. This is the same abstraction principle from programming — hide the details, expose only the necessary interface — applied to an entire global system.

### IP addressing and routing
Every device on the internet gets an IP address, a numeric identifier (like 93.184.216.34) that plays the same role a street address plays for physical mail. Routing is the process by which a packet, carrying a destination IP address, gets passed from router to router — each router doesn't know the entire path to the destination, it only knows "the best next hop to send this toward," the same way you might not know the full driving directions to a stranger's house but you know which highway on-ramp gets you closer. Routers maintain routing tables built from protocols that let them share reachability information with their neighbors, so the network can adapt automatically when a link goes down. The remarkable part: a packet can cross a dozen networks owned by a dozen different organizations and still arrive correctly, because every one of those routers agreed to speak the same addressing and forwarding rules.

### TCP vs. UDP
Once a packet can reach a destination, you need to decide how carefully to deliver it — and there are two dominant answers. TCP (Transmission Control Protocol) is reliable and ordered: it numbers every byte, waits for acknowledgments, retransmits anything lost, and reassembles data in the correct order before handing it to the application — this is what web pages, file downloads, and email need, because a corrupted or missing chunk would break the whole thing. UDP (User Datagram Protocol) is the opposite: it just fires packets off with no acknowledgment, no retransmission, and no guaranteed order, trading reliability for speed and lower overhead. The reason both exist is a real engineering tradeoff: video calls and online games would rather drop a frame than wait half a second for a retransmitted one, so they use UDP, while a bank transfer absolutely cannot tolerate a silently dropped byte, so it uses TCP.

### Walkthrough: what happens when you type a URL
Trace one concrete request end to end: you type `example.com` into a browser, and first your machine needs an IP address, so it sends a DNS query — effectively "what's the phone number for this name?" — to a DNS resolver, which answers with an IP address after possibly asking several other DNS servers up the chain. With that IP address in hand, your browser opens a TCP connection to the server (a "handshake" that sets up reliable delivery), and then sends an HTTP request — a plain-text-ish message that says "GET me this page" — over that connection. The server processes the request and sends back an HTTP response containing the page's HTML, which travels back down through TCP (broken into segments, reassembled in order), IP (routed hop by hop), and the link layer, arriving at your browser to be rendered. Point out explicitly how every layer discussed so far shows up here: DNS and HTTP are application-layer, TCP is transport-layer reliability, IP is network-layer routing, and it all happens in a fraction of a second, invisibly, thousands of times a day.

### Key takeaway and close
Close by reinforcing that this entire system works because of agreement on interfaces, not central control — no one owns the internet, but everyone who connects to it agrees to speak IP, TCP/UDP, DNS, and HTTP correctly. That's why a phone from one manufacturer can talk to a server from a completely different company on a different continent without either side needing to know anything about the other's hardware or software.

## Key Takeaway
The internet works because of layered, agreed-upon protocols — not central coordination — and a single page load is really four or five independent systems (DNS, HTTP, TCP, IP, the link layer) cooperating without any of them needing to understand the others' internals.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.829 — Computer Networks](https://ocw.mit.edu/courses/6-829-computer-networks-fall-2002/pages/syllabus/)
