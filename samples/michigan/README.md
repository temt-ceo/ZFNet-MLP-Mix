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
# (dataの説明を行う)
# Explanation of the data set and the problem oerview
# import essential modules and helper functions from NumPy, Mathplotlib and scikit-learn

"""
データの内容: Movieのrating(スター数, コメント)がpositiveとnegativeが25000件ずつ。
           スター数が7以上をpositive(label=1)、スター数が4以下をnegative(label=0)とする。
           train/testは50/50に分割する
Features: bag of 1-grams with TF-IDF values:
  Extremely sparse feature matrix (close to 97% are zeros)
Model: Logistic regression
  p(y=1|x) = σ(w(T)x)
  can handle sparse data
  Fast to train
"""
# Loading the dataset
import pandas as pd
df = pd.read_csv('data/movie_data.csv')
df.head(10) # reviewカラム(コメント), sentiment(0or1)の２つのカラムのdfが表示される(indexは0開始のinteger)

# review全文を見る
df['review'][0]
```
**Transforming Documents into Feature Vectors**<br>
```
# (テキストをsparse feature vectorsに変換する)
# Information retrieval
# Represent text data using the bag-of-words model
# Construct the vocabulary of the bag-of-words model
# Transform the sentences into sparse feature vectors

```
**Term Frequency-Inverse Document Frequency**<br>
```
# (非常に頻出のワードを観察する) 
# Observe words that crop up across our corpus of documents.
# These words can lead to bad performance because they don't contain useful information.
# Implement a useful statistical technique, Term frequency-inverse document frequency(tf-idf).
# (The tf-idf is the product of the term frequency and the inverse document frequency)
# Using this method, downweight these class of words in the feature vector representation.

```
**Calculate TF-IDF of the Term 'Is'**<br>
```
# (tf-idf値を求める)
# Manually calculate the tf-idf.
# Apply scikit-learn's TfIdfTransformer to convert text into a vector of tf-idf values.
# Apply the L2-normalization to it.

```
**Data Preparation**<br>
```
# (Cleaningをする)
# Cleaning and pre-processing text data is a vital process in data analysis and especially in natural language processing.
# Skip the data set of reviews of irrelevant characters including HTML tags, punctuation and emojis using regular expression.


```
**Tokenization of Documents**<br>
```
# (k-meansのcluster数を変えたらどのようにimageが変わるかを可視化する)
# Repurpose the data preprocessing and k-means clustering logic from previous tasks.
# Operate k-means image compression.
# Visualize how the image changes as the number of clusters fed to the k-means algorithm is varid.


```
**Documents Classification Using Logistic Regression**<br>
```
# (データを分割し、grid searchを行う)
# Split the data into training and test sets of equal size.
# Create a pipeline to build a logistic regression model
# Emply cross-validated grid-search to estimate the best parameters and model.

```
**Load Saved Model from Disk**<br>
```
# (GridSearchは時間がかかるのでpre-trainしたmodelを読み込む)
# Although the time it takes to train logistic regression model is very little,
# estimating the best parameters for our model using GridSearchCV can take hours for some data amount.

```
**Model Accuracy**<br>
```
# (テストdatasetを使い感情の予測)
# Take a look at the best parameter settings, cross-validation score and how well
# out model classifiers the sentiments of reviews from the test set.

```


