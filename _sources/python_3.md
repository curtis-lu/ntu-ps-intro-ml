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

# 3.資料型態：進階主題

## 資料型別的可變與不可變

之前講到在python中所有資料都是物件，而變數是物件的標籤。(請參考：[0.資料型態：bool, int, float, str]。)

我們將資料賦值到一個變數的這個過程就叫做**實例化(initialization)**。
例如：

```
pi = 3.1415926
```

我們把一個float物件實例化為pi這個變數，並把3.1415926賦值給該變數。
而所謂資料型態的可變或不可變，就是指當資料實例化後，它的值能不能夠被改變。

**資料型態與是否可變**

| type | example | immutable |
| --- | --- | --- |
| bool | True, False | immutable |
| int | 42, 19, 112 | immutable |
| float | 3.14, 0.13 | immutable |
| str | 'Hi', "Hello", '''Bye!''', """Bye!""" | immutable |
| list | ['a', 'b', 'c', 'd', 'a'] | mutable |
| tuple | ('a', 'b') | immutable |
| dict | {'a': 1, 'b': 2} | mutable |
| set | {'a', 'b', 'c'} | mutable |

**可變的資料型態**

```{code-cell}
a_list = [1, 2, 3]
a_list[0] = 'a'

print(a_list)
```

**不可變的資料型態**

```{code-cell}
a_tuple = (1, 2, 3)

a_tuple[0] = 'a' # tuple為不可變的資料型態
```

知道資料型態可不可變，so what?
事實上這決定了一些程式的行為。

請先看以下這段code：

```{code-cell}
x = [1, 2, 3]

y = x

x[0] = 'a'
```

我們知道list是可變的，請猜猜看print(y)的話會輸出什麼？
答案是：

```{code-cell}
print(y)
```

發現到雖然我們改變的是x，但y的內容也一併被改變了！

這是因為x和y都僅僅是一個list物件的標籤而已，
x和y都指向一開始值為[1,2,3]的list物件，
而list是可變的，
所以當我們改變list的內容時，
另一個貼著這個list的標籤y也就被影響了。

```{code-cell}
# x 和 y 指向同一個物件。

print(id(x))
print(id(y))
```

最後，list是可變的，而tuple是不可變的，

這也是為什麼對list做以下操作是合法的，對tuple卻不行。

```{code-cell}
a_list = [1, 2, 3]

# indexing後賦值
a_list[0] ='a'
a_list.append('d')

```

## 日期與時間

python內建的時間與日期處理，需要引入一個datetime”模組”。
所謂模組可以想像是一個函式庫，讓我們可以快速取用各種工具，就不用「自己造輪子了」。

要取用函式庫需要先做引入(import)的動作：

```{code-cell}
from datetime import datetime
```

以下語法會產生出一個datetime物件指出現在的時間，該物件儲存了年月日時分秒與毫秒。

```{code-cell}
today = datetime.now()

type(today)
print(today)
```

datetime物件的相關操作方法：

```{code-cell}
print(today.year)
print(today.month)
print(today.day)
print(today.hour)
print(today.minute)
print(today.second)
print(today.weekday())
```

其他建立datetime物件的方法：

```{code-cell}
birthday = datetime(2022, 11, 30)
print(birthday)
```

日期的運算需要利用datetime模組中另一個物件timedelta：

```{code-cell}
from datetime import datetime, timedelta

sign_on_dt = datetime(2024, 2, 1)

save_dt = sign_on_dt  + timedelta(days=180)

print(save_dt)
```

### 文字轉日期

常用的操作是把文字字串轉成日期型態再去做運算

```{code-cell}
today = '20231212'
the_big_day = '20200520'

today_dt = datetime.strptime(today, '%Y%m%d')
the_big_day_dt = datetime.strptime(the_big_day , '%Y%m%d')

days_together = today_dt - the_big_day_dt 

type(days_together)
print(days_together.days)
```

### 日期轉文字

```{code-cell}
today = datetime.now()

print(today.strftime('%Y%m%d'))

```

### 格式列表

| format | 描述 | 例子 |
| --- | --- | --- |
| %y | 補零后，以十進制數表示的，不帶世纪的年份。 | 00, 01, ..., 99 |
| %Y | 十進制數表示的帶世纪的年份。 | 0001, 0002, ..., 2013, 2014, ..., 9998, 9999 |
| %m | 以零填充的並以十進位數字表示的月份。 | 01, 02, ..., 12 |
| %d | 補零后，以十進制數顯示的月份中的一天。 | 01, 02, ..., 31 |
| %H | 以補零后的十進制数表示的小時（24 小時制）。 | 00, 01, ..., 23 |
| %I | 以補零后的十進制數表示的小時（12 小時制）。 | 01, 02, ..., 12 |
| %M | 補零后，以十進制數顯示的分鐘。 | 00, 01, ..., 59 |
| %S | 補零后，以十進制數顯示的秒。 | 00, 01, ..., 59 |
| %w | 以十進制數顯示的工作日，其中0表示星期日，6表示星期六。 | 0, 1, ..., 6 |
| %W | 以補零后的十進制數表示的一年中的週序號（星期一作為每週的第一天）。 在新的一年中第一個星期一之前的所有日子都被視為是在第 0 週。 | 00, 01, ..., 53 |
