# RAG-Guided Named Entity Recognition (NER)

## Overview

This project implements a **Retrieval-Augmented Named Entity Recognition (RAG-NER)** pipeline that improves entity detection accuracy using external knowledge.

It combines a lightweight **DistilBERT-based NER model** with a **retrieval + refinement stage**, enabling better handling of ambiguous or low-confidence entities.

---

## Notes & Research

All detailed notes, experiments, and research references are available in:

📁 **Research and Materials/**

---

## Architecture

The system works in two stages:

### 1. Initial NER (Distilled Model)

* Fast DistilBERT-based NER model
* Outputs:

  * High-confidence entities
  * Candidate spans for further validation

### 2. Retrieval-Augmented Refinement

* Expand candidate entities using external knowledge (WordNet, KBs)
* Retrieve relevant semantic context (RAG layer)
* Add structured metadata (type hints, definitions, related terms)
* Pass enriched input to a refined NER model

### Final Output

* Improved entity predictions
* Higher confidence scores
* Reduced ambiguity

---

## Key Features

* Two-stage NER pipeline (fast + accurate)
* Retrieval-Augmented Generation (RAG)
* Candidate expansion via lexical + semantic sources
* Context-aware entity disambiguation
* Designed for scalability and modular upgrades

---

## Improvements & Research Directions

* Dense retrieval (DPR-style) instead of lexical matching
* Cross-encoder re-ranking for noise reduction
* Structured metadata injection (not raw text)
* Word Sense Disambiguation (WSD) for WordNet
* Joint retriever + NER training (RA-NER)
* Confidence calibration (ECE, temperature scaling)
* Entity linking for final canonicalization

---
## Limitations

* Retrieval noise can affect performance
* WordNet introduces polysemy issues without WSD
* Requires careful calibration for confidence scores
* Distribution shift between training and augmented inputs

---

## Future Work

* End-to-end RA-NER training
* Better retriever fine-tuning
* Entity linking integration
* Human-in-the-loop validation