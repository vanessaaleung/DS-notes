# Text Mining in Python
- Regex
- Non-ASCII Characters
- Spelling Recommender
- [Naive Bayes Classifiers](#naive-bayes-classifiers)
- [Naive Bayes Variants](#naive-bayes-variants)
- [Support Vector Machines](#support-vector-machines)
- [Text Classifiers in Python](#text-classifiers-in-python)
- [Topic Modeling](#topic-modeling)

## Regex
### Character matches
  - `.`: wildcard, matches a single character
  - `^`: start of a string
  - `$`: end of a string
  - `[]`: matches one of the set of characters within []
  - `[^abc]`: matches a character that is not a, b, or, c
  - `a|b`: matches either a or b
  - `()`: scoping for operators
  - `\`: escape characters (\t, \n, \b)
  
### Character symbols
- `\b`: matches word boundary
- `\d`: any digit
- `\D`: any non-digit
- `\s`: any whitespaces
- `\S`: any non-whitespace
- `w`: alphanumeric
- `\W`: non-alphanumeric

### Repetitions
- `*`: matches 0+ times
- `+`: matches 1+ times
- `?`: matches 0 or 1 times
- `{n}`: exactly n times
- `{n,}: at least n repetitions
- `{,n}: at most n repetitions
- `{m, n}`: at least m and at most n

### Regex in Python
```python
import re
[w for w in text if re.search('@\w+', w)]
re.findall(r'[aeiou]', text)
```

## Non-ASCII Characters
### ASCII
_American Standard Code for Information Interchange_
- 7-bit long, 128 valid codes
- Range: 0x00 - 0x7F
- Includes alphabets(upper & lower), digits, punctuations, control characters, common symbols

### Unicode
_Industry standard for encoding and representing text_
- UTF-8: 1-4 bytes
  - Unicode Transformational Format - 8-bits
- UTF-16: one or two 16-bit code units
- UTF-32: one 32-bit code unit

## Basic NLP tasks with NLTK
- Frequency of words
```python
dist = FreqDist(text)
```
- Stemming
- Lemmatization: stemming, but resulting stems are all valid words
- Tokenization: built-in tokenizer
```python
nltk.word_tokenize(text)
```
- Sentence Splitting
```python
nltk.sent_tokenize(text)
```
- Part-of-speech (POS) Tagging
```python
nltk.help.upenn_tagset('MD')
nltk.pos_tag(text)
```
- Parsing Sentence Structure
```python
grammar = nltk.CFG.fromstring("""
  S -> NP VP
  VP -> V NP
  NP -> 'Alice' | 'Bob'
"""
grammar = nltk.data.load('grammar.cfg')
)
```
```python
parser = nltk.ChartParser(grammar)
trees = parser.parse_all(text)
for tree in trees:
  print(tree)
```
- Parse Tree Collection
```python
from nltk.corpus import treebank
text = treebank.parsed_sent('wsj.mrg')[0]
```

## Spelling Recommender
- Jaccard distance: comparing set-similarity
- edit distance: the number of characters that need to be substituted, inserted, or deleted, to transform s1 into s2

## Naive Bayes Classifiers
_Probabilistic model_
- Assumption: (why called naive) features are assumed to be independent of each other, given the class label
- Provides baseline for text classification problems
- Update the likelihood of the class given new information
- Parameters:
  - Prior probability: P(y) for all y in Y, e.g. P(Zoology)
  - Likelihood: probability of seeing a particular feature in y, e.g. P(Python|Zoology)
    - Count how many times feature xi appears in class y
- Posterior probability: P(Entertainment|Python)
- <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(x|y)=\frac{P(x)\times&space;P(y|x)}{P(y)}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;P(x|y)=\frac{P(x)\times&space;P(y|x)}{P(y)}" title="P(x|y)=\frac{P(x)\times P(y|x)}{P(y)}" /></a>
- Smoothing: if feature xi never occurs in class y, P(xi|y) = 0
  - Laplace/Additive smoothing: add a dummy count, e.g. add count of one to every word in every class
  - <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;P(x_i|y)=(k&plus;1)/(p&plus;n),&space;n=&space;\text{number&space;of&space;features}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;P(x_i|y)=(k&plus;1)/(p&plus;n),&space;n=&space;\text{number&space;of&space;features}" title="P(x_i|y)=(k+1)/(p+n), n= \text{number of features}" /></a>
  
## Naive Bayes Variants
- Multinomial Naive Bayes
  - Data follows multinomial distribution
  - Number of times a word occurs is important
  - Each feature value is a count
  - Very common
- Bernoulli Naive Bayes
  - Data follows a multivariate Bernoulli distribution
  - Each feature is binary (word present/absent), count doesn't matter

## Support Vector Machines
_Linear classifiers sthat find a hyperplane to separate two classes of data_
- Pros
  - Tend to be most accurate in high-dimensional data
- Cons
  - Handles only numeric features, need normalization
  - Hyperplane is hard to interpret
- Decision Boundaries
  - Linear: easy to find/evaluate, more generalizable
    - perceptron
    - linear discriminative analysis
    - linear least squares
  - A resonable boundary: maximize margin, find the thickest band that seperates the positive and negative points
- SVM is a Maximum-margin Classifiers, work only for binary classification problems
- Multi-class
  - One vs Rest, n classfiers learn n class
  - One vs One, C(n, 2) classifiers
- Parameters
  - Regularization: how much importance should give individual data points
    - More regularization: more tolerant to errors on individual data points, more generalized
  - Regularization parameter c: the larger c, less regularization
  - **Linear kernels work best for text data**
  - multi_class: ovr (one-vs-rest)
  - class_weight: different classes can get different weights

## Text Classifiers in Python
- NaiveBayesClassifier
```python
from sklearn import naive_bayes
clfrNB = naive_bates.MultinomialNB()
clfrNB.fit(train_data, train_labels)
predicted_labels = clfrNB.predict(test_data)
metrics.f1_score(test_labels, predicted_labels, average='micro')
```

- SVM classifier
```python
from sklearn import svm
clfrSVM = svm.SVC(kernel='linear', C=0.1)
clfrSVM.fit(train_data, train_labels)
predicted_labels = clfrSVM.predicT(test_data)
```

- Model Selection
  - Hold out data
  ```python
  from sklearn import model_selection
  X_train, X_test, y_train, y_test = model_selection.train_test_split(train_data, train_labels, test_size=0.333, random_state=0)
  ```
  - Cross-validation: average the result
  ```python
  predicted_labels = model_selection.cross_val_predict(clfrSVM, train_data, train_labels, cv=5)
  ```

- TFIDF
  - `min_df`: remove words appear in fewer than 5 documents
  ```python
  from sklearn.feature_extraction.text import TfidfVectorizer
  vect = TfidfVectorizer(min_df=5).fit(X_train)
  X_train_vectorized = vect.transform(X_train)
  model = LogisticRegression()
  model.fit(X_train_vectorized, y_train)
  predictions = model.predict(vect.transform(X_test))
  ```
  
- n-grams: add word context
  - `ngram_range=(1,2)`: will add single and also 2-gram words
  ```python
  vect = CountVectorizer(min_df=5, ngram_range=(1,2)).fit(X_train)
  X_train_vectorized = vect.transform(X_train)
  ```

## Topic Modeling
### Semantic Similarity
- Wordnet
  - semantic dictionary of English words, interlinked by semantic relations
  - includes linguistic information: part of speech, word senses, synonyms, etc.
  - organize information in a hhierarchy
  - `deer.n.01`: noun, the first meaning of that
  ```python
  import nltk
  from nltk.corpus import wordnet as wn
  deer = wn.synset('deer.n.01')
  ```
- Path similarity: find the shortest path between two concepts
  - pathlen(c1, c2)  = 1 + number of  edges in the shortest path between two nodes
  - range from 0-1
  - simpath(c1, c2) = 1 \ pathlen(c1, c2)
  - wordsim(w1, w2) = max  sim(c1, c2)
  ```python
  deer.path_similarity(horse)
  ```
- Lowest Common Subsumer (LCS): find the closest ancestor to both concepts
- Lin SimilaritY: based on the information contained in the LCS of the two conecpts
  - <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;LinSim(u,&space;v)=2&space;\times&space;log&space;P(LCS(u,&space;v))&space;/&space;(log&space;P(u)&plus;log&space;P(v))" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;LinSim(u,&space;v)=2&space;\times&space;log&space;P(LCS(u,&space;v))&space;/&space;(log&space;P(u)&plus;log&space;P(v))" title="LinSim(u, v)=2 \times log P(LCS(u, v)) / (log P(u)+log P(v))" /></a>
  - P(u) is given by the information content learnt over a large corpus
  ```python
  from nltk.corpus import wordnet_ic
  brown_ic = wordnet_ic.ic('ic-brown.dat')
  deer.lin_simliary(horse, brown_ic)
  ```
- Collocations and Distributional similarity
  - two words that frequently appears in similiar contexts are more likely to be sematically related
  - distributional similarity: context, words before, after, within a small window
  - **Pointwise Mutual Information**: <a href="https://www.codecogs.com/eqnedit.php?latex=\inline&space;PMI(w,&space;c)=log[P(w,c)/P(w)P(c)]" target="_blank"><img src="https://latex.codecogs.com/svg.latex?\inline&space;PMI(w,&space;c)=log[P(w,c)/P(w)P(c)]" title="PMI(w, c)=log[P(w,c)/P(w)P(c)]" /></a>
  ```python
  from nltk.collocations import *
  bigram_measures = nltk.collocations.BigramAssocMeasures()
  finder = BigramCollocationFinder.from_words(text)
  finder.nbest(bigram_measures.pmi, 10) # get the top 10 pairs
  finder.find_freq_filter(10)
  ```
  
### Topic Modeling
_Text clustering problem_
- topics are represented as a word distribution, each word has a probability of occuring in the topic
- a document is assumed with a mixture of topics
- Known: # of topics, text collection
- Not known: the actual topic, topic distribution
- Approaches
  - Probabilistic Latent Semantic Analysis (PLSA)
  - Latent Dirichlet Allocation (LDA)
  
### Generative Models and LDA
- LDA
  - Choose length of document d
  - Choose a mixture of topics for d
  - Use a topic's multinomial distribution to output words to fill that topic's quota
- Preprocessing text: tokenize, normalize (lowercase), stop word removal, stemming
- Convert tokenized documents to a document-term matrix
```python
doc_set: set of pre-processed text documents
import gensim
from gensim import corpora, models
dictionary = corpora.Dictionary(doc_set) # map id-word
corpus = [dictionary.doc2bow(doc) for doc in doc_set]  # create document-term matrix
ldamodel = gensim.models.ldamodel.LdaModel(corpus, num_topics=4, id2word=dictionary, passes=50)
print(ldamodel.print_topics(num_topics=4, num_words=5))
```
- Can be used for feature extraction to filter features coming from a specific topic




