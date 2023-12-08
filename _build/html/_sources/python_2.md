# Python - 資料型態與變數(3)

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

## 進階主題

### 資料型別的可變與不可變

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

### 資料型態統整

參考資料

introducing python: modern computing in simple packages