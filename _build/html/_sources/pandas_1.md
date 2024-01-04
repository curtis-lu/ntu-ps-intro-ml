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

# Pandas常用操作

上一章的內容主要介紹如何擷取資料的基本資訊，例如資料的維度、大略地看資料內容、統計值、遺漏值、欄位有無異常值等等。

本節開始要介紹對DataFrame或是欄位的一些操作及轉換。

同樣使用相同的資料，首先讀入資料：

```{code-cell}
import pandas as pd

df = pd.read_csv('./data/credit_customers.csv')
```

## 基本操作

**改變欄位名稱**

由於其中一個欄位名稱"class"與python關鍵字相同，故建議是更改名稱，避免後許使用的困擾。

此外，剛好該欄位是該資料集用來預測是否違約的標籤，因此可以命名為"label"。

```{code-cell}
df.rename(columns={'class': 'label'})
```

注意到，這邊結果是回傳一個DataFrame，並不會改變原本DataFrame的內容：

```{code-cell}
# 該欄位的名稱仍為"class"
df.columns
```

如果要直接修改的話必須帶```inplace=True```參數：

```{code-cell}
df.rename(columns={'class': 'label'}, inplace=True)
```

發現到若加上```inplace=True```參數後，執行該語法並不會回傳任何東西，而是對DataFrame的內容直接做修改。

```{code-cell}
df.columns
```

```inplace=True```在很多DataFrame的方法中都有。

然而，實務上並不建議直接使用```inplace=True```，原因是這會直接修改資料，若要復原操作，就要反向再執行一次，或是重新讀入DataFrame，造成時間上會付出較大的代價。建議是在有明確理由的情況下使用。

比較好的做法是，將結果賦值到新變數中。一來程式的易讀性較高（賦值的動作明確表達出有新資料產出），二來若想修改為其他名稱也只需要在這個步驟重來就好。

```{code-cell}
df_processed = df.rename(columns={'class': 'label'})
```

但缺點是會造成記憶體空間佔用以及需要命名新的物件，所以需要一般會是配合幾個相關的處理需求，利用chaining或是```.pipe()```方法一起執行(後面章節會再詳細說明)。

**填補遺漏值**

可以用以下語法填補資料的遺漏值，但須注意資料格式，建議用相同格式：

```{code-cell}
df['other_payment_plans'].fillna('other')
```

一樣，要加上```inplace=True```才會直接改變資料內容：

```{code-cell}
df['other_payment_plans'].fillna('other', inplace=True)
```

```{code-cell}
df['other_payment_plans'].isna().sum()
```

**欄位值轉換**

後續若要進行建模，通常會需要把文字的欄位值轉換爲數字才能夠丟給模型。

例如我們想把label的值，從good/bad改為0/1，可以把轉換的對應關係儲存成一個字典，然後搭配```.map()```方法：

```{code-cell}
mapper = {
    'good': 0,
    'bad': 1
}

df['label'].map(mapper)
```

注意，如果是mapper沒定義到的欄位值，```.map()```之後會轉為空值。

```{code-cell}
mapper = {
    'bad': 1
}

df['label'].map(mapper)
```

```.map()```方法是回傳一個DataFrame，但沒有修改原始的DataFrame。而```.map()```方法也沒有```inplace```參數，通常的做法會是新建一個欄位。

如何新建一個欄位？下一章會詳細說明，這邊先劇透其中一個方法：

```{code-cell}
df.loc[:, "label_new"] = df['label'].map(mapper)
```

```{code-cell}
df.filter(like='label')
```

## 數值欄位操作

針對數值型的欄位，可能會有一些組合運算，接下來介紹一些方法。

**欄位運算**

**加法**

```{code-cell}
df['age'] + 1
```

```{code-cell}
df['age'].add(1)
```

**減法**

```{code-cell}
df['age'] - 1
```

```{code-cell}
df['age'].sub(1)
```

**乘法**

```{code-cell}
df['installment_commitment'] * 0.01
```

```{code-cell}
df['installment_commitment'].mul(0.01)
```

**除法**

```{code-cell}
df['duration'] / df['age']
```

```{code-cell}
df['duration'].div(df['age'])
```

**取商數**

```{code-cell}
df['age'] // 12
```

```{code-cell}
df['age'].floordiv(12)
```

**取餘數**

```{code-cell}
df['age'] % 12
```

```{code-cell}
df['age'].mod(12)
```

**進位運算**

括弧內的數字代表進位到第幾位。

注意到這邊rounding的行為是"四捨五入到最接近的偶數"，5.5捨去小數點到整數會是6，4.5捨去小數點到整數會是4。這種rounding又稱作是round-to-even。

參考：

1. [python - Strange behavior of numpy.round - Stack Overflow](https://stackoverflow.com/questions/45021268/strange-behavior-of-numpy-round)。
2. [算錢學問大 | iThome](https://www.ithome.com.tw/voice/112663)

```{code-cell}
df.loc[:, 'relative_duration'] = df['duration'] / df['age']

df['relative_duration'].round(1)
```

**取次方**

括弧內可指定次方項。

```{code-cell}
# 取平方

df['relative_duration'].pow(2)
```

**取絕對值**

```{code-cell}
df['age'].sub(df['age'].mean())
```

```{code-cell}
df['age'].sub(df['age'].mean()).abs()
```

**限定值的範圍**

第一個參數是下界，第二個參數是上界。

```{code-cell}
df['relative_duration'].clip(0.05, 2)
```

**數值離散化**

有時要將數值型變數切割，會比較方便做一些描述統計或視覺化分析。

其中參數```bins```是據以切割的數值，```right=False```代表左邊是```[```，而右邊是```)```。

以下面的例子來看，第一個bin會是：0 ≤ age < 20，第二個bin則是：20 ≤ age < 40……以此類推。

```{code-cell}
df.loc[:, "age_bins"] = pd.cut(df['age'], bins=[0, 20, 40, 60, 80], right=False)
```

```{code-cell}
df['age_bins'].value_counts().sort_index()
```

另外一種方式則是直接透過資料的百分位數來做切割。

```{code-cell}
pd.qcut(df['age'], q=10)
```

## 字串欄位操作

以下範例來自pandas官方API文件：

**字串截取**

```{code-cell}
s = pd.Series(["koala", "dog", "chameleon"])
```

```{code-cell}
s.str.slice(start=1) # = s.str[1:]
```

```{code-cell}
s.str.slice(start=-1) # = s.str[-1:]
```

```{code-cell}
s.str.slice(stop=2) # = s.str[:2]
```

```{code-cell}
s.str.slice(step=2) # = s.str[::2]
```

```{code-cell}
s.str.slice(start=0, stop=5, step=3) # = s.str[0:5:3]
```

**判斷字串是否存在**

```{code-cell}
import numpy as np

s1 = pd.Series(['Mouse', 'dog', 'house and parrot', '23', np.nan])
```

判斷特定字串是否包含在值當中

```{code-cell}
s1.str.contains('og')
```

設定參數```na```代表當遇到空值要填入什麼值，下面設定填入```False```

```{code-cell}
s1.str.contains('og', na=False)
```

以下用法稱作**正規表達式（regular expression）**，正規表達式專門處理字串，但內容頗多，附上資源供自行參考。

```{code-cell}
s1.str.contains('house|dog', regex=True)
```

```{code-cell}
s1.str.contains('\\d', regex=True)
```

參考：

[The Ultimate Guide to using the Python regex module](https://towardsdatascience.com/the-ultimate-guide-to-using-the-python-regex-module-69aad9e9ba56)

[Regular Expressions: Regexes in Python (Part 1) – Real Python](https://realpython.com/regex-python/)

[Regular Expressions: Regexes in Python (Part 2) – Real Python](https://realpython.com/regex-python-part-2/)

**字串取代**

```{code-cell}
s = pd.Series(['foo', 'fuz', np.nan]).str.replace('f.', 'ba', regex=True)
```

可以單純將字串替換成別的字串。

```{code-cell}
s.str.replace('f', 'b')
```

也可以使用正規表達式，```.```代表任一字元，所以"f" 以及"f"後面1個字元被取代成"ba"。

```{code-cell}
s.str.replace('f.', 'ba', regex=True)
```

## 時間欄位操作

**生成時間序列**

可以透過指定起始與結束日期來建立連續的日期序列：

```{code-cell}
pd.date_range(start='20231129', end='20231207')
```

也可以只指定起始日期，然後利用```periods```參數指定長度：

```{code-cell}
pd.date_range(start='20231129', periods=9)
```

也可以使用參數```freq="M"```：

```{code-cell}
pd.date_range(start='20230101', periods=12, freq='M')
```

但結果會是月底。可改用```DateOffset```物件來處理：

```{code-cell}
from pandas.tseries.offsets import DateOffset

pd.date_range(start='20230101', periods=12, freq=DateOffset(months=1))
```

若是要固定隔n天(或n週, n秒等等)，可以用```Timedelta```物件來處理：

```{code-cell}
pd.date_range(start='20230101', end='20231231', freq=pd.Timedelta(days=15))
```

**計算日期**

利用```Timedelta```物件來處理：

```{code-cell}
pd.to_datetime("19930103", format='%Y%m%d') - pd.Timedelta(days=765)
```

```DateOffset```物件同樣可以做到：

```{code-cell}
pd.to_datetime("19930103", format='%Y%m%d') - DateOffset(days=765)
```

**計算兩個日期之間的天數**

其實就是直接相減就可以了：

```{code-cell}
(pd.to_datetime("20240314", format='%Y%m%d') - 
 pd.to_datetime("20200524", format='%Y%m%d')).days
```

**時間轉文字**

首先透過```pd.date_range```產生日期的時間序列。

```{code-cell}
dates = pd.date_range(start='20231129', end='20231207')

print(dates)
```

可以透過以下方法將時間轉成文字格式

```{code-cell}
dates.strftime('%Y%m%d')
```

**文字轉時間**

先將上一步的結果借來用。

```{code-cell}
dates = dates.strftime('%Y%m%d')
```

需要使用的方法是```pd.to_datetime()```

```{code-cell}
pd.to_datetime(dates, format='%Y%m%d')
```

這邊時間日期的表示法與python內建的表示法相同，可參考前面的章節。
