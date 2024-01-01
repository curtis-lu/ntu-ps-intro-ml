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

# Intro to Pandas

pandas 是使用Python進行資料分析，或是快速進行一些資料處理時必用的套件。

在小型專案中，可能也適合使用pandas來建立資料流程的管線。

但在較大型的專案中，資料量大或是資料處理流程的複雜度高的情況下，搭配使用其他工具可能就比較適合了（例如spark或是dbt），端看使用的情境以及專案團隊或組織的資源。

總而言之，pandas是使用Python進行資料分析必學的套件，就讓我們開始吧！

以下假設你已經建立好jupyter notebook的環境（也可以使用google colab），並且安裝好pandas套件。

## 什麼是 DataFrame？

首先先來瞭解什麼是DataFrame。

DataFrame是pandas讀取資料表後的形式，跟excel資料表非常相似。

首先第一步是把pandas套件import進來。

```{code-cell}
import pandas as pd
```

定義一個DataFrame

```{code-cell}
# pd.Timestamp 是pandas建立時間資料的方法
# pd.Categorical 是pandas建立類別資料的方法。類別資料與字串不同，具有其他特性，之後會說明。

df = pd.DataFrame(
    {
        "col_1": [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
        "col_2": [i/10 for i in range(1,11)],
        "col_3": ['a', 'i', 'u', 'e', 'o', 'a', 'e', 'i', 'o', 'u'],
        "col_4": pd.Timestamp("20240101"),
        "col_5": [True, False] * 5,
        "col_6": pd.Categorical(["low"] * 5 + ["high"] * 5)
    }
)
```

直接執行它，就會印出DataFrame的內容。

```{code-cell}
df # or display(df)
```

具有以下組成部分：

- **索引(index)：** 最左側數字0~1的部分

```{code-cell}
    df.index
```

- **欄位(column)**：上方col_1 ~ col_6的部分

```{code-cell}
df.columns
```

- **資料(data)**：資料內容本身。

```{code-cell}
df.values
```

可以注意到印出的結果有```array```的字樣，其實pandas是基於另外一個套件numpy而延伸出來的工具。

numpy也是一個python套件，提供大量的陣列運算功能，而且速度非常快。

numpy的基本資料結構被稱作```array```。

事實上numpy array是一個可以容納多維的陣列，稱作```ndarray``` (n-dimensional array)。

```{code-cell}
type(df.values)
```

建立一個numpy array：

```{code-cell}
import numpy as np

arr = np.array([1, 2, 3, 4, 5])
```


Pandas DataFrame的每一欄具有特定的資料型態。

```{code-cell}
df.dtypes
```

pandas資料型態大致如下：

- Int64: integer
- float64: float
- object: strings (注意當資料型態混用的時候也會是object)
- category: pandas特殊的資料型態，基本上也是string，但具有其他方便的特性。
- bool: 布林值
- datetime64[ns]: 時間

## 認識你的資料

以下的資料內容取自在kaggle找到的資料集：[credit_risk_customers (kaggle.com)](https://www.kaggle.com/datasets/ppb00x/credit-risk-customers/data)。可以自行下載成csv檔。

這是一個跟銀行客戶信用風險相關的資料集，資料內容說明請見：[OpenML](https://www.openml.org/search?type=data&sort=runs&status=active&id=31)。

首先透過以下語法讀入該csv檔：

```{code-cell}
import pandas as pd

df = pd.read_csv('./data/credit_customers.csv')
```

在進行資料分析的一開始，通常會先大致瞭解一下資料的長相。

### 快速查看DataFrame內容

以下語法可以快速查看DataFrame的相關資訊：

```{code-cell}
df.info()
```

查看DataFrame前幾筆資料，可以使用```df.head()```，預設是顯示5筆，但也可指定筆數。

```{code-cell}
df.head(3)
```

反過來就是：

```{code-cell}
df.tail()
```

若要隨機查看的話：

```{code-cell}
df.sample(3)
```

注意到，由於資料的欄位數較多，實際顯示出來的欄位數較少，中間會以"..."的方式略過。

若想要查看完整的欄位，可以使用以下語法：

```{code-cell}
pd.set_option("display.max_columns", None)
```

意思是告訴pandas要顯示出全部的欄位數：

```{code-cell}
df.head()
```

查看資料的維度：

```{code-cell}
df.shape
```

回傳的是一個tuple，第1個元素是index的長度，就是列(row)的數量，第2個元素是column的長度，就是欄(column)的數量。

若要快速得知資料的統計資訊：

```{code-cell}
df.describe()
```

注意到只有數值型的資料才會列出。若要納入非數值型的資料的話：

```{code-cell}
df.describe(include='all')
```

### 檢查遺漏值

大致瞭解資料長相之後，可以開始進一步確認資料的細節。資料是否有遺漏值？欄位值有哪些取值？欄位的最大最小值等統計資訊？等等。

首先先來看跟遺漏值相關的語法：

```{code-cell}
df.isna()
```

以上這個語法會回傳每個欄位每一列是否為遺漏值的DataFrame，False代表該欄位值沒有遺漏值。True代表有遺漏值。

但通常我們關心的是哪些欄位有遺漏值？有多少筆資料有遺漏值？

所以會想要在把上面的資料根據欄位「加總」，因為True代表1，False代表0，加總後就可以知道遺漏值的數量。

可以這樣寫：

```{code-cell}
df.isna().sum()
```

上面的語法結構值得說明一下，由於```df.isnull()```回傳的是一個DataFrame，所以我們可以直接接著call其他DataFrame的方法。

而```.sum()```是一個把DataFrame資料加總的方法，預設是by欄位加總。可以藉由調整參數``axis```來改變加總的方向。

不指定axis參數的話，預設是```axis=0```，代表沿著axis 0， 把值一個一個加起來，所以是by欄位加總。

```axis=1```代表by列加總，就可以知道哪幾筆資料有遺漏值。

```{code-cell}
df.isna().sum(axis=1)
```

如果不關心筆數，也可以使用```.any()```，代表只要有一筆資料為True就會輸出True：

```{code-cell}
df.isna().any()
```

如何確認資料是否有任何遺漏值？

```{code-cell}
df.isna().any().any()
```

反向的語法是：

```{code-cell}
df.notna().sum()
```

### 確認欄位值

如果想知道特定欄位有哪些取值，例如想知道客戶來申辦貸款的目的有哪些？可以對purpose這個欄位使用以下操作：

```{code-cell}
df['purpose'].unique()
```

就可以把資料中purpose實際上出現哪些不同值給列出來。

其中，```df['purpose']```是篩選出欄位資料的語法。也可以使用：

```{code-cell}
df.purpose
```

注意到```.purpose```的語法事實上是把欄位當作是DataFrame的「屬性(attribute)」來取用，所以若欄位名稱與DataFrame原本就有的屬性重複的話會無法使用。

補充說明一下，這裡回傳的東西，其實是pandas Series。Series是pandas的另一個資料結構，DataFrame中每一個欄位都儲存為Series。

```{code-cell}
type(df.purpose)
```

可以把Series理解為單一維度的numpy array，與numpy array不同的地方在於Series內含具有標籤的索引(index)。

```{code-cell}
s = pd.Series([1, 2, 3, 4, 5], index=["a", "b", "c", "d", "e"])

print(s)
print(s.index)
```

除了看哪些欄位取值，還可以直接算出該欄位有幾種不同取值：

```{code-cell}
df['purpose'].nunique()
```

或是每個取值有多少筆資料：

```{code-cell}
df['purpose'].value_counts()
```

進一步看佔比：

```{code-cell}
df['purpose'].value_counts(normalize=True)
```

或是看個別欄位的前幾大或倒數前幾大的值是哪些（參數可指定列數）：

```{code-cell}
df['duration'].nlargest()
```

```{code-cell}
df['duration'].nsmallest()
```

或是可以用排序的方法：

```{code-cell}
df['duration'].sort_values(ascending=False).head()
```

```{code-cell}
df['duration'].sort_values().head()
```

### 資料的統計資訊

除了使用```.describe()```快速查看資料統計數據外，還可以使用各種方法來依照需求做計算。

```{code-cell}
df['duration'].min()
```

```{code-cell}
df['duration'].max()
```

```{code-cell}
df['duration'].mean()
```

```{code-cell}
df['duration'].median()
```

```{code-cell}
df['duration'].var()
```

```{code-cell}
df['duration'].std()
```

```{code-cell}
df['duration'].quantile(0.8)
```

```{code-cell}
df['duration'].quantile([0.1,0.3,0.45,0.6,0.8,1])
```

還可以很方便算多個欄位的共變數、相關係數：

```{code-cell}
df[['age', 'duration', 'credit_amount']].cov()
```

```{code-cell}
df[['age', 'duration', 'credit_amount']].corr()
```

## 查看你的資料

有了對資料的大致認識後，可能會想對資料的部分欄位或部份樣本做一些後續處理，例如填補遺漏值、對欄位做加工等等。

但在此之前需要先知道怎麼擷取你想要的特定資料。

### 抓特定欄位的資料

首先第一種方法已經介紹過了，就是直接在DataFrame後面加```[想要抓的欄位]```：

```{code-cell}
df['class']
```

當要一次抓取多個欄位時，必須放入一個欄位名稱的list：

```{code-cell}
df[['age', 'duration', 'credit_amount']]
```

另一種方法是使用```.loc[]```，使用方式如下：

```{code-cell}
df.loc[:, 'class']
```

多個欄位的話一樣是要放list：

```{code-cell}
df.loc[:, ['age', 'duration', 'credit_amount']]
```

與```.loc[]```類似的，```iloc[]```也可以做到篩選，只是要指定的是「第幾個」欄位：

```{code-cell}
df.iloc[:, [12, 1, 4]]
```

注意到```:```的使用，可以一次指定多個欄位，使用方式跟list的index一樣：

```{code-cell}
df.iloc[:, 0:4] 
```

```.loc[]```也可以使用```:```，但就必須符合原本的欄位順序：

```{code-cell}
print(df.columns)

# 順序錯了，選不到東西
df.loc[:, "purpose":"duration"]
```

```{code-cell}
df.loc[:, "duration":"purpose"]
```

再來一種方法是使用```.filter()```搭配```items=[欄位名稱]```的參數：

```{code-cell}
df.filter(items=['age', 'duration', 'credit_amount'])
```

根據資料型態抓欄位：

```{code-cell}
df.select_dtypes(include='float')
```

根據欄位名稱的一些特徵來抓資料：

```{code-cell}
df.filter(like='_')
```

若要把DataFrame欄位根據指定的順序做排序的話，就必須先設定好欄位順序在list內，然後再把資料抓出來：

```{code-cell}
ordered_cols = ['class', 'credit_amount', 'age', 'duration', 'existing_credits']

df[ordered_cols]
```

### 抓特定列的資料

直接根據列名稱去抓資料的情境雖然比較少，但一樣可以透過```.loc[]```或```.iloc[]```做到：

```{code-cell}
df.loc[7:10, :]
```

在這邊，注意到因為index的順序跟label名稱一樣，但```.loc[]```的行為是判斷label名稱，```.iloc[]```則是類似list做indexing的概念，並不會包含到指定的最後一個index：

```{code-cell}
df.iloc[7:10, :]
```

### 依條件判斷抓取資料

最直接的方法，例如要抓age > 40以上的資料：

```{code-cell}
df[df['age'] > 40]
```

也可以利用```.loc[]```：

```{code-cell}
df.loc[df['age'] > 40, :]
```

或是使用```.query()```方法：

```{code-cell}
df.query('age > 40')
```

邏輯條件可以放多個：

```{code-cell}
df[(df['age'] > 40) & (df['duration'] > 50)]
```

```{code-cell}
df.query('age > 40 & duration > 50')
```

為了避免條件太多，造成閱讀困難，可以做一些前處理：

```{code-cell}
c1 = df['age'] > 40
c2 = df['duration'] > 50
c3 = df['credit_amount'] > 10000

cond = c1 & c2 & c3

df[cond]
```

或是搭配f-string以及```.query()```：

```{code-cell}
c1 = 'age > 40'
c2 = 'duration > 50'
c3 = 'credit_amount > 10000'

cond = f'{c1} & {c2} & {c3}'

df.query(cond)
```
