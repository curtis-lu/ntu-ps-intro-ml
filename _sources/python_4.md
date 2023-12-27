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

# 4.流程控制

之前的內容主要在介紹python的資料型態和相關操作，
接下來的章節介紹的內容是讓程式動起來的基礎：邏輯判斷以及流程控制。
瞭解這些後就可以進入實戰，做到很多事情！

## if, elif, else

### 基本結構

if 陳述式的最簡單結構如下方。
- 當if陳述句的條件判斷為True的話就會做if區塊指定的事情，
- 如果不為```True```，就接著判斷```elif```敘述，由上而下照順序判斷。
- 如果都不為```True```就會跑到```else```敘述，做else區塊的事情。
- if, elif, else句子的最後方都需要冒號。
- 冒號下面的區塊必須縮排(indentation)，且必須有相同的格數。通常是用4個空格，而非tab鍵。
- Python利用縮排來判斷程式結構，所以不能弄亂縮排的格數。

```{code-cell}
score = 88

if score >= 85:
    print('do task 1')
elif score >= 70:
    print('do task 2')
else:
    print('do task 3')
```

**elif 敘述可以有多個**

```{code-cell}
score = 88

if score >= 90:
    print('do task 1')
elif score >= 80:
    print('do task 2')
elif score >= 70:
    print('do task 3')
else:
    print('do task 4')
```

**elif & else 都是非必要的**

```{code-cell}
score = 88

if score >= 70:
    print('do something')
```

### 條件判斷的形式

陳述句中的條件判斷可以放很多種形式，
不只```>=```, ```==```等比較運算子。
基本上只要能產生布林值的都可以：

**布林值本身：**

```True```

**比較運算子：**

```==```, ```!=```, ```<```, ```<=```, ```>```, ```>=```

**邏輯運算子**

```and```, ```or```, ```not```

**成員運算子**

```in``` ,  ```not in```

成員運算子的使用方式如下：

```{code-cell}
sentence = 'I am a sentence'

if 'o' in sentence:
    print('There is an o')
else:
    print('There is no o')
```

```{code-cell}
a_list = [1, 2, 3, 5, 6]

if 4 in a_list:
    print('There is a 4')
```

```{code-cell}
a_set = ['a', 'b', 'c', 'd', 'e']

if 'f' not in a_set:
    print('There is no f')

```

**以下東西都會被視為False：**

- 布林值 ```False```
- 空值 ```None```
- 整數 ```0```
- 浮點數 ```0.0```
- 空字串 ```‘’```
- 空list ```[]```
- 空tuple ```()```
- 空dict ```{}```
- 空set ```set()```

```{code-cell}
a_list = []

if a_list:
    print('do something')
else:
    print('the list is empty')
```

### Conditional expression

通常用在變數賦值，而非流程控制。語法結構如下：

```{code-cell}
age = 18

is_can_vote = True if age >= 18 else False
print(is_can_vote)
```

## for loop

在python中，很多東西都可以拿來迭代：
```list```, ```tuple```, ```set```, ```dict```, ```range```, ```enumerate```, ```zip```等等這些東西統稱為**可迭代物(iterable)**。

### 以list做迭代

以list做迭代的用法範例如下：

```{code-cell}
a_list = [1, 2, 3, 4, 5]

for i in a_list:
    print(i)
```

上面的```i```可以取任何名稱，通常會取一個好辨認的名稱，增加程式的可讀性。例如：

```{code-cell}
weekdays = ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']

for weekday in weekdays:
    print(weekday)
```

tuple, set跟list做迭代的方式很像，就不贅述了。

特別值得一提的是，依資料結構的不同特性，會適合不同的使用情境。

- list有順序，但可變。
- tuple有順序，但不可變。
- set無順序，且元素不重複。

### 以dict做迭代

dict的迭代方法如下：

**根據key迭代**

```{code-cell}
a_dict = {'a':1, 'b':2, 'c':3}

for k in a_dict.keys():
   print(k)
```

**根據value迭代**

```{code-cell}
a_dict = {'a':1, 'b':2, 'c':3}

for v in a_dict.values():
   print(v)
```

**key & value 同時**

```{code-cell}
a_dict = {'a':1, 'b':2, 'c':3}

for k, v in a_dict.items():
   print(k, v)
```

### 以enumerate做迭代

enumerate做迭代的方式其實跟list, set, tuple差不多，但使用enumerate可以多輸出index，有時候會比較方便。

enumerate裡面可以放任何iterable進去。

```{code-cell}
a_set = set(['a', 'b', 'c'])

for idx, e in enumerate(a_set):
    print(idx, e)

a_dict = {'a':1, 'b':2, 'c':3}

for idx, (k, v) in enumerate(a_dict):
    print(idx, k, v)
```

### 以range做迭代

range是一個專門用來做迭代的iterable，可以很方便地去迭代一個等差數列。
最簡單的使用方法如下，第一個參數是```start```，第二個參數是```end```：

```{code-cell}
for i in range(1, 10):
    print(i)
```

注意到，不會印出10，因為range的序列不會包含```end```(第二個參數)。

參數也可以省略```start```，但```start```的預設值就會是0

```{code-cell}
for i in range(10):
    print(i)
```

range也可以指定步伐(```step```)，當指定```step```時，就必須指定```start```。

```{code-cell}
for i in range(0, 20, 3):
    print(i)
```

```step```可以是負數，但若```step```為負數，```stop```須小於```start```。

```{code-cell}
for i in range(10, 1, -3):
    print(i)
```

### 利用zip迭代多個序列

若有多個序列想要一起做迭代，可以使用```zip```：

```{code-cell}
x_list = ['a', 'b', 'c']
y_list = [1, 2, 3]

for x, y in zip(x_list, y_list):
    print(x, y)
```

當```zip```起來的多個序列長短不一，會迭代短的元素結束為止：

```{code-cell}
longer = ['a', 'b', 'c', 'd', 'e']
shorter = ['a', 'b', 'c']

for l, s in zip(longer, shorter):
    print(l, s)
```

## break, continue 陳述式

**break**

當你想要迴圈符合某個條件時，馬上跳出迴圈，就可以使用```break```。

```{code-cell}
for i in range(0,5):
    if i == 3:
        break
    else:
        print(i)
    print('---')
```

**continue**

則是當想要迴圈符合某個條件時，就馬上跳到「下一個迴圈」時使用。

```{code-cell}
for i in range(0,5):
    if i == 3:
        continue
    else:
        print(i)
    print('---')
```

## while loop

當條件為```True```時就繼續執行迴圈，跟for loop一樣需有冒號和縮排。
千萬要記得設定好停止條件，
例如下方的```count <= 5```以及```count += 1```，
否則會導致無限迴圈。

```{code-cell}
count = 0
while count <= 5:
    print(count)
    count += 1 # 也可以寫成count = count + 1
```

**while loop中的else子句**

while loop中的```else```子句只有在loop正常結束完成時，才會執行。
例如以下程式，會去判斷```numbers```中是否有不被3整除的數字，
若有的話就會因為```break```而停止程式，
若沒有的話就會執行最後的```else```。

```{code-cell}
numbers = [3,6,9,12,15]
i = 0

while i < len(numbers):
    number = numbers[i]
    if number % 3 != 0:
        print(f'found strange number: {number}.')
        break
    i += 1
else:
    print('finished')
```

這邊```else```的概念跟```if, else```中的```else```，有點不一樣，
可以把這邊的```else```當作```break```的檢查器，只有當迴圈正常結束，沒有呼叫```break```時才會執行。

## try, except, finally陳述句

```try```與```except```主要的功能是做例外處理。

例如兩個數字相除，若分母為0，python會丟出錯誤。

```{code-cell}
a = 42
b = 0

print(a/b) # ZeroDivisionError: division by zero
```

可以使用```except```來捕捉錯誤做例外處理：

```{code-cell}
a = 42
b = 0

try:
    print(a/b)
except:
    print('infinite')
```

通常比較好的做法是會指定錯誤類型。

```{code-cell}
a = 42
b = 0

try:
    print(a/b)
except ZeroDivisionError:
    print('infinite')
```

然後根據不同可能的例外情境去使用不同的except敘述。

```{code-cell}
import math

a = 0
b = 42

try:
    print(a/b)
    print(math.log(a/b))
except ZeroDivisionError:
    print('infinite')
except ValueError:
    print('illegal number for log')
```

最後，finally敘述則無論程式是否丟出例外，都會執行。

```{code-cell}
a = 42
b = 0

try:
    print(a/b)
except ZeroDivisionError:
    print('infinite')
finally:
    print('run this no matter what.')
```