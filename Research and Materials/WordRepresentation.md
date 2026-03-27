# Word Representation

TAGS: #NLP 
BUILT-ON:
ENABLES:
PREREQUSIES:
RESOURCES: [YT: Stanford](https://www.youtube.com/watch?v=ERibwqs9p38)

---
## Word Representation
### Early Era
Early Era used Symbolic and discrete representation methods.
These approaches treated meaning as symbolic and structured.
1. Taxonomy Based Meanings
	- Meaning was defined through **relations between concepts**.
	- Resource: [[WordNet]] 
	- Representation forms: Graphs or Trees
	- Advantages:
		  Explicit Semantics
		  Human Interpretable
	- Disadvantages:
		  Manual Construction
		  No Statistical Learning
		  Poor Scalability
		  Lacks Quantitative Similarity measurement 
2. Discrete Feature Based Representation
	- Words are represented as handcrafted feature vectors to describe their semantic meaning.
	- These are sparse, Binary or categorical and Human-designed

### Count Based Vector Space Models
1. One Hot Encodings
	- Each word in the vocabulary was represented as a vector dimension.
	- Disadvantages:
		  Sparse
		  Doesn't Capture Similarity
		  Huge Dimensionality
2. Co-occurrence Matrix
	- Count how often words appear near each other.
3. Statistical Weighting
	- Used to reduce noise and improve signal
	- TF-IDF
	  PPMI (Positive Pointwise Mutual Information)
	- Disadvantages:
		  Very High Dimensional
		  Memory Heavy
		  Static (one meaning per word)

### Distributed Hypothesis
~={purple}“You shall know a word by the company it keeps.” — J. R. Firth=~
Meanings of words came from contexts, if words appear in similar contexts they were considered as semantically similar. 

### PREDICTION BASED EMBEDDINGS
True revolution in word representation based on its context.
Learn Vectors by predicting context.
1. [[Word2Vec]]
	- Instead of counting context, context is predicted.
	- Values are learned automatically
	- Word Embeddings was revolutionary as
		  Embeddings capture semantic similarity
		  Embeddings had a linear structure allowing linear operations 
2. [[Glove]]:
	 - Hybrid of Global co-occurrence and Neural Learning
3. [[Fast Text]]:
	- Represents words as sum of character n-grams
	- Handles rare words better

### CONTEXTUAL EMBEDDINGS
Words in prediction based embeddings had a fixed vector and hence failed to separate word sense disambiguation.
- [[BERT]]:
	- Based on transformer architecture from google.
	- Word embeddings are a function of word and the sentence its present in.
- Transformer representing Meaning
	- ......

### MODERN LARGE LANGUAGE MODELS
Modern LLMs are built on transformer architecture, massive data and billions of parameters. In modern LLMs meaning is not just a vector its distributed across layers, attention heads and neuron activation.
Modern LLMs encode semantic, syntactic, and relational information. 
Mathematically, a word `w` in sentence `S` can be represented as:
$$
Meaning(w)=fθ​(w,S)
$$
Where
- f = neural network
- θ = learned parameters
- S = context
