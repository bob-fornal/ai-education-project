# Repository Structure

[Back to docs](README.md)

```text
ai-education-project/
├── README.md                          # Project entry point
├── docs/                              # Curriculum guides + learning paths (this folder)
├── scientific-method/                 # Diagnostic method, experiment log, 8 domain playbooks
├── mentoring/                         # Tiered mentoring model: both tracks + operations
│
├── computer-science-in-ai-curriculum/
│   ├── README.md                      # Backbone index + priority rankings
│   ├── LICENSE                        # MIT
│   ├── talks/                         # 32 full 50-minute talk outlines (01–32)
│   ├── curriculum/                    # 7 university checklists (~3,300 talk titles)
│   ├── documentation/CHAT-HISTORY.md  # How the curriculum was built, and why
│   └── images/                        # Cover art (.jpg, plus a GIMP .xcf source)
│
└── computer-science-software-design-and-architecture/
    ├── README.md                      # 33-topic index, summaries, homework, capstone
    ├── curriculum/
    │   └── part-N/part-N-M--topic-name/
    │       ├── README.md              # Session plan for the topic
    │       ├── outline.md             # (some topics) expanded session outline
    │       ├── homework.md            # (some topics) homework notes and hints
    │       └── examples/*.py          # (some topics) starter code
    └── images/
```

## Scientific Method and Mentoring

**`scientific-method/`** has an overview (`README.md`), an `experiment-log-template.md`, and one playbook per domain: `frontend.md`, `backend.md`, `database.md`, `quality-assurance.md`, `devops.md`, `cloud.md`, `infrastructure.md`, and `security.md`. Every playbook uses the same sections: typical symptoms, observation tooling, common hypotheses with discriminating experiments, a worked example, experiment techniques, AI prompts, and links to theory in the curricula.

**`mentoring/`** has an overview (`README.md`), the two tracks (`senior-to-mid.md` and `mid-to-junior.md`), and `program-operations.md` for rollout and measurement.

## Computer Science in AI curriculum

**`talks/`** has 32 files named `NN-subject-name.md`. Numbers 01–27 are the backbone subjects. Numbers 28–32 are algorithm deep-dives that outgrew their parent talk. Every talk follows the same template:

1. Title, backbone number, and duration
2. **The One-Sentence Pitch**
3. **Audience & Prerequisites**
4. **50-Minute Outline**: a time-boxed segment table that sums to exactly 50 minutes
5. One section per segment with enough detail to deliver the talk from the outline alone
6. **Key Takeaway**
7. **Go Deeper**: links to the matching school checklists and free MIT OCW or Harvard PLL courses

**`curriculum/`** has one checklist per school: Amherst, BGSU, Purdue, MIT OCW, Harvard SEAS, Stanford, and CMU. Each file opens with a note on its source and methodology, then lists courses (linked to the syllabus used) with `- [ ]` talk items you can tick off as talks get scheduled or delivered.

**`documentation/CHAT-HISTORY.md`** is the build log. It records the decisions, data-quality problems found in the sources (a stale Purdue syllabus page, a BGSU title mismatch, Stanford's 2002–03 bulletin), and why the backbone landed at 27 subjects.

## Software Design & Architecture curriculum

Topics are numbered 1–33 and grouped into Parts 1–8. Folder names follow `part-<part>-<topic>--<slug>`, for example `part-3-12--architectural-patterns`.

Every topic folder has a `README.md` with:

- One-sentence pitch
- Learning objectives, written as "Can ..." statements
- A ~55-minute session outline table with segments and minutes
- Prose for each segment

Three topics carry extra material:

| Topic | Extras |
|---|---|
| 9. Design Patterns (GoF & PoSA) | `outline.md`, `homework.md`, `examples/homework2_before.py`, `examples/posa_starter.py` |
| 12. Architectural Patterns | `outline.md`, `homework.md`, `examples/mvc_order.py`, `examples/cqrs_order.py` |
| 17. Scaling Applications | `examples/example1_externalized_sessions.py`, `examples/example2_service_registry.py` |

The Python examples are starter skeletons with `TODO` markers, not finished solutions. Some depend on third-party packages (Flask and Redis for the session example).

## Conventions

- **Markdown only.** No build step. Everything renders on GitHub or in any Markdown viewer.
- **Relative links everywhere.** Files link back to their parent index, so you can navigate without the root README.
- **Sources are cited, not invented.** Both curricula record where each item came from and flag gaps rather than filling them with guesses.
- **Free companions only.** External resources were checked for paywalls and signup walls. Where nothing free existed (TOGAF, BABOK), the README says so.
- **Checklists are for tracking.** The `- [ ]` items in the school files are meant to be checked off in your own fork.
