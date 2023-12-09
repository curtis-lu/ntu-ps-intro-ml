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

# Python - 資料型態與變數(1)

## **Python的基本資料型態**

Python常用的資料型態基本上就如下表列這幾種：

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

其中bool、int、float、str是最基本的資料型態。

而list, tuple, dict, set可以理解成用不同方式來組織**物件**的資料型態，各有各的特點和使用情境。

後面會說明list, tuple, dict, set的使用方式。

```{code-cell}
# type() 是一個可以用來檢查資料型別的function。
# 所謂function就是丟input進去，會有output跑出來。
# "type"是這個function的名稱，
# "True"是這個function的input，而function跑出來的output會列在下方。

type(True)
```

```{code-cell}
type(42)
```

```{code-cell}
type(3.14)
```

## **Python的資料 = 物件**

在python中，所有資料都是「**物件(object)**」。

物件佔用記憶體的一部分位置，儲存了以下元素：

- 物件的**id**，也就是唯一識別碼，指出了在倉庫中的位置。
- 物件的**型態(type)**，說明物件可以做什麼。
- 物件的**值**，也就是物件的內容。

如果把電腦的記憶體想像成很多排架子的倉庫，每排架子裡有很多個格子，

```{figure} ./images/rack.jpg
---
scale: 30%
---
<a href="https://www.freepik.com/free-ai-image/futuristic-library-with-shelves-symmetry-generated-by-ai_41283616.htm#fromView=search&term=warehouse&page=1&position=9&track=ais_ai_generated&regularType=ai&uuid=65da5c56-20aa-4bd1-8206-2c73a350cecc">Image By vecstock</a>
```

我們可以把一個物件當作是一個盒子，佔用了倉庫中某些格子的位置。

有些盒子比較大，佔用比較多格子，有些盒子比較小，只佔用一些格子。

**變數(variable)** 則是物件的名稱，可以想像是盒子的標籤。

標籤貼著盒子，幫助我們利用標籤去找出我們要的資料。


```{figure} ./images/package_with_tag.jpg
---
scale: 30%
---
<a href="https://www.freepik.com/free-photo/mail-package-with-label_859014.htm#query=Rectangle%20carton%20box%20with%20tags&position=3&from_view=search&track=ais&uuid=af514f95-961e-46ba-909d-cffeb7c9ebc6">Image by kstudio</a> on Freepik
```

例如我想把一串數字用一個簡單的名字來命名，其中"="代表賦值：

```{code-cell}
my_number = 20221130
```

然後就可以呼叫變數名稱來使用它：

```{code-cell}
print(my_number)
```

**變數名稱的規則**

- a-z, A-Z, 0-9, _
- 區分大小寫，apple ≠ APPLE ≠ Apple
- 開頭不能使用數字。
- 以底線開頭的變數有特殊意義。
- 不能是python保留字。

```{code-cell}
print("id: ", id(True))
print("type: ", type(True))
print("value: ", True)
```

以下為python保留字，也可參考：[Python Keywords: An Introduction – Real Python](https://realpython.com/python-keywords/)

```{code-cell}
help("keywords")
```

python的合法賦值方法：

```{code-cell}
a = b = c = 0

print(a)
print(b)
print(c)
```

```{code-cell}
a, b = 1, 2

print(a)
print(b)
```

## 基礎資料型別: **布林**

bool型態就只有兩種：True & False，可以透過bool()將其他資料轉換為布林型態。

```{code-cell}
# 以下這些都是True

print(bool(1))
print(bool(1.0))
print(bool(100.001))
print(bool(-1))
print(bool('a'))
```

```{code-cell}
# False

print(bool(0))
print(bool(0.0))
```

布林邏輯運算使用and和or關鍵字。

```{code-cell}
print(True and True)
print(True and False)
print(True or True)
print(True or False)
print(False and False)
print(False or False)
```

## 基礎資料型別： **整數與浮點數**

可以透過int()和float()做資料型別轉換。

```{code-cell}
int(True)
```

```{code-cell}
float(True)
```

```{code-cell}
float('1')
```

**python的運算子**如下列表：

| 運算子 | 說明 | 範例 | 結果 |
| --- | --- | --- | --- |
| + | 加法 | 1+1 | 2 |
| - | 減法 | 100 - 1 | 99 |
| * | 乘法 | 3 * 2 | 6 |
| / | 除法 | 9 / 2 | 4.5 |
| // | 取商數 | 9 // 2 | 4 |
| % | 取餘數 | 9 % 2 | 1 |
| ** | 次方 | 2 ** 3 | 8 |

## 基礎資料型別：**字串**

用''、""、''''''、""""""括起來的資料就是str。
也可以透過str()來將資料轉換為字串。

```{code-cell}
str(42.0)
```

```{code-cell}
str(True)
```

### "\\\"轉譯

在python中"\\\"是一個特殊的符號，後面接不同字母代表不同意思：

"\n"代表換行；"\t"代表tab；"\\\\\"則代表"\\\"

```{code-cell}
print('a\nbc')
```

```{code-cell}
print('a\tbc')
```

```{code-cell}
print('a\\bc')
```

在字串前多加一個r，會取消"\\\"轉譯。

這種字串稱作原始字串，常用於表示window作業系統的路徑。

```{code-cell}
print(r'a\nbc')
```

### **f-strings**

f-strings可以將變數值帶入字串，或是用來格式化。

使用方法是在字串前加一個字母f，然後想要置換的東西要用"{}"包起來。

可參考：[Guide to String Formatting in Python Using F-strings | Built In](https://builtin.com/data-science/python-f-string)


```{code-cell}
name = 'Curtis'
print(f'Hi, {name}!')
```

```{code-cell}
number = 12.3456789

print( f'{number:.2f}')
print( f'{number:%}')
print( f'{number:,%}')
print( f'{number:,.2%}')
```

```{code-cell}
# {} 內也可以做運算

first_name = 'Curtis'
last_name = 'Lu'

print(f'{first_name + last_name}')
```

### 字串的其他基本用法

#### **字串的切片**

```{code-cell}
letters = 'abcdefghijklmnopqrstuvwxyz'

print(letters[0])
print(letters[-1])
print(letters[0:2])
print(letters[-2:])
print(letters[0:6:2])
print(letters[::-1])
```

#### 判斷字串是否包含字串

```{code-cell}
'a' in 'apple'
```

#### 字串的加法與乘法運算

```{code-cell}
print('1' + '2')
print('1' * 5)
print('1' + '2' * 5)
```

#### 字串的拆分.split()

這邊用到一個新的物件操作方法，即在一個字串型別的變數後面加上".”然後用呼叫方法名稱。

例如一個拆分字串的方法".split()”：

```{code-cell}
name = 'National Taiwan University'

print(name.split())
```

其中括弧內也可以輸入引數(argument)來指定要使用的切割字元（不輸入的話預設就是一個空白鍵）：

```{code-cell}
name = 'National Taiwan University'

print(name.split('a'))
```


#### 字串的剝除str.strip()

```{code-cell}
name = '    Curtis Lu    '
name.strip()
```

```{code-cell}
name = '    Curtis Lu....!!!    '
name.strip(' .!')
```

#### 其他字串操作

```{code-cell}
name = 'curtis lu'

print(name.capitalize())
print(name.title())
print(name.upper())
print(name.lower())
print(name.swapcase())
```

