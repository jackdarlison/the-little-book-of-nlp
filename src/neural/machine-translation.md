# Machine Translation

Machine translation is a sequence-to-sequence based task, therefore making use of the encoder-decoder pattern. 

Machine translations are trained on parallel corpuses, or bi-texts. 

#### Language Differences

Languages have many differences which make the translation from one to another difficult, such as:
- Morphological differences: characters per word
- Event-framing: e.g. verb or satellite
- Word order typology: e.g. Subject-Verb-Object or SOV or VSO...

#### Low Resource Translating

Finding datasets suitable for training large encoder-decoder models is not always easy.

**Back translation** can be used when we have small parallel corpuses but have access to larger target mono-lingual corpuses. The parallel corpus is extended by:
- Training a target-to-source model on the small bi-text
- Translate the mono-lingual corpus to the source language, and add the aligned sentence to the bi-text
- Train a source-to-target model on the extended bi-text

**Multilingual** models can be trained given many parallel sentences in different pairs of languages. This can improve the performance on low resource languages by drawing on information used for other translations.

There are however **Sociotechnical Issues** as low resource language dataset may not be accurate or even correct. In addition, there is a large focus on english-other and little data for other language pairs