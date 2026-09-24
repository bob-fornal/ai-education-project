
# Natural Language Processing

**Backbone Course #20** · **Duration:** 50 minutes

## The One-Sentence Pitch
Natural language processing turns messy human text into numbers a model can compute with, and the biggest leap in that pipeline was letting a model weigh every word against every other word at once instead of reading strictly left to right.

## Audience & Prerequisites
For learners who have the basic machine learning framing (models, training) from earlier talks in this series; no linguistics background is required.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Framing: language is not naturally numeric |
| 0:05–0:20 | The NLP pipeline: text to task-specific model |
| 0:20–0:32 | Word embeddings: meaning as geometry |
| 0:32–0:45 | The transformer/attention breakthrough |
| 0:45–0:50 | Real applications and close |

### Framing: language is not naturally numeric
Computers fundamentally operate on numbers, but language arrives as strings of characters with no inherent numeric structure — "cat" and "dog" are just as different from a computer's native perspective as "cat" and "xyz123." Natural language processing is the field devoted to bridging that gap: converting text into representations a model can actually compute with, while trying to preserve the meaning and relationships that make language useful in the first place. This is genuinely hard because language is ambiguous, context-dependent, and full of exceptions — the same word can mean different things in different sentences, and the "rules" of grammar are riddled with exceptions no hand-written system fully captures, which is part of why NLP moved toward learned, data-driven approaches rather than hand-coded grammar rules.

### The NLP pipeline: text to task-specific model
Most NLP systems, regardless of the specific task, follow the same basic pipeline. First, tokenization breaks raw text into smaller units — words, sub-word pieces, or characters — because a model needs discrete, countable units to work with rather than one long string. Next, each token is converted into some numerical representation, since a model can only operate on numbers, not characters. Finally, that numeric representation is fed into a task-specific model — for classification (is this review positive or negative?), for translation (convert this sentence to another language), for generation (predict the next word), or for many other tasks. The pipeline stages are shared across nearly all of NLP; what differs from task to task is mainly the final model and what it's trained to predict.

### Word embeddings: meaning as geometry
The "numerical representation" step used to be crude — assigning each word an arbitrary, meaningless number or a giant sparse vector with a single "1" marking that word's identity, which captured no relationship between words at all. Word embeddings changed this by representing each word as a dense vector (a list of numbers) positioned in a high-dimensional space, learned so that words used in similar contexts end up near each other in that space. This geometric structure captures relationships in a genuinely useful way — the famous illustrative example is that vector arithmetic on embeddings, "king minus man plus woman," lands close to the vector for "queen," because the embedding space has implicitly learned something like a "royalty" direction and a "gender" direction from patterns of usage across huge amounts of text. The practical payoff is that a model operating on embeddings can generalize better: it can infer something about a rare word from its position near other, more common words with similar usage, rather than treating every word as a totally isolated symbol.

### The transformer/attention breakthrough
Early sequence models processed text strictly left to right, one word at a time, carrying forward a running summary — which meant that by the time the model reached word fifty, information from word two had often faded or gotten diluted. The transformer architecture broke that constraint with an attention mechanism: when interpreting any given word, the model can directly look at and weigh every other word in the input simultaneously, deciding for itself which other words matter most for understanding this one, regardless of distance in the sentence. Concretely, resolving what "it" refers to in a long sentence requires connecting back to a noun that might be many words earlier — attention lets the model make that connection directly rather than hoping the information survived a long left-to-right relay. This architectural shift — parallel, direct connections between all words instead of a strict sequential chain — is the foundation underneath essentially all modern large language models, and it's also why they can be trained efficiently on massive amounts of text: the parallel structure fits modern GPU hardware far better than a strictly sequential one does.

### Real applications and close
These pieces combine into systems people use constantly: machine translation (converting text between languages while preserving meaning), summarization (compressing a long document into its key points), and conversational agents (holding a coherent, context-aware dialogue across many turns). All three rest on the same underlying pipeline — tokenize, embed, then run through an attention-based model — applied to different training objectives. The abstraction is the same throughout; only the task-specific target changes.

## Key Takeaway
NLP works by converting text into geometric, meaning-preserving numeric representations, and the attention mechanism in transformers — letting a model weigh every word against every other word directly — is the architectural shift behind today's language models.

## Go Deeper
- [Purdue](../curriculum/purdue-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [Stanford](../curriculum/stanford-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.864 — Advanced Natural Language Processing](https://ocw.mit.edu/courses/6-864-advanced-natural-language-processing-fall-2005/pages/syllabus/)
