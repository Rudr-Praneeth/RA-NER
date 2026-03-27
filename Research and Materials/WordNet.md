# WordNet

TAGS: #TEMPLATES 
BUILT-ON:
ENABLES:
PREREQUSIES:

---
## WordNet
- WordNet is a large lexical database containing information about noun, verbs, adjectives and adverbs of English developed at Princeton University. 
- Each entry representing a distinct concept/sense, and interlinks them through semantic and lexical relations.
- Senses contain 
	1. ~={yellow}Gloss=~ (definition of the sense).
	2. ~={yellow}Synsets=~ (sets of cognitive synonyms).
- Relationships between senses:
	1. ~={yellow}Hypernym=~: Relation between word and its superordinate.
		   Dog(superordinate) is a Hypernym of Jeremy.
	2. ~={yellow}Hyponym=~: Relation between word and its subordinate
		   Jeremy(subordinate) is a Hyponym of Dog.
	3. ~={yellow}Meronym=~: Relation between part and its whole
		   Fur is a Meronym of Teddy.
	4. ~={yellow}Holonym=~: Relation between whole and a part.
		   Teddy is a Holonym of Fur.
	5. ~={yellow}Antonym=~: Relation between two semantically opposite concepts
		   Jeremy's calmness is an Antonym of Teddy's Chaos
	6. ~={yellow}Synonym=~: Relation between semantically similar concepts
		   Dogs and Pups are synonyms.
- It is a  foundational resource for computational linguistics and natural language processing (NLP).
### STRUCTURE AND HEIRARCHY
- Unlike a dictionary arranged alphabetically, WordNet is organized conceptually. Each synsets groups words that share the same meaning, while relations such as synonymy, antonymy, hypernymy (“is-a”), meronymy (“part-of”), and troponymy (“manner-of”) connect concepts across parts of speech.
- Nouns form deep taxonomic hierarchies.
- Verbs link by entailment and troponymy.
- Adjectives cluster around antonym pairs.
### APPLICATIONS IN LANGUAGE TECHNOLOGY
WordNet underpins tasks in:
- Word-sense Disambiguation
- Information Retrieval
- Semantic Search
- Machine Translation
- Though later neural models learn semantics automatically, WordNet remains a benchmark for structured lexical knowledge and a bridge between symbolic and statistical representations of meaning.
