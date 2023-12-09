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

y = x
print(y)

x = 29
print(x)

print(y)
```

```{code-cell}
name = 'Curtis'
name[0] = 'K'
```

**可變**

```{code-cell}
x = [1, 2, 3]
y = x
x[0] = 'a'

print(y)
```

## 資料型態統整

參考資料

introducing python: modern computing in simple packages