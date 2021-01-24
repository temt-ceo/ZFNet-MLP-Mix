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

