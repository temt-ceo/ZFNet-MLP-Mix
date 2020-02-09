ABテストを行いたいときに用いる関数
ABテスト:商品を高いときに売りたいときに帯にWebサイトを書いた時と作者のことを書いたときどちらがより高くで売れるかなど、いろいろ使う
→ 行うためには２つの集団がいる。一つの効果が与えられている集団と与えられていない（または異なる）集団

p : 2つの間に差がない確率（「実は違いがない」となる確率の度合い） (population:母集団)（たまたまで起こる確率）
t-test : ２つの間に違いがない仮定（Ho:帰無仮説）がpにより棄却されて違いがあると見るtest（眉唾かどうかを判定するテスト）


違いがないという仮説　　　　p <= 0.05  棄却 (有意な違いがある)　（たまたまで起こる確率は5%以下）
　　　　　　　　　　　　　　p > 0.05   たまたまかも（データを増やさないと分からない＝＞増やせばp値が下がるかもしれない）

違いがあるという仮説は１つのそうでない値によって棄却できてしまう。断言はできない。（データ数は無限に集めても足りない。たった一つでもそうではないと証明できるから）
では、違いがあるという事を証明するためにはどうすればいいか？
A. 違いがないという仮説を棄却して、違いがあるということを証明すれば少ないサンプル数で仮説を受け入れることが可能


Population
 μ:母平均 => 全体のmean
 σ2:母分散
 p:母比率
Samples
 m:標本平均 => 標本のmean
 S2:標本分散
 Pa:標本比率
 n:サンプル数
T検定
　帰無仮説H　　棄却  採択 (Normal Distribution:正規分布が1%以下であればそんな珍しいことは起こらないから棄却する:Normal Distributionは下記式で求める)
　対立仮説H’　 採択  棄却(帰無仮説が棄却されて対立仮説が採択される)
　有意水準α　（0.05や0.01）
standard deviation(標準偏差:σ)：母集団の中でどの範囲に入るかということ。正規分布では標準偏差の範囲に*68%*が入る。２倍にすると95%が入る。3倍にすると99%が入る。

①正規母集団で母分散が既知の場合:
((標本平均 - 母平均) / √(母分散/サンプル数))  = ((m- μ) / (σ2/√n))

②正規母集団で母分散が未知の場合:
((標本平均 - 母平均) / √(母分散/サンプル数)) :この時母分散は既知であり、平均は知りたい値とする = ((m- μ) / √(σ2/n))
　帰無仮説: μ = μ0
　対立仮説: 1.μ ≠ μ0, 2.μ < μ0, 3.μ > μ0　（いずれか）

標準平均と母平均が差がなく、正規分布に従う（1%や5%以上）場合、帰無仮説がacceptされる


import pandas as pd
import numpy as np
from scipy.stats import ttest_ind


import re
wikipedia_univ_homes = pd.read_csv('university_towns.txt', header=None, names = ['city'], sep='\n')
def get_list_of_university_towns():
    '''Returns a DataFrame of towns and the states they are in from the 
    university_towns.txt list. The format of the DataFrame should be:
    DataFrame( [ ["Michigan", "Ann Arbor"], ["Michigan", "Yipsilanti"] ], 
    columns=["State", "RegionName"]  )
    
    The following cleaning needs to be done:

    1. For "State", removing characters from "[" to the end.
    2. For "RegionName", when applicable, removing every character from " (" to the end.
    3. Depending on how you read the data, you may need to remove newline character '\n'. '''
    citis = list(wikipedia_univ_homes['city'])
    state = ''
    dflist = []
    for data in citis:
        if '[edit]' in data:
            state = data.replace('[edit]', '')
        else:
            town = re.sub('\s?\(.*$', '', data)
            dflist.append([state, town])
    df = pd.DataFrame(dflist, columns=["State", "RegionName"])
    
    return df
get_list_of_university_towns()




# 一覧確認用
gdp = pd.read_excel('gdplev.xls', skiprows=219, parse_cols='E,G', names=['Quarter', 'GDP chained'])
prev_gdp = 0
resession_flg = False
def loop_func(data):
    global prev_gdp
    global resession_flg
    answer = ''
    if prev_gdp < data['GDP chained']:
         resession_flg = False
    else:
        if resession_flg is True:
            answer = data['Quarter']
        resession_flg = True
    prev_gdp = data['GDP chained']
    return resession_flg, answer, data['GDP chained']
res = gdp.apply(loop_func, axis=1)
print(res[0:50])




def get_recession_start():
    '''Returns the year and quarter of the recession start time as a 
    string value in a format such as 2005q3'''
    prev_gdp = 0
    resession_flg = False
    answer = ''
    for index, row in gdp.iterrows():
        if prev_gdp < row['GDP chained']:
             resession_flg = False
        else:
            if resession_flg is True and answer == '':
                answer = index - 1
            resession_flg = True
        prev_gdp = row['GDP chained']
    return gdp.iloc[answer]['Quarter']
get_recession_start()




def get_recession_end():
    '''Returns the year and quarter of the recession end time as a 
    string value in a format such as 2005q3'''
    prev_gdp = 0
    resession_flg = False
    resession_start = None
    answer = ''
    for index, row in gdp.iterrows():
        if prev_gdp < row['GDP chained']:
             if resession_start is not None and resession_flg is False and answer == '':
                    answer = index
             resession_flg = False
        else:
            if resession_flg is True and resession_start is None:
                resession_start = index - 1
            resession_flg = True
        prev_gdp = row['GDP chained']
    return gdp.iloc[answer]['Quarter']
get_recession_end()




def get_recession_bottom():
    '''Returns the year and quarter of the recession bottom time as a 
    string value in a format such as 2005q3'''
    start = np.where(gdp['Quarter'] == get_recession_start())
    end = np.where(gdp['Quarter'] == get_recession_end())
    # Get the first index
    span_st = start[0][0]
    span_ed = end[0][0]
    bottom = None
    ibottom = None
    for index, row in gdp.iloc[span_st:span_ed+1].iterrows():
        if bottom is None:
            bottom = row['GDP chained']
        else:
            if bottom > row['GDP chained']:
                bottom = row['GDP chained']
                ibottom = index
    return gdp.iloc[ibottom]['Quarter']
get_recession_bottom()




def convert_housing_data_to_quarters():
    '''Converts the housing data to quarters and returns it as mean 
    values in a dataframe. This dataframe should be a dataframe with
    columns for 2000q1 through 2016q3, and should have a multi-index
    in the shape of ["State","RegionName"].
    
    Note: Quarters are defined in the assignment description, they are
    not arbitrary three month periods.
    
    The resulting dataframe should have 67 columns, and 10,730 rows.
    '''
    housing_df = pd.read_csv('City_Zhvi_AllHomes.csv')
    housing_df['State'] = housing_df['State'].apply(lambda a: states[a])
    housing_df.set_index(['State', 'RegionName'], inplace=True)
    housing_df.pop('RegionID')
    housing_df.pop('Metro')
    housing_df.pop('CountyName')
    housing_df.pop('SizeRank')
    aggregated_df = housing_df.groupby(pd.PeriodIndex(housing_df.columns, freq='Q'), axis=1).mean()
    aggregated_df.columns = aggregated_df.columns.strftime("%Yq%q")
    target_quarters = []
    for quarter in aggregated_df.columns:
        if quarter.startswith('2'):
            target_quarters.append(quarter)
    aggregated_df = aggregated_df[target_quarters]
    
    return aggregated_df
convert_housing_data_to_quarters()





def run_ttest():
    '''First creates new data showing the decline or growth of housing prices
    between the recession start and the recession bottom. Then runs a ttest
    comparing the university town values to the non-university towns values, 
    return whether the alternative hypothesis (that the two groups are the same)
    is true or not as well as the p-value of the confidence. 
    
    Return the tuple (different, p, better) where different=True if the t-test is
    True at a p<0.01 (we reject the null hypothesis), or different=False if 
    otherwise (we cannot reject the null hypothesis). The variable p should
    be equal to the exact p value returned from scipy.stats.ttest_ind(). The
    value for better should be either "university town" or "non-university town"
    depending on which has a lower mean price ratio (which is equivilent to a
    reduced market loss).'''
    housing_df = convert_housing_data_to_quarters()
    st_column = get_recession_start()
    bt_column = get_recession_bottom()
    univ_towns = get_list_of_university_towns()

    before_st_column = housing_df.columns[housing_df.columns.get_loc(st_column) - 1] # リセッション開始前のクォーターを取得
#     print(before_st_column, st_column)

    housing_df['price_ratio'] = housing_df[before_st_column].div(housing_df[bt_column]) # pandas.divの方が　 / よりメモリが軽くなる

    univ_towns_list = univ_towns.values.tolist()
    def is_university_town(row):
        if list(row.name) in univ_towns_list:
             return 1
        else:
             return 0
        
    housing_df['has_univ'] = housing_df.apply(is_university_town, axis=1)
    housing_df_univ = housing_df[housing_df['has_univ'] == 1][['price_ratio']]
    housing_df_norm = housing_df[housing_df['has_univ'] == 0][['price_ratio']]
#     print(len(housing_df_univ))
#     print(len(housing_df_norm))
    
    p = ttest_ind(housing_df_norm['price_ratio'], housing_df_univ['price_ratio'], nan_policy='omit').pvalue
    different = None
    if p < 0.01:
        different = True
    else:
        different = False
    value_for_better = None
    if housing_df_univ['price_ratio'].mean() < housing_df_norm['price_ratio'].mean():
        value_for_better = 'university town'
    elif housing_df_univ['price_ratio'].mean() > housing_df_norm['price_ratio'].mean():
        value_for_better = 'non-university town'
    return different, p, value_for_better
run_ttest()