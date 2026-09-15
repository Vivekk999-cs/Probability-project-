# Probability-Based Word Predictor Using N-Gram Models

## Overview

This project explores how probability and statistical models can be used to predict the next word in a sequence.

The project starts with a simple unigram probability model and gradually introduces contextual information through bigram and higher-order N-gram models.

The main objective is to understand the mathematical foundations of language prediction before moving toward more advanced statistical and neural language models.

The project is implemented in Python.

---

## Problem Statement

Given a sequence of words, predict the most probable next word.

For example:

    I want to learn ___

The model should use the available information to estimate the probability of possible next words and select the most probable candidate.

For a general language model:

    P(w_i | w_1, w_2, ..., w_{i-1})

In the current bigram model, only the previous word is used as context:

    P(w_i | w_{i-1})

---

## Motivation

Word prediction is used in many real-world applications, including:

- Autocomplete
- Search suggestions
- Text editors
- Email suggestions
- Speech recognition
- Assistive technologies
- Conversational systems

This project is also motivated by the goal of understanding how fundamental probability concepts can be applied to a real computational problem.

Instead of beginning with a neural network, the project starts with word frequencies and conditional probabilities and gradually increases the complexity of the model.

---

## Probability Foundation

Let W represent the word selected from the corpus.

W is a discrete random variable because its possible outcomes are individual words from the vocabulary.

If V represents the vocabulary:

    W ∈ V

The probability mass function for the unigram model is:

    P(W = w) = C(w) / N

where:

- C(w) = number of occurrences of word w
- N = total number of word tokens
- V = vocabulary

The probabilities satisfy:

    Σ P(W = w) = 1

Therefore, the word probabilities form a discrete probability distribution.

---

## Week 1: Unigram Model

The first stage of the project implemented a unigram probability model.

A unigram consists of one word.

For example:

    I
    like
    programming
    probability

The unigram model does not consider the previous word. It only uses the frequency of each word in the corpus.

The probability of a word is calculated using:

    P(W = w) = C(w) / N

For the current training corpus:

- Total tokens = 50
- Vocabulary size = 13

For example, if `programming` occurs 5 times:

    P(programming) = 5 / 50
                     = 0.10

The Python implementation performs the following operations:

1. Loads the training text.
2. Converts the text to lowercase.
3. Tokenizes the text into words.
4. Counts word frequencies.
5. Calculates the probability of each word.
6. Checks that the probabilities sum to 1.

Python's `Counter` is used to calculate word frequencies.

---

## Week 2: Bigram Model

The second stage introduces contextual information using a bigram model.

A bigram consists of two consecutive words.

For example:

    I like
    like programming
    I want
    want to
    to learn

A bigram can be represented using two consecutive random variables:

    (W_i, W_{i+1})

The bigram model estimates the probability of the next word given the current word.

This is a conditional probability:

    P(w | c)

where:

- c = current word
- w = next word

The probability is estimated using:

    P(w | c) = C(c, w) / C(c)

where:

- C(c, w) = number of times the bigram (c, w) occurs
- C(c) = number of times c occurs as a context

---

## Example of Conditional Probability

Consider the word:

    want

In the current training corpus, every occurrence of `want` is followed by `to`.

Therefore:

    C(want) = 3

and:

    C(want, to) = 3

Thus:

    P(to | want) = 3 / 3
                 = 1

Another example is the word `i`.

The word `i` is followed by:

    enjoy  → 5 times
    like   → 3 times
    want   → 3 times

Therefore:

    P(enjoy | i) = 5 / 11 ≈ 0.4545

    P(like | i) = 3 / 11 ≈ 0.2727

    P(want | i) = 3 / 11 ≈ 0.2727

The model therefore predicts `enjoy` as the most probable next word after `i`.

---

## Prediction

The prediction rule is:

    w* = argmax P(w | c)

The model examines the possible next words for a given context and selects the word with the highest conditional probability.

The model can also return the Top-K most probable words rather than only one prediction.

For example:

    Input: i

    enjoy       0.4545
    like        0.2727
    want        0.2727

This provides multiple possible predictions and their probabilities.

---

## Statistical Model

The current system is a maximum-likelihood N-gram language model.

The probabilities are estimated directly from observed frequencies in the training corpus.

For the bigram model:

    P(w_i | w_{i-1})
    =
    C(w_{i-1}, w_i) / C(w_{i-1})

This is a maximum-likelihood estimate of the conditional probability distribution.

---

## Current Project Progress

The project has currently completed:

- [x] Training corpus creation
- [x] Text loading
- [x] Tokenization
- [x] Word frequency counting
- [x] Unigram probability distribution
- [x] Bigram generation
- [x] Bigram frequency counting
- [x] Conditional probability
- [x] Next-word prediction
- [x] Top-K prediction

---

## Observations So Far

The unigram model shows that word frequency directly determines the estimated probability of a word.

The bigram model improves the representation by introducing context. Instead of estimating only:

    P(w)

the model estimates:

    P(w | previous word)

This allows the model to distinguish between different possible next words.

The results also show that some contexts are highly predictable while others have several possible continuations.

Increasing the amount of contextual information can potentially improve prediction because the model has more information about what came before. However, increasing the N-gram size does not guarantee higher accuracy because longer sequences are less frequent and can lead to data sparsity.

---

## Limitations

The current implementation has several limitations.

### Small Dataset

The training corpus is very small and therefore does not represent general English language usage.

### Data Sparsity

Many possible word combinations do not occur in the training corpus.

If:

    C(c, w) = 0

the unsmoothed model assigns:

    P(w | c) = 0

even when that word combination may be possible in real language.

### Limited Context

A bigram model considers only one previous word.

Therefore, it cannot directly use longer contexts such as:

    I want to learn ___

when making its prediction.

### No Semantic Understanding

The model learns statistical relationships between words but does not understand their meaning.

### Dependence on Training Data

Predictions depend strongly on the words and patterns present in the training corpus.

## Research Direction

The main research question is:

> How does increasing contextual information in N-gram models affect next-word prediction performance?

The planned progression is:

    Unigram
        ↓
    Bigram
        ↓
    Trigram
        ↓
    4-gram
        ↓
    5-gram

The different models will eventually be compared using measures such as:

- Top-1 accuracy
- Top-5 accuracy
