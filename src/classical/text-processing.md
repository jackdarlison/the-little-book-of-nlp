# Text Processing

## Regex

Regex is a language used to find text that matches a certain pattern.

Some useful pattern notations:
- The common operator with characters is concatenation
- Brackets match a single character from a range of characters, e.g. \[a-zA-Z\] or \[abcdef\]
- Carat at the start of brackets indicates "not" in range, e.g. \[^a\] is not "a"
- A ? indicates the preceding symbol is optional
- A * indicates 0 or more of the preceding symbol, + is 1 or more
- {x, X} indicates a range between x and X of the preceding characters, variations of {x}, {x,} and {,X} exist
- A . indicates any character
- ^ and $ indicate the start and end of lines respectively
- \\b is a word boundary
- The | indicates the "or" operator
- \\s (\\S) indicate (not) whitespace characters
- \\w (\\W) indicate (not) word characters
- \\d (\\D) indicate (not) digit characters
- () indicate a capture group, where \\1 .. \\n can be used to reference the specific capture group match. 
- (?:) indicates a non-capturing group, useful with the | operator. 
- (?=) (?!) indicate positive and negative lookahead (non-matching)
- (?<=) (?<!) indicate positive and negative lookbehind (non-matching)

## Text Processing

Given a text, we may want to separate the text into individual words. This process is known as word tokenisation, other forms exist which may tokenise the text into sub-word pieces or spans of words.

**Byte Pair Encoding (BPE)** is an automated method of tokenising a text into sub-words. It works as follows:
- Start with a set of all characters, the vocabulary.
- Split the text into characters
- Find the most frequent concatenation of two tokens from the vocabulary present within words in the text, merge them, and add the merged token into the vocabulary
- repeat until \\( k \\) merges have been made.

**Wordpiece** tokenisation is similar to BPE but instead of concatenating the most frequent, a language model is trained and the concatenation which improves it the most is chosen. Repeat until there are a certain number of tokens.

**Unigram** or **SentencePiece** tokenisation starts from the opposite side to the previous tokenisation. A giant vocabulary is created using individual characters and frequent sequences of characters (including spaces). Tokens are iteratively removed until a desired vocabulary size is created

It may be useful to normalise words into a single form, of which there are two methods:
- Lemmas are the root (dictionary) form of words, e.g. sing is the lemma of sing, sang, and sung
- Stems are the main body of the word with modifying suffixes removed

## String Similarity

Many NLP applications may need to know how similar two strings are. 

To do this minimum edit distance is used, which scores the number of edits needed to change a string to another. There are three common operations:
- Insertion, weighted 1
- Deletion, weighted 1
- Substitution, weighted 1 using Levenshtein distance, or commonly 2 

This application uses dynamic programming, by recursively finding the best substring and building up to the whole string. Best substrings are scored by the lowest operation cost. 

To create the best alignment between two strings, a table is built where each cell is the substrings score, and back pointers are used to keep track of the previous state. Then the lowest cost path back through the table is the best alignment.

