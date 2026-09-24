
# Machine Learning

**Backbone Course #18** · **Duration:** 50 minutes

## The One-Sentence Pitch
Machine learning is the practice of letting a program infer patterns from data instead of being told the rules outright — and almost every practical difficulty in the field comes down to one tension: generalizing well versus memorizing the training data.

## Audience & Prerequisites
For learners comfortable with basic math notation (averages, distance between points) and basic programming concepts; no statistics or prior ML background is assumed.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Framing: learning from data instead of rules |
| 0:05–0:15 | The three big categories of ML |
| 0:15–0:35 | K-Means Clustering, mechanically |
| 0:35–0:45 | Training, generalization, and overfitting |
| 0:45–0:50 | Bias-variance tradeoff and close |

### Framing: learning from data instead of rules
Traditional programming is: a human writes explicit rules, and the computer follows them. Machine learning flips this: you give the computer examples, and it infers the rules (in the form of a mathematical model) itself. Concretely, a "model" is just a function with adjustable internal numbers (parameters), and "training" is the process of automatically adjusting those numbers so the function's outputs match the examples you gave it as closely as possible. This is powerful precisely where hand-written rules break down — tasks like recognizing handwriting or predicting customer churn have patterns too complex and too full of exceptions for a human to enumerate by hand, but a model can pick up on statistical regularities directly from examples.

### The three big categories of ML
Machine learning problems generally fall into three families. Supervised learning trains on labeled examples — inputs paired with the correct answer — like a dataset of house features paired with their sale prices, so the model learns to predict price from features on new houses. Unsupervised learning works with unlabeled data and looks for structure on its own, like grouping customers into segments based on purchasing behavior with no predefined categories — the model discovers the groups rather than being told them. Reinforcement learning is different again: an agent takes actions in an environment and learns from reward or penalty signals over time, like a program learning to play a game well purely by playing many games and being told its score at the end, without ever being shown a "correct" move. Most of what a working engineer touches day to day is supervised learning, but unsupervised learning is where some of the most intuitive, hands-on algorithms live — which is where we'll spend real time next.

### K-Means Clustering, mechanically
K-Means is the canonical unsupervised-learning algorithm, and it's worth walking through exactly how it works because it's simple enough to trace by hand. First, you choose a number k — how many groups (clusters) you want — and place k initial "centroids" (points representing the center of each cluster), often just k random data points to start. Then you repeat two steps until nothing changes: assign every data point to whichever centroid is nearest it (using ordinary distance), and then recompute each centroid as the average position of all the points now assigned to it. As centroids shift, some points switch which cluster they belong to, and the process repeats — average, reassign, average, reassign — until the centroids stop moving and the clusters stabilize. K-Means is used heavily for market segmentation (grouping customers by purchasing behavior so a business can target each group differently) and for general pattern recognition (finding natural groupings in any numeric dataset, like grouping similar sensor readings or documents). Its main limitations are worth stating plainly: you have to choose k yourself before running it, and there's no single "correct" k for most real datasets — you're often trying a few values and inspecting the result; and it implicitly assumes clusters are roughly round (spherical) and similar in size, so it does poorly on data with elongated, irregular, or very differently-sized natural groups.

### Training, generalization, and overfitting
Once you have a model, how do you know if it's actually good? The standard approach is to split your data into a training set (which the model learns from) and a test set (which it never sees during training), then measure performance only on the test set. This matters because a model that scores perfectly on the data it trained on can still be a bad model — it may have simply memorized the training examples, including their noise and quirks, rather than learning the underlying pattern. This failure mode is called overfitting, and it's detectable exactly because of the train/test split: a model that does great on training data but poorly on test data has memorized rather than generalized. Generalization — performing well on new, unseen data — is the actual goal of machine learning; performing well on training data is only ever a means to that end, and by itself proves very little.

### Bias-variance tradeoff and close
The bias-variance tradeoff is the one-sentence explanation for why machine learning is fundamentally hard: a model that's too simple can't capture the real pattern in the data (high bias, leading to underfitting), while a model that's too complex captures the pattern and the noise (high variance, leading to overfitting), and there is no single model complexity that avoids both problems at once — you're always trading one risk against the other. Every practical ML decision — how many features to use, how deep a tree to grow, how long to train a neural network — is, underneath, a decision about where to sit on this tradeoff. Keep this one framing after today: fitting the training data is easy; the actual challenge of machine learning is fitting it just enough.

## Key Takeaway
Machine learning trades hand-written rules for patterns inferred from data, and nearly every hard decision in the field — including choosing k in K-Means — is really a decision about balancing underfitting against overfitting.

## Go Deeper
- [BGSU](../curriculum/bgsu-cs-curriculum-talks-checklist.md) · [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Harvard](../curriculum/harvard-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.867 — Machine Learning](https://ocw.mit.edu/courses/6-867-machine-learning-fall-2006/pages/syllabus/)
- Harvard PLL: [Machine Learning and AI with Python](https://pll.harvard.edu/course/machine-learning-and-ai-python) · [Data Science: Building Machine Learning Models](https://pll.harvard.edu/course/data-science-building-machine-learning-models)
