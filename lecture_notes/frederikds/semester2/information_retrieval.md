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

Ranking function (sometimes) also known as similarity/distance function in some space, that is, it is implemented by a similarity/distance function.

The *term vector* representation is an oversimplification of a document, there is in fact much more information in a document, we will later look at
these kinds of additional information (latent semantic topic models, word embeddings, ...).

> Relevance feedback := feedback from the user about the relevance of the retrieved documents. Can be used to improve ranking performance.

Similarity function in boolean model is very strict: a document is only relevant if and only if it has the exact same weights as the query.

**Extended Boolean model** doesn't actually use boolean weights in its documents, but it does still receive boolean queries.

> ? In the **extended Boolean model**, are the document vectors generated during query time? If not, isn't it just the same as the vector space model?
>
> Answer: No they are not generated during query time. All the weights are defined during "document preprocessing", the exact vector that is used during
> ranking is then constructed from these weights. It is different than the vector space model in that only the relevant weights
> (from the terms present in the query) are considered, while in the vector space model the query has a weight for each possible term.

Idea: More complex Boolean queries (such as $(W_1 \wedge W_2) \vee W_3$) in the **extended Boolean model** can be answered by converting it to one big
disjunction/conjunction then inverting the weights for the negative words in the query, e.g., $1-W_3$ when $\lnot W_3$ is in the query, and then applying
the same similarity function with the transformed document weights.

Term vector, in general, will be very sparse (since there are a lot more words than there are words in a specific document).

Note how in the **vector space model** distance != similarity.

To see why **cosine similarity** is useful, look at the visual representation and look how the $cos(\alpha)$ goes to 1 when the document and query are
more and more equal.

Vector space model assumes that there is no correlation between different terms, though this really is a big deal in natural language.

The orthogonality-assumption of the vector space model can be a "big deal", e.g., synonims are expected to be unrelated with 0 similarity according to
the $sim$-function!

A model is a bag-of-words representation when it does not hold into account any of the sequential information, i.e., the ordering of the terms in a sentence.

Repeat: we (usually) calculate the term weights for document by $TF \cdot IDF$.

Using **log-odds** to rank documents allows us to not need $P(D, Q)$.

> ? How do we estimate $P(W_i \vert Q, R)$? Usually estimated by max likelyhood, so I'm guessing there is some counting involved?
>
> Answer: Yes, you count the probability of that word in the number of documents relevant to this query and divide it by the total number of documents relevant
> to the query, this will work well when we already have pretty good estimations. Yet what if we don't have this, e.g., at the start? This is the
> **cold start problem**, to solve it, you try using accurate guesses, either by the formulas provided in the slides, or by some other means, e.g.,
> relevance feedback.

Informally, the **language model** assumes there is just one document that correctly "generates" the query and that the user knows something about this document
(or guesses it correctly). The probability that the model generates the query is $P(Q \mid D)$.

If there is any formula we should know by heart, it should be the formula for the smoothed language model.

The smoothed language model uses a document collection $C$, such that $P(w_i \mid C)$ is the language model for the document collection, e.g., the language model
for the document containing all documents. This is a useful form of smoothing because data sparseness will be less of a problem over the entire document collection
than it is over a specific document.

The language model also works when the probability of a query given a document is close to 0.

> Language model := $p(\cdot \mid D, r)$

The inference model constructs a belief network integrating both the documents and queries (including the words, concepts, etc. they introduce) and performs
belief propagation on this to calculate probabilities. The way the probability of a node given its parents is computed is not by a conditional probability
table per se, but could also be some very specific way to get some nice final results, e.g., in a way that it behaves as in Boolean logic.

Lesson 3: Monolingual probabilistic topic modeling
--------------------------------------------------

In **probabilistic topic models** we assume the following factorization holds: $P(w \mid d) = P(w \mid z) P(z \mid d)$, i.e., this is how a topic is related to the documents
and words. Note how the name implies there can be non-probabilistic topic models, i.e., this factorization might not be used in some topic models.

> Latent semantic topic model := An unsupervised (or semi-supervised) model that captures the semantics of a document by its topic.

> LSI (Latent Semantic Indexing) := A very simple latent semantic topic model that assumes the semantical information can be derived from the word-document matrix ($P(w \mid D)$).

Major issue of LSI: it cannot express multiple meanings of the same word.

From the assumed factorization of probabilistic topic models it is clear we should try to find $P(w \mid z)$ and $P(z \mid d)$ - if we have this, we can find $P(w \mid d)$,
which can be used to rank documents - they are approximated from data.

> Latent variable := hidden variable

The best we can say about some hidden variables $\theta$ is that they "generated" the observed variables $X$, so it makes sense to find the values of $\theta$
that maximize the likelihood $P(x \mid \theta) = L(\theta \mid x)$, e.g., we would like to find $\theta'$ such that $argmax_\theta L(\theta \mid x) = \theta'$.

> pLSA := Assumes the joint probability assumptions from the belief network in slide 14. From this simplify the joint and train it by maximizing the log-likelihood, e.g., by using EM.

The joint $P(d_j, w_i)$ can be simplified in *pLSA* as follows: $P(d_j, w_i) = P(d_j) P(w_i \mid d_j)$ and $P(w_i \mid d_j) = \sum_{k=1}^K P(w_i \mid z_k) P(z_k \mid d_j)$.
This is simple to prove if you remember from the independency assumptions of *pLSA* that $P(w_i \mid z_k) = P(w_i \mid z_k, d_j)$ since $W$ is independent to $D$ given $Z$.

A story for generating documents using *pLSA*: (1) Sample a document $d$ with probability $P(d)$ (2) Sample a topic $z$ with probability $P(z \mid d)$ (3) Sample a word with probability $P(w \mid z)$ (4) Go back to step 1 until enough words.

Proof for E-step of *pLSA*:
$P(z_k \mid d_j, w_i) = \frac{P(z_k, d_j, w_i)}{P(d_j, w_i)} = \frac{P(z_k \mid d_j) P(w_i \mid z_k)}{\sum_{l=1}^K P(d_j, w_i, z_l)} = \frac{P(z_k \mid d_j) P(w_i \mid z_k)}{\sum_{l=1}^K P(w_i \mid z_l) P(z_l \mid d_j)}$

Disadvantages of *pLSA*:

- $P(z_k \mid d_j)$ only works for the documents it is trained for, however, $P(w_i \mid z_k)$ can be reused for new documents since it is not dependent on them, doing this is called **folding in**.
- Every time a document is added, $K$ topics have to be introduced in the belief network, e.g., the number of latent variables grows linearly with respect to the number of documents.

> LDA (Latent Dirichlet Allocation) := A latent semantic topic model that assumes the independency assumptions given in slide 20.
>
> $\theta$ := Per-document topic distribution
>
> $\phi$ := Per-topic word distribution

> Conjugate prior := A conjugate prior is a distribution that is both a prior of some posterior and is a conjugate distribution. Let $P(X \mid \Theta) \sim A$ with prior $P(\Theta) \sim B$. $P(\Theta)$ is a prior, if $P(\Theta \mid Y) \sim B$ it is a conjugate prior.

> $\sim$ := follows distribution, e.g., $P(X \mid \theta) \sim A$ = $P(X \mid \theta)$ follows distribution $A$.

> Variational inference := The principle of approximating a distribution as $P(\theta \mid X) \approx Q(\theta)$ with $\theta$ a vector of latent variables and $Q$ a
> *variational distribution*. In general $Q(\cdot)$ should always be of a simpler form than $P(\cdot \mid X)$.

Applying variational inference in *LDA* is done by assuming a simpler model and finding the hyperparamaters - e.g. by means of EM - such that the KL-divergence
between the initial model and the variational model is minimal. This simplified model can then be used to (more easily) approximate topic-document
distributions in the future.

> Corpus := Collection of all documents.

Topic models have lots of uses, one would be to label documents with topic vectors and use it:

- to rank
- to organize
- to summarize
- to incorporate it in a language model
- for recommendation
- for text classification
- ...
