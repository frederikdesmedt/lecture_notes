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

> LSI (Latent Semantic Indexing) := A latent semantic topic model that assumes the semantical information can be derived from the word-document matrix ($P(w \mid D)$).

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

Lesson 4: Advanced Text Representations: Part 2
-----------------------------------------------

3 types of representation:

- Unstructured representation: Text as unordered set of words, e.g., bag of words.
- Weakly-structured: Groups of terms with additional importance parameters
- Latent representations: Discovering of (latent) topics/concepts as part of text representation, e.g., LSI, LDA, word embeddings, ...

A lot of text representations are a combination of different representations, e.g., vector space representation + language model + additional metadata.

> *one hot* representation := Vector space representation where each word is a unique unit vector in exactly one of the dimensions (union of all word representations is a basis of the vector space).

> *dense* word representation := Vector space representation where $dimension << size of vocabulary$, reduces dimensionality a lot, but you might lose information.

LSI will try to reduce the dimension of the vector space (to a $k$-dimensional space) by decomposing the word-document matrix to its SVD-factorization.
From this SVD-factorization, only the $k$ largest singular values are kept in $\Sigma$ (the others are set to 0), and only the corresponding eigenvectors
in $U$ and $V$ are kept. The resulting matrix $\hat{A} = U_k \Sigma_k V_k$ is the a matrix of dimension $k$ such that the least-square error between
$A$ and $\hat{A}$ is minimal.

What should $k$ be in LSI? -> Open problem, but usually somewhere between 50 and 300 and you need to be sure that $k \ll t$ (number of terms)
and $k \ll m$ (number of documents).

In LSI we can convert queries and document to coordinate vectors in a reduced space and perform cosine similarity on the coordinate vectors to get a ranking.

A neural net with one hidden layer can represent any function, but we use deep neural nets because otherwise the number of nodes in the single hidden layer can explode.

> Word embedding := A technique for finding word vector representations that also holds additional contextual information, e.g.,
> the words that went before and after a word in the document.

Why are word embeddings useful? Because context can change the meaning of the word, e.g., "not bad" != "not" + "bad"
and "almost correct" != "almost" + "correct".

Word embeddings are trained from its context, e.g., CBOW, Skip-gram NN-LM, GloVe, etc.

> ? Can a word embedding be more powerful than the word vector in a posssibly reduced vector space (like in LSI)? I would say so, because the word embedding
> uses more information than just the word in a document, but also the context of that word.

> CBOW model := Used for learning word embeddings: uses a neural net that will predict the current word based on the neighbourhood/context
> (some of the previous words and some of the next words).
>
> Skip-gram NNLM model := Used for learning word embeddings: uses a neural net with as input a one-hot representation of the current word and
> predicts the neighbourhood/context.

Disadvantage of skip-gram model: activation function on output nodes uses *softmax*, which is computationally expensive since it requires
a (complex) summation over all output nodes.

> *word2vec* := Optimized code for calculating CBOW and skip-gram models.

Open question: How many dimensions should we allocate for a word? I.e., how large should a word vector be? I.e., how many hidden nodes should we put
in our neural net word embeddings?

> Truncated backpropagation in RNN's := Only backpropagate over the time steps in the **immediate future**.

The weird dot with a circle around it in **LSTM** is point-wise multiplication.

Now that we know how to create word vectors, how can we use it for information retrieval? Different models:

- Vector space model:
  - Centroid model: Document and queries are represented as a normalized sum of their word vectors. Possible to mix this with traditional bag-of-words model.
- Neural translation language model: Uses the probabilistic approach of ranking (e.g., finding a language model) estimates $P(w \mid z)$ by cosine similarity of word vector and "topic vector" normalized over sum of cosine similarity between the word vector and all "topic vectors".

Checkout:

- http://colah.github.io/
- https://medium.com/mlreview/understanding-lstm-and-its-diagrams-...

Lesson 5: Advanced Text Representations: Part 2
-----------------------------------------------

Term-document co-occurrence matrix can be used to understand something about a document, but also to understand something about a term. Then you could
represent a similarity relation between two terms by performing vector similarity between the two rows of the terms, e.g., cosine similarity.
The same with documents, sentences, ...

This term-document co-occurrence matrix is very large and sparse, so we want to perform dimensionality reduction. This is what happens in word embeddings, LSI, ...

In LSI, dimensionality reduction is performed by converting the term-document matrix to an SVD-factorization and reducing the rank of the SVD to $k$
(which is considered easy to do for an SVD).

With a term-document matrix $A$, one can see that $A^{T} A$ is a document(-to-document)-correlation matrix, similarly $A A^{T}$ can be seen as a
term-correlation matrix.

Lesson 5: Indexing structures and Search techniques
---------------------------------------------------

> Semantic hashing := A kind of hashing such that items with similar hashes are similar themselves, e.g., have similar topics, ...

Semantic hashing basically solves the problem of similarity with items such as images, so of course they can be difficult to compute, e.g., deep semantic hashing,
but the moment you have generated them, they allow for very fast searching.

Lesson 5: Evaluation Measures
-----------------------------

Different kinds of relevance: topical: we found documents of the correct topic, motivational/interpretational relevance: finding the documents the user needs.

In information retrieval motivational relevance is too difficult to handle, so we use weaker notions of relevance. Yet, topical relevance is only the first
step in relevance in information retrieval.

Lots of evaluation measures that aren't relevance measures: execution efficiency, space efficiency, "retrieval effectiveness" (precision, recall, ...), ...


Lecture 6: Text categorization
------------------------------

> Text categorization := Assigning descriptors to documents.

Text categorization seems to usually be done by supervised learning (where we get a training set with document-descriptor pairs).

Feature selection is useful in reducing curse of dimensionality.

Ways of doing feature selection:

- Unsupervised
 - Frequent item sets: find words that often occur together in the document collection.
 - Hyperclique patterns: Similar to frequent item sets, yet with the additional constraint that there exists some kind of relation between the items in the item set. This is useful for reducing the amount of work in finding frequent item sets.
- Supervised: Computes relevance or correlation between a term and the classification.
 - $\chi^2$: Find correlation between class distribution where some term is present and class distribution of document collection where term is not present.
 - Entropy: Using entropy to calculate information gain when introducing some feature, if it's significant perhaps we would like to keep it.

Feature extraction is used to transform features to other features containing some specific kinds of information.

$P(c \mid ...) >$ threshold is unreliable so watch out.

"Metric learning"?

Lecture 7: Text categorization
------------------------------

> Receptive field of node $n$ := All nodes in a neural net in a previous layer that influence the node $n$ (nodes connected to $n$).

How to use CNN's for text categorization? Considering hidden nodes start representing more abstract features, you can use some of the hidden layers
(and output layer) as a classification. The different layers can be used for different levels in case of hierarchy categorization. Loss-function is applied
at all these layers.

Lecture 7: Web Information Retrieval
------------------------------------

> Sink := node that doesn't have outgoing links.

> $PR(p)$ := PageRank of page $p$.

> In PageRank, applying ranking to pages can be done in a single run with the matrix formula, but because of the huge size of this matrix, the $PR(p)$-formula
> will usually be applied iteratively. Why iteratively? Because you need the probabilities of the other pages to get a good estimate of the probability of the
> current web page.

Lecture 8: Information Extraction and Search
--------------------------------------------

> Language model := Model with as input a sequence of terms and as output some kind of score.

Lecture 9: Clustering
---------------------

> Distance function := Function expressing distance between two objects.
>
> Proximity function := Function that expresses "distance" between two clusters.

Problem with single pass algorithm: ordering of input dataset influences the final clusters. Yet, still used often in practice.

Difference between centroid and medoid: A medoid is an actual instance from the dataset, while the centroid is some mathematical average.
A centroid is useful if you want some actual representative for a cluster, for example, in image clustering you might want an actual image
as the representative of a cluster.

Actual time complexity of k-means is $O(n \cdot k \cdot r)$ with $k$ number of clusters and $r$ number of iterations, however in practice
$n >> k, n >> r$ and so time complexity can be approximated by $O(n)$.

**Slide 36**: Fitness function defines normalized difference between average similarity with other objects in same cluster and most similar
object in another cluster.

Clustering objectives can be used to improve performance in supervised learning, e.g., in a classification problem trying to increase
intra-cluster/intra-class similarity and decrease inter-cluster/inter-class similarity.

> Triplet loss := Supervised learning where each input is a triplet such that the first two components of the triplet are part of the same
> class and the third component is part of a different class. This can be used to encode clustering objectives
(inter-cluster/intra-cluster similarity constraints).