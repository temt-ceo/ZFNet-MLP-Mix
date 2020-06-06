### Text Mining
**Recommend a correct word for the misspelled word**<br>
[TextMiningAssignment2](TextMiningAssignment2.ipynb)<br>

**the Sentiment Analysis and distinguishing positive review**<br>
[TextMiningSentimentAnalysis](TextMiningSentimentAnalysis.ipynb)<br>

**Document Similarity and Topic Modeling**<br>
[TextMiningAssignment4](TextMiningAssignment4.ipynb)<br>

### Applied Data Science Recap
**【recap(総まとめ)】Community Model <最も関連のあるノードを見つける>**<br>
[AppliedSocialNetworkAnalysisAssignment4](AppliedSocialNetworkAnalysisAssignment4.ipynb)<br>

## Text Mining Hands-on
### Perform Sentiment Analysis with scikit-learn
**Importing the Data**<br>
```
# Explanation of the data set and the problem oerview
# import essential modules and helper functions from NumPy, Mathplotlib and scikit-learn

```
**Transforming Documents into Feature Vectors**<br>
```
# Information retrieval
# Represent text data using the bag-of-words model
# Construct the vocabulary of the bag-of-words model
# Transform the sentences into sparse feature vectors

```
**Term Frequency-Inverse Document Frequency**<br>
```
# Observe words that crop up across our corpus of documents.(非常に頻出のワードを観察する) 
# These words can lead to bad performance because they don't contain useful information.
# Implement a useful statistical technique, Term frequency-inverse document frequency(tf-idf).
# (The tf-idf is the product of the term frequency and the inverse document frequency)
# Using this method, downweight these class of words in the feature vector representation.

```
**Calculate TF-IDF of the Term 'Is'**<br>
```
# Manually calculate the tf-idf.
# Apply scikit-learn's TfIdfTransformer to convert text into a vector of tf-idf values.
# Apply the L2-normalization to it.

```
**Data Preparation**<br>
```
# Cleaning and pre-processing text data is a vital process in data analysis and especially in natural language processing.
# Skip the data set of reviews of irrelevant characters including HTML tags, punctuation and emojis using regular expression.


```
**Tokenization of Documents**<br>
```
# Repurpose the data preprocessing and k-means clustering logic from previous tasks.
# Operate k-means image compression.
# Visualize how the image changes as the number of clusters fed to the k-means algorithm is varid.
# (k-meansのcluster数を変えたらどのようにimageが変わるかを可視化する)

```
**Documents Classification Using Logistic Regression**<br>
```
#
#
#

```
**Load Saved Model from Disk**<br>
```
#
#
#

```
**Model Accuracy**<br>
```
#
#
#

```


