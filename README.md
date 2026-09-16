# WordPiece Tokenization From Scratch

## Introduction

WordPiece is a **subword tokenization algorithm** widely used in BERT-family models. Instead of treating every complete word as a single token, WordPiece breaks words into smaller meaningful subword units.

This project implements the basic working of **WordPiece Tokenization from scratch using Python**. It demonstrates how an initial vocabulary is created, token frequencies and pair frequencies are calculated, WordPiece scores are generated, and the best token pair is merged to build new vocabulary tokens.

---

## Objective

The main objective of this project is to understand and implement the fundamental concepts of WordPiece Tokenization.

### Objectives

* Create an initial vocabulary from training words.
* Split words into subword tokens.
* Calculate token frequencies.
* Calculate adjacent pair frequencies.
* Calculate WordPiece scores.
* Identify the best pair for merging.
* Merge selected subword tokens.
* Tokenize new words using the learned vocabulary.
* Convert tokens into numerical token IDs.
* Handle unknown words using `[UNK]`.

---

## Training Data

The project uses the following sample training words:

```text
hug
hugs
pug
```

Initial subword representation:

```text
hug  → h ##u ##g
hugs → h ##u ##g ##s
pug  → p ##u ##g
```

Here, `##` indicates that the token is a continuation of the previous token.

---

## WordPiece Score

WordPiece uses a scoring mechanism to determine which pair of tokens should be merged.

The score is calculated as:

```text
Score = Pair Frequency / (First Token Frequency × Second Token Frequency)
```

The pair with the highest score is selected for merging.

For example:

```text
('h', '##u')
```

may be merged to create:

```text
hu
```

The process continues until the required vocabulary is created.

---

## Example

### Input

```text
hugs
```

### Tokenization

After learning the vocabulary, the word can be tokenized as:

```text
hugs → hug ##s
```

### Tokens

```python
['hug', '##s']
```

### Token IDs

```text
[7, 5]
```

Token IDs are numerical representations assigned to vocabulary tokens.

---

## Features

* Initial vocabulary creation
* Subword splitting
* Token frequency calculation
* Pair frequency calculation
* WordPiece score calculation
* Best-pair selection
* Token merging
* New word tokenization
* Token-to-ID conversion
* `[UNK]` handling

---

## Technologies Used

* **Python**
* **Collections**
* **Counter**

---

## Working Process

```text
Training Words
      ↓
Split Words into Subwords
      ↓
Calculate Token Frequencies
      ↓
Calculate Pair Frequencies
      ↓
Calculate WordPiece Scores
      ↓
Find Best Pair
      ↓
Merge Best Pair
      ↓
Update Vocabulary
      ↓
Tokenize New Word
      ↓
Convert Tokens into IDs
      ↓
Handle Unknown Words
```

---

## Sample Output

```text
Token Frequencies:
Counter({
    '##u': 4,
    '##g': 4,
    'h': 3,
    '##s': 1,
    'p': 1
})

Pair Frequencies:
Counter({
    ('##u', '##g'): 4,
    ('h', '##u'): 3,
    ('##g', '##s'): 1,
    ('p', '##u'): 1
})

WordPiece Scores:

('h', '##u') = 0.25
('##u', '##g') = 0.25
('##g', '##s') = 0.25
('p', '##u') = 0.25

Best Pair: ('h', '##u')
New Token: hu

Input Word: hugs

Tokens: ['hug', '##s']

Token IDs: [7, 5]
```

---

## Unknown Token Handling

WordPiece uses the special token:

```text
[UNK]
```

If a word cannot be completely represented using the available vocabulary, the tokenizer returns:

```python
['[UNK]']
```

For example:

```text
Input: xyzabc

Output:
['[UNK]']
```

This prevents the tokenizer from failing when it encounters an unknown word.

---

## How to Run

### Step 1: Install Python

Make sure Python is installed on your system.

You can verify it using:

```bash
python --version
```

### Step 2: Save the Program

Save the Python file as:

```text
wordpiece.py
```

### Step 3: Run the Program

Open the terminal in the project folder and execute:

```bash
python wordpiece.py
```

---

## Project Structure

```text
WordPiece-Tokenization/
│
├── wordpiece.py
└── README.md
```

---

## Applications

WordPiece tokenization is useful in Natural Language Processing applications such as:

* Text classification
* Sentiment analysis
* Question answering
* Named Entity Recognition
* Language understanding
* Transformer-based NLP models

It is especially associated with **BERT and other transformer-based language models**.

---

## Learning Outcomes

Through this project, the following concepts are understood:

* Subword tokenization
* Vocabulary construction
* Token frequency
* Pair frequency
* Statistical token merging
* Token-to-ID mapping
* Unknown token handling
* Basic NLP preprocessing
* Fundamental concepts behind BERT-style tokenization

---

## Conclusion

This project demonstrates the basic implementation of **WordPiece Tokenization from scratch using Python**. It shows how words can be divided into subword units and how token frequencies, pair frequencies, and WordPiece scores are used to determine which tokens should be merged.

The project also demonstrates the tokenization of new words, conversion of tokens into token IDs, and handling of unknown words using `[UNK]`.

Overall, this project provides a practical understanding of **subword tokenization and its role in modern Natural Language Processing and BERT-family models**.
