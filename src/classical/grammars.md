# Grammars

Grammars enable the formalisation of language and grammar. This has wide uses in NLP such as sense disambiguation, grammatical checking, etc.

## Context Free Grammars (CFGs)

CFGs grammars consist of a set of production rules, containing a non-terminal symbol and the symbols it generates, shown though an arrow.

CFGs split language into constituents, which is the idea that groups of words can be split into units, or constituents. 

Applying a set of productions to a text is a derivation, which are shown through parse trees:
- Terminal nodes (words) are the leaves
- Non-terminal nodes define lexical categories (e.g. POS tags)
- The root node is the usually the start symbol (S)

There can be multiple derivations of a single sentence using the same grammar, this can lead to ambiguity to the intended parse. 

Sentences which cannot be represented using the grammar are known as ungrammatical, the set of all possible generations are the grammatical sentences. This is known as structural ambiguity. 

**Treebanks** are corpuses in which every sentence is annotated using a parse tree. E.g. the Penn Treebank POS tagset

#### CKY Parsing

Cocke-Kasami Younger (**CKY**) parsing is a classic dynamic programming (chart parsing) approach to parsing.

For CKY grammars must be in Chomsky Normal Form, where productions are either two non-terminals or a single terminal. All CFGs can be converted into CNF through the addition of new dummy non-terminals. 

CKY represents all possible parses of a sentence by filling a 2D matrix where each cell \\((i, j)\\) represents possible constituents in the fenceposted span between \\(i\\) and \\(j\\). The matrix is filled left-to-right and bottom-up, ensuring that we have the solutions to all sub problems of a specific cell. Backpointers are kept to show where the individual parses were derived 

Filling the parse representation matrix gives us all possible parses (by follow back pointers from (0, n)) However it does not tell us which is best. 

To choose the best parse a neural constituency parser is created. This assigns a score to each constituent then a modified CKY is used to combine the scores in a grammatically valid way

To evaluate parsers F1 score is used, based on the constituents being in the same starting, ending points and non-terminal symbol 

## Dependency Grammars

Typed Dependency grammars take the form directed acyclical graphs. Where each node is a word and each arc is a label for the grammatical relation. Every word only has one incoming relation and there must be a path to every word from the root (which has no incoming)

This system provides useful information for tasks such as information extraction, semantic parsing and question answering.

Dependency grammars are much less structural than CFGs as no positional information is given for the words in a sentence, only the relations between two words are given. 

The arcs are directed, going from the "head" word to the "dependent" word. An arc is projective if there is a path from the head to all words between the head and the dependent. The whole tree is projective if every arc is.

**Transition-based parsing** is used to parse dependency grammars. It consists of:
- Stack: where the parse is built, initialised with the root node
- An input buffer: the sentence (in order)
- A set of relations: the created dependency tree
- A set of transition operators. e.g. 
	- shift: move word from buffer to stack
	- left arc: create head-dependent relation between top and second, remove second
	- right arc: create head-dependent relation between second and top, remove top

At each stage:
- Shift a word onto the stack from the buffer
- Examine the top two elements and choose to apply a transition operator if possible. 

This method can only produce projective dependency trees

**Graph-based parsing** uses the maximum spanning trees to create dependency structures. 

This works by creating a graph which is fully connected, weighted and directed where the vertices are input words and all possible relations are shown through arcs. A root node with arcs to all nodes is added.

The weights of each arc reflect the score for a possible head dependent relation. This can be assigned either using feature based methods or neural based methods

The best possible dependency parse is equivalent to the maximum spanning tree. Which is a sub-graph of the original starting from the root with one path to all nodes with the highest score among the edges. 

Both parsing approaches are trained using supervised machine learning using data from tree banks. 
