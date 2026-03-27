# PROJECT OVERVIEW 

TAGS: #ArtificialIntelligence 
BUILT-ON: [[NER Recognition - DistilBERT]] [[Retrieval Argumented Generator]]
ENABLES: [[NER Recognition - RAG-Guided DistilBERT]]
PREREQUISITES:

---
## PLAN OVERVIEW
Named Entity Recognition (NER) system enhanced with Retrieval-Augmented Generation (RAG) to improve entity detection accuracy and confidence.
The pipeline works in multiple stages:
### Initial NER Prediction (Distilled Model)
A lightweight distilled NER model first processes the input sentence and predicts potential entities.
This step prioritizes speed and recall, producing both:
- High-confidence entities
- Possible candidate entities that may need further verification.
### Entity Candidate Expansion
The predicted entities and candidate spans are then expanded using external lexical knowledge sources, primarily Princeton WordNet.
From this resource, the system retrieves:
~={blue}Synonyms=~
- Semantic relations
- Related word forms
This helps generate additional context about each candidate entity.
Knowledge Retrieval (RAG Layer)
For each candidate entity, the system retrieves supplementary information and semantic metadata from knowledge sources.
The retrieved information is structured into metadata fields such as:
- Entity type hints
- Definitions or descriptions
- Related terms
- Contextual attributes
- Context Augmentation
The retrieved metadata is formatted and appended to the original sentence.
This produces a context-enriched representation of the input text, giving the model more semantic signals to reason about the entities.
### Refined NER Inference
The augmented input is then passed to a second NER model trained specifically on enriched contextual inputs.
With the additional retrieved knowledge, this model can:
- Resolve ambiguous entities
- Improve classification accuracy
- Increase confidence in predictions.
### Final Output
The system outputs refined NER predictions with improved confidence and reduced ambiguity compared to the initial lightweight model.

## CEVEATS & PITFALLS
- **Span–Retrieval mismatch (granularity).**  
    You retrieve documents per _entity candidate_ but may be using document-level retrieval or surface-form expansion that isn’t targeted to the exact span semantics; this yields noisy or irrelevant passages and hurts the refined model. (RAG authors emphasize passage-level vs token-level conditioning choices.)
- **Weak retriever (lexical vs dense).**  
    Using only WordNet/lexical expansion + BM25-style matching underperforms modern dense retrievers for semantic matching, especially for rare/ambiguous entities. DPR/dense retrievers significantly outperform lexical baselines on passage retrieval and downstream tasks.
- **Noisy knowledge insertion (format & hallucination).**  
    Appending raw retrieval text or long lists of synonyms can confuse the refined model and increase hallucination or label drift (especially when retrieved passages contradict local sentence context). RAG work shows gains only when retrieval is cleanly integrated and conditioned correctly.
- **No retrieval ranking / re-ranking stage.**  
    If all retrieved items are treated equally, low-quality items dilute signal. Cross-encoder re-ranking or sequence-level scorers are essential to prioritize highly relevant context.
- **Insufficient training/evaluation alignment (silver noise).**  
    If your refined model is trained on vanilla NER data but evaluated on augmented inputs, you get distribution shift. Also, WordNet expansions can inject sense ambiguity unless sense-disambiguation is used. Past WordNet-NER work needed explicit rules to avoid polysemy errors.
- **Poor confidence calibration & decision gating.**  
    Relying on raw softmax scores from either stage gives miscalibrated confidence. RAG-style systems need calibrated thresholds and uncertainty-aware routing for when to call the heavy model.

## A — Improve retrieval quality & granularity
1. **Switch to span-aware dense retrieval (DPR-style) for candidate→passage mapping.**
    - Train a dual-encoder retriever where queries are _entity-span + local sentence context_, and passages are short knowledge snippets (50–300 tokens). DPR shows large retrieval gains vs BM25. Use FAISS/annoy for indexing.
    - Practical knobs: embed queries as “[SPAN] + sentence” and retrieve top-k where k ∈ {5, 10, 20}, then re-rank. DPR papers typically evaluate top-20 but RAG readers often use smaller k (e.g., 5) for generation efficiency; experiment with k tradeoffs.
2. **Use passage-level indexing (not whole-doc) and include source IDs (Wikidata/Wikipedia/WN synsets).**
    - Store metadata (KB id, synset id, type hints) alongside text so the downstream model can use structured signals (not just raw text). RAG work benefits when retrieved content is short and relevant.

## B — Rank & filter retrieved content
3. **Add a cross-encoder re-ranker (cross-encoder on candidate span + passage).**
    - After retrieving top-k with a dual encoder, run a cross-encoder to produce a high-quality rank and confidence score. This sharply reduces noise (empirical in passage retrieval literature).
4. **Uncertainty gating: route only low-confidence or ambiguous spans to the heavy pipeline.**
    - Compute a calibrated uncertainty score on the distilled model; only call full RAG for spans above an ambiguity threshold. RAG-style systems benefit from this two-stage compute control (cost + quality).

## C — Integrate KB data more robustly (don’t just append text)
5. **Attach retrieved metadata as structured features, not raw appended text.**
    - Instead of concatenating long definitions/synonym lists, add structured tokens or special features: e.g., `<type_hint:PERSON>`, `<synset:XXXXX>`, and a short 1–2 sentence canonical description. RAG paper and knowledge-enhanced NER approaches stress structured conditioning over raw dumps.
6. **Sense disambiguation for WordNet expansions.**
    - If you use Princeton WordNet-derived synonyms, first run a Word Sense Disambiguation (WSD) step to select the correct synset for the span; otherwise WordNet polysemy will add noise. Historical WordNet-NER systems used predicate filtering / rules to avoid polysemy errors.

## D — Training & objective alignment
7. **Fine-tune retriever + reader end-to-end on NER objectives (RA-NER approach).**
    - Recent RA-NER approaches train retriever and reader jointly (or fine-tune reader on retrieval-augmented inputs) for entity-labeling tasks rather than QA/generation only. This aligns retrieval with the NER objective; follow RA-NER training recipes (retrieve relevant KB passages and minimize NER loss on the reader).
8. **Augment training data with retrieval-augmented inputs (data augmentation).**
    - Synthesize training examples where correct KB snippets are attached to spans; train the refined model to use them. Also use distant supervision from KBs (create silver labels via entity linking) but clean with heuristics. Knowledge-enhanced NER surveys support this practice.

## E — Robustness & calibration
9. **Calibrate confidences (temperature scaling / isotonic regression) and track ECE.**
    - Calibrate final probabilities; use Expected Calibration Error (ECE) to measure and tune. Use calibrated scores for downstream gating and human-in-the-loop routing.
10. **Add re-check stage: entity linking + canonicalization.**
    - After refined labels, run an entity-linker (candidate generation + ranking) to map spans to KB IDs (Wikidata/QName). Use the linker’s score as an extra signal for final confidence and to reduce label ambiguity. RA-NER-style systems and knowledge-enhanced pipelines commonly combine NER + linking.





