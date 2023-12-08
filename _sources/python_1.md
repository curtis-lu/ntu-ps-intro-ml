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

# Python - 資料型態與變數(2)

## list

是一種集合了多個元素且元素之間具有順序的資料型態。

list中的元素可以放任何物件，bool, int, float, str都可以，甚至是 list自己本身，所以當然就還包括dict, tuple, set……其他任何物件。

**建立list**

```{code-cell}
# 以下兩種皆可

a_list = ['a','b','c']
a_list = list(['a','b','c'])
```

```{code-cell}
# list中的list

a_list = ['a', 'b', 'c', [1, 2, 3]]
```

**切片(slice)**

```{code-cell}
a_list = ['a','b','c']
print(a_list[0])
print(a_list[-1])
print(a_list[1:])
print(a_list[0:1])
print(a_list[3])
print(a_list[::-1])

a_list = ['a', 'b', 'c', [1, 2, 3]]
a_list[3][2]
```

**利用切片為list賦值**

```{code-cell}
a_list = ['a','b','c']
a_list[0] = 1
print(a_list)

a_list[1:] = 2, 3 # (2, 3) or [2, 3] or {2, 3} 皆可
print(a_list)
```

**取得list中元素的位置(.index())**

```{code-cell}
a_list = ['a','b','c']
print(a_list.index('a'))

print(a_list.index('d'))
```

**在尾端加入元素(.append())**

```{code-cell}
a_list = ['a','b','c']

a_list.append('d')  # 直接會改變list的內容 且不回傳結果
print(a_list)
```

**刪除list中的元素(.remove())**

```{code-cell}
a_list = ['a','b','c', 'd', 'e']

a_list.remove('a') # 直接會改變list的內容 且不回傳結果
```

```{code-cell}
# 另外一種方式
a_list.pop() # 會取出最後一個元素回傳並從list中刪除
```

```{code-cell}
print(a_list)
```

```{code-cell}
a_list.pop(0)
```

```{code-cell}
print(a_list)
```

```{code-cell}
# 還有另外一種方式
del a_list[1] # del 事實上並非是list方法，而是python內建方法，會將物件和名稱分開。
```

```{code-cell}
print(a_list)
```

**檢查元素是否存在(in)**

```{code-cell}
a_list = ['a','b','c']
```

```{code-cell}
'a' in a_list
```

```{code-cell}
'd' in a_list
```

**list中元素的排序**

```{code-cell}
a_list = ['b','d','a','c','e']
a_list.sort() # 直接會改變list的內容 且不回傳結果
print(a_list)
['a', 'b', 'c', 'd', 'e']
a_list.sort(reverse=True) # 降冪排列
print(a_list)
['e', 'd', 'c', 'b', 'a']

a_list = ['b','d','a','c','e']
sorted(a_list) # 只會回傳資料的「副本」
['a', 'b', 'c', 'd', 'e']
print(a_list)
['b','d','a','c','e']
```

**取得list的長度**

```{code-cell}
a_list = [1,2,3]
len(a_list)
3
```

**list的加法與乘法運算**

```{code-cell}
a_list = ['a','b','c']
d_list = ['d','e','f']
a_list + d_list
['a', 'b', 'c', 'd', 'e', 'f']

# 補充：用append的話，argument會被當作元素加到list最尾端
a_list.append(d_list) # 直接會改變list的內容且不回傳結果
print(a_list)
['a', 'b', 'c', ['d', 'e', 'f']]

a_list = ['a','b','c']
a_list * 2
['a', 'b', 'c', 'a', 'b', 'c']

```

**list的其他方法**

```{code-cell}
# .reverse()
a_list = ['a','b','c']
a_list.reverse() # 直接會改變list的順序 且不回傳結果
print(a_list)
['c', 'b', 'a']

# .extend()
a_list = ['a','b','c']
d_list = ['d','e','f']
a_list.extend(d_list) # 直接會改變list的內容 且不回傳結果
['a', 'b', 'c', 'd', 'e', 'f']

# .clear()
a_list = ['a','b','c']
a_list.clear() # 直接會改變list的內容 且不回傳結果
print(a_list)
[]

# .count()
a_list = ['a','b','c','a']
a_list.count('a')
2
```

## tuple

tuple是

**建立tuple**

```{code-cell}
a_tuple = (1, 2, 3)
a_tuple = tuple([1,2,3])
a_tuple = 1, 2, 3
```

**切片(slice)**

```{code-cell}
a_tuple = (1, 2, 3)
a_tuple[0]
1
a_tuple[1:]
(2, 3)
a_tuple[0:1]
(1,)
```

**tuple unpacking**

```{code-cell}
name = ('Frieren', 'Fern', 'Stark')
a, b, c = name
print(a)
'Frieren'
print(b)
'Fern'
print(c)
'Stark'
```

**檢查元素是否存在(in)**

```{code-cell}
a_tuple = (1,2,3)
4 in a_tuple
False
```

**取得tuple的長度**

```{code-cell}
a_tuple = (1,2,3)
len(a_tuple)
3
```

**tuple的加法與乘法運算**

```{code-cell}
a_tuple = (1,2,3)
b_tuple = (4,5,6)
a_tuple + b_tuple
(1, 2, 3, 4, 5, 6)

a_tuple * 2
(1, 2, 3, 1, 2, 3)

```

**tuple與list的差異**

不能切片後賦值。

不能新增元素，沒有類似.append()或.insert()方法，要增加元素只能新建一個tuple。

沒有sort方法。

```{code-cell}
# 不能切片後賦值
a_list = [1,2,3]
a_list[0] = 'a'

a_tuple = (1,2,3)
a_tuple[0] = 'a'
TypeError

# 不能新增元素，沒有append()方法，要增加元素只能新建一個tuple
a_tuple = (1,2,3)
id(a_tuple) # 物件的唯一識別碼
4414453632
a_tuple = a_tuple + (4,)
id(a_tuple)
4415677888

a_list = [1,2,3]
id(a_list)
4415648320
a_list.append(4)
id(a_list)
4415648320

# 沒有sort方法，要排序tuple中的元素的話，可以用sorted內建函數，但回傳的是一個list
a_tuple = (4,5,3,1,2)
sorted(a_tuple)
[1, 2, 3, 4, 5]

```

## dict

dict是dictionary的略稱，字典這種資料型態的組成是一群成對的key與value。

key在字典裡不能重複，通常會使用字串或tuple來當作key。至於value則沒有什麼限制。

dict跟後面等等會介紹的set一樣，都是沒有順序的，不能使用切片。

**建立dict**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
a_dict = dict(a=1, b=2, c=3)
```

**dict的基本用法**

```{code-cell}
a_dict['a']
1
a_dict.get('a')
1
a_dict['d']
KeyError
a_dict.get('d') # 不會回傳任何東西
a_dict.get('d', 'missing')
'missing'
```

**dict新增項目**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
a_dict['d'] = 4
print(a_dict)
{'a': 1, 'b': 2, 'c': 3, 'd': 4}
a_dict.update({'e': 5})
print(a_dict)
{'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
```

**dict刪除項目**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}

# 方法1
a_dict.pop('a')
1
print(a_dict)
{'b': 2, 'c': 3}
a_dict.pop('d')
KeyError
a_dict.pop('d', 'Nothing happened')
'Nothing happened'
print(a_dict)
{'b': 2, 'c': 3}

# 方法2
del a_dict['b']
print(a_dict)
{'c': 3}
```

**dict中的key值不能重複**

當要新增項目時，dict中對應的key值若已經存在，會以新的為準。

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
b_dict = {'b': 'ni', 'c': 'san', 'd': 'shi'}
a_dict.update(b_dict)
print(a_dict)
{'a': 1, 'b': 'ni', 'c': 'san', 'd': 'shi'}
```

**取得dict的內容**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}

# 取得key的list
list(a_dict.keys())
['a', 'b', 'c']

# 取得value的list
list(a_dict.values())
[1, 2, 3]

# 取得key跟value的list，注意到其中是一個一個tuple
list(a_dict.items())
[('a', 1), ('b', 2), ('c', 3)]
```

**取得dict長度**

```{code-cell}
# 指的是key的數量
a_dict = {'a': 1, 'b': 2, 'c':3}
len(a_dict)
3
```

**檢查key是否存在**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
'c' in a_dict
True
```

## set

set是具有不重複元素的一種資料型別。

操作跟數學上的集合很像。

set沒有順序的概念，因此不支援切片。

**建立set**

```{code-cell}
a = {1,2,3}
a = set([1,2,3])
```

**加入元素至set**

```{code-cell}
a = {1,2,3}
a.add(4)
print(a)
{1,2,3,4}

# .add()已經存在的元素不會改變set。
a.add(1) 
print(a) 
{1,2,3,4}
```

**刪除set中的元素**

```{code-cell}
a = {1,2,3}
a.remove(1)
print(a)
{2, 3}
```

**取得set的長度**

```{code-cell}
a = {1,2,3}
len(a)
3
```

**set的運算**

```{code-cell}
a = {1,2,3}
b = {3,4,5}
c = {1,2,3,4,5}

# 交集
a & b
{3}
a.intersection(b)
{3}

# 聯集
a | b
{1, 2, 3, 4, 5}
a.union(b)
{1, 2, 3, 4, 5}

# 差集
a - b
{1, 2}
a.difference(b)
{1, 2}

b - a
{4, 5}
b.difference(a)
{4, 5}

# 取互斥的元素
a ^ b
{1, 2, 4, 5}
a.symmetric_difference(b)
{1, 2, 4, 5}

# 判斷是否為子集合
a <= b
False
a.issubset(b)
False

# 判斷是否為真子集
a < b
False

# 判斷是否為超集合
a >= b
False
a.isiperset(b)
False

# 判斷是否為真超集
a > b
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

```{code-cell}
x = 5
print(x)
5
y = x
print(y)
5
x = 29
print(x)
29
print(y)
5
```

```{code-cell}
name = 'Curtis'
name[0] = 'K'
TypeError
```

**可變**

```{code-cell}
x = [1, 2, 3]
y = x
x[0] = 'a'
print(y)
['a', 2, 3]
```

## 資料型態統整

參考資料

introducing python: modern computing in simple packages