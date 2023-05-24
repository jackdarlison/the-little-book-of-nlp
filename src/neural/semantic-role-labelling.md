# Semantic Role Labelling

Semantic roles are the different roles that an argument can take of a predicate in an event. 

**Thematic roles** capture the semantic commonality around participants in an event.  E.g. Agent, experiencer, theme, goal...

**PropBank** is a resource of semantic role annotated sentences. It focuses on verbs, giving a set of arguments and modifiers.

Arg0 and Arg1 are present on most verbs and define the agent (causer) and patient (receiver), other arguments are verb specific. 

ArgMs are used to indicate modifiers such as time and location, common across all events

**FrameNet** is a another semantic role resource however focuses on specific word senses instead of words. Frames are a set of word senses that share the same core roles (similar to arguments) and non-core roles (similar to modifiers)

**Semantic Role Labelling (SRL)** is the task of assigning roles to spans of text in sentences. Making it a sequence labelling task. 

SRL can be done using feature based methods, however, it is more commonly done as neural system. 

The input consists of the event constituent (sentence normally) followed by a separator and the prediciate again

Follows the steps:
- BERT embed the input, concatenating a mark for the predicate. 
- The embeddings are then fed into a neural encoder such as BiLSTM or transformer layers. 
- Concatenate each hidden layer output with the predicate output
- The output is then fed into a MLP + softmax to classify 

The output is given as BIO tags from SRL resources such as PropBank or FrameNet. 

