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

需注意切片是從0開始。

超過index會報錯：IndexError。

```{code-cell}
a_list = ['a','b','c']
print(a_list[0])
print(a_list[-1])
print(a_list[1:])
print(a_list[0:1])
print(a_list[::-1])
```

```{code-cell}
a_list = ['a', 'b', 'c', [1, 2, 3]]

print(a_list[3][2])
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
```

```{code-cell}
a_list.sort(reverse=True) # 降冪排列
print(a_list)
```

```{code-cell}
a_list = ['b','d','a','c','e']
sorted(a_list) # 只會回傳資料的「副本」
```

```{code-cell}
print(a_list)
```

**取得list的長度**

```{code-cell}
a_list = [1,2,3]

len(a_list)
```

**list的加法與乘法運算**

```{code-cell}
a_list = ['a','b','c']
d_list = ['d','e','f']

print(a_list + d_list)
```

```{code-cell}
# 補充：用append的話，argument會被當作元素加到list最尾端

a_list.append(d_list) # 直接會改變list的內容且不回傳結果
print(a_list)
```

```{code-cell}
a_list = ['a','b','c']

print(a_list * 2)
```

**list的其他方法**

```{code-cell}
# .reverse()
a_list = ['a','b','c']
a_list.reverse() # 直接會改變list的順序 且不回傳結果
print(a_list)
```

```{code-cell}
# .extend()
a_list = ['a','b','c']
d_list = ['d','e','f']
a_list.extend(d_list) # 直接會改變list的內容 且不回傳結果
```

```{code-cell}
# .clear()
a_list = ['a','b','c']
a_list.clear() # 直接會改變list的內容 且不回傳結果
print(a_list)
```

```{code-cell}
# .count()
a_list = ['a','b','c','a']
a_list.count('a')
```

## tuple

**建立tuple**

```{code-cell}
a_tuple = (1, 2, 3)
a_tuple = tuple([1,2,3])
a_tuple = 1, 2, 3
```

**切片(slice)**

```{code-cell}
a_tuple = (1, 2, 3)

print(a_tuple[0])
print(a_tuple[1:])
print(a_tuple[0:1])
```

**tuple unpacking**

```{code-cell}
name = ('Frieren', 'Fern', 'Stark')
a, b, c = name

print(a)
print(b)
print(c)
```

**檢查元素是否存在(in)**

```{code-cell}
a_tuple = (1,2,3)

4 in a_tuple
```

**取得tuple的長度**

```{code-cell}
a_tuple = (1,2,3)

len(a_tuple)
```

**tuple的加法與乘法運算**

```{code-cell}
a_tuple = (1,2,3)
b_tuple = (4,5,6)

print(a_tuple + b_tuple)
print(a_tuple * 2)
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
# a_tuple[0] = 'a' -> 會報錯：TypeError
```

```{code-cell}
# 不能新增元素，沒有append()方法，要增加元素只能新建一個tuple
a_tuple = (1,2,3)
print(id(a_tuple)) # 物件的唯一識別碼

a_tuple = a_tuple + (4,)
print(id(a_tuple))

a_list = [1,2,3]
print(id(a_list))

a_list.append(4)
print(id(a_list))
```

```{code-cell}
# 沒有sort方法，要排序tuple中的元素的話，可以用sorted內建函數，但回傳的是一個list
a_tuple = (4,5,3,1,2)
print(sorted(a_tuple))
```

