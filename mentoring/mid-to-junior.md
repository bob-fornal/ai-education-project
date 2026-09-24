# Mid-level → Junior Track: AI as a Developer in the Current Codebases

[Back to mentoring](README.md)

## Purpose

Teach Junior developers to do real work in the team's existing codebases with AI as a core tool, and to do it safely. By the end of this track, a Junior should be able to:

- Use AI to orient in unfamiliar code, and verify what it says against the code itself
- Break a ticket into well-specified pieces an AI can help with
- Read every line of AI-generated code critically, and explain every line they merge
- Test AI output properly, starting from a failing test
- Debug with the [Scientific Method](../scientific-method/README.md), using AI as a lab assistant, not an oracle
- Keep secrets and customer data out of AI tools
- Recognize when to stop prompting and think, read, or ask a person

## Why Mid-levels teach this

- **They use this workflow every day** in the same codebases the Junior will work in, with the same tools and conventions.
- **They remember being Junior.** They know which parts are confusing because they were confused by them recently.
- **Teaching makes them better.** Explaining why a piece of AI output is wrong forces the Mid-level to articulate knowledge they may only hold intuitively. That's also what the [Senior → Mid track](senior-to-mid.md) is building.
- **It frees Seniors** to spend their time on the deeper material.

## Core competencies

### 1. Codebase orientation with AI

- Ask AI to explain a module, trace a request path, or summarize a file's responsibilities.
- **Then check.** Open the files it named. Set a breakpoint or add a log line and confirm the path is real.
- Keep a personal "codebase map" note, and correct it when you find the AI was wrong.
- Read the project's instructions files (`CLAUDE.md`, `AGENTS.md`, `.github/copilot-instructions.md`, `CONTRIBUTING.md`, or whatever the team uses). They tell both you and the AI how this codebase works.

**Mentor exercise:** give the Junior a module. They ask AI for a walkthrough, then find two things the AI got wrong or left out. There's almost always something.

### 2. Decomposition and specification

A vague prompt gets vague code. A good request to an AI looks like a good ticket:

- **Context:** the files, types, and conventions involved
- **Goal:** what should be true when it's done
- **Constraints:** what not to change, which libraries to use or avoid, performance or security needs
- **Acceptance criteria:** ideally as tests
- **Size:** small enough that you can review every line of the result

This is [CS01 Introduction to Programming](../computer-science-in-ai-curriculum/talks/01-introduction-to-programming.md) in a new form: decomposition is still the core skill.

**Mentor exercise: prompt retro.** After a task, review the Junior's prompts together. Which prompts produced good output, and why? Which produced churn? Rewrite one together.

### 3. Reviewing AI output

Treat every AI change like a pull request from a fast, confident contributor who has never seen your production system. Check:

- [ ] Can I explain what every line does and why it's there?
- [ ] Does it follow this codebase's conventions, or generic ones?
- [ ] Did it change anything I didn't ask for?
- [ ] Are the APIs, functions, config keys, and packages it uses real? (Hallucinated APIs are common.)
- [ ] Error handling: what happens on null, empty, timeout, or failure?
- [ ] Is there a simpler way that the AI skipped?
- [ ] Security: input validation, authorization, secrets, injection (see the [security review checklist](../scientific-method/security.md#reviewing-ai-generated-code-for-security))
- [ ] Performance: loops inside loops, queries inside loops, loading whole datasets

The [Clean Code Principles](../computer-science-software-design-and-architecture/curriculum/part-1/part-1-1--clean-code-principles/README.md) topic is the standard to hold AI output to.

### 4. Testing AI output

- **Write or generate the failing test first.** Watch it fail. Then have AI help with the implementation.
- Review generated tests with the [AI-generated test checklist](../scientific-method/quality-assurance.md#reviewing-ai-generated-tests). Break the code on purpose and confirm the test catches it.
- Run the full relevant suite locally before pushing, not just the new test.

### 5. Debugging with the Scientific Method

- Don't paste the error into AI and apply whatever fix comes back.
- Do use AI to brainstorm competing hypotheses, write repro scripts, and explain stack traces.
- Write an [experiment log](../scientific-method/experiment-log-template.md) for any bug that takes more than an hour. The Mid-level reviews it.
- Start with the playbook for the layer where the symptom shows: [frontend](../scientific-method/frontend.md), [backend](../scientific-method/backend.md), [database](../scientific-method/database.md), and so on.

### 6. Security and data hygiene

- Never paste secrets, credentials, tokens, customer data, or production logs with personal data into AI tools, unless the tool is approved for that data.
- Use only the AI tools the organization has approved, with the settings it has approved.
- Check that any dependency the AI adds actually exists, is maintained, and is the package you think it is.

### 7. Knowing when to stop

Stop prompting and change approach when:

- You've gone three rounds on the same problem and the AI keeps producing variations of the same wrong answer
- You can no longer explain the code you have
- The change is growing well beyond the ticket
- The problem involves domain rules or business context the AI can't know

Then read the code, write down a hypothesis, or ask your mentor. Asking is part of the job.

### 8. Owning the change

- You're the author of anything you merge, whoever typed it.
- Write PR descriptions that explain *why*, not just what. Mention AI assistance where team norms ask for it.
- In review, be ready to answer "why is this line here?" for every line.

## Stages

The track uses gradual release: I do, we do, you do.

| Stage | Weeks | What happens | Exit signal |
|---|---|---|---|
| **1. Shadow** | 1–2 | Junior watches the Mid-level work tickets with AI, narrating prompts, rejections, and checks out loud. Junior sets up tools and reads team conventions. | Junior can describe the Mid-level's workflow and why each check matters |
| **2. Pair** | 3–6 | Junior drives, Mid-level navigates, AI assists. Small, well-bounded tickets. Daily short check-ins. | Junior completes tickets with the Mid-level mostly observing |
| **3. Guided independence** | 7–12 | Junior works tickets alone. Mid-level reviews every PR and one experiment log per week. Two sessions a week. | PRs pass review with only minor comments, and the Junior catches AI mistakes before the Mid-level does |
| **4. Independent** | 12+ | Normal team review process. Weekly session for harder topics and career questions. | Junior starts helping newer Juniors in small ways |

Timing is a guide. Move on when the exit signal is clear, not when the calendar says so.

## Session formats

| Format | How it works |
|---|---|
| **Pair programming with AI** | Junior drives, Mid-level navigates, AI is a third participant. Mid-level asks "why did you accept that?" often. |
| **Prompt retro** (20 min) | Review the Junior's prompt history for a task. What worked, what caused churn, how to write it better. |
| **PR walk-through** (30 min) | Junior explains their PR line by line before submitting. Anything they can't explain gets reworked. |
| **"What's wrong with this?"** (20 min) | Mid-level brings a piece of AI-generated code with planted or real mistakes. Junior finds them. |
| **Guided bug hunt** (60 min) | Pick a real bug. Junior runs the Scientific Method loop, and the Mid-level coaches on hypothesis quality. |
| **Codebase tour, verified** (45 min) | Junior uses AI to explain a subsystem, then proves or corrects each claim with the debugger. |

## Ground rules for Juniors

1. Don't merge code you can't explain.
2. Tests before trust. A change without a test that would catch it breaking isn't done.
3. AI output is a draft, never a source. Verify APIs, facts, and citations.
4. Keep sensitive data out of AI tools.
5. If you've been stuck for an hour, write down what you've tried and ask.

## Supporting reading for Juniors

Alongside this track, Juniors work through the Junior path in [learning paths](../_DOCS/learning-paths.md). The core of it is the Critical talks most relevant to daily work: [CS01](../computer-science-in-ai-curriculum/talks/01-introduction-to-programming.md), [CS02](../computer-science-in-ai-curriculum/talks/02-data-structures.md), [CS14](../computer-science-in-ai-curriculum/talks/14-software-engineering.md), [CS15](../computer-science-in-ai-curriculum/talks/15-computer-and-network-security.md), [CS11](../computer-science-in-ai-curriculum/talks/11-database-systems.md), and Part 1 of the [Software Design & Architecture curriculum](../computer-science-software-design-and-architecture/README.md#part-1--foundations-of-code-quality).

## What Mid-level mentors should avoid

| Avoid | Why | Instead |
|---|---|---|
| Fixing the Junior's code for them | They learn that someone else will catch it | Ask questions that lead them to the problem |
| Letting AI do the teaching | The Junior learns to prompt, not to reason | Use AI in sessions, then ask the Junior to explain the result without it |
| Banning AI "until they learn the basics" | That's not how the team works, and it delays the real skill | Use AI from day one, with verification built into every step |
| Only reviewing the final PR | Mistakes in approach are expensive to fix late | Check in on approach before the Junior writes much code |
| Taking on too many Juniors | Quality drops, and the Mid-level's own growth stalls | One or two Juniors per Mid-level at most |
| Hiding what you don't know | Juniors learn that not knowing is shameful | Say "let's find out" and run the experiment together. Escalate to your Senior when needed. |
