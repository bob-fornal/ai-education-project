
# Computer Graphics

**Backbone Course #22** · **Duration:** 50 minutes

## The One-Sentence Pitch
Every image you see on a screen is the end result of a pipeline that transforms a 3D model through a sequence of matrix operations and lighting approximations down to a 2D grid of colored pixels, and the whole field is shaped by one tradeoff — real time versus physical accuracy.

## Audience & Prerequisites
For learners comfortable with basic vectors and matrices (or willing to take the matrix composition claim on faith); no prior graphics or linear algebra course is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Framing: from 3D model to 2D pixels |
| 0:05–0:20 | The rendering pipeline |
| 0:20–0:32 | Coordinate transformations and why matrices compose |
| 0:32–0:44 | Lighting and shading: the Phong intuition |
| 0:44–0:50 | Real-time vs. offline rendering, and close |

### Framing: from 3D model to 2D pixels
Computer graphics answers a deceptively simple question: given a description of a 3D scene — shapes, positions, materials, lights, and a camera — how do you produce the flat 2D image of colored pixels that a screen actually displays? Every rendered image, from a simple 3D game to a big-budget animated film, is the output of essentially the same underlying pipeline, just tuned differently for speed versus realism. Understanding that pipeline demystifies both ends of the spectrum: why games sometimes look a little "flat" compared to film, and why film rendering can take hours per frame while a game needs a fresh frame roughly sixty times every second.

### The rendering pipeline
The rendering pipeline has a few conceptual stages that any renderer performs, in order. It starts with a 3D model: a description of shapes, typically as a mesh of connected triangles in three-dimensional space, plus material and color information. Geometric transformations then move, rotate, and scale that model into the correct position within the overall 3D scene, and further transform the whole scene into the camera's point of view. Projection then flattens that 3D scene onto a 2D viewport, the way a real camera lens projects the 3D world onto a flat sensor — this is where 3D coordinates become 2D screen coordinates, with size and position adjusted for camera distance. Finally, that 2D layout has to become actual pixel colors, which happens via one of two strategies: rasterization, which figures out which pixels each triangle covers and fills them in (fast, and the default for real-time graphics), or ray tracing, which works backward from each pixel, tracing a ray out into the scene to see what light it would receive (slower, but far more physically accurate). The pipeline is the same conceptual shape everywhere; what changes across applications is mostly how much time and accuracy you can afford at the rasterization/ray-tracing stage.

### Coordinate transformations and why matrices compose
Moving objects around a 3D scene requires three basic operations: translation (sliding an object to a new position), rotation (turning it around an axis), and scaling (changing its size). Each of these can be represented as a matrix, and applying a transformation to a point is just multiplying that point's coordinates by the matrix. The reason this representation is so useful is that matrices compose cleanly: if you want to rotate an object and then move it, you can multiply the rotation matrix and the translation matrix together ahead of time into a single combined matrix, and then apply that one combined matrix to every point in the model just once, rather than applying two separate operations to every point individually. This matters enormously at scale — a model might have tens of thousands of points, and a scene might have a camera transform, several object transforms, and a projection transform all chained together; representing the whole chain as one precomputed matrix per object, rather than a long sequence of separate operations, is what makes real-time transformation of complex scenes computationally feasible.

### Lighting and shading: the Phong intuition
Once geometry is in place, a renderer needs to decide what color each visible point should actually be, which depends on lighting. The classic Phong shading model approximates real light with three simple components added together: ambient light, a flat baseline amount of light assumed to reach every surface regardless of direction, roughly modeling all the indirect bounced light in a scene without actually simulating it; diffuse light, which depends on the angle between the surface and the light source — a surface facing the light directly is brighter than one facing away, which is what gives shapes their sense of rounded, three-dimensional form; and specular light, the bright, tight highlight you see on shiny surfaces, which depends on the angle between the light, the surface, and the viewer, and is what makes plastic or metal look different from matte cloth. None of these three components is a physically exact simulation of light — they're a cheap, fast approximation tuned to look convincing to a human eye, which is exactly the kind of tradeoff that shows up constantly in graphics.

### Real-time vs. offline rendering, and close
The single tradeoff that explains most differences you notice between games and film is time budget. A game needs to render roughly sixty new frames every single second to feel responsive, which leaves only a few milliseconds per frame — nowhere near enough time for physically accurate light simulation, so games lean on fast approximations like rasterization and Phong-style shading, accepting some visual inaccuracy in exchange for speed. Film rendering has no such real-time constraint — a single frame can take minutes or hours to render on a render farm before it's ever seen by an audience — so it can afford physically-based ray tracing, simulating how individual rays of light actually bounce, refract, and scatter through a scene for much more realistic results. Both are solving the same rendering pipeline; they just sit at different points on the speed-versus-accuracy dial, and that single dial explains most of what looks different between a game and an animated film.

## Key Takeaway
Rendering is one pipeline — transform, project, then color pixels via rasterization or ray tracing — and nearly every visible difference between "real-time" and "cinematic" graphics comes down to how much of that pipeline you can afford to do exactly versus approximately.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.837 — Computer Graphics](https://ocw.mit.edu/courses/6-837-computer-graphics-fall-2012/pages/syllabus/)
