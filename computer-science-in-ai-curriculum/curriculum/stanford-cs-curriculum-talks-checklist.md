# Stanford CS Curriculum — 50-Minute Talk Checklist

Sources:
- [Stanford-CS-Course README (isLinXu, GitHub)](https://github.com/isLinXu/Stanford-CS-Course/blob/main/README.en.md) — the current curated list of ~150 Stanford CS course pages, organized by topic
- [Stanford Bulletin 2002-03, Computer Science excerpt (PDF)](https://web.stanford.edu/dept/registrar/bulletin_past/bulletin02-03/pdf/CompSci.pdf) — official Stanford Registrar course catalog with full descriptions

**A note on this one — it's the messiest of the six schools so far.** The GitHub README is a link aggregator: course codes and titles with links to each course's own (often archived, sometimes video-only) website, but no descriptions of its own. It's not a syllabus source the way Amherst's or Harvard's catalog pages were. The 2002-03 Registrar bulletin *is* a real syllabus-equivalent source — official course-by-course descriptions — but it's 23 years old. Stanford CS course numbers have been reused, retired, and reassigned repeatedly since then (CS107 meant something different then than now; CS246 was "Information Integration," not "Mining Massive Data Sets"; CS275 was a music-information course, not bioinformatics), so a naive number-match would produce wrong talks for a lot of courses.

So: I cross-referenced every course number in the current README against the 2002-03 bulletin, kept only the ~50 courses where the number **and** the subject matter clearly correspond to the same course lineage (allowing for a title that evolved over two decades, e.g. CS229 "Statistical Learning and Pattern Classification" → today's "Machine Learning"), and built talks from the bulletin's own topic lists for those. For the roughly 100 remaining README entries — mostly newer courses that didn't exist in 2002-03 (CS231N, CS224N's modern deep-learning-era successor content, CS230, CS329S, etc.) or courses whose number now maps to unrelated content — there's nothing in the provided sources to build a talk from, so they're listed by category at the bottom **without invented talks**, with a pointer back to the README's live links for anyone who wants to check the current syllabus directly.

## Introduction to Computer Science

**CS103 — Mathematical Foundations of Computing** *(bulletin: CS103A/B/X, Discrete Mathematics/Structures)*
- [ ] Logic, Sets, and Proof Techniques
- [ ] Combinatorics, Recursion, and Recurrence Relations
- [ ] Analyzing Algorithms: Mathematical Models of Data
- [ ] Regular Expressions and Formal Grammars

**CS105 — Introduction to Computers**
- [ ] How Computers Work: Hardware and Program Construction Basics
- [ ] Programming Fundamentals for Non-Majors
- [ ] Surveying Internet Technology

**CS106A — Programming Methodologies**
- [ ] Software Engineering Principles: Decomposition and Abstraction
- [ ] Programming Style and Testing Fundamentals

**CS106B — Programming Abstractions**
- [ ] Data Abstraction and Modules
- [ ] Stacks, Queues, and Data-Directed Design
- [ ] Recursion and Recursive Data Structures
- [ ] Introduction to Time and Space Complexity

**CS106X — Programming Abstractions in C++** *(bulletin: Programming Methodology and Abstractions, Accelerated)*
- [ ] Accelerated C++ Programming Fundamentals
- [ ] Abstraction, Recursion, and Data Structures at Speed

## Data Structures & Algorithms

**CS161 — Design and Analysis of Algorithms**
- [ ] Sorting, Searching, and Selection Algorithms
- [ ] Recurrences, Asymptotics, and Balanced Data Structures
- [ ] Divide-and-Conquer, Dynamic Programming, and Greedy Design
- [ ] Graph Algorithms: Traversal, Components, and Shortest Paths
- [ ] Network Flow and String Searching Algorithms

## Computer Systems

**CS107 — Programming Paradigms**
- [ ] Advanced C: Memory Management and Multithreading
- [ ] The Functional Paradigm in Lisp
- [ ] Object-Oriented Programming in Java
- [ ] Generic Programming with C++ Templates

**CS110 — Principles of Computer Systems** *(bulletin: Introduction to Computer Systems and Assembly Language Programming)*
- [ ] Computer Organization: Buses, Registers, and Memory Systems
- [ ] Data Representation and Computer Arithmetic
- [ ] Assembly Language Programming: Subroutines, Interrupts, and Traps
- [ ] Operating Systems Concepts at the Systems-Programming Level

**CS140 — Operating Systems**
- [ ] Operating System Structure and Process Management
- [ ] Synchronization and Communication Mechanisms
- [ ] Memory Management and Virtual Memory
- [ ] File Systems and I/O Device Management

**CS143 — Compilers**
- [ ] Lexical Analysis and Parsing Theory
- [ ] Symbol Tables, Scoping, and Type Systems
- [ ] Semantic Analysis and Intermediate Representations
- [ ] Runtime Environments and Code Generation

**CS240 — Advanced Topics in Operating Systems**
- [ ] Advanced Virtual Memory Management
- [ ] OS Extension Techniques and Fault Tolerance
- [ ] Protection and Security in Operating Systems

**CS244b — Distributed Systems**
- [ ] Distributed Shared Memory and System Design
- [ ] Atomic Transactions and Time Synchronization
- [ ] Remote Procedure Call and Process Migration

**CS315B — Parallel Programming** *(bulletin: Parallel Programming Project)*
- [ ] Parallel Programming Models: Shared Memory and Message Passing
- [ ] Performance Debugging of Parallel Programs

**CS344 — Topics in Computer Networks** *(bulletin: Projects in Computer Networks)*
- [ ] Network Research Projects: From Physical Layer to Applications
- [ ] Analyzing Network Traffic and Performance

## Theoretical Computer Science

**CS154 — Introduction to the Theory of Computation** *(bulletin: Automata and Complexity Theory)*
- [ ] Finite Automata and Regular Languages
- [ ] Context-Free Grammars and Pushdown Automata
- [ ] Turing Machines and Undecidability
- [ ] NP-Completeness and Cook's Theorem

**CS157 — Computational Logic** *(bulletin: Logic and Automated Reasoning)*
- [ ] Propositional and Predicate Logic Foundations
- [ ] Models, Validity, and Proof
- [ ] Automated Deduction: Resolution and Unification

**CS242 — Programming Languages**
- [ ] Functional, Imperative, and Object-Oriented Paradigms
- [ ] Formal Semantics and Modern Type Systems
- [ ] Closures, Continuations, and Exception Handling
- [ ] Language Runtime Support and Implementation

**CS243 — Program Analysis and Optimizations** *(bulletin: Advanced Compiling Techniques)*
- [ ] Intermediate Representations and Control-Flow Graphs
- [ ] Data Flow Analysis for Optimization
- [ ] Register Allocation and Global Code Optimization
- [ ] Interprocedural Program Analysis

**CS261 — Optimization and Algorithmic Paradigms**
- [ ] Network Flow, Matching, and Assignment Problems
- [ ] Linear Programming and LP Duality
- [ ] Approximation Algorithms for NP-Complete Problems
- [ ] Randomized and Online Algorithms

**CS351 — Topics in Complexity Theory and Lower Bounds**
- [ ] Complexity Classes, Reductions, and Complete Problems
- [ ] Lower-Bound Techniques: Decision Trees and Communication Games
- [ ] Approximation Complexity and Pseudo-Randomness

**CS358 — Programming Language Foundations** *(bulletin: Topics in Programming Language Theory)*
- [ ] Operational Semantics and Domain Theory
- [ ] Semantics of Concurrency
- [ ] Type Disciplines and Representation Independence

## Artificial Intelligence & Machine Learning

**CS221 — Artificial Intelligence: Principles and Techniques**
- [ ] Search, Planning, and Constraint Satisfaction
- [ ] Knowledge Representation and Probabilistic Models
- [ ] Machine Learning and Neural Networks in AI
- [ ] AI Applications: Vision, Robotics, and Language

**CS224N — Natural Language Processing with Deep Learning** *(bulletin: Natural Language Processing)*
- [ ] Morphological and Syntactic Processing
- [ ] Semantic Processing: Linguistic and Algorithmic Views
- [ ] Statistical and Corpus-Based NLP Techniques

**CS227B — General Game Playing** *(bulletin: CS227, Reasoning Methods in Artificial Intelligence)*
- [ ] Propositional Satisfiability and Constraint Satisfaction
- [ ] AI Planning, Scheduling, Diagnosis, and Repair

**CS228 — Probabilistic Graphical Models: Principles and Techniques** *(bulletin: Probabilistic Models in Artificial Intelligence)*
- [ ] Bayesian Belief Networks
- [ ] Hidden Markov Models and Dynamic Bayesian Networks
- [ ] Decision Making with Influence Diagrams and MDPs
- [ ] Applications of Graphical Models: Speech, Diagnosis, Data Mining

**CS229 — Machine Learning** *(bulletin: Statistical Learning and Pattern Classification)*
- [ ] Statistical Pattern Recognition and Density Estimation
- [ ] Linear and Nonlinear Classifiers, Decision Trees
- [ ] Bayesian Networks and Neural Networks for Learning
- [ ] Reinforcement Learning and Learning Theory
- [ ] Boosting and Support Vector Machines

## Computer & Network Security

**CS155 — Computer and Network Security**
- [ ] Network Attacks and Defenses
- [ ] Operating System and Application Security
- [ ] Viruses, Social Engineering, and Privacy

**CS255 — Introduction to Cryptography**
- [ ] Symmetric and Public-Key Encryption
- [ ] Digital Signatures and Authentication
- [ ] Zero-Knowledge Protocols and PKI
- [ ] Cryptography for Electronic Commerce

**CS355 — Advanced Topics in Cryptography**
- [ ] Pseudo-Random Generation and Zero-Knowledge Protocols
- [ ] Elliptic Curve and Threshold Cryptography
- [ ] Random Oracle Security Analysis

## Information Management & Analytics

**CS145 — Data Management and Data Systems** *(bulletin: Introduction to Databases)*
- [ ] The Relational Model, SQL, and Relational Algebra
- [ ] Database Design: Functional Dependencies and Normal Forms
- [ ] Transactions, Indexes, and Authorization
- [ ] Data Warehousing and Mining Fundamentals

**CS245 — Principles of Data-Intensive Systems** *(bulletin: Database System Principles)*
- [ ] File Organization and Buffer Management
- [ ] Query Optimization in Database Systems
- [ ] Transaction Management, Recovery, and Concurrency Control

**CS276 — Information Retrieval and Web Search** *(bulletin: Text Information Retrieval, Mining, and Exploitation)*
- [ ] Text Indexing and Retrieval Models
- [ ] Web Search: Crawling and Link-Based Ranking
- [ ] Document Clustering, Classification, and Information Extraction
- [ ] Question Answering and Text Mining

## Human-Computer Interaction

**CS147 — Introduction to Human-Computer Interaction Design**
- [ ] Usability, Affordances, and Direct Manipulation
- [ ] Interface Metaphors and Conceptual Models
- [ ] Ergonomics and Case Studies in Interface Design

**CS247 — Interaction Design Studios** *(bulletin: HCI: Interaction Design Studio)*
- [ ] Needs Analysis and User Observation Techniques
- [ ] From Sketches to Storyboards: Concept Generation
- [ ] Usability Analysis and Market Strategy for Interfaces

**CS547 — Human-Computer Interaction Seminar**
- [ ] Current Research Directions in HCI: A Seminar Survey

## Graphics

**CS148 — Introduction to Computer Graphics and Imaging** *(bulletin: Introductory Computer Graphics)*
- [ ] Scan Conversion and Geometric Transformations
- [ ] Curves, Surfaces, and 3D Viewing
- [ ] Hidden Surface Removal and Illumination Models
- [ ] Practical Graphics Programming with OpenGL

**CS248 — Interactive Computer Graphics** *(bulletin: Introduction to Computer Graphics)*
- [ ] Scene Modeling and 2D/3D Transformations
- [ ] Visible Surface Determination Algorithms
- [ ] Local and Global Shading Models
- [ ] Real-Time Rendering Techniques

**CS348A — Computer Graphics: Geometric Modeling & Processing**
- [ ] Homogeneous Coordinates and Perspective Transformations
- [ ] Bezier Curves, B-Splines, and Subdivision Surfaces
- [ ] Solid Representations and Mesh Generation

**CS348C — Computer Graphics Animation and Simulation**
- [ ] Traditional Animation Principles and Physical Simulation
- [ ] Motion Capture and Motion Re-Targeting
- [ ] Kinematic and Dynamic Modeling for Animation

**CS348E — Character Animation: Modeling, Simulation & Control** *(bulletin: CS448 special topic, Physics Based Animation for Modeling Virtual Humans)*
- [ ] Rigid Body Dynamics for Character Skeletons
- [ ] Deformable Models for Tissue, Skin, and Clothing
- [ ] Collision Detection and Physics-Based Character Animation

**CS448 — Computational Photography** *(bulletin: Topics in Computer Graphics, rotating special topics)*
- [ ] Emerging Input and Display Technologies in Graphics
- [ ] Research Topics in Modeling Shape and Motion

## Robotics

**CS223A/ME320 — Introduction to Robotics**
- [ ] Robot Kinematics and Dynamics
- [ ] Motion Planning and Trajectory Generation
- [ ] Robot Control Systems

**CS225A — Experimental Robotics**
- [ ] Compliant Motion and Force Control
- [ ] Sensor-Based Collision Avoidance
- [ ] Robot-Human Interfaces

**CS326 — Topics in Advanced Robotic Manipulation** *(bulletin: CS326A, Motion Planning)*
- [ ] Configuration Space and Collision-Free Path Planning
- [ ] Motion Planning Under Uncertainty
- [ ] Applications: Assembly Planning and Radiosurgery

**CS327A — Advanced Robotic Manipulation**
- [ ] Whole-Body Control and Task-Posture Decomposition
- [ ] Cooperative Robots and Haptic Teleoperation
- [ ] Human-Friendly Robot Design

## Computing & Society / Other

**CS206 — Technical Foundations of Electronic Commerce**
- [ ] Online Auctions and Trading Mechanisms
- [ ] Safe Exchange, Copyright Protection, and Online Payments
- [ ] Personalization and Web Infrastructure for E-Commerce

**CS198 — Teaching Computer Science**
- [ ] Teaching Introductory Programming: Skills and Techniques

**CS298 — Seminar on Teaching Introductory Computer Science**
- [ ] Topics in Teaching Introductory Computer Science

## Healthcare Informatics

**BIODS220 (CS271, BIOMEDIN220) — Artificial Intelligence in Healthcare** *(bulletin: CS271, Introduction to Clinical Systems)*
- [ ] Clinical Information and Imaging Systems
- [ ] Decision-Support Technology in Healthcare

---

## Not covered by the 2002-03 bulletin

These courses appear in the current README course list but have no corresponding description in the 2002-03 bulletin — either because they didn't exist yet (most of the deep-learning/LLM-era offerings) or because their course number has since been reassigned to unrelated content. No talks are listed for them; each has a live link in the [source README](https://github.com/isLinXu/Stanford-CS-Course/blob/main/README.en.md) worth checking directly if you want to build talks from the current syllabus.

- **Intro to CS:** CS101, CS106E
- **Systems:** CS107A, CS107E, CS140E, CS149, CS240LX, CS348K, CS357S
- **Theory:** CS103A (modern version), CS106L, CS109, CS151, CS168, CS334A/EE364A, CS334B/EE364B, MS&E213/CS269O, CS254, CS254B, CS265, CS349D
- **AI & Machine Learning:** CS229M, CS205L, CS131, CS231A, CS231N, CS236G, CS239, CS224W, CS224U, CS224V, CS294S/294W, CS224S, CS20, CS230, CS234, CS329S, CS236, CS330, CS331B, CS320, CS217, CS472, CS335, CS348I, CS528 (current MLSys Seminar — unrelated to the 2002-03 CS528 colloquium of the same number)
- **Cybersecurity:** CS110L, CS144, CS244 (current "Advanced Topics in Networking"), CS253, CS350, CS356 (current version — unrelated to the 2002-03 CS356)
- **Analytics:** CS102, CS124, CS166, CS246 (current "Mining Massive Data Sets" — unrelated to the 2002-03 CS246 "Information Integration"), CS246H, CS448B
- **HCI:** CS377E, CS377U, CS422, CS347 (current version — unrelated to the 2002-03 CS347 "Transaction Processing and Distributed Databases")
- **Graphics:** CS233, CS468 (current ML-focused version — unrelated to the 2002-03 CS468 "Topics in Geometric Algorithms")
- **Development:** CS142, CS190, CS193A, CS193P, CS193Q, CS193X, CS194A, CS47
- **Games:** CS193U, CS269I
- **Computing & Society:** CS106S, CS181/181W, CS182, CS184, CS202, CS204, CS21SI, CS278, CS322, CS325B, CS384, CS398, CS51 (current "CS+Social Good" — unrelated to the 2002-03 CS51 "Introduction to Quantum Computing")
- **Biocomputation:** CS235, CS273A, CS275 (current bioinformatics course — unrelated to the 2002-03 CS275A/B, which were music-information courses), CS279
- **Healthcare:** CS342, CS371, CS372, CS522, CS523
- **Other:** CS007, CS349F, CS251, CS208E, CS249i, CS97SI, CS9, CS197 (current version — unrelated to the 2002-03 CS197 "Mainframe and Workstation Computer Consulting"), CS238 (current "Decision Making under Uncertainty" — unrelated to the 2002-03 CS238 "Parallel Methods in Numerical Analysis"), EE185/285/CS241 (current version — unrelated to the 2002-03 CS241 "Internet Technologies and Systems"), CS476A, CS520, CS41, CS46N, CS83

---

**Total: ~157 talks across 50 matched courses**, plus ~100 further README-listed courses left uncovered by design (see above) rather than guessed at. This is by far the largest gap between "what's in the source" and "what's in the aggregator" of the six schools covered — worth treating this checklist as a strong starting core (especially for CS103/106/107/110/140/143/154/161/221/229/242/255 — Stanford's long-running canonical sequence) rather than a complete picture of Stanford's current CS catalog.
