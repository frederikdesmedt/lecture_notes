Text-based information retrieval
================================

Overall information
-------------------

Publications put on Toledo are for more information, but they are not meant for learning.

"What have we learned?"-slides are pretty much "what to know"-slides?

Lesson 1
--------

Information retrieval can be looked at like finding a function mapping a query to an (ordered) list of documents.

Bag-of-words-representation: Tokenization -> Stemming -> removal of stopwords
Stemming is useful in "morphological-rich languages".

Lemmatization <-> stemming

The stem or root (the result of stemming) might not correspond to the lemma of the word!

n-grams: sequence of sequences of words/characters of size n. So a 1-gram (unigram) is a sequence of single
words/characters, while a 2-gram (bigram) is a sequence of sequences of two words/characters, etc.

> Term weighting := define weights to terms to signal the importance of a term in a text.

Law of Zipf is useful to get an idea on the ranking of words based on the frequency.
Word with highest frequency: rank 1, word with second highest frequency: rank 2, ...
The underlying assumption is that words with a high frequency signal main topics of the text.
The term weight would then be proportional to the frequency of the word.

Inverse document frequency assumes words that occur exceptionally in other texts must be words related to the topic of the document.
So words such as "and" and "the" will have a weight very close to 0.

Compromise for arguments of law of Zipf and of inverse document frequency: let the weight be the product of the two:
$w_{ij} = tf_i*log(\frac{N}{n_i})$ with $tf_i$ the term frequency of term $i$, $N$ the total number of documents and $n_i$ the
number of documents containing term $i$.

Term frequency can be unfair: when finding relevant documents of a query longer documents will, in general, have higher term frequencies.
The result is that longer documents will more often be considered "more relevant". If this is unwanted, length normalization can be used
such that all normalized term frequencies fall between 0 and 1, then there will not be a preference to the size of the document.
If a small preference to longer is required, one can use an augmented normalized term frequency, where the importance of the size of the
document can be adjusted.

> Retrieval model := Model for finding relevant documents of a query given the representation for queries and documents, aka the ranking function.

The exact implementation of the retrieval model obviously depends on the representations for queries and documents, e.g., a retrieval model for
term vectors as representations can be a vector similarity function (inner product, cosine similarity, ...). This points out the relation between
the retrieval model and the representations.
In practice one would like to find a trade-off between the two: try to get a representation that
encapsulates as much information as possible, yet at the same time keep the time and memory complexity at compile-time (i.e. the retrieval model +
conversion to the representation) as small as possible!

`Representation model := Data representation x matching algorithm`

> Semantic labeling := Labeling of a document with some kind of semantics (such as the topic of the document)

Lesson 2: Information retrieval models
--------------------------------------

Pretty much all ranking functions are found by unsupervised learning.

Ranking function also known as similarity/distance function in some space.

> Relevance feedback := feedback from the user about the relevance of the retrieved documents. Can be used to improve ranking performance.

Similarity function in boolean model is very strict: a document is only relevant if and only if it has the exact same weights as the query.

**Extended Boolean model** doesn't actually use boolean weights in its documents, but it does still receive boolean queries.

Term vector, in general, will be very sparse (since there are a lot more words than there are words in a specific document).

Note how in the **vector space model** distance != similarity.

To see why **cosine similarity** is useful, look at the visual representation and look how the $cos(\alpha)$ goes to 1 when the document and query are
more and more equal.

Vector space model assumes that there is no correlation between different terms, though this really is a big deal in natural language.

A model is a bag-of-words representation when it does not hold into account any of the sequential information, i.e., the ordering of the terms in a sentence.

Repeat: we (usually) calculate the term weights for document by $TF \cdot IDF$.

Using **log-odds** to rank documents allows us to not need $P(D, Q)$.

If there is any formula we should know by heart, it should be the formula for the language model.

The language model also works when the probability of a query given a document is close to 0.
