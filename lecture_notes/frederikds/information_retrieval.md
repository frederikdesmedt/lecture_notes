Text-based information retrieval
================================

Information retrieval can be looked at like finding a function mapping a query to an (ordered) list of documents.

Bag-of-words-representation: Tokenization -> Stemming -> removal of stopwords
Stemming is useful in "morphological-rich languages".

Lemmatization <-> stemming

The stem or root might not correspond to the lemma of the word!

n-grams: sequence of words/characters based on frequency?

!Term weighting!

Law of Zipf is useful to get an idea on the ranking of words based on the frequency.
Word with highest frequency: rank 1, word with second highest frequency: rank 2, ...

In inverse document frequency words such as "and" will have a weight very close to 0.

Length normalization is required because otherwise we are favouring longer documents.

Term vector, in general, will be very sparse (since there are a lot more words than there are words in a specific document).

`Representation model = Data representation x matching algorithm`

> Semantic labeling := Labeling of document with some kind of semantics (such as the topic of the document)

Publications put on Toledo are for more information, but they are not meant for learning.
