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


# Shell & IDE 介紹

要進行python資料分析或程式開發就必須建立python的開發環境。

建立開發環境的方式有非常多種，本章介紹的方法對初學者而言會有點複雜，但絕對值得跟著一步一步完成。

除了能夠有結構的建立穩健的python開發環境以外，也能夠更熟悉對電腦的瞭解，往後學習更進階的部分時，例如套件開發、使用docker等等，可以比較快上手。

要建立python開發環境基本上有以下幾大步驟：

- 安裝IDE工具：VScode
- 安裝homebrew (for macOS)
- 用homebrew安裝pyenv
- 用pyenv安裝python
- 用homebrew安裝pipx
- 用pipx安裝poetry
- 用poetry建立虛擬環境
- 用poetry安裝套件

在開始安裝之前，需要先介紹shell是什麼。

## Shell 介紹

### 什麼是Shell

我們知道電腦由各種硬體組成，包括CPU、記憶體，與硬碟等。

管理電腦硬體的程式稱作Kernal，例如管理CPU處理程序、管理記憶體資源、管理硬碟存取等等。

Kernal是作業系統的核心部分，透過機器語言來控制硬體。

機器語言是一堆010101的邏輯，人類難以理解，無法直接進行程式設計。

所以需要一個介面讓使用者控制Kernal，這個介面就是Shell。

可以把Shell理解成操作作業系統的程式語言。

Shell只是這類語言的統稱，不同作業系統有各種不同shell，甚至同一個作業系統也有不同shell可以使用。例如適用於Windows作業系統就有cmd和Powershell，或是適用於Unix類作業系統（包括Mac）的bash、z shell(zsh)、c shell(tcsh)等等。

shell除了可以操作作業系統，也可以執行程式，我們主要使用shell的目的就是為了要去執行程式。

例如我們可以把python程式儲存為副檔名是```.py```的檔案，shell可以透過呼叫python編譯器來執行```.py```的程式。

所謂編譯器是指把人類編寫的程式轉譯成機器能理解的語言，python編譯器指的就是把python程式碼轉譯成機器可以理解的指令。

### **如何使用Shell**

一般是使用終端機（terminal），可以提供畫面讓使用者輸入shell指令並顯示結果。

另外也可以使用IDE，下一部分的內容會介紹IDE。

**shell的常用指令**

關於shell其實我們只需要瞭解以下幾個指令：```pwd```、```cd```、```ls```基本上就夠了。

就讓我們用一個例子開始示範吧！假設資料夾結構如下：

```
my_project
├── contents
│   ├── week1
│   └── week2
├── exam
│   ├── final.txt
│   ├── grading.py
│   └── midterm.txt
├── grading.py
└── people
    └── students.txt
```

1. **```pwd```：印出當前工作資料夾(working directory)**

directory的意思可以理解成folder，也就是資料夾。在終端機輸入```pwd```指令就可以在終端機上印出當前工作資料夾。

```
pwd

>>> /user/curtislu/my_project
```

假設有一個python的程式檔grading.py就在當前工作資料夾，那麼我們只要在終端機輸入以下指令，shell就會幫我們告訴電腦我們要執行這隻python程式：

```
# 代表shell呼叫python編譯器去執行grading.py這支python程式
python grading.py
```

而所謂**當前工作資料夾**代表shell執行程式時，該程式的路徑參照位置。也就是說程式**並不是以程式存放的位置**來當作路徑的參照位置。

具體是什麼意思請參考以下範例。

假設my_project目錄下的grading.py的程式內容如下：

```
import pandas as pd
df  = pd.DataFrame({'col': [1,2,3]})
df.to_csv('data1.csv', index=False)
print('data saved.')
```

而exam目錄下的grading.py的程式內容如下：

```
import pandas as pd
df  = pd.DataFrame({'col': [1,2,3]})
df.to_csv('data2.csv', index=False)
print('data saved.')
```

兩者都會產出一個csv檔，只是檔名不同。

假設我們當前的工作資料夾在my_project，分別執行兩個```grading.py```：

```
python grading.py
```

```
python ./exam/grading.py
```

注意到上方的語法中，```.```代表當前位置的意思，```/```代表下一層資料夾。整段代表我們要執行的是```./exam```目錄中的```grading.py```。

執行完後會發現兩個csv檔都儲存在my_project這個資料夾，代表其實兩支程式都是在當前工作資料夾執行的：

```
my_project
├── contents
│   ├── week1
│   └── week2
├── data1.csv
├── data2.csv
├── exam
│   ├── final.txt
│   ├── grading.py
│   └── midterm.txt
├── grading.py
└── people
    └── students.txt
```

1. ```cd```：**更改當前工作目錄**

所以如果我們希望所執行的程式的路徑參照位置更改的話，就要更改當前工作目錄。

只要使用```cd``` ＋ 路徑位置就可以了。路徑位置的指定方式有兩種：

一個是剛剛看過的**相對路徑**：

```
# .代表當層
cd ./contents/week1
```

```
# ..代表上一層
cd ..
```

另一個是使用**絕對路徑**，也就是從根目錄開始的完整路徑：

```
cd /user/curtislu/my_project/contents/week1
```

1. **```ls```：列出當前工作資料夾中的檔案或資料夾清單**

最後一個要介紹的指令比較單純，就只是把當前目錄中有的檔案列印出來，可以確認目前有哪些檔案及資料夾。

```
ls

>>> contents        exam            grading.py      people
```

還可以向下列出目前工作資料夾內所有檔案或資料夾清單。這邊-R就像是指定一個參數，希望ls這個函數多提供什麼功能：

```
ls -R

>>> contents        exam            grading.py      people
>>>
>>> ./contents:
>>> week1   week2
>>>
>>> ./contents/week1:
>>>
>>> ./contents/week2:
>>>
>>> ./exam:
>>> final.txt       midterm.txt
>>>
>>> ./people:
```

參考：

[Difference between Shell and Kernel - GeeksforGeeks](https://www.geeksforgeeks.org/difference-between-shell-and-kernel/)

[Chapter 1 掌握你的電腦 | RLads Lab](https://rlads2021.github.io/LabBook/ch01)

[What exactly is current working directory?](https://stackoverflow.com/questions/45591428/what-exactly-is-current-working-directory)

總而言之，shell就是操作電腦的工具，可以下載軟體（可以理解成作業系統的套件），也可以執行程式。安裝python和建立環境都需要透過它。

那麼就讓我們開始建立python開發環境吧！

首先介紹接下來所有步驟要使用的工具VScode，VScode是一種IDE工具。

## 什麼是IDE

IDE是Integrated Development Environment的縮寫，直接翻譯就是「整合開發環境」。

意思是一個整合多種功能的開發工具，包括了程式編輯器、終端機、版本控制工具等等，把很多功能整合在單一一個介面當中，方便進行程式開發。

如果不用IDE的話，程式語法你可能要寫在文字檔上，例如文字編輯器或記事本。然後要另外開啟作業系統的終端機來執行你寫的程式。不僅凌亂很難管理，介面也很不方便。

在資料分析的學習過程中，一般大家可能常看到的是使用jupyter notebook來寫python。

但jupyter notebook並不是IDE，jupyter notebook目的是提供一個互動式的編輯界面，允許使用者以區塊的方式運行程式碼，可以即時呈現資料分析的結果，並且支援視覺化圖表等等。

jupyter notebook的設計很符合資料分析的需求，但jupyter notebook比較適合用於教學或是小型的分析專案，一般進行開發仍是建議使用IDE。

支援python常見的IDE有：

- pycharm
- VScode
- Jupyter Lab
- Spyder
- Atom

想要推薦大家使用的是Visual Studio code (vscode)，vscode是由微軟開發的開源軟體，支援各種平台（windows、macOS、 Linux都可以用）以及上百種程式語言（C++, C#,  Go, Java, JavaScript,  Julia, Python, R等等  ）。

### **Visual Studio code的功能**

vscode有許多方便的功能：

程式碼輔助編輯功能：包括語法顏色突顯、程式碼折疊、自動完成、程式碼尋找

擴充性工具生態系統：有超級多的擴充套件，例如可以把jupyter notebook整合進來。

內建終端機：不必另外開啟終端機，可以直接在vscode中使用。

整合版本控制：在vscode中可以直接操作git相關功能。

參考：

[Getting started with Visual Studio Code](https://code.visualstudio.com/docs/introvideos/basics)

### **安裝Visual Studio code (vscode)**

[step 1] 至官網下載安裝檔

基本上只要照著安裝就可以了

[vscode官網](https://code.visualstudio.com/)

[step 2] 安裝擴充工具

必備的擴充工具有：

- python: 在vscode上開發python必備的擴充工具，
- jupyter: 在vscode上開啟jupyter notebook
- path Intellisense: 方便編寫路徑
- Auto Docstring: 自動產生Docstring的格式
- Excel viewer: 方便在vscode查看csv檔

VScode自己會依據你開啟的程式語法檔推薦你可以安裝的擴充套件。

VScode的擴充套件有非常多，可再自行挖掘。

安裝方法：

```{figure} ./images/vscode_extension.png

```

本章主要著重在VScode的介面中建立python開發環境的內容，其他相關操作可以再自行參考Vscode官方教學影片和其他資源。

[vscode - Introductory Videos](https://code.visualstudio.com/docs/getstarted/introvideos)

[Top 10 VS Code Extensions For Data Science - GeeksforGeeks](https://www.geeksforgeeks.org/vs-code-extensions-for-data-science/)
