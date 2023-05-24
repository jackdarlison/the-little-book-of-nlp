# Word Sense Disambiguation

Words are ambiguous as the same word may have multiple meanings, **Word Sense Disambiguation (WSD)** is the process of deciding the sense of a word given its context.

**WordNet** is a large hand-annotated corpus giving word sense definitions and relations between word senses. A **synset** is a collection of word senses which all share the same meaning, and a gloss (description) of the general meaning of the set.

WordNet also describes the lexicographical categories of semantic fields, called **supersenses**. E.g. for nouns some of the supersenses are location, act, feeling...

WSD is a sequence labelling task, which involves picking the correct sense for each word based on the context.

A baseline algorithm would be to pick the most common word sense for each word. 

A better method of WSD is to:
- contextual embeddings (e.g. BERT)
-  For each sense of words in the corpus: average each of the token embeddings for the sense to get a sense embedding
- Then use a nearest neighbour classifier (cosine similarity) to find the nearest word sense

If we don't have sense for a word in our data, we can:
- Fall back to most common sense
- Use relations in Wordnet, in order:
	- If has children: average their embeddings
	- If has hypernyms: average their embeddings
	- else average the supersense embeddings
