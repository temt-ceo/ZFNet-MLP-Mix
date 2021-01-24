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

```
