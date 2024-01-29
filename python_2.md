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

# 2.資料型態：dict與set

## 群集資料型別：dict

dict是dictionary的略稱，字典這種資料型態的組成是一群成對的key與value。

key在字典裡不能重複，通常會使用字串或tuple來當作key。至於value則沒有什麼限制。

dict跟後面等等會介紹的set一樣，都是沒有順序的，不能使用切片。

**建立dict**

以下兩種方法都可以建立dictionary。

```{code-cell}
# key: value
a_dict = {'a': 1, 'b': 2, 'c':3}

# key=value
a_dict = dict(a=1, b=2, c=3)
```

### dict的基本用法

 **dict的基本用法**

利用key來取出value：
```{code-cell}
a_dict['a']
```

利用key來取出value的另一種方法：
```{code-cell}
a_dict.get('a')
```
若key值不存在的話，用這個方法python會丟出錯誤。
```{code-cell}
a_dict['d']
```

若key值不存在的話，用```.get()```方法python不會丟出錯誤。
```{code-cell}
a_dict.get('d') # 不會回傳任何東西
```

```.get()```方法內的參數是當key值不存在時，預設的value。
```{code-cell}
a_dict.get('d', 'missing')
```

### dict 新增與刪除項目

**dict新增項目**

新增項目的方法有兩種：
```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}

# 第一種
a_dict['d'] = 4
print(a_dict)

# 第二種
a_dict.update({'e': 5})
print(a_dict)
```

**dict刪除項目**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
```

方法1

```{code-cell}
a_dict.pop('a')
```

```{code-cell}
print(a_dict)
```

key值若不存在會拋錯。
```{code-cell}
a_dict.pop('d')
```

```.pop()```方法中可以塞預設值。
```{code-cell}
a_dict.pop('d', 'Nothing happened')
```

```{code-cell}
print(a_dict)
```

方法2

```{code-cell}
del a_dict['b']

print(a_dict)
```

**dict中的key值不能重複**

當要新增項目時，dict中對應的key值若已經存在，會以新的為準。

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
b_dict = {'b': 'ni', 'c': 'san', 'd': 'shi'}

a_dict.update(b_dict)
print(a_dict)
```

### dict 操作

**取得dict的內容**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}
```

```{code-cell}
# 取得key的list
list(a_dict.keys())
```

```{code-cell}
# 取得value的list
list(a_dict.values())
```

```{code-cell}
# 取得key跟value的list，注意到其中是一個一個tuple
list(a_dict.items())
```

**取得dict長度**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}

# 長度指的是key的數量
len(a_dict)
```

**檢查key是否存在**

```{code-cell}
a_dict = {'a': 1, 'b': 2, 'c':3}

'c' in a_dict
```

**用{\*\*a, \*\*b}結合dict**

```{code-cell}
dict_1 = {'a': 'apple', 'b': 'beach', 'c': 'cat'}
dict_2 = {'d': 'dog', 'e': 'egg', 'f': 'flower'}

dict_all = {**dict_1, **dict_2}
print(dict_all)
```

## 群集資料型別：set

set是具有不重複元素的一種資料型別。

操作跟數學上的集合很像。

set沒有順序的概念，因此不支援切片。

**建立set**

```{code-cell}
a = {1,2,3}
a = set([1,2,3])
```

### set元素操作

**加入元素至set**

```{code-cell}
a = {1,2,3}
a.add(4)
print(a)
```

.add()已經存在的元素不會改變set。
```{code-cell}
a.add(1)
print(a)
```

**刪除set中的元素**

```{code-cell}
a = {1,2,3}
a.remove(1)

print(a)
```

### set 操作

**取得set的長度**

```{code-cell}
a = {1,2,3}

len(a)
```

**set的運算**

```{code-cell}
a = {1,2,3}
b = {3,4,5}
c = {1,2,3,4,5}
```

交集

```{code-cell}
a & b
```

```{code-cell}
a.intersection(b)
```

聯集

```{code-cell}
a | b
```

```{code-cell}
a.union(b)
```

差集

取差集的順序會有影響。

```{code-cell}
a - b
```

```{code-cell}
a.difference(b)
```

```{code-cell}
b - a
```

```{code-cell}
b.difference(a)
```

取互斥的元素

```{code-cell}
a ^ b
```

```{code-cell}
a.symmetric_difference(b)
```

判斷是否為子集合

```{code-cell}
a <= b
```

```{code-cell}
a.issubset(b)
```

判斷是否為真子集

```{code-cell}
a < b
```

判斷是否為超集合

```{code-cell}
a >= b
```

```{code-cell}
a.issuperset(b)
```

判斷是否為真超集

```{code-cell}
a > b
```
