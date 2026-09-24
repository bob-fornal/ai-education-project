
# Computer Vision

**Backbone Course #21** · **Duration:** 50 minutes

## The One-Sentence Pitch
A computer never sees an "object" — it sees a grid of numbers — and computer vision is the field devoted to closing that gap, first with hand-engineered feature detectors and now mostly with features a network learns automatically from data.

## Audience & Prerequisites
For learners with the basic machine learning framing (models, training) and ideally the deep learning talk's CNN intuition; no image-processing background is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:07 | How an image looks to a computer |
| 0:07–0:20 | Classical features vs. learned features |
| 0:20–0:37 | CNN intuition: filters sliding across an image |
| 0:37–0:46 | Real applications |
| 0:46–0:50 | Failure modes and close |

### How an image looks to a computer
To a human, a photo is instantly full of objects, edges, and depth. To a computer, an image is nothing more than a grid of numbers — each cell (pixel) holding an intensity value, or three values for red, green, and blue in a color image. There is no inherent notion of "edge" or "object" anywhere in that grid; a boundary between a cat and the background is, numerically, just a place where adjacent pixel values happen to change sharply, and the computer has no built-in concept that this particular change is meaningful while some other change (a shadow, a texture) is not. Every technique in computer vision is, at bottom, a strategy for extracting meaningful structure out of this undifferentiated grid of numbers.

### Classical features vs. learned features
Before learned approaches dominated, computer vision relied on hand-engineered feature detectors: algorithms explicitly designed by researchers to find specific patterns, like edge detectors that flag pixels where intensity changes sharply in a particular direction, or corner detectors that flag points where edges meet at an angle. These handcrafted detectors work, and they're still useful in resource-constrained settings, but they share a fundamental limitation: a human has to correctly guess in advance which patterns matter for the task, and that guess has to be re-made for every new problem. Learned features flip this: instead of a human specifying what to look for, a model (typically a convolutional neural network) is shown many labeled images and automatically discovers, through training, which visual patterns are actually useful for the task at hand — patterns a human might never have thought to hand-engineer, and that can be re-discovered automatically for a new task simply by retraining.

### CNN intuition: filters sliding across an image
A convolutional neural network's core operation is the convolution: a small filter — really just a small grid of learnable numbers, maybe 3x3 or 5x5 — slides across the full image, at each position computing a simple weighted combination of the pixels underneath it. Early in training, a filter might end up detecting something simple, like a vertical edge or a patch of a specific color, because that's a useful, generic pattern for many tasks. The network stacks many such convolutional layers, and because each layer operates on the output of the one before it, deeper layers combine simple detected patterns into progressively more abstract ones — edges combine into corners and textures, which combine into parts like an eye or a wheel, which combine into whole objects like a face or a car. Crucially, the same small filter is reused across the entire image (rather than learning a separate detector for every pixel position), which is both computationally efficient and matches a sensible assumption about images: a useful pattern like "edge" is just as useful to detect in the top-left corner as the bottom-right.

### Real applications
This machinery underlies a wide range of everyday systems. Object detection locates and labels multiple objects within a single image (a self-driving car identifying pedestrians, other vehicles, and traffic signs simultaneously). Image segmentation goes further, classifying every individual pixel as belonging to a particular object or region, rather than drawing one box around it (useful in medical imaging, for outlining a tumor's exact boundary rather than just its rough location). Facial recognition matches a detected face against a database of known faces, which is genuinely useful for things like phone unlocking, but also raises the sharpest ethical questions in this field around surveillance and consent.

### Failure modes and close
It's worth being honest about where computer vision breaks. Adversarial fragility: models trained this way can be fooled by tiny, often human-imperceptible changes to an image — a few strategically altered pixels can make a model confidently misclassify something a human would recognize instantly and correctly, revealing that the model's learned features don't always align with human visual understanding the way they seem to. Bias in training data: if a training dataset underrepresents certain skin tones, ages, or lighting conditions, the resulting model performs measurably worse for people in those underrepresented groups — a well-documented, real problem in deployed facial recognition and other vision systems, and a reminder that "the model learned it from data" is not automatically a guarantee of fairness or reliability.

## Key Takeaway
An image is just a grid of numbers with no built-in concept of "object," and CNNs bridge that gap by learning, from data, the filters that turn pixels into increasingly abstract patterns — but that same data-driven process makes these systems vulnerable to adversarial inputs and to biases baked into their training data.

## Go Deeper
- [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.801 — Machine Vision](https://ocw.mit.edu/courses/6-801-machine-vision-fall-2020/pages/syllabus/)
