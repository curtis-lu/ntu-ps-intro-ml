# Python 1: variables and data types

<aside>
💡 情境：
list → to-do list
tuple → questionnaire
dict → menu
set → categories

</aside>

# 資料與變數

## **Python的基本資料型態**

Python常用的資料型態基本上就如下表列這幾種。

其中bool、int、float、str是最基本的資料型態。

而list, tuple, dict, set可以理解成用不同方式來組織**"物件"**的資料型態，各有各的特點和使用情境。

後面會說明list, tuple, dict, set的使用方式。

| type | example |
| --- | --- |
| bool | True, False |
| int | 42, 19, 112 |
| float | 3.14, 0.13 |
| str | 'Hi', "Hello", '''Bye!''', """Bye!""" |
| list | ['a', 'b', 'c', 'd', 'a'] |
| tuple | ('a', 'b') |
| dict | {'a': 1, 'b': 2} |
| set | {'a', 'b', 'c'} |

```python
# "type()" -> function ; "True" -> function input (or argument)
# type() 可以用來檢查資料型別

>>> type(True)
bool
>>> type(42)
int
>>> type(3.14)
float

... # 以下忽略
```

## **Python的資料 = 物件**

如果把電腦的記憶體想像成很多排架子的倉庫，每排架子裡有很多個格子，

我們可以把一個物件當作是一個盒子，佔用了倉庫中某些格子的位置。

[img]

也就是說，物件佔用記憶體的一部分位置，儲存了以下元素：

- 物件的id，也就是唯一識別碼，指出了在倉庫中的位置。
- 物件的型態，說明物件可以做什麼。
- 物件的值，也就是物件的內容。

[img]

## 什麼是**變數**

**概念**

變數(variable)是物件的名稱，可以想像是盒子的標籤。

[img]

**變數名稱的規則**

- a-z, A-Z, 0-9, _
- 區分大小寫，apple ≠ APPLE ≠ Apple
- 開頭不能使用數字。
- 以底線開頭的變數有特殊意義。
- 不能是python保留字。
    - [link] [Python Keywords: An Introduction – Real Python](https://realpython.com/python-keywords/)
    - 在程式中查看：
        
        ```python
        >>> help("keywords")
        ```
        

**變數的賦值**

```python
>>> a = 42
>>> b = 'Hello'
>>> c = [1, 2, 3]
>>> print(a)
42
>>> print(b)
Hello
>>> print(c)
[1, 2, 3]
```

```python
>>> a = b = c = 0
>>> print(a)
0
>>> print(b)
0
>>> print(c)
0
```

```python
>>> a, b = 1, 2
>>> print(a)
1
>>> print(b)
2
```

# 基礎資料型別操作與運算

## **布林**

可以透過bool()將其他資料轉換為布林型態。

```python
>>> bool(1)
True
>>> bool(1.0)
True
>>> bool(100.001)
True
>>> bool(-1)
True
>>> bool('a')
True
>>> bool(0)
False
>>> bool(0.0)
False
```

```python
>>> True and True
True
>>> True and False
False
>>> True or True
True
>>> True or False
True
>>> False and False
False
>>> False or False
False
```

## **整數與浮點數**

可以透過int()和float()做資料型別轉換。

```python
>>> int(True)
1
>>> float(True)
1.0
>>> float('1')
1.0
```

**運算子**

| 運算子 | 說明 | 範例 | 結果 |
| --- | --- | --- | --- |
| + | 加法 | 1+1 | 2 |
| - | 減法 | 100 - 1 | 99 |
| * | 乘法 | 3 * 2 | 6 |
| / | 除法 | 9 / 2 | 4.5 |
| // | 取商數 | 9 // 2 | 4 |
| % | 取餘數 | 9 % 2 | 1 |
| ** | 次方 | 2 ** 3 | 8 |

## **字串**

可以透過str()來將資料轉換為字串。

```python
>>> str(42.0)
'42.0'
>>> str(True)
'True'
```

**'\'轉譯**

\n代表換行；\t代表tab；\\則代表\

```python
>>> print('a\nbc')
a
bc
>>> print('a\tbc')
a    bc
>>> print('a\\bc')
a\bc
```

原始字串r''會取消\轉譯，常用於表示window作業系統的路徑。

```python
>>> print(r'a\nbc')
a\nbc
```

**f-strings**

f-strings可以將變數值帶入字串，或是用來格式化。

[link] [Guide to String Formatting in Python Using F-strings | Built In](https://builtin.com/data-science/python-f-string)

```python
>>> name = 'Curtis'
>>> print(f'Hi, {name}!')
Hi, Curtis!

>>> number = 12.3456789
>>> print( f'{number:.2f}')
12.35
>>> print( f'{number:%}')
1234.56789%
>>> print( f'{number:,%}')
1,234.56789%
>>> print( f'{number:,.2%}')
1,234.57%

>>> first_name = 'Curtis'
>>> last_name = 'Lu'
>>> print(f'{first_name + last_name}')
CurtisLu
```

**字串的切片**

```python
>>> letters = 'abcdefghijklmnopqrstuvwxyz'
>>> letters[0]
'a'
>>> letters[-1]
'z'
>>> letters[0:2]
'ab'
>>> letters[-2:]
'yz'
>>> letters[0:6:2]
'ace'
>>> letters[::-1]
'zyxwvutsrqponmlkjihgfedcba'
```

**判斷字串是否包含字串**

```python
>>> 'a' in 'apple'
True
```

**字串的加法與乘法運算**

```python
>>> '1' + '2'
'12'
>>> '1' * 5
'11111'
>>> '1' + '2' * 5
'122222'
```

**字串的拆分(.split())**

這邊用到一個新的物件操作方法，即在一個字串型別的變數後面加上".”然後用呼叫方法名稱，例如一個拆分字串的方法".split()”，其中括弧內也可以輸入引數(argument)來指定該方法的使用邏輯：

```python
>>> name = 'National Taiwan University'
>>> name.split()
['National', 'Taiwan', 'University']
>>> name.split('a')
['N', 'tion', 'l T', 'iw', 'n University']
```

**字串的剝除str.strip()**

```python
>>> name = '    Curtis Lu    '
>>> name.strip()
'Curtis Lu'
>>> name = '    Curtis Lu....!!!    '
>>> name.strip(' .!')
'Curtis Lu'
```

**其他字串操作**

```python
>>> name = 'curtis lu'
>>> name.capitalize()
'Curtis lu'
>>> name.title()
'Curtis Lu'
>>> name.upper()
'CURTIS LU'
>>> name.lower()
'curtis lu'
>>> name.swapcase()
'CURTIS LU'
```

## **不同資料型別之間的運算**

```python
>>> 1 + 2.0
3.0
>>> True + 2.0
3.0
>>> True + 2
3
>>> '1' + 2
TypeError
```

# Python 主要資料型別

## list

是一種集合了多個元素且元素之間具有順序的資料型態。

list中的元素可以放任何物件，bool, int, float, str都可以，甚至是 list自己本身，所以當然就還包括dict, tuple, set……其他任何物件。

**建立list**

```python
# 以下兩種皆可
>>> a_list = ['a','b','c']
>>> a_list = list(['a','b','c'])
```

```python
# list中的list
>>> a_list = ['a', 'b', 'c', [1, 2, 3]]
```

**切片(slice)**

```python
>>> a_list = ['a','b','c']
>>> a_list[0]
'a'
>>> a_list[-1]
'c'
>>> a_list[1:]
['b', 'c']
>>> a_list[0:1]
['a']
>>> a_list[3]
IndexError
>>> a_list[::-1]
['c', 'b', 'a']
>>> print(a_list)
['a','b','c']

>>> a_list = ['a', 'b', 'c', [1, 2, 3]]
>>> a_list[3][2]
3
```

**利用切片為list賦值**

```python
>>> a_list = ['a','b','c']
>>> a_list[0] = 1
>>> print(a_list)
[1, 'b', 'c']
>>> a_list[1:] = 2, 3 # (2, 3) or [2, 3] or {2, 3} 皆可
>>> print(a_list)
[1, 2, 3]
```

**取得list中元素的位置(.index())**

```python
>>> a_list = ['a','b','c']
>>> a_list.index('a')
0
>>> a_list.index('d')
ValueError
```

**在尾端加入元素(.append())**

```python
>>> a_list = ['a','b','c']
>>> a_list.append('d')  # 直接會改變list的內容 且不回傳結果
>>> print(a_list)
['a', 'b', 'c', 'd']
```

**刪除list中的元素(.remove())**

```python
>>> a_list = ['a','b','c', 'd', 'e']
>>> a_list.remove('a') # 直接會改變list的內容 且不回傳結果
>>> print(a)
['b', 'c', 'd', 'e']

# 另外一種方式
>>> a_list.pop() # 會取出最後一個元素回傳並從list中刪除
'e'
>>> print(a_list)
['b', 'c', 'd']
>>> a_list.pop(0)
'b'
>>> print(a_list)
['c', 'd']

# 還有另外一種方式
>>> del a_list[1] # del 事實上並非是list方法，而是python內建方法，會將物件和名稱分開。
>>> print(a_list)
['c']
```

**檢查元素是否存在(in)**

```python
>>> a_list = ['a','b','c']
>>> 'a' in a_list
True
>>> 'd' in a_list
False
```

**list中元素的排序**

```python
>>> a_list = ['b','d','a','c','e']
>>> a_list.sort() # 直接會改變list的內容 且不回傳結果
>>> print(a_list)
['a', 'b', 'c', 'd', 'e']
>>> a_list.sort(reverse=True) # 降冪排列
>>> print(a_list)
['e', 'd', 'c', 'b', 'a']

>>> a_list = ['b','d','a','c','e']
>>> sorted(a_list) # 只會回傳資料的「副本」
['a', 'b', 'c', 'd', 'e']
>>> print(a_list)
['b','d','a','c','e']
```

**取得list的長度**

```python
>>> a_list = [1,2,3]
>>> len(a_list)
3
```

**list的加法與乘法運算**

```python
>>> a_list = ['a','b','c']
>>> d_list = ['d','e','f']
>>> a_list + d_list
['a', 'b', 'c', 'd', 'e', 'f']

# 補充：用append的話，argument會被當作元素加到list最尾端
>>> a_list.append(d_list) # 直接會改變list的內容且不回傳結果
>>> print(a_list)
['a', 'b', 'c', ['d', 'e', 'f']]

>>> a_list = ['a','b','c']
>>> a_list * 2
['a', 'b', 'c', 'a', 'b', 'c']

```

**list的其他方法**

```python
# .reverse()
>>> a_list = ['a','b','c']
>>> a_list.reverse() # 直接會改變list的順序 且不回傳結果
>>> print(a_list)
['c', 'b', 'a']

# .extend()
>>> a_list = ['a','b','c']
>>> d_list = ['d','e','f']
>>> a_list.extend(d_list) # 直接會改變list的內容 且不回傳結果
['a', 'b', 'c', 'd', 'e', 'f']

# .clear()
>>> a_list = ['a','b','c']
>>> a_list.clear() # 直接會改變list的內容 且不回傳結果
>>> print(a_list)
[]

# .count()
>>> a_list = ['a','b','c','a']
>>> a_list.count('a')
2
```

## tuple

tuple是

**建立tuple**

```python
>>> a_tuple = (1, 2, 3)
>>> a_tuple = tuple([1,2,3])
>>> a_tuple = 1, 2, 3
```

**切片(slice)**

```python
>>> a_tuple = (1, 2, 3)
>>> a_tuple[0]
1
>>> a_tuple[1:]
(2, 3)
>>> a_tuple[0:1]
(1,)
```

**tuple unpacking**

```python
>>> name = ('Frieren', 'Fern', 'Stark')
>>> a, b, c = name
>>> print(a)
'Frieren'
>>> print(b)
'Fern'
>>> print(c)
'Stark'
```

**檢查元素是否存在(in)**

```python
>>> a_tuple = (1,2,3)
>>> 4 in a_tuple
False
```

**取得tuple的長度**

```python
>>> a_tuple = (1,2,3)
>>> len(a_tuple)
3
```

**tuple的加法與乘法運算**

```python
>>> a_tuple = (1,2,3)
>>> b_tuple = (4,5,6)
>>> a_tuple + b_tuple
(1, 2, 3, 4, 5, 6)

>>> a_tuple * 2
(1, 2, 3, 1, 2, 3)

```

**tuple與list的差異**

不能切片後賦值。

不能新增元素，沒有類似.append()或.insert()方法，要增加元素只能新建一個tuple。

沒有sort方法。

```python
# 不能切片後賦值
>>> a_list = [1,2,3]
>>> a_list[0] = 'a'

>>> a_tuple = (1,2,3)
>>> a_tuple[0] = 'a'
TypeError

# 不能新增元素，沒有append()方法，要增加元素只能新建一個tuple
>>> a_tuple = (1,2,3)
>>> id(a_tuple) # 物件的唯一識別碼
4414453632
>>> a_tuple = a_tuple + (4,)
>>> id(a_tuple)
4415677888

>>> a_list = [1,2,3]
>>> id(a_list)
4415648320
>>> a_list.append(4)
>>> id(a_list)
4415648320

# 沒有sort方法，要排序tuple中的元素的話，可以用sorted內建函數，但回傳的是一個list
>>> a_tuple = (4,5,3,1,2)
>>> sorted(a_tuple)
[1, 2, 3, 4, 5]

```

## dict

dict是dictionary的略稱，字典這種資料型態的組成是一群成對的key與value。

key在字典裡不能重複，通常會使用字串或tuple來當作key。至於value則沒有什麼限制。

dict跟後面等等會介紹的set一樣，都是沒有順序的，不能使用切片。

**建立dict**

```python
>>> a_dict = {'a': 1, 'b': 2, 'c':3}
>>> a_dict = dict(a=1, b=2, c=3)
```

**dict的基本用法**

```python
>>> a_dict['a']
1
>>> a_dict.get('a')
1
>>> a_dict['d']
KeyError
>>> a_dict.get('d') # 不會回傳任何東西
>>> a_dict.get('d', 'missing')
'missing'
```

**dict新增項目**

```python
>>> a_dict = {'a': 1, 'b': 2, 'c':3}
>>> a_dict['d'] = 4
>>> print(a_dict)
{'a': 1, 'b': 2, 'c': 3, 'd': 4}
>>> a_dict.update({'e': 5})
>>> print(a_dict)
{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

**dict刪除項目**

```python
>>> a_dict = {'a': 1, 'b': 2, 'c':3}

# 方法1
>>> a_dict.pop('a')
1
>>> print(a_dict)
{'b': 2, 'c': 3}
>>> a_dict.pop('d')
KeyError
>>> a_dict.pop('d', 'Nothing happened')
'Nothing happened'
>>> print(a_dict)
{'b': 2, 'c': 3}

# 方法2
>>> del a_dict['b']
>>> print(a_dict)
{'c': 3}
```

**dict中的key值不能重複**

當要新增項目時，dict中對應的key值若已經存在，會以新的為準。

```python
>>> a_dict = {'a': 1, 'b': 2, 'c':3}
>>> b_dict = {'b': 'ni', 'c': 'san', 'd': 'shi'}
>>> a_dict.update(b_dict)
>>> print(a_dict)
{'a': 1, 'b': 'ni', 'c': 'san', 'd': 'shi'}
```

**取得dict的內容**

```python
>>> a_dict = {'a': 1, 'b': 2, 'c':3}

# 取得key的list
>>> list(a_dict.keys())
['a', 'b', 'c']

# 取得value的list
>>> list(a_dict.values())
[1, 2, 3]

# 取得key跟value的list，注意到其中是一個一個tuple
>>> list(a_dict.items())
[('a', 1), ('b', 2), ('c', 3)]
```

**取得dict長度**

```python
# 指的是key的數量
>>> a_dict = {'a': 1, 'b': 2, 'c':3}
>>> len(a_dict)
3
```

**檢查key是否存在**

```python
>>> a_dict = {'a': 1, 'b': 2, 'c':3}
>>> 'c' in a_dict
True
```

## set

set是具有不重複元素的一種資料型別。

操作跟數學上的集合很像。

set沒有順序的概念，因此不支援切片。

**建立set**

```python
>>> a = {1,2,3}
>>> a = set([1,2,3])
```

**加入元素至set**

```python
>>> a = {1,2,3}
>>> a.add(4)
>>> print(a)
{1,2,3,4}

# .add()已經存在的元素不會改變set。
>>> a.add(1) 
>>> print(a) 
{1,2,3,4}
```

**刪除set中的元素**

```python
>>> a = {1,2,3}
>>> a.remove(1)
>>> print(a)
{2, 3}
```

**取得set的長度**

```python
>>> a = {1,2,3}
>>> len(a)
3
```

**set的運算**

```python
>>> a = {1,2,3}
>>> b = {3,4,5}
>>> c = {1,2,3,4,5}

# 交集
>>> a & b
{3}
>>> a.intersection(b)
{3}

# 聯集
>>> a | b
{1, 2, 3, 4, 5}
>>> a.union(b)
{1, 2, 3, 4, 5}

# 差集
>>> a - b
{1, 2}
>>> a.difference(b)
{1, 2}

>>> b - a
{4, 5}
>>> b.difference(a)
{4, 5}

# 取互斥的元素
>>> a ^ b
{1, 2, 4, 5}
>>> a.symmetric_difference(b)
{1, 2, 4, 5}

# 判斷是否為子集合
>>> a <= b
False
>>> a.issubset(b)
False

# 判斷是否為真子集
>>> a < b
False

# 判斷是否為超集合
>>> a >= b
False
>>> a.isiperset(b)
False

# 判斷是否為真超集
>>> a > b
False
```

# 進階主題

## 資料型別的可變與不可變

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

**不可變**

```python
>>> x = 5
>>> print(x)
5
>>> y = x
>>> print(y)
5
>>> x = 29
>>> print(x)
29
>>> print(y)
5
```

```python
>>> name = 'Curtis'
>>> name[0] = 'K'
TypeError
```

**可變**

```python
>>> x = [1, 2, 3]
>>> y = x
>>> x[0] = 'a'
>>> print(y)
['a', 2, 3]
```

## 資料型態統整

參考資料

introducing python: modern computing in simple packages