---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.16.0
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Pandas 資料分析

本章要介紹的方法是pandas的重點功能：

1. 資料分組後運算
2. 新增修改資料欄位
3. 組合多個資料表

## 資料分組後運算

資料分組後運算通常有3大步驟：groupby → 運算 → 合併。

**groupby**是將資料分組，例如依據性別、學歷、年齡級距等等。

資料分組後的**運算**通常會是這三種動作之一：**聚合(aggregation)、轉換(transformation)、過濾(filteration)**。

- 聚合是將分組後的資料，一組一組個別計算統計值。例如原始資料根據性別，計算收入平均數。
- 轉換是將分組後的資料，一組一組進行邏輯運算，產生出新的欄位。例如原始資料根據日期，累加每日的營業額。
- 過濾是將分組後的資料，一組一組根據指定的條件做篩選，產生處篩選後的資料。例如原始資料根據選區分組，篩選出執政黨得票率超過50%的選區。

**合併**的步驟就是把上一步產出的資料合併起來。

- 假設原始資料筆數是1000筆、性別紀錄有2種，**groupby + aggregation**後，資料就是2筆。
- 假設原始資料筆數是1000筆，**groupby + transformation**後，資料仍然是1000筆。
- 假設原始資料筆數是1000筆，**groupby + filteration**後的資料筆數不確定，要看符合篩選條件的資料筆數。

首先介紹```groupby```方法：

### Groupby

本節主要使用的資料集為台灣2005年信用卡客戶的繳款及還款資訊，內含基本的性別、年齡、婚姻狀況等資訊。

變數的定義及說明請參考資料來源：[Default of Credit Card Clients Dataset (kaggle.com)](https://www.kaggle.com/datasets/uciml/default-of-credit-card-clients-dataset)。

首先讀入套件和資料：

```{code-cell}
import pandas as pd
```

```{code-cell}
df = pd.read_csv('./data/UCI_Credit_Card.csv')
```

檢視資料的基本資訊：

```{code-cell}
df.info()
```

groupby的基本用法：

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].mean()
```

上述語法把客戶根據性別分組，分別計算信用卡額度平均數。

語法應該很直觀好懂，讓我們來進一步解析。

```groupby```可以使用不同方式將資料分組：

1. **直接指定欄位名稱**

```{code-cell}
df.groupby('EDUCATION')['LIMIT_BAL'].mean()
```

2. **一個mapping index跟分組標籤的物件，可以是pd.Series或dictionary：**

```{code-cell}
df.groupby(df['EDUCATION'])['LIMIT_BAL'].mean()
```

因為pd.Series有index跟分組標籤的對應關係：

```{code-cell}
df['EDUCATION']
```

事實上```df.groupby('EDUCATION')```是```df.groupby(df['EDUCATION'])```的略寫版本。

3. **一個function，輸入為index，輸出為分組標籤：**

這種方式比較不直觀，但其實也只是為了創造index跟分組標籤的對應關係。這邊```lambda x```的```x```傳入的就是```df```的index。
原始index是0~29999，將index除以10000再四捨五入到整數，就變成0~3了。

```{code-cell}
df.groupby(lambda x: round(x / 10000))['LIMIT_BAL'].mean()
```

如果忘記lambda function的用法可以回去參考前面python的章節。

簡而言之，```groupby```的功能就是要把資料從**原本index的層級**轉換到**分組標籤的層級。**

接下來就進入分組後的運算，首先是Aggregation。

### Aggregation

上面的語法其實就屬於aggregation類的運算。

**單一統計值計算**

例如想看信用卡額度有沒有性別差異（1=male, 2=female），可以把客戶根據性別分組，分別計算信用卡額度平均數。

使用方式如下：

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].mean()
```

注意到這邊回傳的是一個Series。

如果改用以下方式的話，回傳的會是一個DataFrame。

```{code-cell}
df.groupby('SEX')[['LIMIT_BAL']].mean()
```

差別在於被計算的欄位在選用時，是```[]```還是```[[]]```。

其中```.mean()```是pandas內建的統計方法，還有許多可以用：

例如：

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].median()
```

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].std()
```

如果要一次算多個欄位，就只要在```groupby```後指定多個欄位名稱：

```{code-cell}
df.groupby('SEX')[['LIMIT_BAL', 'BILL_AMT1']].mean()
```

如果分組要依據的欄位不只一個，就只要把要分組的欄位名稱包成一個list放入```groupby```中：

```{code-cell}
df.groupby(['SEX', 'EDUCATION'])[['LIMIT_BAL']].count()
```

**多個統計值計算**

如果要計算多個的統計值的話該怎麼做？

可以透過```.agg()```方法，方法內指定要計算的統計值名稱：

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].agg(['mean','std'])
```

常用的還有：```'min'```、```'max'```、```'median'```、```'count'```、```'nunique'```等等。

一次算多個欄位也可以：

```{code-cell}
df.groupby('SEX')[['LIMIT_BAL', 'BILL_AMT1']].agg(['mean','std'])
```

但會注意到欄位名稱會變成有兩個level。

DataFrame的欄位有兩個level的話，在後續處理會比較麻煩。

因此，建議是用以下的寫法，雖然要打比較多字，但寫法比較彈性，可以針對不同欄位計算不同統計值，也可避免產生多個level。

```{code-cell}
(df
 .groupby('SEX')
 .agg(
    LIMIT_BAL_mean = ('LIMIT_BAL', 'mean'), 
    LIMIT_BAL_std = ('LIMIT_BAL', 'std'), 
    BILL_AMT1_mean = ('BILL_AMT1', 'mean'), 
    BILL_AMT1_std = ('BILL_AMT1', 'std')
))
```

注意到上面的語法有```()```包在最外層，否則無法在```.```前換行，這樣換行會讓結構比較清楚易讀。

其實aggregation還有很多種寫法，可參考：[Pandas GroupBy Applications that Everyone Should Know | by Pradeep | Medium](https://medium.com/@er.iit.pradeep09/pandas-groupby-applications-that-everyone-should-know-4f54395ea1ef)。

### Transformation

直接看例子就可以知道Transformation跟Aggregation的差異：

**transformation**

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].transform('mean')
```

**aggregation**

```{code-cell}
df.groupby('SEX')['LIMIT_BAL'].agg('mean')
```

屬於transformation類的方法還有：```cumsum```, ```cummax```, ```cummin```, ```rank```等等。

假設紀錄了兩位客戶今天的交易資料（已照交易順序排序），使用方式如下：

```{code-cell}
txn_df = pd.DataFrame({
    'cust_id': [0, 0, 0, 1, 1],
    'nth_txn': [1, 2, 3, 1, 2],
    'txn_amt': [15, 25, 100, 80, 30]
})
```

```{code-cell}
txn_df
```

以下語法可以計算每位客戶截至每一筆交易的累計金額：

```{code-cell}
txn_df.groupby('cust_id')['txn_amt'].cumsum()
```

或是計算每位客戶截至每一筆交易的最大金額：

```{code-cell}
txn_df.groupby('cust_id')['txn_amt'].cummax()
```

或是計算排序：

```{code-cell}
txn_df.groupby('cust_id')['txn_amt'].rank()
```

說到排序當然也可以用降冪排序：

```{code-cell}
txn_df.groupby('cust_id')['txn_amt'].rank(ascending=False)
```

### Filteration

filteration的主要功能是篩選資料，功能跟```.loc[]```或```query()```類似，只是並非直接根據欄位值做篩選，而是根據聚合後的統計值。

最簡單的使用方式是搭配```.sort_values```方法，例如想要取各學歷組別中，額度（LIMIT_BAL）最高的人的資料：

```{code-cell}
df.sort_values(by='LIMIT_BAL').groupby('EDUCATION').tail(1)
```

有```tail()```當然也就有```head()```：

```{code-cell}
df.sort_values(by='LIMIT_BAL').groupby('EDUCATION').head(1)
```

或是第n筆（起始點為0）：

```{code-cell}
df.sort_values(by='LIMIT_BAL').groupby('EDUCATION').nth(0)
```

或是可以客製化依需求過濾，舉一個案例如下：

回到信用卡的資料集，假設我們想查看那些違約率高的年齡組別的人：

```{code-cell}
df.groupby('AGE').filter(lambda d: d['default.payment.next.month'].mean() > 0.5 )
```

（'default.payment.next.month'是下個月繳款是否違約，1為違約，0為正常繳款，所以取平均數即為違約率）

## 新增修改資料欄位

以上groupby相關方法，特別是**aggregation**和**transformation**對進行特徵工程很有幫助，可以把計算出來的結果併入資料集，給後續分析或是建模使用。

那麼如何新增欄位呢？

假設我們要新增一個欄位是近一期帳單金額相較於前一期帳單金額的增率，最簡單的做法：

```{code-cell}
df['billamt_gwr'] = (df['BILL_AMT1'] - df['BILL_AMT2']) / df['BILL_AMT2']
```

但這樣的做法有幾個缺點：

原始資料集會被異動，要回溯異動前的狀態就必須重新產一次異動前的資料。

解決方式是在進行欄位新增或修改的操作前，先把DataFrame複製下來。

複製的方法要使用```.copy()```，不使用```.copy()```的話並不會真的複製一個物件，只是多了一個變數名稱指向原始資料物件而已：

```{code-cell}
df_processing = df.copy()
```

另外，這種做法每次新增或修改一個欄位就要獨立寫一行，多行之間可能會沒有清楚的聯繫，語法會比較難以閱讀。

所以推薦的作法是使用```.assign()```方法：

```{code-cell}
df_processing = (df
                .assign(
                    billamt_gwr1 = lambda d: (d['BILL_AMT1'] - d['BILL_AMT2']) / d['BILL_AMT2'],
                    billamt_gwr2 = lambda d: (d['BILL_AMT2'] - d['BILL_AMT3']) / d['BILL_AMT3'],
                    billamt_gwr3 = lambda d: (d['BILL_AMT3'] - d['BILL_AMT4']) / d['BILL_AMT4'],
                ))
```

其中```lambda d```敘述句是產生一個匿名函數，以```d```作為參數，```d```可以任意命名，這邊習慣命名為```d```是為了代稱data，因為傳入這個匿名函數的input就是前面的```df```這個DataFrame。

```.assign()```方法會直接產生一個新的物件，不會影響到原有的資料集。

新建的DataFrame：

```{code-cell}
df_processing[['billamt_gwr1', 'billamt_gwr2', 'billamt_gwr']]
```

原本的DataFrame：

```{code-cell}
df[['billamt_gwr1', 'billamt_gwr2', 'billamt_gwr']]
```

```.assign()```方法很方便，幾乎是處理欄位首選的方法。

無論填補遺漏值、改名、格式轉換、transformation等等各種操作都可以寫在``.assign()```裡面。

而且當下新增的欄位也可以在同一個.assign中使用（看下面```age_group_mean```）的例子。

例如以下的語法展示了各種欄位的操作方式：

```{code-cell}
df_processing = (df
                .assign(
                    # 遺漏值填補
                    billamt_gwr1 = lambda d: ((d['BILL_AMT1'] - d['BILL_AMT2']) / d['BILL_AMT2']).fillna(0),
                    # 更改名稱（實際上是新建欄位）
                    target = lambda d: d['default.payment.next.month'],
                    # 格式轉換
                    limit_bal = lambda d: d['LIMIT_BAL'].astype(int),
                    # 切割
                    age_group = lambda d: pd.qcut(d['AGE'], 7, duplicates='drop'),
                    # transformation
                    age_group_mean = lambda d: d.groupby('age_group', observed=True)['limit_bal'].transform('mean'),
                    # 新增固定值
                    cnt = 1
                ))
```

查看結果：

```{code-cell}
df_processing.sample(5)
```

## 結合多個DataFrame

進行資料分析時常常需要使用的欄位是散落在各個資了表中，因此合併資料表是一個常用的功能。

這邊介紹pandas主要的3種方法：```pd.concat()```、```.join()```、```.merge()```。

### pd.concat()

```pd.concat()```**通常用在多個相同的資料表上**。

例如信用卡業務統計數據每個月都會有一份，這時候就很適合使用```pd.concat()```。

使用方式舉例如下：

```{code-cell}
df_202312 = pd.DataFrame({
    'yyyymm': ['202312', '202312', '202312'],
    'bank_no': ['001', '002', '003'],
    'new_card': [12198, 17390, 23209],
})
```

```{code-cell}
df_202401 = pd.DataFrame({
    'yyyymm': ['202401', '202401', '202401'],
    'bank_no': ['001', '002', '003'],
    'new_card': [14692, 16729, 27390],
})
```

```{code-cell}
df_202312
```

```{code-cell}
df_202401
```

上面兩個資料集基本上是同樣的資料集，只是記錄的是不同月份的數據，因此適合根據列的方向來合併(axis=0)：

```{code-cell}
pd.concat([df_202312, df_202401], axis=0)
```

注意到index的部分也一起合併起來了，造成index重複。

index重複會讓後續計算產生很多bug，千萬要注意不能讓index重複，所以根據axis=0合併資料表的話通常要加上```ignore_index=True```參數：

```{code-cell}
pd.concat([df_202312, df_202401], axis=0, ignore_index=True)
```

或是使用```keys```參數，指定可以區別資料的index：

```{code-cell}
pd.concat([df_202312, df_202401], axis=0, keys=['202312', '202401'])
```

當然，合併資料前盡量要注意欄位是否一致，雖然pd.concat會比對欄位名稱來合併資料：

```{code-cell}
df_202402 = pd.DataFrame({
    'yyyymm': ['202402', '202402', '202402'],
    'new_card': [12920, 13028, 18921],
    'bank_no': ['001', '002', '003'],
})
```

注意到這個資料集的欄位順序不一樣。

```{code-cell}
df_202402
```

```{code-cell}
pd.concat([df_202312, df_202401, df_202402], axis=0, ignore_index=True)
```

以上的情境是把資料表縱向地合併起來，但也可以橫向合併，這時要比對的就不是欄位名稱而是index了。

### .join()

```.join()```**通常使用的情境是兩個資料表的層級不一樣，一張資料表用欄位值去串另一張資料表的index**。

例如一個資料表的一個列代表的是交易，另一個資料表的一列代表的是客戶。

因為一個客戶會有多筆交易，所以我們說交易資料表有比較細的層級，而客戶資料表是比較粗的層級。

使用```.join()```的情境就是，使用較細的資料表的欄位，去結合比較粗的資料表的index。

如下範例：

```{code-cell}
df1 = pd.DataFrame({
    'user_id': ['a', 'b',  'b', 'c', 'd', 'e', 'e'], 
    'txn_amt': [2821, 320, 1238, 8912, 7628, 1212, 2443],
})
```

```{code-cell}
df2 = pd.DataFrame({
    'age': [22, 30, 28, 34, 26],
    'gender': ['F', 'M', 'F', 'F', 'M'],
},  index=['a', 'b', 'c', 'd', 'e'])
```

```{code-cell}
df1
```

```{code-cell}
df2
```

```.join()```是DataFrame方法，所以必須在DataFrame後面呼叫它，並指定要join的欄位。

指定的欄位名稱是df1的欄位，df2不需要指定，因為是在index。

```{code-cell}
df1.join(df2, on=['user_id'])
```

```.join()```通常只用在橫向合併。

### .merge()

```.merge()```則是**可以根據欄位也可以根據index做結合**，但一次只能合併兩張資料表。

放在左側的資料表稱作left table，右側當然就叫right table。

用欄位合併的話參數名稱是```left_on=[’欄位名稱’]```或```right_on=[’欄位名稱’]```，

用index合併的話，是```left_index=True```或```right_index=True```

要將df1跟df2合併的話，是使用df1的欄位跟df2的index，所以語法是這樣寫：

```{code-cell}
df1.merge(df2, left_on='user_id', right_index=True)
```

注意到上面的結果跟使用```.join()```是一樣的，但```.join()```會比較快一些。

```.merge()```通常也只用在橫向合併。

### 統整比較

- pd.concat
    - pandas function
    - 可以縱向或橫向合併
    - 可以一次合併多個資料表
    - 只能根據欄位名稱（縱向）或是根據index（橫向）合併。
    - 舉例情境：不同月份的相同資料表。
- .join()
    - DataFrame method
    - 只能橫向合併
    - 可以一次合併多個資料表
    - left table可以使用index或欄位，但right table只能根據index來合併。
    - 舉例情境：客戶交易資料串入客戶特徵資料。
- .merge()
    - DataFrame method
    - 只能橫向合併
    - 一次只能合併兩張資料表
    - 無論left or right，index和欄位都可以用來合併。
    - 舉例情境：都可以。