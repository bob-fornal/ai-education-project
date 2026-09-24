# Robotics

**Backbone Course #24** · **Duration:** 50 minutes

## The One-Sentence Pitch
A robot is fundamentally a loop — perceive, decide, act, repeat — and almost every hard problem in robotics comes from the fact that every step in that loop is uncertain.

## Audience & Prerequisites
For learners comfortable with basic programming and everyday trigonometry (angles, sin/cos); no prior robotics, control theory, or linear algebra background is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: robots are loops, not scripts |
| 0:05–0:20 | The Sense-Plan-Act loop |
| 0:20–0:32 | Kinematics and coordinate frames |
| 0:32–0:42 | Motion planning and configuration space |
| 0:42–0:47 | Why robotics is hard: uncertainty everywhere |
| 0:47–0:50 | Key takeaway and close |

### Hook: robots are loops, not scripts
Open by contrasting a "robot" as imagined in movies (an autonomous decision-maker) with what a robot actually is at the software level: a continuous loop running many times per second. Ask the audience to picture a self-driving car or a robot vacuum — neither runs a single "do this" script; both are constantly re-sensing and re-deciding. Frame the talk's throughline up front: everything a robot does can be described as perceive, decide, act, repeat, and everything that makes robotics hard traces back to imperfection somewhere in that loop.

### The Sense-Plan-Act loop
Introduce the three phases as the master framework for the entire talk. Sense: gather data about the world through sensors — cameras, LIDAR, encoders on joints, force sensors — producing a snapshot (usually noisy and incomplete) of the current state. Plan: given that snapshot and a goal, decide what to do next, which can range from a simple reflex rule to a full path-planning computation. Act: send commands to actuators — motors, servos, grippers — which move the physical world, changing what will be sensed next time around. Emphasize that this is a closed loop, not a pipeline: the "act" step changes the world, which changes the next "sense," so a robot is constantly correcting rather than executing a fixed plan once. Give a concrete walkthrough: a warehouse robot senses a box's position with a camera, plans a path to pick it up, actuates its arm and gripper, then re-senses to confirm the grasp succeeded before moving on.

### Kinematics and coordinate frames
Explain kinematics as the math connecting a robot's internal joint state to where its parts actually are in space. Forward kinematics answers "given these joint angles, where is the hand?" — each joint contributes a rotation and/or translation, and chaining them together (conceptually, multiplying transformations down the arm) tells you the end-effector's position and orientation. Inverse kinematics is the harder, more useful direction: "I want the hand at this specific point — what joint angles get me there?" — and note this can have zero, one, or many valid solutions (an arm can often reach the same point several different ways, like your own elbow having freedom of movement even with your hand fixed in place). Introduce coordinate frames as the bookkeeping tool that makes this tractable: each joint and sensor has its own local reference frame, and robotics software constantly translates between frames (the camera's view, the arm base's frame, the world frame) to keep everything consistent. Use a simple example: a robot arm bolted to a table needs to convert "the object is 10cm in front of the camera" into "rotate joint 2 by 15 degrees" — that conversion is exactly what kinematics and frame transforms are for.

### Motion planning and configuration space
Introduce path/motion planning as answering "how do I move from here to there without hitting anything?" Define configuration space (C-space) conceptually: instead of thinking about the robot's physical shape moving through a room, collapse the robot down to a single point in an abstract space where each dimension is one degree of freedom (e.g., one joint angle), and obstacles get "expanded" into that space too. Make this concrete with a 2D maze analogy: picture planning a path for a dot through a maze of walls — that is literally what path planning looks like in C-space for a simple robot, and the walls in C-space represent configurations where the real robot would collide with something. Mention that classic algorithms like A* (a heuristic-guided search) or sampling-based approaches (like Rapidly-exploring Random Trees) search this space for a collision-free route, then that route gets translated back into an actual sequence of joint motions. The key intuition to land: planning is search, but search happens in a transformed space that captures the robot's real constraints, not in physical 3D space directly.

### Why robotics is hard: uncertainty everywhere
Bring the talk together by explaining why robotics is a fundamentally different discipline from pure software, even though it uses the same programming skills. In pure software, if you compute 2+2 you get exactly 4, every time. In robotics, every sensor reading has noise (a distance sensor might report 1.02m when the true distance is 1.00m), every actuator has imperfect execution (a motor commanded to move 90 degrees might land at 89.7), and the physical world itself is unpredictable (a box might be slightly heavier than expected, a floor slightly more slippery). Explain that this is why robotics leans heavily on probability and feedback rather than open-loop exact execution — techniques like Kalman filters exist specifically to combine noisy sensor readings into a best-guess estimate of true state, and control systems constantly re-check and re-correct rather than trusting a single computed plan. Land the point: robotics is control theory and probability applied to the sense-plan-act loop, precisely because nothing physical is ever exact.

### Key takeaway and close
Recap the loop — sense, plan, act, repeat — as the mental model to carry forward, and remind the audience that kinematics, path planning, and probabilistic estimation are all just tools in service of making that loop work despite constant uncertainty.

## Key Takeaway
Every robot is running a sense-plan-act loop, and nearly every technique in robotics — from kinematics to path planning to probabilistic filtering — exists to make that loop robust against the fact that sensors, actuators, and the physical world are never exact.

## Go Deeper
- [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.832 — Underactuated Robotics](https://ocw.mit.edu/courses/6-832-underactuated-robotics-spring-2022/pages/syllabus/)
