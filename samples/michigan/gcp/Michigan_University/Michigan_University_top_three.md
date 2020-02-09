
# 種別でTOP Threeの合計が最も大きいもの
def answer_six():
    state_df = census_df[census_df['SUMLEV'] == 40]
    county_df = census_df[census_df['SUMLEV'] == 50]
    list_state = list(set(county_df['STNAME']))

    pdict = {}
    for state in list_state:
        county = county_df.loc[county_df['STNAME'] == state]
        sum_pop = county.sort_values(by=['CENSUS2010POP'], ascending=False)[0:3].sum(axis=0)['CENSUS2010POP']
        pdict[state] = sum_pop

    new_df = pd.Series(pdict)
    new_df.sort(axis=1, ascending=False)

    return list(new_df.iloc[0:3].keys())

answer_six()


# カラム間で最も差が大きいもの
def answer_seven():
    county_df = census_df[census_df['SUMLEV'] == 50][['CTYNAME', 'POPESTIMATE2010', 'POPESTIMATE2011', 'POPESTIMATE2012', 'POPESTIMATE2013', 'POPESTIMATE2014', 'POPESTIMATE2015']]
    county_df['diff_max'] = county_df.max(axis=1) - county_df.min(axis=1)
    ret = county_df.sort_values(by=['diff_max'], ascending=False)[['CTYNAME', 'diff_max']]
    return ret.iloc[0]['CTYNAME']

answer_seven()

# 上記をより高速で行う方法(for全loopが遅い)
import numpy as np
def min_max(row):
    data = row[['POPESTIMATE2010',
                'POPESTIMATE2011',
                'POPESTIMATE2012',
                'POPESTIMATE2013',
                'POPESTIMATE2014',
                'POPESTIMATE2015']]
    return pd.Series({'min': np.min(data), 'max': np.max(data)})
    #row['max'] = np.max(data)
    #row['min'] = np.min(data)
    #return row

df.apply(min_max, axis=1)

 # GroupByを使う（グループ別に処理ができる）
def fun(item):
    if item[0]<'M':
        return 0
    if item[0]<'Q':
        return 1
    return 2

for group, frame in df.groupby(fun):
    print('There are ' + str(len(frame)) + ' records in group ' + str(group) + ' for processing.')

＃ Group Byしたものを戻す
df.groupby('STNAME').agg({'CENSUS2010POP': np.average})　←キーに指定したカラムに対しバリューに指定した関数を実行する

# 又はaggを使わず、applyでsumする
def func1(row):
  return sum(row['Weight (oz.)'] * row['Weight (oz.)'])
agged_df = df.groupby('Category').apply(func1)





