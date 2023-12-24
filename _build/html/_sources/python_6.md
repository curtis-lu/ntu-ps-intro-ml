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

# 6.進階主題

## 生成式(comprehension)

建立list, dict, set時，有一個十分常用的技巧稱作生成式(comprehension)。
生成式的語法比起使用迴圈簡潔許多，執行速度也比使用迴圈快。

### list comprehension

例如想建立一個內含1~10數字元素的list，該怎麼做？

可能想到的做法是先建立一個空的list，然後利用range()來append每個元素進去：

```{code-cell}
a_list = []

for i in range(1, 11):
    a_list.append(i)

print(a_list)
```

但一個符合python風格的寫法是透過生成式：

```{code-cell}
a_list = [i for i in range(1, 11)]

print(a_list)
```

生成式有許多靈活的應用方法：

例如，可以對每個元素做加工：

```{code-cell}
a_list = [i**2 for i in range(1, 11)]

print(a_list)
```

也可以對元素做篩選：

```{code-cell}
a_list = [i for i in range(1, 11) if i % 2 == 0]

print(a_list)
```

### dictionary comprehension

一個常用生成式來建立字典的時機是結合zip()：

```{code-cell}
keys = ['a','b','c']
values = [1, 2, 3]

a_dict = {k: v for k, v in zip(keys, values)}

print(a_dict)
```

或是當你想要交換key跟value的對應關係時：

```{code-cell}
reverse_a_dict = {v: k for k, v in a_dict.items()}

print(reverse_a_dict)
```

### set comprehension

set也有生成式，寫法如下：

```{code-cell}

a_set = {i // 4 for i in range(30)}

print(a_set)
```

## 裝飾器(decorator)

裝飾器的主要作用是為了在不更動function原始程式碼的情況下，添加或改變function的行為。

一個印出function執行時間的範例如下：

```{code-cell}
from datetime import datetime

def my_timer(func):
    def wrapper(*args, **kwargs):
        start = datetime.now()
        print(f'{func.__name__} starts at {start}')
        result = func(*args, **kwargs)
        end = datetime.now()
        print(f'{func.__name__} ends at {end}')
        print(f'total execution time: {end - start}')
        return result
    return wrapper
```

當你要使用這個裝飾器時，只要在目標function的定義陳述式上面加上”@”以及裝飾器名稱即可：

```{code-cell}
import time

@my_timer
def lazy_square(number):
    time.sleep(0.5)
    return number**2
```

使用該function時就會被添加該function原本沒有的功能：

```{code-cell}
lazy_square(99)
```

裝飾器的使用時機在於當想一次對多個function添加一些行為時，
如果不使用裝飾器的話就必須一個一個function去修改程式碼。
除了很麻煩以外，也容易造成錯誤。

以上就是裝飾器的基本用法，但這樣的做法會有個小問題。

請看以下程式：

```{code-cell}
help(lazy_square)
```

印出的結果是裝飾器中的wrapper()的名稱。

要解決這個問題必須使用python標準函式庫中的一個套件```functools```，
在內層的wrapper上面加上一個裝飾器```@functools.wraps()```：

```{code-cell}
from datetime import datetime
import functools # 加這行

def my_timer(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = datetime.now()
        print(f'{func.__name__} starts at {start}')
        result = func(*args, **kwargs)
        end = datetime.now()
        print(f'{func.__name__} ends at {end}')
        print(f'total execution time: {end - start}')
        return result
    return wrapper
```

重新定義一次function。

```{code-cell}
import time

@my_timer
def lazy_square(number):
    time.sleep(0.5)
    return number**2
```

檢查修改後的結果：

```{code-cell}
help(lazy_square)
```

## 名稱空間

我們知道變數名稱是一個標籤，貼在盒子（物件）上面，
當我們呼叫變數時，python會去取用盒子裡面的資料。

但是如果有多個一樣的變數名稱呢？到底要取用哪個物件就會造成混淆。

Python透過名稱空間（namespace）去界定變數名稱的搜尋範圍，
不同名稱空間有不同優先順序，在哪個空間先找到變數名稱，就去取用該變數名稱對應到的物件。

名稱空間依序如下：
- local
- enclosing
- global
- built-in

### 說明

**Build-in namespace**

Build-in namespace在python啟動時就會建立，直到python編譯器終止為止。

以下是python內建的變數名稱：

```{code-cell}
dir(__builtins__)
```

在定義變數時，要小心不要使用到這些變數名稱，否則會覆蓋掉。

**global namespace**

global namespace包含了在主程式中定義的變數名稱，所謂主程式可以先理解成就是正在使用中的jupyter notebook。

****The Local and Enclosing Namespaces****

至於local namespace就是function在執行時內部的變數名稱空間。

而enclosing namespace則是指當function是多層的時候，
例如雙層的function，外層function的namespace就是所謂的enclosing namespace。

請看下方釋例說明：

```{code-cell}
def outer():
    print('start outer function')

    def inner():
        print('>> start inner function')
        print('>> end inner function')

    enclosed()
    print('end outer function')

enclosing()
```

當我們呼叫outer()時，python會為outer建立新的namespace。
當outer內部呼叫inner()時，python也會為inner建立另一個獨立的namespace。

此時outer的namespace稱作enclosing namespace，
而inner的namespace則是local namespace。

### 範例

以下範例靈感主要來自於[Namespaces and Scope in Python – Real Python](https://realpython.com/python-namespaces-scope/#variable-scope)。

**範例一**

```x``` 定義在```f()```和```g()```的外面，所以x定義在global namespace中。

```{code-cell}
x = 'global'

def f():
    def g():
        print(x)
    g()

f()
```

**範例二**

```x``` 定義了兩次，一個定義在global namespace中，另一個則是在f()裡面，所以是enclosing namespace。

```{code-cell}
x = 'global'

def f():
    x = 'enclosing'

    def g():
        print(x)
    g()

f()
```

**範例三**

```x``` 定義了三次，一個在global namespace、一個在enclosing namespace，最後一個則是在g()裡面，所以是local namespace。

```{code-cell}
x = 'global'

def f():
    x = 'enclosing'

    def g():
        x = 'local'
        print(x)
    g()

f()
```

**範例四**

無法修改超出名稱空間範圍的變數。

例如我們定義了一個revise_x的function，裡面重新對x賦值，
但這邊的賦值行為只在local namespace中生效，並不會影響到global namespace。

```{code-cell}
x = 1

def revise_x():
    x = 2
    print(x)

revise_x()
print(x)
```

**範例五**

同樣地，在雙層的function中也是一樣的概念，裡面那層的賦值行為並不會影響到外面。

```{code-cell}
x = 1

def revise_x_outer():
    x = 2

    def revise_x_inner():
        x = 3
        print('local:', x)

    revise_x_inner()
    print('enclosing:', x)

revise_x_outer()
print('global:', x)
```

### 統整

在取用變數時，python會從local → enclosing → global → build-in逐層搜尋變數名稱。
如果變數名稱都搜尋不到的話，就會丟出```NameError```，說明變數並不存在。
在修改變數時，變數只在local namespace中作用，並不會影響到外面的namespace。

參考：

[Namespaces and Scope in Python – Real Python](https://realpython.com/python-namespaces-scope/)

