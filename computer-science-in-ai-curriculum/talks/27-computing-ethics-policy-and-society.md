# Computing Ethics, Policy & Society

**Backbone Course #27** · **Duration:** 50 minutes

## The One-Sentence Pitch
Every technical decision — what data to collect, what to train a model on, what the default setting is — encodes a value judgment, whether or not the person making it intended one.

## Audience & Prerequisites
For any learner who has used or built software; no technical prerequisites beyond general familiarity with apps, recommendation systems, and online services.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: there's no neutral pipe |
| 0:05–0:15 | The privacy-vs-utility tradeoff |
| 0:15–0:27 | Algorithmic bias and fairness |
| 0:27–0:39 | Intellectual property and open source |
| 0:39–0:47 | The technologist's responsibility: a case study |
| 0:47–0:50 | Key takeaway and close |

### Hook: there's no neutral pipe
Open by challenging a comforting myth: that software engineers are neutral executors just "building what's asked for," with no moral stake in outcomes. Every system has to make choices — what to log, what to default to, what to optimize for — and those choices are never value-free, even when nobody explicitly debated them. Frame the talk as a tour of four places this shows up concretely: privacy tradeoffs, algorithmic bias, intellectual property, and the responsibility that comes with being the one who writes the code.

### The privacy-vs-utility tradeoff
Establish the core tension: nearly every genuinely useful data-driven product works by knowing something about the user, and that knowledge is exactly what creates a privacy cost. Walk through a recommendation-system example: a streaming service that knows your viewing history can recommend shows you'll actually like — genuinely useful — but that same history, if leaked, sold, or subpoenaed, reveals detailed personal information about your habits, health, or beliefs. Do the same with location data: a maps app needs your location to route you, and aggregated location data can improve traffic prediction for everyone, but that same data trail can reveal where you live, work, worship, or seek medical care, with no easy way to "opt out" of the inference once enough data points exist. Emphasize that there is no version of these products with zero privacy cost — the real design question is always how much data is truly necessary, how long it's kept, and who else gets access, not whether to collect any.

### Algorithmic bias and fairness
Introduce the core mechanism of algorithmic bias: a model trained on historical data learns the patterns in that data, including patterns that reflect past discrimination — and because the model treats those patterns as signal to optimize for, it can reproduce and even amplify the bias at scale. Give the hiring-model example concretely: a resume-screening model trained on a company's past hiring decisions will learn whatever biases were present in those human decisions (for example, favoring certain schools or demographics), and because it's applied automatically to thousands of applicants, it can systematize a bias that used to be inconsistent and human-scale into something consistent and much larger-scale. Note the lending-model parallel: a credit model trained on historical loan data can learn to associate creditworthiness with proxies for race or zip code, denying credit unfairly even without ever using a protected attribute directly, because correlated features leak the same signal. Land the key point: "the model wasn't told to be biased" is not a defense — biased outputs from biased training data are the default outcome unless someone actively audits for and corrects it.

### Intellectual property and open source
Explain how copyright and licensing shape the software supply chain: code, unlike physical goods, can be copied at zero marginal cost, so copyright law exists to give creators exclusive rights over copying, modification, and distribution — which means every piece of software you build on top of carries legal terms about what you're allowed to do with it. Contrast proprietary licensing (permission to use under specific restrictions, often no right to see or modify source) with open source licensing, and stress that open source is a deliberate legal and social model, not simply "free" — licenses like MIT, Apache 2.0, or GPL each make different explicit choices about whether derivative works must also be open, whether attribution is required, and whether patent rights are granted. Note the practical stakes: choosing the wrong license, or ignoring license terms of a dependency, can create real legal exposure for a company, and the open-source movement itself was a values-driven response to a world where software was becoming increasingly locked down.

### The technologist's responsibility: a case study
Bring the theme of "you are not a neutral pipe" to life with a defaults/dark-patterns example: a subscription service where signing up is a single click but canceling requires navigating multiple confirmation screens, phone calls, or hidden menus. Point out that this isn't an accident of bad UX — it is a deliberate design decision, made by an engineer and product team, that encodes the value "maximize retention" over "make user intent easy to act on." Contrast it with an opt-in-by-default privacy setting versus an opt-out-by-default one: functionally similar to build, but they produce dramatically different real-world outcomes because most users never change a default. Use this to make the responsibility framing concrete: the engineer who implements the confirmation-screen gauntlet, or who sets the default toggle position, is not a passive executor — they are the person whose choice determines the actual behavior of the system in the world.

### Key takeaway and close
Recap the four threads — privacy tradeoffs, algorithmic bias, IP/licensing, and dark patterns — as different facets of the same truth: technical decisions are never neutral, and the people who make them carry real responsibility for the values those decisions encode.

## Key Takeaway
There is no such thing as a neutral technical decision — every choice about data collection, model training, licensing, or default settings encodes a value judgment, and recognizing that is the first step toward making that judgment a deliberate and defensible one.

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.805 — Ethics and the Law on the Electronic Frontier](https://ocw.mit.edu/courses/6-805-ethics-and-the-law-on-the-electronic-frontier-fall-2005/pages/syllabus/)
