# ミシガン大学応用データサイエンス Summary

## Pandas Visualization

```
importpandas as pd
import numpy as np
import matplotlib.pyplot as plt

# Using matplotlib in jupyter notebooks
%matplotlib notebook

# 利用可能なスタイル
plt.style.available

# 'seaborn-colorblind'のスタイルを使う
plt.style.use('seaborn-colorblind')

np.random.seed(1230)
# 乱数でデータセットを作る. X軸は１年間.
df = pd.DataFrame(['A': np.random.randn(365).custom(0),
                   'B': np.random.randn(365).custom(0) + 20,
                   'C': np.random.randn(365).custom(0) - 20],
                   index=pd.date_range('1/1/2017', periods=365))
# 線グラフ
df.plot(); # add a semi-colon to the end of the plotting call to suppress unwanted output

# 散布図(X軸: A, Y軸: B)
df.plot('A', 'B', kind='scatter');

# 散布図(X軸: A, Y軸: B, 色,大きさ: C)
df.plot.scatter('A', 'C', c='B', s=df['B'], colormap='viridis')

# X軸とY軸のアスペクトをそろえる
ax = df.plot.scatter('A', 'C', c='B', s=df['B'], colormap='viridis')
ax.set_aspect('equal')

# ヒストグラム(Y軸: Frequency)
df.plot.hist(alpha=0.7)
df.plot.kde() # Kernel density estimation plots
```

### pandas.tools.plotting
```
# ハナショウブ
iris = pd.read_csv('iris.csv')

# それぞれのデータ間の相関をマトリックスで確認
pd.tools.plotting.scatter_matrix(iris)

# 名称ごとに線グラフ色分け(X軸: 名称以外カラム, Y軸: 値(全てのカラムが同じようなデータ幅である必要がある))
plt.figure()
pd.tools.plotting.parallel_coordinates(iris, 'Name');
```

### Seaborn
```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

%matplotlib notebook
np.random(1234)

# Series
v1 = pd.Series(np.random.normal(0,10,1000), name='v1')
v2 = pd.Series(2 * v1 + np.random.normal(60,15,1000), name='v2')

# ちょっと綺麗なヒストグラム
plt.figure()
plt.hist(v1, alpha=0.7, bins=np.arrange(-50, 150, 5), label='v1');
plt.hist(v2, alpha=0.7, bins=np.arrange(-50, 150, 5), label='v2');
plt.legend();

# 積み上げ棒グラフ + density
plt.figure()
plt.hist([v1, v2], histtype='barstacked', normal=True);
v3 = np.concatenate((v1, v2)) # Seriesを合わせる
sns.kdeplot(v3)

# グラフの色を変更
plt.figure()
sns.distplot(v3, hist_kws={'color': 'Teal'}, kde={'colot': 'Navy'});

# (オススメ)散布図(X軸: v1, Y軸: v2)とヒストグラム(頻度)の組み合わせ
sns.jointplot(v1, v2, alpha=0.4); # データが重なるので少し透明に

# 値間隔をそろえる
grid = sns.jointplot(v1, v2, alpha=0.4)
grid.ax_joint.set_aspect('equal')
```

### Introduction Assignment 2 <データ・クリーニング>
```
import pandas as pd

# CSVの１行目を読み飛ばす（csvはhttps://en.wikipedia.org/wiki/All-time_Olympic_Games_medal_tableにある）
# 2行目をcolumns名として使用する
df = pd.read_csv('olympics.csv', index_col=0, skiprows=1) # 1列名はindex(世界の国名)

# columnsのカラム名を変更する 
for col in df.columns:
    if col[:2] == '01':
        df.rename(columns={col: 'Gold'+col[4:]}, inplace=True)
    if col[:2] == '02':
        df.rename(columns={col: 'Silver'+col[4:]}, inplace=True)
    if col[:2] == '03':
        df.rename(columns={col: 'Bronze'+col[4:]}, inplace=True)
    if col[:1] == '№':
        df.rename(columns={col: '#'+col[1:]}, inplace=True)

# 1列目のindexを' ('でsplit
names_ids = df.index.str.split('\s\(')

# index名を変更する
df.index = names_ids.str[0]

# DataFrameにIDカラムを新設
df['ID'] = names_ids.str[1].str[:3]

# 必要のないカラムを除去
df = df.drop('Totals')
df

# function1 データフレームの1行目をSeriesで返す
return df.iloc[0]

# function2 データフレームの目的の行の1行目1列目をStringで返す
return df.loc[df['Gold'] == df['Gold'].max()].to_records()[0][0]

# function3 カラム間の開き(差)が最も大きいものを返す
df['Gold-diff'] = df['Gold'] - df['Gold.1']
return df.loc[df['Gold-diff'] == df['Gold-diff'].max()].to_records()[0][0]

# function4 カラム間の開き(差)が相対的に最も大きいものを返す
_df = df[df['Gold'] > 0]
_df = df[df['Gold.1'] > 0] # 'Gold.2はTotalなのでGoldとGold.1のどちらかが1以上であれば0は無い'
_df['Gold-diff'] = (_df['Gold'] - _df['Gold.1']) / _df['Gold.2']
return df.loc[df['Gold-diff'] == _df['Gold-diff'].max()].to_records()[0][0]

# function5 'Points' Seriesを新規で作り返す
df['Points'] = df['Gold.2'] * 3 + df['Silver.2'] * 2 + df['Bronze.2'] * 1
return df['Points']

# function6~8 は複雑なのでsamples/michigan/IntroductionAssignment2.ipynb 参照
```

### Introduction Assignment 3 <Pandasを使いこなす>
```
# function1 元のデータフレームをクリーニングして新しいデータフレームを返す

# 1,2列目は不要、残りのカラムの名称を以下に変更する
energy = pd.read_excel('Energy Indicators.xls', skiplows=17, skip_footer=38,
                       names=['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable'],
                       parse_cols='C:F')
GDP = pr.read_csv('world_bank.csv', skiprows=4)

# 国名をリネーム
def convert_country_name(val):
    # (長いので)途中略
    return val

energy['Country'] = energy['Country'].apply(convert_country_name)
# カラム名をそろえる 
GDP.rename(columns={'Country Name': 'Country'}, inplace=True 
# (途中略)
df.merge(energy, GDP, how='inner', left_on='Country', right_on='Country')
# 必要なカラムに絞る
df = df[['Country', 'Rank', ' .... ', '2015']]
df = df.sort('Rank')
df = df[df['Rank'] <= 15]
df.set_index(['Country'], inplace=True)
return df

# function2
# function3
# function4
# function5
# function6
#     :
# function12 は複雑なのでsamples/michigan/IntroductionAssignment3.ipynb 参照
```

### Introduction Assignment 4 <仮説のテスト(ttest)>
```
import pandas as pd
import numpy as np
from scipy.stats import ttest_ind
import re

wikipedia_univ_homes = pd.read_csv('university_towns.txt', header=None, names=['city'], sep='\n')
citis = list(wikipedia_univ_homes['city'])
state = ''
df_list = []
for data in citis:
    if '[edit]' in data:
        state = data.replace('[edit]', '')
    else:
        town = re.sub('\s?\(.*$', '', data)
        df_list.append([state, town])
df = pd.DataFrame(df_list, columns=['State', 'RegionName'])
df
# (途中略)

# ハウジングデータをquarterごとに変換し、平均をデータとするDataframeを作成
housing_df = pd.read_csv('City_Zhvi_AllHomes.csv')
states = {'OH': 'Ohio', 'KY': 'Kentucky', 'AS': ' .... ', 'VA': 'Verginia'}
housing_df['State'] = housing_df['State'].apply(lambda a: states[a])

# 第1カラムをstate, 第2カラムをRegionNameという名のindexにする
housing_df.set_index(['State', 'RegionName'], inplace=True)

# 不要なカラムを全て取る(dropは取り除いたdfを返し、popは取り除いたカラムを返す)
housing_df.pop('RegionID')
# (途中略)

aggregated_df = housing_df.groupby(pd.PeriodIndex.columns, freq='Q'), axis=1).mean()
targets = []
for quarter in aggregated_df.columns:
    if quarter.startswith('2'): #2000年以降
        targets.append(quarter)
aggregated_df = aggregated_df[targets]

# OhioのAkronとDayton地域の2010q3と2015q2を表示する
aggregated_df.loc[[('Ohio','Akron), ('Ohio', 'Dayton')]].loc[:, ['2010q3', '2015q2']]

# ttest
from scipy.stats import ttest_ind
# (途中略 以下初見のメソッドがあるがsamples/michigan/IntroductionAssignment4.ipynb　を参照)
st_column = get_recession_start()
bt_column = get_recession_bottom()

# リセッション前のクォーターを取得
before_st_column = aggregated_df.columns[aggregated_df.columns.get_loc(st_column) - 1]

# pandas.divの方が / よりメモリが軽くなる
housing_df = aggregated_df[before_st_column].div(aggregated_df[bt_column])

univ_towns_list = df.values.to_list()
def is_university_town(row):
    if list(row.name) in univ_towns_list:
        return 1
    else:
        return 0
housing_df['has_univ'] = housing_df.apply(is_university_town, axis=1)
housing_df_1 = housing_df[housing_df['has_univ'] == 1][['price_ratio']]
housing_df_0 = housing_df[housing_df['has_univ'] == 0][['price_ratio']]

p = ttest(housing_df_0['price_ratio'], housing_df_1['price_ratio'], nan_policy='omit').pvalue
return p < 0.01
```

### Plotting Assignment 1 <実践>
```
# samples/michigan/PlottingChartingAssignment2.ipynb 参照
```

### Plotting Assignment 2 <Custom Visualization>
```
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
  
np.random.seed(1234)
df = pd.DataFrame([np.random.normal(32000, 200000, 3650),
                  np.random.normal(43000, 100000, 3650),
                  np.random.normal(43500, 140000, 3650),
                  np.random.normal(48000, 70000, 3650)],
                  index=[1992,1993,1994,1995])
df

# データの標準偏差などの統計情報を取得
df.describe()

# 3650のカラムを全て合計して統計を求める(表形式)
df.mean(axis=1)
df.std(axis=1)

# 3650のカラムを全て合計して統計を求める(indexとvalueを分ける)
values = df.mean(axis=1)
values.index.get_values()
values.values

# standard error = std sample/ sqrt(number of samples)
std_values = df.std(axis=1)
standard_error = std_values  / (len(std_values) ** 0.5)

#(長いので)残り省略 samples/michigan/PlottingChartingAssignment3.ipynb 参照
```

### Plotting Assignment 3 <実践>
```
# samples/michigan/PlottingChartingAssignment4.ipynb 又は
# https://www.kaggle.com/takashitahara/joshi-japanese-postposition-analysis　参照
# 一部抜粋
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# kaggleにアップロードしたデータを取得する
# import od
# for dirname, _, filenames in os.walk('../input'):
#     for filename in filenames:
#         print(os.path.join(dirname, filename))
jp_df = pd.read_csv('./input/japanesewordsfrequency/japanese_lemmas.csv')
en_df = pd.read_csv('./input/englishwordsfrequency/unigram_freq.csv')

# 使用頻度TOP100だけ選ぶ
jp_df = jp_df[0:100]
en_df = en_df[0:100]

# これはrename使った方がメモリ的にいいな
#  jp_df.rename(columns={'lemma': 'word'}, inplace=True)
jp_df['word'] = jp_df['lemma']
jp_df['frequency'] = jp_df['frequency'].apply(lambda x: int(x))
jp_df = jp_df[['word', 'frequency', 'rank']]

# グラフに表示可能なようにアスペクトをそろえる
en_df['frequency'] = (en_df['count'] * (jp_df.iloc[0]['frequency'] / en_df.iloc[0]['count'])).apply(lambda x: int(x))

# en_dfにranカラムを作る
en_df['rank'] = None
en_df = en_df[['word', 'frequency', 'rank']]

jp_df['word length'] = jp_df['word'].apply(lambda x: len(x))
en_df['word length'] = en_df['word'].apply(lambda x: len(str(x)))

# 途中省略
prepositions = pd.readcsv('./input/prepositions/prepositions-in-english.csv')
for idx, row in en_df.iterrows():
    en_df.iloc[idx, en_df.columns.get_loc('rank')] = idx + 1
    for preposition in prepositions['Word']:
        if row['word'] == preposition:
            en_df.iloc[idx, en_df.columns.get_loc('Preposition or Joshi')] = 'Yes'

df = pd.concat([jp_df, en_df])
g = sns.factorplot(x='Word Length', y='frequency', data=df, hue='Preposition or Joshi', kind='swarm', col='Language')
g.set_axis_labels('Word Length', 'Words Frequency').set_titles('{col_name} {col_var} Top 100 Words')
plt.show()
```

### ClassificationとKNN(K nearest neighbors)
```
%matplotlib notebook
import numpy as np
import matplotlib.pyplot as plt
import pandas as pd
from sklearn.model_selection import train_test_split

fruits = pd.read_table('readonly/fruit_data_with_colors.txt')

# ユニークなIDとフルーツ名を取得しmapを作成する
lookup_fruit_name = dict(zip(fruits.fruit_label.unique(), fruits.fruit_name.unique()))
lookup_fruit_name

# 検証
from matplotlib import cm
X = fruits[['height', 'width', 'mass', 'color_score']]
y = fruits['fruit_label']
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)

# gnuplotと呼ばれるColor Mapを使用する
cmap = cm.get_cmap('gnuplot')
scatter = pd.scatter_matrix(X_train, c=y_train, marker='o', s=40, hist_kwds={'bins': 15}, figsize=(9,9), cmap=cmap)

# 3Dの散布図
from mpl_toolkits.mplot3d import Axes3D

fig = plt.figure()
ax = fig.add_subplot(111, projection='3d')

# widthをX軸, heightをy軸, color_scoreをz軸にし、yを色別の〇印でプロット
ax.scatter(X_train['width'], X_train['height'], X_train['color_score'], c=y_train, marker='o', s=100)
ax.set_xlabel('width')
ax.set_ylabel('height')
ax.set_zlabel('color_scale')
plt.show()

# 省略(KNNについてはsamples/michigan/textbooks/AppliedMachineLearningTextbook1.ipynb　参照
```

### Supervised Part1
```
# 途中省略 (前提条件はsamples/michigan/textbooks/AppliedMachineLearningTextbook2.ipynb　参照)
from sklearn.neighbors import KNeighborsRegressor
X_train, X_test, y_train, y_test = train_test_split(X_R1, y_R1, random_state=0)
knnreg = KNeighborsRegressor(n_neighbors=5).fit(X_train, y_train)

print(knnreg.predict(X_test))
print('R-squared test score: {:.3f}'.format(knnreg.score(X_test, y_test)))

# 横にグラフを並べてプロットする
fig, subaxes = plt.subplots(1, 2, figsize=(8, 4))

# 等間隔の数値のsetを用意
X_predict_input = np.linspace(-3, 3, 50).reshape(-1, 1)
X_train, X_test, y_train, y_test = train_test_split(X_R1[0::5], y_R1[0::5], random_state=0)

# KNN regression(K=1とK=3を比較)
for thisaxis, K in zip(subaxes, [1, 3]):
    knnreg = KNeighborsRegressor(n_neighbors=K).fit(X_train, y_train)
    y_predict_output = knnreg.predict(X_predict_input)
    thisaxis.set_xlim([-2.5, 0.75])
    # 散布図に結果をプロット
    thisaxis.plot(X_predict_input, y_predict_input, '^', markersize=10,
                  label='Predicted', alpha=0.8)
    thisaxis.plot(X_train, y_train, 'o', label='True Value', alpha=0.8)
    thisaxis.set_xlabel('Input feature')
    thisaxis.set_ylabel('Target value')
    thisaxis.set_title('KNN regression (K={})'.format(K))
    thisaxis.legend()
# グラフ出力
plt.tight_layout()

# 途中省略(KNNは散布図で予測できる（単純）ものに向く。結果も視覚化しやすい) samples/michigan/textbooks/AppliedMachineLearningTextbook2.ipynb 参照

# Linear regression
from sklearn.linear_model import LinearRegression

X_train, X_test, y_train, y_test = train_test_split(X_R1, y_R1, random_state=0)
linreg = LinearRegression().fit(X_train, y_train)

print('linear model coeff (w) : {}'.format(linreg.coef_))
print('linear model intercept (b) : {:.3f}'.format(linreg.intercept_))
print('R-squared score (training) : {:.3f}'.format(linreg.score(X_train, y_train)))
print('R-squared score (test) : {:.3f}'.format(linreg.score(X_test, y_test)))

# 途中省略

# Ridge regression(多重回帰曲線の弊害をペナルティにより誤差を軽減するテクニック)
from sklearn.linear_model import Ridge

X_train, X_test, y_train, y_test = train_test_split(X_crime, y_crime, random_state=0)
linridge = Ridge(alpha=20.0).fit(X_train, y_train)

print('ridge regression linear model coeff (w) : {}'.format(linridge.coef_))
print('ridge regression linear model intercept (b) : {:.3f}'.format(linridge.intercept_))
print('R-squared score (training) : {:.3f}'.format(linridge.score(X_train, y_train)))
print('R-squared score (test) : {:.3f}'.format(linridge.score(X_test, y_test)))
print('Number of non-zero features: {}'.format(np.sum(linridge.coef_ != 0)))

# Ridge regression with feature normalization 
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()

from sklearn.linear_model import Ridge
X_train, X_test, y_train, y_test = train_test_split(X_crime, y_crime, random_state=0)

X_tain_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

linridge = Ridge(alpha=20.0).fit(X_train_scaled, y_train)

print('ridge regression linear model coeff (w) : {}'.format(linridge.coef_))
print('ridge regression linear model intercept (b) : {:.3f}'.format(linridge.intercept_))
print('R-squared score (training) : {:.3f}'.format(linridge.score(X_train_scaled, y_train)))
print('R-squared score (test) : {:.3f}'.format(linridge.score(X_test_scaled, y_test)))
print('Number of non-zero features: {}'.format(np.sum(linridge.coef_ != 0)))

# Ridge regression with regularization parameter: alpha
print('Ridge regression: effect of alpha regularization parameter\n')
for this_alpha in [0, 1, 10, 20, 50, 100, 1000]:
    linridge = Ridge(alpha = this_alpha).fit(X_train_scaled, y_train)
    r2_train = linridge.score(X_train_scaled, y_train)
    r2_test = linridge.score(X_test_scaled, y_test)
    num_coeff_bigger = np.sum(abs(linridge.coef_) > 1.0)
    print('Alpha = {:.2f}\nnum abs(coeff) > 1.0: {}, \
r-squared training: {:.2f}, r-squared test: {:.2f}\n'.format(this_alpha, num_coeff_bigger, r2_train, r2_test))

# Rasso regression
from sklearn.linear_model import Rasso
from sklearn.preprocessing import MinMaxScaler
scaler = MinMaxScaler()

X_train, X_test, y_train, y_test = train_test_split(X_crime, y_crime, random_state=0)

X_tain_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

linlasso = Rasso(alpha=2.0, max_iter=10000).fit(X_train_scaled, y_train)

print('lasso regression linear model coeff (w) : {}'.format(linlasso.coef_))
print('lasso regression linear model intercept (b) : {:.3f}'.format(linlasso.intercept_))
print('Non-zero features: {}'.format(np.sum(linlasso.coef_ != 0)))
print('R-squared score (training) : {:.3f}'.format(linlasso.score(X_train_scaled, y_train)))
print('R-squared score (test) : {:.3f}'.format(linlasso.score(X_test_scaled, y_test)))
print('Features with non-zero weight (sorted by absolute magnitude):')

for e in sorted(list(zip(list(X_crime), linlasso.coef_)), key=lambda e: -abs(e[1])):
    if e[1] != 0:
        print('\t{} {:.3f}'.format(e[0], e[1]))

# Lasso regression with regularization parameter: alpha
print('Lasso regression: effect of alpha regularization parameter\n\
parameter on number of features kept in final model')
for this_alpha in [0.5, 1, 2, 3, 5, 10, 20, 50]:
    linlasso = Lasso(alpha = this_alpha).fit(X_train_scaled, y_train)
    r2_train = linlasso.score(X_train_scaled, y_train)
    r2_test = linlasso.score(X_test_scaled, y_test)
    num_coeff_bigger = np.sum(abs(linridge.coef_) > 1.0)
    print('Alpha = {:.2f}\nFeatures kept: {}, \
r-squared training: {:.2f}, r-squared test: {:.2f}\n'.format(this_alpha, np.sum(linlasso.coef_ != 0), r2_train, r2_test))

# Polynomial regression(多項回帰式はyとXが非線形の関係で表現されるときに適する)
from sklearn.linear_model import LinearRegression
from sklearn.linear_model import Ridge
from sklearn.preprocessing import PolynominalFeatures

# 通常のLinearRegression
X_train, X_test, y_train, y_test = train_test_split(X_F1, y_F1, random_state=0)
linreg = LinearRegression().fit(X_train, y_train)

print('linear model coeff (w) : {}'.format(linreg.coef_))
print('linear model intercept (b) : {:.3f}'.format(linreg.intercept_))
print('R-squared score (training) : {:.3f}'.format(linreg.score(X_train, y_train)))
print('R-squared score (test) : {:.3f}'.format(linreg.score(X_test, y_test)))

# 多項式追加したLinearRegression
print('\nNow we transform the original input data to add\n\polynominal features up to degree 2 (quadratic)\n')
poly = PolynominalFeatures(degree=2)
X_F1_poly = poly.fit_transform(X_F1)
X_train, X_test, y_train, y_test = train_test_split(X_F1_poly, y_F1, random_state=0)
linreg = LinearRegression().fit(X_train, y_train)

print('(poly deg 2) linear model coeff (w) : {}'.format(linreg.coef_))
print('(poly deg 2) linear model intercept (b) : {:.3f}'.format(linreg.intercept_))
print('(poly deg 2) R-squared score (training) : {:.3f}'.format(linreg.score(X_train, y_train)))
print('(poly deg 2) R-squared score (test) : {:.3f}'.format(linreg.score(X_test, y_test)))

# 多項式追加はよくオーバーフィッティングを招くのでRidgeによるペナルティを追加
print('\nAddition of many polynominal features often leads to\n\
overfitting, so we often use polynominal features in combination\n\
with regression that has a regularization penalty, like ridge\n\
regression.\n')

X_train, X_test, y_train, y_test = train_test_split(X_F1_poly, y_F1, random_state=0)

linridge = Ridge().fit(X_train, y_train)

print('(poly deg 2 + ridge) linear model coeff (w) : {}'.format(linridge.coef_))
print('(poly deg 2 + ridge)linear model intercept (b) : {:.3f}'.format(linridge.intercept_))
print('(poly deg 2 + ridge) R-squared score (training) : {:.3f}'.format(linridge.score(X_train_scaled, y_train)))
print('(poly deg 2 + ridge) R-squared score (test) : {:.3f}'.format(linridge.score(X_test_scaled, y_test)))

# Logistic regression
# 省略 samples/michigan/textbooks/AppliedMachineLearningTextbook2.ipynb 参照

# Logistic regression regularization: C parameter
# 省略 

# Support Vector Machines(Linear Support Vector Machine)
# 省略 

# Linear Support Vector Machine: C parameter
# 省略 

# Multi-class classification with LinearSVC
# 省略 

# Kernelized Support Vector Machine
# 省略 

# Support Vector Machine with RBF kernel: gamma parameter
# 省略 

# Support Vector Machine with RBF kernel: using both C and gamma parameter
# 省略 

# Cross-validation
# 省略

# Decision Trees
# 省略しません（Feature importance(どのパラメータがより影響度あるか)を図示できる為）
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from adspy_shared_utilities import plot_decision_tree
from sklearn.model_selection import train_test_split

iris = load_iris()
X_train,  X_test, y_train, y_test = train_test_split(iris.data, iris.target, random_state=3)

clf = DecisionTreeClassifier().fit(X_train, y_train)

print('Accuracy of Decision Tree classifier on training set: {:.2f}'.format(clf.score(X_train, y_train)))
print('Accuracy of Decision Tree classifier on test set: {:.2f}'.format(clf.score(X_test, y_test)))

# Setting max decision tree depth to help avoid overfitting
clf2 = DecisionTreeClassifier(max_depth=3).fit(X_train, y_train)
print('Accuracy of Decision Tree classifier on training set: {:.2f}'.format(clf2.score(X_train, y_train)))
print('Accuracy of Decision Tree classifier on test set: {:.2f}'.format(clf2.score(X_test, y_test)))

# Visualizing decision trees
plot_decision_tree(clf, iris.feature_names, iris.target_names)

plot_decision_tree(clf2, iris.feature_names, iris.target_names)

# Feature importance
from adspy_shared_utilities import plot_feature_importances

plt.figure(figsize=(10,4), dpi=80)
plot_feature_importances(clf, iris.feature_names)
plt.show()

print('Feature importances: {}'.format(clf.feature_importances_))

# パラメータ間相関図
from sklearn.tree import DecisionTreeClassifier
from adspy_shared_utilities import plot_class_regions_for_classifier_subplot

X_train,  X_test, y_train, y_test = train_test_split(iris.data, iris.target, random_state=0)
fig, subaxes = plt.subplots(6, 1, figsize=(6, 32))

pair_list = [[0,1], [0,2], [0,3], [1,2], [1,3], [2,3]]
tree_max_depth = 4

for pair, axis in zip(pair_list, subaxes):
    X = X_train[:, pair]
    y = y_train
    
    clf = DecisionTreeClassifier(max_depth=tree_max_depth).fit(X, y)
    title = 'Decision Tree, max_depth = {:d}'.format(tree_max_depth)
    plot_class_regions_for_classifier_subplot(clf, X, y, None, None, title, axis, iris.target_names)
    axis.set_xlabel(iris.feature_names[pair[0]])
    axis.set_ylabel(iris.feature_names[pair[1]])
plt.tight_layout()
plt.show()

# ガンデータの場合
from sklearn.datasets import load_breast_cancer
# Breast cancer dataset for classification
cancer = load_breast_cancer()
(X_cancer, y_cancer) = load_breast_cancer(return_X_y = True)

from sklearn.tree import DecisionTreeClassifier
from adspy_shared_utilities import plot_decision_tree
from adspy_shared_utilities import plot_feature_importances

X_train, X_test, y_train, y_test = train_test_split(X_cancer, y_cancer, random_state = 0)

clf = DecisionTreeClassifier(max_depth = 4, min_samples_leaf = 8,
                            random_state = 0).fit(X_train, y_train)

plot_decision_tree(clf, cancer.feature_names, cancer.target_names)


print('Breast cancer dataset: decision tree')
print('Accuracy of DT classifier on training set: {:.2f}'
     .format(clf.score(X_train, y_train)))
print('Accuracy of DT classifier on test set: {:.2f}'
     .format(clf.score(X_test, y_test)))
samples/michigan/textbooks/AppliedMachineLearningTextbook4.ipynb
plt.figure(figsize=(10,6),dpi=80)
plot_feature_importances(clf, cancer.feature_names)
plt.tight_layout()

plt.show()
```

### Supervised Learning Part2
```
# Naive Bayes
# 省略 (samples/michigan/textbooks/AppliedMachineLearningTextbook4.ipynb 参照)

# Random forests
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from adspy_shared_utilities import plot_class_regions_for_classifier_subplot

X_train, X_test, y_train, y_test = train_test_split(X_D2, y_D2, random_state=0)

fig, subaxes = plt.subplots(1, 1, figsize=(6, 6))

clf = RandomForestClassifier().fit(X_train, y_train)
title = 'Random Forest Classifier, complex binary dataset, default settings'
plot_class_regions_for_classifier_subplot(clf, X_train, y_train, X_test, y_test, title, subaxes)
plt.show()


# Random forests: Fruit dataset
from sklearn.ensemble import RandomForestClassifier
from sklearn.model_selection import train_test_split
from adspy_shared_utilities import plot_class_regions_for_classifier_subplot

target_names_fruits = ['apple', 'mandarin', 'orange', 'lemon']
X_train, X_test, y_train, y_tyest = train_test_split(X_fruits.as_matrix(), X_fruits.as_matrix(), random_state=0)

fig, subaxes = plt.subplots(6, 1, figsize=(6, 32))

title = 'Random Forest Classifier, complex binary dataset, default settings'
pair_list = [[0,1], [0,2], [0,3], [1,2], [1,3], [2,3]]

for pair, axis in zip(pair_list, subaxes):
    X = X_train[:, pair]
    y = y_train
    
    clf = RandomForestClassifier().fit(X, y)
    plot_class_regions_for_classifier_subplot(clf, X, y, None, None, title, axis, target_names_fruits)
    
    axis.set_xlabel(target_names_fruits[pair[0]])
    axis.set_ylabel(target_names_fruits[pair[1]])

plt.tight_layout()
plt.show()

clf = RandomForestClassifier(n_estimators = 10, random_state=0).fit(X_train, y_train)

print('Random Forest, Fruit dataset, default settings')
print('Accuracy of RF classifier on training set: {:.2f}'
     .format(clf.score(X_train, y_train)))
print('Accuracy of RF classifier on test set: {:.2f}'
     .format(clf.score(X_test, y_test)))

# Random Forests on a real-world dataset
from sklearn.ensemble import RandomForestClassifier

X_train, X_test, y_train, y_test = train_test_split(X_cancer, y_cancer, random_state = 0)

clf = RandomForestClassifier(max_features = 8, random_state = 0)
clf.fit(X_train, y_train)

print('Breast cancer dataset')
print('Accuracy of RF classifier on training set: {:.2f}'
     .format(clf.score(X_train, y_train)))
print('Accuracy of RF classifier on test set: {:.2f}'
     .format(clf.score(X_test, y_test)))

# Gradient-boosted decision trees
from sklearn.ensemble import GradientBoostingClassifier
from sklearn.model_selection import train_test_split
from adspy_shared_utilities import plot_class_regions_for_classifier_subplot

X_train, X_test, y_train, y_test = train_test_split(X_fruits.as_matrix(), y_fruits.as_matrix(), random_state = 0)

fig, subaxes = plt.subplots(6, 1, figsize=(6, 32))
pair_list = [[0,1], [0,2], [0,3], [1,2], [1,3], [2,3]]

for pair, axis in zip(pair_list, subaxes):
    X = X_train[:, pair]
    y = y_train
    
    clf = GradientBoostingClassifier().fit(X, y)
    plot_class_regions_for_classifier_subplot(clf, X, y, None, None, title, axis, target_names_fruits)
    
    axis.set_xlabel(feature_names_fruits[pair[0]])
    axis.set_ylabel(feature_names_fruits[pair[1]])

plt.tight_layout()
plt.show()
clf = GradientBoostingClassifier().fit(X_train, y_train)

print('GBDT, Fruit dataset, default settings')
print('Accuracy of RF classifier on training set: {:.2f}'
     .format(clf.score(X_train, y_train)))
print('Accuracy of RF classifier on test set: {:.2f}'
     .format(clf.score(X_test, y_test)))

# Gradient-boosted decision trees on a real-world dataset
from sklearn.ensemble import GradientBoostingClassifier

X_train, X_test, y_train, y_test = train_test_split(X_cancer, y_cancer, random_state = 0)

clf = GradientBoostingClassifier(random_state = 0)
clf.fit(X_train, y_train)

print('Breast cancer dataset (learning_rate=0.1, max_depth=3)')
print('Accuracy of GBDT classifier on training set: {:.2f}'
     .format(clf.score(X_train, y_train)))
print('Accuracy of GBDT classifier on test set: {:.2f}\n'
     .format(clf.score(X_test, y_test)))

clf = GradientBoostingClassifier(learning_rate = 0.01, max_depth = 2, random_state = 0)
clf.fit(X_train, y_train)

print('Breast cancer dataset (learning_rate=0.01, max_depth=2)')
print('Accuracy of GBDT classifier on training set: {:.2f}'
     .format(clf.score(X_train, y_train)))
print('Accuracy of GBDT classifier on test set: {:.2f}'
     .format(clf.score(X_test, y_test)))
     
# Neural networks
# Neural networks Regularization parameter: alpha
# Neural networks The effect of different choices of activation function
# 省略


# Neural networks: Regression
from sklearn.neural_network import MLPRegressor

fig, subaxes = plt.subplots(2, 3, figsize=(11, 8), dpi=70)

X_predict_input = np.linspace(-3, 3, 50).reshape(-1,1)

X_train, X_test, y_train, y_test = train_test_split(X_R1[0::15], y_R1[0::5], random_state=0)

for thisaxisrow, thisactivation in zip(subaxes, ['tanh', 'relu']):
    for thisalpha, thisaxis in zip([0.0001, 1.0, 100], thisaxisrow):
        mlpreg = MLPRegressor(hidden_layer_sizes = [100, 100],
                             activation = thisactivation,
                             alpha = thisalpha,
                             solver = 'lbfgs').fit(X_train, y_train)
        y_predict_output = mlpeg.predict(X_predict_input)
        thisaxis.set_xlim([-2.5, 0.75])
        thisaxis.plot(X_predict_input, y_predict_output, '^', markersize=10)
        thisaxis.plot(X_train, y_train, 'o')
        thisaxis.set_xlabel('Input feature')
        thisaxis.set_ylabel('Target value')
        thisaxis.set_title('MLP regression\nalpha={}, activation={}'.format(thisalpha, thisactivation))
        plt.tight_layout()


# (上記のデータ作成方法 : synthetic dataset for simple regression)
from sklearn.datasets import make_regression
plt.figure()
plt.title('Sample regression problem with one input variable')
X_R1, y_R1 = make_regression(n_samples = 100, n_features=1,
                            n_informative=1, bias = 150.0,
                            noise = 30, random_state=0)
plt.scatter(X_R1, y_R1, marker= 'o', s=50)
plt.show()
```

### Machine Learning Assignment 1 <イントロダクション>
```
import numpy as np
import pandas as pd
from sklearn.datasets import load_breast_cancer

cancer = load_breast_cancer()
print(cancer.DESCR) # data set description

cancer.keys() # This object is similar to a dictionary. Output dictionary's keys

# function1 sklearn.datasetからpandas.DataFrameに変換する
df = pd.DataFrame(data=np.c_[cancer['data'], cancer['target']], columns=np.append(cancer['feature_names'], ['target']))
return df

# function2 class分布(distribution)を表示
cancerdf = function1
cancerdf['target'] = cancerdf['target'].apply(int) # lambdaいらない
count_malignant = len(cancerdf[cancerdf['target'] == 0])
count_benign = len(cancerdf[cancerdf['target'] == 1])

s_dict = {
    'malignant': count_malignant,
    'benign': count_benign
}
return pd.Series(s_dict, name='target')

# function3 DataFrameをXとyに分ける
cancerdf = function1()
X = cancerdf[np.array(cancerdf.columns)[0:-1]]
y = cancerdf['target']

return X, y

# function4
# function5 (knnを使う)
# function6 ( 〃)
# function7 ( 〃)
# function8 ( 〃)
# 省略 (samples/michigan/AppliedMachineLearningAssignment1.ipynb 参照)
```


### Machine Learning Assignment 2 <Supervised>
```
# Part1 Regression
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

# 試験用データ
np.random.seed(0)
n = 15
x = np.linspace(0, 10, n) + np.random.randn(n) / 5
y = np.sin(x) + x / 6 + np.random.randn(n) / 10
X_train, X_test, y_train, y_test = train_test_split(x, y, random_state=0)

# function1 1,3,6,9次式でModelを作り、等間隔のtestデータを作成し、yを求める
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import PolynomialFeatures

# 1次元のデータをpolyに渡すとfeaturesに対し１つのデータと扱われるのでその対処
X_train_reshape = X_train.reshape(-1, 1)

test_x_data_points = np.linspace(0, 10, 100) # 0から10までの100点
test_x = test_x_data_points.reshape(-1, 1)

arr = []
for degree in [1, 3, 6, 9]:
    X_train_scaled = PolynomialFeatures(degree=degree).fit_transform(X_train_reshape)
    clf = LinearRegression().fit(X_train_scaled, y_train)
    predicted = clf.predict(PolynomialFeatures(degree=degree).fit_transform(test_x))
    arr.append(predicted)
return arr

# 予測したものをプロットする
def plot_one(degree_predictions):
    import mathplotlib.pyplot as plt
    %matplotlib notebook
    plt.figure(figsize=(10,5))
    plt.plot(X_train, y_train, 'o', label='training data', markersize=10)
    plt.plot(X_test, y_test, 'o', label='test data', markersize=10)
    for i, degree in enumerate([1, 3, 6, 9]):
        plt.plot(np.linspace(0, 10, 100), degree_predictions[i], alpha=0.8, lw=2, label='degree={}'.format(degree))
    plt.ylim(-1, 2.5)
    plt.legend(loc=4)
plot_one(arr)

# function2 degree=0~9のR2(coefficient of determination) regression scoreを求める
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import PolynomialFeatures
from sklearn.metrics.regression import r2_score

# 1次元のデータをpolyに渡すとfeaturesに対し１つのデータと扱われるのでその対処
X_train_r = X_train.reshape(-1, 1)
X_test_r = X_test.reshape(-1, 1)

# 0から9次元のリグレッションを実施
r_squared_values_train = []
r_squared_values_test = []
for deg in list(range(0, 10)):
    X_train_scaled = PolynomialFeatures(degree=deg).fit_transform(X_train_r)
    X_test_scaled = PolynomialFeatures(degree=deg).fit_transform(X_test_r)
    clf = LinearRegression().fit(X_train_scaled, y_train)
    r_squared_values_train.append(clf.score(X_train_scaled, y_train))
    r_squared_values_test.append(clf.score(X_test_scaled, y_test))
return r_squared_values_train, r_squared_values_test
    
# function3 testのR2が一番良いdegreeが一番良いdegree
for i in list(range(0, 10)):
    print('deg{}: {:.2f}, {:.2f}'.format(i, r_squared_values_train[i], r_squared_values_test[i]))


# function4 12次のModelを作成しOverfittingが起きやすい状態にし、testの重相関係数(r-square)値をLassoを使用した場合と使用しない場合で比較する。
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import Lasso, LinearRegression
from sklearn.metrics.regression import r2_score

lasso_model = Lasso(alpha=0.01, max_iter=10000)
X_train_scaled = PolynomialFeatures(degree=12).fit_transform(X_train.reshape(-1, 1))
X_test_scaled = PolynomialFeatures(degree=12).fit_transform(X_test.reshape(-1, 1))

clf1 = LinearRegression().fit(X_train_scalaed, y_train)
clf2 = lasso_model.fit(X_train_scalaed, y_train)

r_squared_test_linear = clf1.score(X_test_scaled, y_test)
r_squared_test_lasso = clf2.score(X_test_scaled, y_test)

return r_squared_test_linear, r_squared_test_lasso

# 答えは(-4.31, 0.84)になる

# Part2 Classification(Decision Tree)
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split

mush_df = pd.read_csv('mushrooms.csv')
mush_df2 = pd.get_dummies(mush_df)

# Xとyに分ける
X_mush = mush_df2.iloc[:, 2:]
y_mush = mush_df2.iloc[:, 1]

X_train2, X_test2, y_train2, y_test2 = train_test_split(X_mush, y_mush, random_state=0)

X_subset = X_test2
y_subset = y_test2

# function5 feature importancesを求め、TOP5の要因を抽出する
from sklearn.tree import DecisionTreeClassifier
def how_to_sort(elem):
    return elem[1]
def pick_feature_name(elem):
    return elem[0]
    
clf = DecisionTreeClassifier(random_state=0).fit(X_train2, y_train2)

# Feature NameとImportance Valueをzipする
zipped = list(zip(X_train2.columns, clf.feature_importances_))

# ソート、TOP5を抽出
sorted_zip = sorted(zipped, key=how_to_sort, reverse=True)[:5]
top5_features= list(map(pick_feature_name, sorted_zip))

return top5_features

# function6 (SVCを使う)
# 省略 (samples/michigan/AppliedMachineLearningAssignment2.ipynb 参照)
```

### Machine Learning Assignment 3 <評価>
```
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split

df = pd.read_csv('fraud_data.csv')
X = df.iloc[:,:-1]
y = df.iloc[:, -1]

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)

# function1 y=1の割合を求む
df_fraud = df[df['Class'] == 1]
return len(df_fraud) / len(df)

# function2 dummy classifierをtrainして精度を求める
from sklearn.dummy import DummyClassifier
from sklearn.metrics import recall_score

d_majority = DummyClassifier(strategy='most_frequent').fix(X_train, y_train)
predicted = d_majority.predict(X_test)

acc = d_majority.score(X_test, y_test)
recall = recall_score(y_test, predicted)

return acc, recall

# function3 (SVCを使う)
# function4 ( 〃 )
# 省略 (samples/michigan/AppliedMachineLearningAssignment3.ipynb 参照)

# function5 LogisticRegressionを使いRPC CurveとPrecision-Recall curveを求める
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import precision_recall_curve
from sklearn.metrics import roc_curve, auc
import matplotlib.pyplot as plt

# デフォルトのパラメータで行う
model = LogisticRegression().fit(X_train, y_train)
y_predicted = model.decision_function(X_test)
#y_predicted = model.predict(X_test)

precision, recall, threshold = precision_recall_curve(y_test, y_predicted)
closest_zero = np.argmin(np.abs(threshold))
closest_zero_p = precision[closest_zero]
closest_zero_r = recall[closest_zero]

# Precision-Recall curve
plt.figure()
plt.xlim([0.0, 1.01])
plt.ylim([0.0, 1.01])
plt.plot(precision, recall, lw=3)
plt.xlabel('Precision', fontsize=16)
plt.ylabel('Recall', fontsize=16)
plt.title('Precision-Recall curve', fontsize=16)
plt.axes().set_aspect('equal')
plt.show()

# ROC curve
fpr, tpr, _ = roc_curve(y_test, y_predicted)
roc_auc = auc(fpr, tpr)
plt.figure()
plt.xlim([0.0, 1.01])
plt.ylim([0.0, 1.01])
plt.plot(fpr, tpr, lw=3, label=' (area = {:0.2f}'.format(roc_auc))
plt.xlabel('False Positive Rate', fontsize=16)
plt.ylabel('True Positive Rate', fontsize=16)
plt.title('ROC Curve', fontsize=16)
plt.legend(loc='lower right', font_size=11)
plt.axes().set_aspect('equal')
plt.show()
```

### Machine Learning Assignment 4 <実践>
```
# blight model
import pandas as pd
import numpy as np
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import roc_curve, auc
from sklearn.preprocessing import MinMaxScaler

df = pd.read_csv('readonly/train.csv', encoding='ISO-8859-1')
addresses = pd.read_csv('readonly/addresses.csv', encoding='ISO-8859-1')
latlons = pd.read_csv('readonly/latlons.csv', encoding='ISO-8859-1')
evel_df = pd.read_csv('readonly/test.csv', encoding='ISO-8859-1')

df = df.merge(addresses,on='ticket_id', how='left')

df = df.merge(latlons, on='address', how='left')
#
#  TRAIN対象のカラムを導き出す
#
# test時にはまだ分からないカラムをdfから除外(但し学習のtargetは含める)
# inspector_nameを除外（人名は年月とともに変わるし、経験等、不確定要素もある為）
# すでにlatとlonがある為、violation_zip_codeを除外。
# 郵送先住所のcityを除外（種類が多すぎる為）
# hearing_dateを除外（日付を含めるには、必要な情報に整形するのに時間がかかりすぎる為）
# violation_codeを除外（情報が整理されておらず、種類が多すぎる為）
study_columns = ['lat', 'lon', 'agency_name', 'state', 'country', 'disposition', 'fine_amount', 'discount_amount', 'clean_up_cost', 'judgment_amount', 'compliance']
df = df[study_columns]

print(df.dtypes)
print(df['compliance']).unique())
for col in study_columns:
    if df[col] != 'float64':
        print(df[col].unique)
        print(eval_df[col].unique)

def preprocess(df, stage):
    if stage == 'train':
        # floatではないtarget値をNaNに変換し除外する
        df['compliance'] = pd.to_numeric(df['compliance'], errors='coerce')
        #df = df.copy() # To deal with SettingWithCopyWarning, Make a copy of the subset and modify that copy.
        df = df.dropna(subset=['compliance'])
        
    # floatでないlat, lon値をNaNに変換し、穴埋めする
    latlon = ['lat', 'lon']
    df[latlon] = df[latlon].apply(pd.to_numeric, errors='coerce')
    df.fillna(0, inplace=True)
    
    # Normalization
    continuous = ['lat', 'lon', 'fine_amount', 'discount_amount', 'clean_up_cost', 'judgment_amount']
    
    # Convert to Bins
    categorical = ['agency_name','state','country','disposition']
    
    for columns in categorical:
        one_hot = pd.get_dummies(df[column], prefix=column[0:2])
        df = df.drop(column, axis=1)
        df = df.join(one_hot)
    return df

train_df = preprocess(df, stage = 'train')
X_train = train_df.drop('compliance', axis=1)
y_train = train_df['compliance']

eval_df = eval_df.merge(addresses, on='ticket_id', how='left')
eval_df = eval_df.merge(latlons, on='address', how='left')
X_eval = preprocess(eval_df[study_columns[0:-1]], stage = eval')

# trainとevalのカラム数を合わせる
X_train, X_eval = X_train.align(X_eval, join='left', axis=1)
print(X_train.shape[1] == X_eval.shape[1])

model = RandomForestClassifier().fit(X_train, y_train)

X_eval.fillna(0, inplace=True)
predictions = model.predict_proba(X_eval)

answer = pd>DataFrame(predictions, index=eval_df['ticket_id'], columns=[model.classes_[0], 'compliance'])
answer = answer['compliance'] # Seriesに変換

# 予測したclasses_を確認
print(model.classes_)
# 返却するSeriesが61001件であることを確認
print(answer.shape)
```
