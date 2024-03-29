### Text Mining
**Recommend a correct word for the misspelled word**<br>
[TextMiningAssignment2](TextMiningAssignment2.ipynb)<br>

**the Sentiment Analysis and distinguishing positive review**<br>
[TextMiningSentimentAnalysis](TextMiningSentimentAnalysis.ipynb)<br>

**Document Similarity and Topic Modeling：類似文書を検索、トピック(主題)抽出モデルの実装**<br>
[TextMiningAssignment4](TextMiningAssignment4.ipynb)<br>

### Applied Data Science Recap
**【recap(総まとめ)】Community Model <最も関連のあるノードを見つける>**<br>
[AppliedSocialNetworkAnalysisAssignment4](AppliedSocialNetworkAnalysisAssignment4.ipynb)<br>

## Hands-on Text Mining (Perform Sentiment Analysis with scikit-learn)

**1. Importing the Data**<br>
```
# (dataの説明を行う)
# Explanation of the data set and the problem oerview
# import essential modules and helper functions from NumPy, Mathplotlib and scikit-learn

"""
データの内容: Movieのrating(スター数, コメント)がpositiveとnegativeが25000件ずつ。
           データの出所: http://ai.stanford.edu/~amaas/data/sentiment
           スター数が7以上をpositive(label=1)、スター数が4以下をnegative(label=0)とする。
           train/testは50/50に分割する
Features: bag of 1-grams(uni-grams) with TF-IDF values:
  Extremely sparse feature matrix (close to 97% are zeros)　<=feature数が膨大で疎なデータな為matrixはほとんど0で構成される
Model: Logistic regression
  p(y=1|x) = σ(w(T)x)
  can handle sparse data <= feature数が膨大な疎なデータに対応できる
  Fast to train          <= 早い (基本的に悪いレビューの中にある単語はそのfeature自体が0に傾かせるようなweightになる)
  (疎なデータとは、nonzero(0以外)のelementが非常に少ないことをいう。LogisticRegressionはこういったデータ形式に強い。)
"""
# Loading the dataset
import pandas as pd
df = pd.read_csv('data/movie_data.csv')
df.head(10) # reviewカラム(コメント), sentiment(0or1)の２つのカラムのdfが表示される(indexは0開始のinteger)

# review全文を見る
df['review'][0]
```
**2. Transforming Documents into Feature Vectors**<br>
```
# (テキストをsparse feature vectorsに変換する)
# Information retrieval
# Represent text data using the bag-of-words model
# Construct the vocabulary of the bag-of-words model
# Transform the sentences into sparse feature vectors

"""
             Bag of words/Bag of N-grams model
       Transforming documents into feature vectors
Below, we will call the fir_transform method on CountVectorizer.
This will construct the vocabulary of the bag-of-words model and transform the
following three sentences into sparse feature vectors.
 1. The sun is shining
 2. The weather is sweet
 3. The sun is shining, the weather is sweet, and one and one is two.
"""
import numpy as np
from sklearn.feature_extraction.text import CountVectorizer

count = CountVectorizer()
docs = np.array(['The sun is shining',
                'The weather is sweet',
                'The sun is shining, the weather is sweet, and one and one is two.'])
# bag of words modelにtransform
bag = count.fit_transform(docs) # これだけの文章でも数秒はかかる

print(count.vocabulary_) # => {'the': 6, 'sun': 4, 'is': 1, 'shining': 3, 'weather': 8, 'sweet': 5, 'and': 0, 'one': 2, 'two': 7}

# feature vectorsをプリントアウト
print(bag.toarray())

# => [[0 1 0 1 1 0 1 0 0]     the(6)はどの文にも出るので上から1 1 2となっている。 
#     [0 1 0 0 0 1 1 0 1]     and(0)は最後の文にだけ２回登場するので上から0 0 2となっている。
#     [2 3 2 1 1 1 2 1 1]]
```
**3. Term Frequency-Inverse Document Frequency**<br>
```
# (非常に頻出のワードを観察する) 
# Observe words that crop up across our corpus of documents.　These words can lead to bad performance because they don't contain useful information.
# Apply scikit-learn's TfIdfTransformer to convert text into a vector of tf-idf values.
# Using this method, downweight these class of words in the feature vector representation.
# (downweight:頻出するものは重要度が下がるのでweightも下がる。tf-idfの理論(inverse document frequency)に沿ったもの)
# Apply the L2-normalization to it.

"""
    式:    df(出現回数)が多いほど idf()の値は下がる↓　tf(term frequency)と掛け合わせてtf-idfを求める。
        tf-idf(t, d) = tf(t,d) × idf(t,d)    where nd is the total number of documents and
        idf(t,d) = log(nd / (1 + df(d,t)))   df(d,t) is the number of documents d that contain the term t.
        1 + は0でないことを保証する為。log()はdfが多いほどpenalizeする為。（どの文章にも良く出るtheなんかはidfが極めて小さくなる。）
"""
from sklearn.feature_extraction.text import TfidfTransformer

# Normalizationは(出力のunit sizeに合わせて)l2にする(vector elementsをsquareしたものをsumしたものが１と等しくなるように。)
# smooth_idfは1,2回しか出ないwordによって0割されるのを防ぐ
tfidf = TfidfTransformer(use_idf=True, norm='l2', smooth_idf=True)

# tf-idfをプリントアウト
np.set_printoptions(precision=2) # npの少数の出力精度を２桁までにする(だからnpを使っているのかな)
#print(tfidf.fit_transform(bag))
print(tfidf.fit_transform(count.fit_transform(docs)).toarray())

# => [[0. 0.43 0. 0.56 0.56 0. 0.43 0. 0.]     特定の文章にだけ出る単語の方がweightが大きくなっている。 
#     [0. 0.43 0. 0. 0. 0.56 0.43 0. 0.56]     最後の文章でis(1)は3回出現するがand(0)(2回出現)の方が大きいので"is"は重要でないと計算から求められた。
#     [0.5 0.45 0.5 0.19 0.19 0.19 0.3 0.25 0.19]]
```
**4. Data Preparation**<br>
```
# (Cleaningをする)
# Cleaning and pre-processing text data is a vital process in data analysis and especially in natural language processing.
# Skip the data set of reviews of irrelevant characters including HTML tags, punctuation and emojis using regular expression.

# 不必要なwordがないか観察する
df.loc[0, 'review'][-50:] #=> 'is seven.<br /><br />Title (Brazil): Not Available' 最後の５０文字

import re
def preprocessor(text):
  text = re.sub('<[^>]*>', '', text) # htmlタグ
  emotions = re.findall('(?::|;|=)(?:-)?(?:\)|\(|D|P)', text) :-) スマイル, :\, :-(sad face, :Dを検知
  text = re.sub('[\W]+', ' ', text.lower()) +\
      ' '.join(emotions).replace('-', '') # Non-Wordを除去した後、スマイルなどの顔文字を文章の最後に移動。 スマイルを:)に統一。
  return text

preprocessor(df.loc[0, 'review'][-50:])
# => 'is seven title brazil not available'

preprocessor('</a>This :) is a :( test :-)!')
# => 'this is a test :) :( :)'

df['review'] = df['review'].apply(preprocessor)
```
**5. Tokenization of Documents**<br>
```
# (トーカナイズ)
# Repurpose the data preprocessing and k-means clustering logic from previous tasks.
# Operate k-means image compression.
# Visualize how the image changes as the number of clusters fed to the k-means algorithm is varid.

from nltk.stem.porter import PorterStemmer
porter = PorterStemmer()

# 文章を半角スペースで区切りwordのリスト化をし、Stemming(ベースの単語に統一)を行う
def tokenizer(text):
    return text.split()

def tokenizer_porter(text):
    return [ porter.stem(word) for word in text.split() ]

tokenizer('runners like running and thus they run')        #=> ['runners', 'like', 'running', 'and', 'thus', 'they', 'run']
tokenizer_porter('runners like running and thus they run') #=> ['runner', 'like', 'run', 'and', 'thu', 'they', 'run']

# stop-wordを実装してtheやandなどの単語を除く
import nltk
nltk.download('stopwords')

from nltk.corpus import stopwords
stop = stopwords.words('english') # 英語以外もある..
[w for w in tokenizer_porter('a running like running and runs a lot')[-10:] if w not in stop]

#=> ['run', 'like', 'run', 'run', 'lot'] andやaはstop wordなので除去される。
```
**6. Transform Text Data into Vectors**<br>
```
# (dfのTF-IDF Vectors化)
from sklearn.feature_extraction.text import TfidfVectorizer
# initialize
tfidf = TfidfVectorizer(strip_accents=None, # preprocessorをすでに下処理している為無効にする
                        lowercase=False,    # preprocessorをすでに下処理している為無効にする
                        preprocessor=None,  # preprocessorをすでに下処理している為無効にする
                        tokenizer=tokenizer_porter,
                        use_idf=True,
                        norm='l2',
                        smooth_idf=True) # 0割防止
y = df.sentiment.values
X = tfidf.fit_transform(df.review)
```

**7. Documents Classification Using Logistic Regression**<br>
```
# (データを分割し、grid searchを行う)
# Split the data into training and test sets of equal size.
# Create a pipeline to build a logistic regression model
# Emply cross-validated grid-search to estimate the best parameters and model.
# Although the time it takes to train logistic regression model is very little,
# estimating the best parameters for our model using GridSearchCV can take hours for some data amount.

from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=1, test_size=0.5,
                                                    shuffle=False) # X: tf-idf value, y: sentiments, 結果を再生産可能にする

# ModelをDisc保存できるライブラリをimport
import pickle
# 公式->Cross-validation estimators are named EstimatorCV and tend to be roughly equivalent to GridSearchCV(Estimator(), ...).
# LogisticRegressionにはチューニングが必要なCパラメータなどがあるが
# それらのチューニングを自動で行いたいのでcross-validationのライブラリを使う
from sklearn.linear_model import LogisticRegressionCV

clf = LogisticRegressionCV(cv=5,                # K-Foldsのkの数
                           scoring='accuracy',　# Measurement
                           random_state=0,
                           n_jobs=-1,           # parallelでCPUをいくつ利用するか(-1:全てのプロセッサーを使う)
                           verbose=3,           # computationのoutputを行う
                           max_iter=300).fit(X_train, y_train)
                           # default:100(100だとCVでは不十分かもしれない(データが多いほど増やした方がいい))
                           
# LogisticRegression自体は高速だが、GridSearchでは時間がかかるので直後にModelをDiscに保存する
saved_model = open('saved_model.sav', 'wb') # bite単位で書き込む
pickle.dump(clf, saved_model)
saved_model.close()

# => [Parallel(n_jobs=-1)]: Usingbackend LokyBackend with 2 concurrent workers.
#    [Parallel(n_jobs=-1)]: Done 5 out of 5 | elapsed: 2.6min finished   <= 2.6分かかっている。(30Mほどのサイズのファイルが出来上がる)
```
**8. Load Saved Model from Disk / Model Accuracy**<br>
```
# (TrainされたModelを使い感情を予測する)
# Take a look at the best parameter settings, cross-validation score and how well
# out model classifiers the sentiments of reviews from the test set.

# Discから保存されたModelを読み込む
filename = 'saved_model.sav'
saved_clf = pickle.load(open(filename, 'rb'))

# score(予測精度=Accuracy)を確認する
saved_clf.score(X_test, y_test)
# => 0.89604 # 感情予測としてはとても良い(ほぼ90%なのでとても良いと言える)精度が出ている。
```

# ミシガン大学応用データサイエンス Text-Mining Summary
### Working With Text
```
text1 = "Ethics are built right into the ideals and objectives of the United Nations "
len(text1) # length of text1

text2 = text1.split(' ')
len(text2) # length of list of words

# 特定のwordのみに絞る
[w for w in text2 if len(w) > 3]
[w for w in text2 if w.endswith('s')]

# set()を使いuniqueな単語を抽出
len(set(text2))
set(text2)
len(set([w.lower() for w in text2]))

# regular expression
import re
[w for w in text2 if re.search('@[A-Za-z0-9_]+', w)] #@から始まるすべての語
```

### Sentiment Analysis
```
import pandas as pd
import numpy as np

df = pd.read_csv('Amazon_Unlocked_Mobile.csv')
df = df.sample(frac=0.1, random_state=10) # スピードアップのためサンプリング

df.dropna(inplace=True)
df = df[df['Rating' != 3]]

# レーティングが4,5は1
df['Positively Rated'] = np.where(df['Rating'] > 3, 1, 0)
df['Positively Rated'].mean()

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(df['Revies'], df['Positively Rated'], random_state=0)
print('X_train first entry:\n\n', X_train.iloc[0])

print('\n\nX_train shape: ', X_train.shape)

# カウントでベクタライズする(多いほど重要)
from sklearn.feature_extraction.text import CountVectorizer

vect = CountVectorizer().fit(X_train)
vect.get_feature_names()[::2000]

len(vect.get_feature_names())

X_train_vectorized = vect.transform(X_train)
X_train_vectorized

from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fix(X_train_vectorized, y_train)

from sklearn.metrics import roc_auc_score
predictions = model.predict(vect.transform(X_test))
print('AUC: ', roc_auc_score(y_test, predictions))

# coefficientsの順番で並び替え
feature_names = np.array(vect.get_feature_names())
sorted_coef_index = model.coef_[0].argsort()
print('Smallest Coefs:\n{}\n'.format(feature_names[sorted_coef_index[:10]]))
print('Largeest Coefs:\n{}'.format(feature_names[sorted_coef_index[:-11:-1]]))

# Tfidf
from sklearn.feature_extraction.text import TfidfVectorizer
vect = TfidfVectorizer(min_df=5).fit(X_train)
len(vect.get_feature_names())

X_train_vectorized = vect.transform(X_train)
model = LogisticRegression()
model.fit(X_train_vectorized, y_train)

# AUCを出す
predictions = model.predict(vect.transform(X_test))
print('AUC: ', roc_auc_score(y_test, predictions))

# tfidfの最小値、最大値
featue_names = np.array(vect.get_feature_names())
sorted_tfidf_index = X_train_vectorized.max(0).toarray()[0].argsort()
print('Smallest tfidf:\n{}\n'.format(feature_names[sorted_tfidf_index[:10]]))
print('Largest tfidf:\n{}'.format(feature_names[sorted_tfidf_index[:-11:-1]]))

# coef_の最大値、最小値
sorted_coef_index = model.coef_[0].argsort()
print('Smallest Coefs:\n{}\n'.format(feature_names[sorted_coef_index[:10]]))
print('Largest Coefs:\n{}'.format(feature_names[sorted_coef_index[:-11:-1]]))

# 実際にレビューのy値を求める
print(model.predict(vect.transform(['not an issue, phone is working', 'an issue, phone is not working'])))
#-> [0 0]

# n-grams
vect = CountVectorizer(min_df=5, ngram_range=(1,2)).fit(X_train)
X_train_vectorized = vect.transform(X_train)
len(vect.get_feature_names())

model = LogisticRegression()
model.fit(X_train_vectorized, y_train)
predictions = model.predict(vect.transform(X_test))
print('AUC: ', roc_auc_score(y_test, predictions))

featue_names = np.array(vect.get_feature_names())
sorted_coef_index = model.coef_[0].argsort()
print('Smallest Coefs:\n{}\n'.format(feature_names[sorted_coef_index[:10]]))
print('Largest Coefs:\n{}'.format(feature_names[sorted_coef_index[:-11:-1]]))

# 実際にレビューのy値を求める
print(model.predict(vect.transform(['not an issue, phone is working', 'an issue, phone is not working'])))
#-> [1 0]
```

### Working with Text in pandas
```
import pandas as pd
time_sentences = ["Monday: The doctor's appointment is at 2:45pm.", 
                  "Tuesday: The dentist's appointment is at 11:30 am.",
                  "Wednesday: At 7:00pm, there is a basketball game!",
                  "Thursday: Be back home by 11:15 pm at the latest.",
                  "Friday: Take the train at 08:10 am, arrive at 09:00am."]
df = pd.DataFrame(time_sentences, columns=['text'])
df

df['text'].str.len() # それぞれの文の長さ
df['text'].str.split().str.len() # それぞれの文の単語数

# データフレームの中で特定の言葉を含むかどうか全行調査
df['text'].str.contains('appointment')
# データフレームの中で数字を何回生じるか全行調査
df['text'].str().count(r'\d')
# データフレームの中で全行で数字を抽出
df['text'].str.findall(r'\d')
df['text'].str.findall(r'(\d?\d):(\d\d)') # 時間:分
# 置換(曜日を全て???や省略形にする)
df['text'].str.replace(r'\w+day\b', '???')
df['text'].str.replace(r'\w+day\b', lambda x: x.groups()[0][:3])

# マッチしたものを抽出し新しいカラム(0,1)を作って格納する
df['text'].str.extract(r'(\d?\d):(\d\d)')
df['text'].str.extractall(r'((\d?\d):(\d\d) ?([ap]m))') # 0:全体のtime, 1:hour, 2:minuts, 3:am/pmをマッチしただけ抽出する

```
### Assignment1 (全く整理されていないテキストから日付を抽出)
```
def date_sorter():
    
    # Your code here
    
    def func_agg_various_ymd_str(df):
#         print(df['all'])
        # 英語表記Month (これが一番日時として判別しやすい)
        if not pd.isnull(df['word_ymd']):
            mConverter = {'Jan': '1', 'Feb': '2', 'Mar': '3', 'Apr': '4', 'May': '5', 'Jun': '6', 'Jul': '7', 'Aug': '8', 'Sep': '9', 'Oct': '10', 'Nov': '11', 'Dec': '12'}
            
            if not pd.isnull(df['wdpre']):                      # 20 Mar 2009; 20 March 2009; 20 Mar. 2009; 20 March, 2009のタイプ
                return  mConverter[str(df['wm'])[0:3]] + '/'+ df['wdpre'] + '/' +df['wy']
            
            elif not pd.isnull(df['wdpost']):                 # Mar 20th, 2009; Mar 21st, 2009; Mar 22nd, 2009, Mar-20-2009; Mar 20, 2009; March 20, 2009; Mar. 20, 2009; Mar 20 2009;のタイプ
                return mConverter[str(df['wm'])[0:3]]  + '/' + str(int(df['wdpost'].replace('.', ''))) + '/' + df['wy'] # "."は整数変換できないので除去する
            
            else:                                                              # Feb 2009; Sep 2009; Oct 2010のタイプ
                return mConverter[str(df['wm'])[0:3]] + '/1/' + df['wy']                
        # ベーシックスタイル１
        elif not pd.isnull(df['basic_ymd_1']):
            if pd.isnull(df['bfdy']):
                return df['bm_1'].replace('-', '/') + df['bd'].replace('-', '/') + '19' + df['btdy'] # btdy: basic two digit yearの略
            else:
                return df['basic_ymd_1'].replace('-', '/') # dd-mm-yyyyをdd/mm/yyyyに変換する
        # ベーシックスタイル2
        elif not pd.isnull(df['basic_ymd_2']):
            return df['bm_2'].replace('-', '/') + '1/' + df['by']                
        # 年のみ
        elif not pd.isnull(df['year_only']) and int(df['year_only']) > 1900:
                return '1/1/'  + df['year_only']
        # 例外
        else:
            print('...Something wrong!...', df)
            return ''

    # 【1】様々な表記を抽出する。
    # 1. ベーシックスタイル。 "Series.str" can be used to access the values of the series as strings and apply several methods to it.
    df1_1 = df.str.extract(r'(?P<basic_ymd_1>(?P<bm_1>\d{1,2}[/-])(?P<bd>\d{1,2}[/-])(?P<btdy>\d{2})(?P<bfdy>\d{2})?)')
    df1_2 = df.str.extract(r'(?P<basic_ymd_2>(?P<bm_2>\d{1,2}[/-])(?P<by>\d{4}))') # dd-ddは年月で無いので含めないように分けた

    # 2. 英語表記Month
    df2  = df.str.extract(r'(?P<word_ymd>(?P<wdpre>\d{1,2})?.?(?P<wm>(?:Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)[a-z]*)(?P<wdpost>\W{0,2}\d{1,2}\w{0,2})?.{1,2}(?P<wy>\d{4}))')

    # 3. 年のみ
    df3 = df.str.extract(r'(?P<year_only>\d{4})')

    # 【2】様々な表記を１つのカラムに集約する
    df_extracted = pd.concat([df1_1, df1_2, df2, df3], axis=1)
    df_extracted['all'] = df.values
    df_extracted['answer_ymd'] = df_extracted.agg(func_agg_various_ymd_str, axis='columns')

    # デバッグ用に調整
#     print(df_extracted.columns)
    df_extracted = df_extracted[['answer_ymd', 'word_ymd', 'year_only']]
        
    # 【3】年月日でソート
    df_extracted['ymd_datetype'] = pd.to_datetime(df_extracted['answer_ymd'], format='%m/%d/%Y')
    df_sorted = df_extracted.sort_values(by='ymd_datetype')
    print(df_sorted)
    print(df_sorted.index)

    return df_sorted.index # Your answer here

df_sorted = date_sorter()
```
### Assignment2 NLTK
```
# 省略 (samples/michigan/TextMiningAssignment2.ipynb 参照)
```
### Assignment3 実践(CountVectorizer or Tfidf and Naive Bayes or SVM or LogisticRegression)
```
function1
function2 (CountVectorizerを使う)
function3 (Naive Bayesを使う)
function4 (TfidfVectorizerを使う)
function5
function6
function7 (SVMを使う)
function8
function9 (LogisticRegressionを使う)
function10
function11

# 省略 (samples/michigan/TextMiningAssignment3.ipynb 参照)
```
### Assignment4 Document Similarity(NLTKを使う)
```
# 省略 (samples/michigan/TextMiningAssignment4.ipynb 参照)
```
