# Information Theory & Data Compression

**Backbone Course #26** · **Duration:** 50 minutes

## The One-Sentence Pitch
Shannon entropy gives a precise, mathematical answer to "how compressible is this data," and Huffman coding is the classic algorithm that gets real compressors close to that theoretical limit.

## Audience & Prerequisites
For learners comfortable with basic probability (what a frequency distribution is) and basic binary/tree concepts; no prior information theory background required.

## 50-Minute Outline

| Time | Segment |
|---|---|
| 0:00–0:05 | Hook: why can some files compress and others can't |
| 0:05–0:13 | Shannon entropy: measuring information |
| 0:13–0:20 | Lossless vs. lossy compression |
| 0:20–0:42 | Huffman Coding: the algorithm, step by step |
| 0:42–0:47 | Tying Huffman back to the entropy limit |
| 0:47–0:50 | Key takeaway and close |

### Hook: why can some files compress and others can't
Open with an observation everyone has noticed: a text file zips down dramatically, but a file that's already a ZIP or a JPEG barely shrinks at all when you try to zip it again. Pose the question the rest of the talk answers: is there a mathematical reason some data compresses well and other data doesn't? The answer is yes, and it has a name — entropy — and it was formalized in 1948 by Claude Shannon in a way that still underlies every compression tool used today.

### Shannon entropy: measuring information
Define entropy conceptually as a measure of unpredictability, or "how much information is really there" in a message. The core insight: a highly predictable message carries little new information (if a symbol appears 99% of the time, seeing it again barely tells you anything), while a highly unpredictable message carries a lot. Explain that entropy is computed from the probability distribution of symbols in the data — skewed distributions (some symbols much more common than others) have low entropy, uniform distributions (every symbol equally likely) have maximum entropy. Land the crucial practical consequence: entropy sets a hard mathematical lower bound on how small you can losslessly compress data — you cannot, even in principle, compress below a message's entropy without losing information. This is why English text (skewed letter frequencies — 'e' is common, 'z' is rare) compresses well, while already-random data (uniform, maximum entropy) does not compress at all.

### Lossless vs. lossy compression
Draw the fundamental distinction between the two families of compression. Lossless compression preserves every original bit exactly — you can decompress and get back byte-for-byte what you started with — used whenever exactness matters, like ZIP files, text, or source code, and it is fundamentally bounded by entropy as just described. Lossy compression deliberately discards information that's perceptually unimportant to gain much greater size reduction — JPEG throws away color detail the human eye is bad at noticing, and MP3 throws away frequencies humans can barely hear — trading a small, often invisible quality loss for a much bigger file-size win than entropy alone would allow. Emphasize that the choice between them is a deliberate engineering tradeoff based on the data type and tolerance for loss, not one being strictly "better" than the other.

### Huffman Coding: the algorithm, step by step
Introduce Huffman coding as the classic algorithm for lossless compression, and give it real depth as the centerpiece of this talk. The core idea: assign shorter binary codes to more frequent symbols and longer binary codes to rarer symbols, so that on average, encoded messages are shorter than if every symbol got a fixed-length code. Explain the construction as a greedy, bottom-up tree-building process: start with every symbol as its own tiny node, weighted by its frequency; repeatedly take the two lowest-frequency nodes and merge them into a new node (whose frequency is their sum), building a binary tree upward until only one root remains; then each symbol's code is the path of left/right (0/1) branches from the root down to that symbol's leaf. Walk through a small concrete example in words: take the string "ABRACADABRA" — A is by far the most frequent letter, B and R appear a few times, C and D appear once each. Huffman's algorithm would assign A the shortest code (maybe a single bit), give B and R slightly longer codes, and give the rare C and D the longest codes — the opposite of a naive fixed-length scheme where every letter costs the same number of bits regardless of frequency. Note the direct real-world payoff: this is literally the algorithm inside the ZIP format's DEFLATE compression, and it's also used as one stage inside JPEG (after the lossy step discards detail, Huffman coding losslessly compresses what remains).

### Tying Huffman back to the entropy limit
Close the loop between the two big ideas of the talk: for a known, fixed distribution of symbol frequencies, Huffman coding gets remarkably close to the theoretical entropy limit — it is provably optimal among coding schemes that assign a whole number of bits to each symbol. Mention, without dwelling on it, that it isn't perfectly optimal in all cases (skewed distributions with probabilities that aren't clean powers of two leave a small gap, which is why more advanced techniques like arithmetic coding exist), but the intuition to keep is that Huffman coding is entropy's practical, algorithmic counterpart — entropy tells you the limit, Huffman coding is a concrete, efficient way to actually get there.

### Key takeaway and close
Recap: entropy measures how much real information a message contains and sets the floor on lossless compression, and Huffman coding — by giving frequent symbols short codes and rare symbols long codes via a simple greedy tree-building process — is the classic algorithm that gets you close to that floor, still running today inside ZIP and JPEG.

## Key Takeaway
Compression isn't magic — Shannon entropy mathematically defines the limit of how small data can get without losing information, and Huffman coding's greedy, frequency-based tree construction is the classic algorithm that gets real-world compressors like ZIP close to that limit.

## Go Deeper
- [Amherst](../curriculum/amherst-cs-curriculum-talks-checklist.md) · [MIT](../curriculum/mit-ocw-cs-curriculum-talks-checklist.md) · [CMU](../curriculum/cmu-cs-curriculum-talks-checklist.md)
- MIT OCW: [6.441 — Information Theory](https://ocw.mit.edu/courses/6-441-information-theory-spring-2016/pages/syllabus/)
- See also in this folder: `31-fast-fourier-transform.md` (a different, transform-based approach to signal/data compression)
