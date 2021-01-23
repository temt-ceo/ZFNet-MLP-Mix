# Summary

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

