
# dataマージ
import pandas as pd
import numpy as np
import re

print(pd.__version__)

energy = pd.read_excel('Energy Indicators.xls', skiprows=17, skip_footer=38, names=['Country', 'Energy Supply', 'Energy Supply per Capita', '% Renewable'], parse_cols='C:F')
GDP = pd.read_csv('world_bank.csv', skiprows=4)
ScimEn = pd.read_excel('scimagojr-3.xlsx')

def answer_one():

    def convert_data(val, dataType=None):
        val = re.sub('[0-9]*', '', val)
        val = re.sub('\s?\(.*\)', '', val)
        country_renames = {"Republic of Korea": "South Korea",
            "United States of America": "United States",
            "United Kingdom of Great Britain and Northern Ireland": "United Kingdom",
            "China, Hong Kong Special Administrative Region": "Hong Kong",
             "Korea, Rep.": "South Korea", 
            "Iran, Islamic Rep.": "Iran",
            "Hong Kong SAR, China": "Hong Kong"}
        if val in country_renames:
            val = country_renames[val]
        return val


    energy['Energy Supply']  *= 1000000
    energy['Energy Supply'] = energy['Energy Supply'].map(lambda val:  np.nan if type(val) == str else val)
    energy['Energy Supply per Capita'] = energy['Energy Supply per Capita'].map(lambda val: np.nan if type(val) == str else val)
    energy['Country'] = energy['Country'].apply(convert_data)
    GDP.rename(columns={'Country Name': 'Country'}, inplace=True)
    GDP['Country'] = GDP['Country'].apply(convert_data)

    df = pd.merge(energy, GDP, how='inner', left_on='Country', right_on='Country')
    df = pd.merge(df, ScimEn, how='inner', left_on='Country', right_on='Country')
    df = df[['Country', 'Rank', 'Documents', 'Citable documents', 'Citations', 'Self-citations', 'Citations per document', 'H index', 'Energy Supply', 'Energy Supply per Capita', '% Renewable', '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015']]
    df = df.sort('Rank')
    df = df[df['Rank'] <= 15]
    df.set_index(['Country'], inplace=True)
#     df = df.astype({
#         'Rank': 'int64',
#         'Documents': 'int64',
#         'Citable documents': 'int64',
#         'Citations': 'int64',
#         'Self-citations': 'int64',
#         'Citations per document': 'int64',
#         'H index': 'int64',
#         'Energy Supply': 'int64',
#         'Energy Supply per Capita': 'int64'
#     })
    return df
df = answer_one()
df


# 10年のGDP平均
def answer_three():
    Top15 = answer_one()
    Top15['avgGDP'] = Top15.sum(axis=1) / 10　← NaNや数字でないものは無視される
    return Top15['avgGDP']
answer_three()


# 行ごとにapplyする。該当行のインデックス名を取得しprint
def answer_four():
    def calcSvg(row):
        if row.name == 'Iran':
            row['avgGDP'] = row['sumGDP'] / 9
        else:
            row['avgGDP'] = row['sumGDP'] / 10
        return row

    Top15 = answer_one()
    Top15['sumGDP'] = Top15[[ '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015']].sum(axis=1)
    applied_df = Top15.apply(calcSvg, axis=1)

    sorted_df = applied_df.sort_values(by=['avgGDP'], ascending=False)

    sorted_df['maxGDP'] = sorted_df[[ '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015']].apply(max, axis=1)
    sorted_df['minGDP'] = sorted_df[[ '2006', '2007', '2008', '2009', '2010', '2011', '2012', '2013', '2014', '2015']].apply(min, axis=1)
    sorted_df['diffGDP'] = sorted_df['maxGDP'] - sorted_df['minGDP']

    # 最も変化した国名と変化量
#     print(sorted_df[sorted_df['diffGDP'] == sorted_df[0:5]['diffGDP'].max()].index.values[0])
#     print(sorted_df[0:5]['diffGDP'].max())

    # GDP上位６番目のGDPの１０年間での変化量
    return sorted_df.iloc[5]['2015'] - sorted_df.iloc[5]['2006']
answer_four()



# 平均
return Top15['Energy Supply per Capita'].mean()


# 最大とその行のラベルの取得
def answer_seven():
    Top15 = answer_one()
    Top15['ratio'] = Top15['Self-citations'] / Top15['Citations']
    return Top15[Top15['ratio'] == Top15['ratio'].max()].index.values[0], Top15['ratio'].max()
answer_seven()


# 相関係数を求める(corr())
return Top15['Citable docs per Capita'].corr(Top15['Energy Supply per Capita'], method='pearson')


# if-else(->np.where())と中央値(median())
Top15['Renewable H&L'] = np.where(Top15['% Renewable'] >= Top15['% Renewable'].median(), 1, 0)

# カテゴリ化する(cut())
return  pd.cut(Top15['% Renewable'],5)




