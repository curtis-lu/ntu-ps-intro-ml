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

# 5.function

## function

所謂function是接收一些輸入，然後做一些事情，最後吐出輸出。

自定義一個function，對於資料分析來說也是很實用的。

不僅在資料處理與分析的過程中，可以避免一直重複複製貼上同一段語法。

再者也可以有效地組織你的程式，讓程式的結構更清楚，更好維護。

### **定義function**

- 要使用```def```關鍵字
- 為function命名
- function名稱後面接著一個```()```，裡面放「參數(parameter)」，也就是function的輸入。
- ```()```後面接著一個冒號
- 下方縮排4個空格，就是function中處理邏輯的區塊。
- 最後一行是```return```關鍵字，設定要輸出的東西。

```{code-cell}
def my_sum(a, b, c):
    result = a + b + c
    return result
```

### **使用function**

- 實際呼叫function時，只要在function的```()```中放入function需要的輸入，就可以執行了。
- 呼叫時傳入的東西稱作**引數(argument)**，在定義時則稱作**參數(parameter)**。

**位置引數(positional argument)**

不指定參數名稱直接呼叫函數的話，稱作位置引數(positional argument)：

```{code-cell}
my_sum(1, 2, 3)
```

**關鍵字引數(keyword argument)**

如果指定參數名稱的話，稱作關鍵字引數(keyword argument)：

```{code-cell}
my_sum(a=1, b=2, c=3)
```

位置引數跟關鍵字引數可以混用，但**位置引數必須在關鍵字引數**前面：

```{code-cell}
# 合法
my_sum(1, 2, c=3)

# 不合法
my_sum(a=1, 2, 3)
```

**任意引數：*args & **kwargs**

當想要function可以傳入不固定的參數數量時，可以使用可變動參數。

其中```*args```代表位置引數，```**kwargs```代表關鍵字引數。

***args用法範例如下：**

```{code-cell}
def my_sum(*args):
    result = 0
    for number in args:
        result += number
    return result

my_sum(1,2,3,4,5)
```

```{code-cell}
my_sum(1,2,3,4,5,6,7,8,9,10)
```

****kwargs用法範例如下：**

```{code-cell}
def my_func(**kwargs):
    for k, v in kwargs:
        print(f'{k} -> {v}'
    return None

my_func(a=1, b=2, c=3)
```

```{code-cell}
my_func(a=1, b=2, c=3, d=4, e=5)
```

多種參數可以混用：

```{code-cell}
def my_func(a, b, *args, **kwargs):
    print(f'I am standard arg: {a} & {b}')
    print(f'I am positional arg: {args}')
    print(f'I am keyword arg: {kwargs}')
    return None
```

```{code-cell}
my_func(1, 2, c=3)
```

```{code-cell}
my_func(1, 2, 3, d=4, e=5)
```

由上面的例子可以看到，*args是傳入一個tuple，**kwargs則是傳入一個dict。

### **參數預設值(default parameter)**

可以在function定義時，指定參數的預設值，使用該function時就可以不必輸入引數傳入。

```{code-cell}
def my_sum(a, b, c=3):
    result = a + b + c
    return result

my_sum(4, 5)
```

```{code-cell}
my_sum(4, 5, 6)
```

注意，有指定預設值的參數必定是在位置引數之後，否則會搞不清楚引數傳入的順序。

```{code-cell}
# 不合法
def my_sum(a=1, b, c):
    result = a + b + c
    return result
```

注意，可變物件作為參數預設值可能會產生問題。

例如以下程式，假設我們希望這個function當沒有指定list時就初始化一個空的list，然後就把元素append進去：

```{code-cell}
def append_element(e, mylist=[]):
    mylist.append(e)
    return mylist
```

執行第一次，輸出了一個具有單一元素’a’的list，符合預期：

```{code-cell}
append_element(e='a')
```

但執行第二次時，理論上沒有傳入一個list的話，我們預期這個function應該是先產生一個空的list然後再append元素。但實際上的結果是保留了上一次append進去的元素’a’：

```{code-cell}
append_element(e='b')
```

之所以會這樣的原因是，function的預設值在定義階段就會被創建，後續使用時不會再重新創建。

所以第一次執行function時，mylist就已經被建立，後續重新呼叫function並不會再重新創建。

又因為list是一種可變物件，可以不斷把東西塞進去。

可以看到以下程式，每次呼叫時，mylist的id都一樣，代表物件沒有被重新創建。

```{code-cell}
def append_element(e, mylist=[]):
    mylist.append(e)
    print(id(mylist))
    return mylist

append_element(e='a')
```

兩次的id是一樣的。

```{code-cell}
append_element(e='b')
```

解決方法是用None代替空的可變物件。

```{code-cell}
def append_element(e, mylist=None):
    if my_list is None:
        mylist = []
    mylist.append(e)
    print(id(mylist))
    return mylist

append_element(e='a')
```

可以看到不會保留上一次執行時append進去的元素了，而且兩個id是不樣的，代表物件每次呼叫時有被重新創建。

```{code-cell}
append_element(e='b')
```

### 特殊參數

**僅限位置參數 (Positional-Only Parameters)**

只要在參數中加入"/"就可以限制**"/"前的引數都必須是位置引數**。

```{code-cell}
def pos_only_func(a, b, c, /):
    print(a, b, c)
```

```{code-cell}
# 合法
pos_only_func(1, 2, 3)

# 不合法
pos_only_func(1, 2, c=3)
```

**僅限關鍵字引數 (Keyword-Only Arguments)**

只要在參數中加入"*"就可以限制**"*"後的引數都必須是關鍵字引數**。

```{code-cell}
def kwd_only_func(*, f, g):
    print(f, g)
```

```{code-cell}
# 合法
kwd_only_func(f=6, g=7)

# 不合法
kwd_only_func(6, g=7)
```

兩者可以混用：

```{code-cell}
def combined_func(a, b, c, /, d, e, *, f, g):
    print(a, b, c, d, e, f, g)
```

```{code-cell}
combined_func(1, 2, 3, 4, e=5, f=6, g=7)
```

此外，*args後面的參數也都是僅限關鍵字引數(Keyword-Only Arguments)。

```{code-cell}
def example_func(*args, c, d):
    print(args)
    print(c, d)

# 合法
example_func(1, 2, c=3, d=4)
```

```{code-cell}
# 不合法
example_func(1, 2, 3, 4)
```

## 匿名函數：lambda運算式

### 基本操作

之所以稱做匿名函數是因為lambda運算式不需要定義function的名稱就可以使用。

例如以下是一個取平方的function：

```{code-cell}
def square(x):
    return x**2
```

如果用lambda運算式來寫的話，如以下：

```{code-cell}
lambda x: x**2
```

呼叫的方式：

```{code-cell}
(lambda x: x**2)(3)
```

### **實用的情境**

假設一個list如下，如何根據tuple第二位來排序？

```{code-cell}
a_list = [('b', 1), ('C', 5), ('a', 2), ('c', 4), ('B', 3), ('A', 6)]
```

一般的```sorted```會根據第一個元素來排序，元素的資料型別是```str```的話，就會根據字母順序來排序（大寫在前）。

```{code-cell}
sorted(a_list)
```

可以用```sorted()```的參數```key```搭配lambda function：

```{code-cell}
sorted(a_list, key=lambda e: e[1])
```

不使用lambda的話就是要寫比較多的code，而且並不這麼一目了然：

```{code-cell}
def get_second_element(data: tuple):
    return data[1]

sorted(a_list, key=get_second_element)
```

## 定義function的好習慣

### **動態型別(dynamic types)與靜態型別(static type)**

在python中，變數的資料型別是**動態型別(dynamic types)**，意思是：

1. 定義變數時不需要宣告變數的資料型別，而是直接賦值就可以了，而且，
2. 如果要改變型別只要重新賦值即可。

相反地，所謂**靜態型別(static type)**，就是指變數賦值前必須先宣告好資料型別，而且一旦宣告後，除非重新宣告其他型別，否則用其他型別資料賦值就會產生錯誤。

```{code-cell}
myvar = 1
myvar = '1'
myvar = True
```

這樣感覺起來似乎動態型別方便多了！不必像靜態型別一樣有許多限制。事實上，限制帶來的是穩定性與可預測性，比較能夠避免程式出現意料之外的錯誤。

因此，雖然python是動態型別，但仍創造了所謂**型別提示(type hint)**的功能。使用方式如下：

```{code-cell}
is_student: bool = True
age: int = 24
a_list: list[str] = ['a','b','c'] # python 3.9以上支援
a_set: set[int] = {1, 2} # python 3.9以上支援
```

事實上，**型別提示(type hint)**只是提示，python並不會對type hint做任何事，所以即使用錯了型別也不會報錯。

但其實有套件可以替我們做檢查（例如mypy），檢查的方式例如，明明myvar的type hint是str型別，但卻在程式某個地方對myvar使用了.append()方法等等。

總而言之，使用**型別提示(type hint)**可以讓程式更可讀也更好維護。

### **在函式定義中使用型別提示(type hint)**

1. 直接在參數後面寫入資料型別
2. ' -> ' 後面代表的是return的資料型別。

```{code-cell}
def lucky_number(name: str, birthday: str = '19000101') -> int:
    sum_n = 0
    for n in birthday:
        sum_n += int(n)

    return sum_n + len(name)
```

詳細入門介紹請參考：

[Modernize Your Sinful Python Code with Beautiful Type Hints | by Eirik Berge, PhD | Towards Data Science](https://towardsdatascience.com/modernize-your-sinful-python-code-with-beautiful-type-hints-4e72e98f6bf1)

## Documentation

> “Code is more often read than written.”
> 
> 
> — *Guido van Rossum*
> 

程式的易讀性是python哲學中非常重要的一部分，程式有好的文件才會有人願意使用。

例如```pandas```和```scikit-learn```的文件都非常優秀，讓使用者可以找到清楚的說明與範例。

好的文件同時也是為了避免未來的自己或是你的同事，陷入痛苦的困惑當中。

### 為程式加入註解

'#' 是註解符號，符號後面的文字會忽略，不執行。

以下範例來自<精通無瑕程式碼>：

```{code-cell}
import re

text = '''
    Ha! let me see her: out, alas! She's cold:
    Her blood is settled, and her joints are stiff;
    Life and these lips have long been separated:
    Death lies on her like an ultimely frost
    Upon the sweetest flower of all the field.
'''

f_words = re.findall('\\bf\w+\\b', text)
print(f_words)

l_words = re.findall('\\bl\w+\\b', text)
print(l_words)
```

以上程式片段對於不熟悉正規表達式的人，會很難以理解程式碼的作用。

但加上一些註解後就可以消除困惑：

```{code-cell}
import re

text = '''
    Ha! let me see her: out, alas! She's cold:
    Her blood is settled, and her joints are stiff;
    Life and these lips have long been separated:
    Death lies on her like an ultimely frost
    Upon the sweetest flower of all the field.
'''

# 找出所有'f'開頭的字元
f_words = re.findall('\\bf\w+\\b', text)
print(f_words)

# 找出所有'l'開頭的字元
l_words = re.findall('\\bl\w+\\b', text)
print(l_words)
```

但是也要避免不必要的註解，以下範例一樣來自<精通無瑕程式碼>：

```{code-cell}
investments = 10000 # 你的投資金額，必要時可改
yearly_return = 0.1 # 年報酬率
years = 10 # 複利計算的年數

# 逐年計算
for year in range(years):
    # 列印今年的投資價值
    print(investments * (1 + yearly_return)**year)
```

因為很多東西都是顯而易見的，有意義的變數名稱就已經可以說明許多事情，就不必要再做行內註解。（行內註解意思是在同一行程式碼的後面直接加上註解）

而且該知道的常識也沒必要註解，例如大家都知道for迴圈就是為了逐個計算，就不必註解for迴圈的功能。

### Docstring

```docstring```指的是function的```def```敘述句之後的說明片段。範例如下：

```{code-cell}

def my_func(*args, **kwargs):
    '''convert probability to log odds (logit).
    '''
    print(f'Positional args: {args}')
    print(f'Keyword args: {kwargs}')

```

有了docstring後，就可以使用以下方式來讀取你撰寫的說明：

```{code-cell}
print(help(my_func))
```

或是：

```{code-cell}
print(my_func.__doc__)
```

[Defining Your Own Python Function – Real Python](https://realpython.com/defining-your-own-python-function/#function-calls-and-definition)
