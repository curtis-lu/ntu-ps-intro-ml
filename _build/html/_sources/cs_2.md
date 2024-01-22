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

# Python安裝與虛擬環境

## Python安裝(macOS)

瞭解shell的功能以及學會使用IDE之後，就可以來開始著手安裝python了！

如果使用macOS的話，首先第一步：安裝homebrew。

### 安裝homebrew

如果你用的是macOS，需要先使用shell安裝homebrew。

homebrew又是什麼？可以把homebrew想成是macOS的套件管理軟體。

就像python有各種套件一樣，作業系統也有各種套件，homebrew可以為macOS管理套件。

透過homebrew這個工具我們就可以安裝pyenv套件，再透過pyenv來安裝python。

[step 1] 開啟Vscode

```{figure} ./images/vscode_page.png

```

[step 2] VScode中開啟終端機

```{figure} ./images/vscode_terminal.png

```

[step 3] 透過shell指令安裝homebrew

過程中可以需要輸入登入密碼或是按下RETURN鍵，就照著指令操作就可以了。

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

安裝完後建議關閉終端機並重新開啟，關閉的方法如下圖：

```{figure} ./images/vscode_kill.png

```

### 安裝pyenv

pyenv是管理python版本的工具，由於python不斷在更新。

當希望使用python的新功能時，不能直接卸載舊版的python然後安裝新版。

因為可能會造成之前的程式無法如預期運作，或是可能出現錯誤。

所以不想影響舊有的程式的話，就必須讓電腦可以容納多種版本的python。

可以實現這一點的工具就是pyenv。

pyenv可以透過homebrew來安裝，安裝方法非常簡單，同樣在終端機輸入以下：

```
brew install pyenv
```

安裝完成後可以測試是否設定成功：

```
pyenv --version
```

如果有設定成功的話，就會顯示你安裝的pyenv版本，目前最新是到2.3.35。

如果產生錯誤訊息，告訴你不認識```pyenv```指令的話，代表shell還不認識```pyenv```。

就需要把pyenv的資訊告訴shell，請同樣在終端機輸入以下：

```
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

這段的目的是讓shell知道，當我在終端機輸入```pyenv```指令時，是要shell執行pyenv程式。所以需要告訴shell，pyenv的執行路徑在哪裡。

設定完記得重啟shell，可以關閉終端機，或是在終端機輸入以下指定：

```
exec "$SHELL"
```

### 安裝python

接著就可以透過pyenv來安裝python了！

[step 1] 安裝python

以下指令可以確認可以安裝的python版本：

```
pyenv install -l
```

決定好要安裝的python版本時，例如想要的版本是3.11.0以及3.9.18：

```
pyenv install 3.11.0
pyenv install 3.12.1
```

等待pyenv跑完就安裝完成了！

安裝python只是把python下載下來，並安裝在電腦某個地方。

我們還需要透過pyenv告訴作業系統，我指定的python版本：

```
pyenv global 3.11.0
```

如此一來，系統預設的python版本就會是3.11.0版。

可以透過以下指令確認：

```
pyenv global
```

或是

```
python --version
```

[step 2] 新增專案資料夾

接著，假設我們要開啟一個新的專案，我們可以在想要的路徑下新增資料夾。

手動新增或是輸入以下shell指定都可以：

```
mkdir data_projects
```

並且把當前工作目錄移到該路徑：

```
cd data_projects
```

結果如以下：

```{figure} ./images/vscode_cd.png

```
[step 3] 指定python版本

pyenv多版本python的管理，就是透過為不同資料夾目錄指定不同python版本來做到。

這樣在該資料夾底下執行python程式的時候，作業系統就知道要執行哪個版本的python編譯器。

透過pyenv為當前工作資料夾目錄指定python版本：

```
pyenv local 3.12.1
```

確認版本是否指定正確：

```
pyenv local
```

```
python --version
```

參考資料：

[GitHub - pyenv/pyenv: Simple Python version management](https://github.com/pyenv/pyenv?tab=readme-ov-file#set-up-your-shell-environment-for-pyenv)

## 建立虛擬環境

終於來到最後一步，為專案資料夾指定python版本後，就可以來建立虛擬環境並安裝套件。

完成後只要啟動環境就能夠動手做專案了！

### 什麼是虛擬環境？為什麼需要虛擬環境？

就像我們可能會使用不同python版本一樣，就算是同一個python版本，不同專案也會使用各種不同套件，例如pandas、numpy、scikit-learn等等。

不同的套件版本，套件的使用方法有可能會不一樣。例如同一個函數的參數預設值在A版本可能為```True```但在B版本為```False```。

所以就算是同一支程式，程式語法一模一樣，如果安裝的套件版本不一樣，可能程式就會有不一樣的行為或甚至是錯誤發生。

而一個環境下的一個套件就只能使用一種版本，所以隨著專案越做越多，需要更新的套件越來越多，可能會導致過去寫的程式無法運作的情況。

因此，我們需要根據不同專案建立不同的「環境」，不同環境下可以安裝各自的套件，環境之間不會互相干擾。

而這些可以彼此區隔的環境的就叫做「虛擬環境」。

建立虛擬環境的方法有很多種，python內建的venv、第三方套件virtualenv或pipenv、anaconda的conda等等。

今天要介紹的是結合套件管理功能的**Poetry**。

Poetry方便的的地方在於套件解析功能，讓套件安裝與卸載都很簡單，有效避免版本衝突的問題。

只要透過```pyproject.toml```這個設定檔，把套件及相關資訊寫在上面，就可以決定虛擬環境該怎麼建立。

如果把虛擬環境類比成一棟建築的話，```pyproject.toml```就是建築設計圖。

有了```pyproject.toml```，就可以讓poetry依照我們想要的樣子來建立虛擬環境並安裝套件。

### 安裝Poetry

Poetry本身是一個python套件，所以需要先安裝它。

由於我們要透過Poetry來建立虛擬環境並管理套件，Poetry必須跟我們各個專案使用的虛擬環境分開，也就是說Poetry必須在一個獨立的環境。

這邊我們透過```pipx```這個工具來安裝，透過這個工具就可以確保Poetry是在一個獨立的環境。

```pipx```跟pip、virtualenv都是來自同一個社群開發的工具。

參考：

[pypa/pipx: Install and Run Python Applications in Isolated Environments (github.com)](https://github.com/pypa/pipx)

[step 1] 安裝pipx

透過homebrew安裝

```
brew install pipx
```

安裝完成後必須設定好路徑

```
pipx ensurepath
```

[step 2] 安裝poetry

```python
pipx install poetry
```

### 建立虛擬環境

[step 1] 建立專案子環境資料夾

```
poetry new newproject
```

[step 2] 當前工作資料夾移至該資料夾

```
cd newproject
```

[step 3] 建立虛擬環境

```
poetry env use 3.12.1
```

確認是否設定正確

```
python --version
```

### 啟動虛擬環境並安裝套件

[step 1] 啟動虛擬環境

```
poetry shell
```

如果設定正確的話，會看到如下畫面：

```{figure} ./images/poetry_shell.png

```

終端機最前面的地方，會多了虛擬環境的名稱。

[step 2] 安裝套件

安裝套件的方法：

```
poetry add jupyter
poetry add pandas
poetry add plotly
poetry add scikit-learn
```

注意到```pyproject.toml```裡面的內容，自動加上了套件資訊：

```
[tool.poetry]
name = "dataviz"
version = "0.1.0"
description = ""
authors = ["curtis-lu <pclu79@gmail.com>"]
readme = "README.md"

[tool.poetry.dependencies]
python = "^3.12"
jupyter = "^1.0.0"
pandas = "^2.2.0"
plotly = "^5.18.0"
scikit-learn = "^1.4.0"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
```

### 開啟jupyter notebook

開啟command palette

```{figure} ./images/command_palette.png

```

點選"create: New Jupyter Notebook"

```{figure} ./images/create_jupyter.png

```

輸入語法並測試是否可以正常執行

```{figure} ./images/test_jupyter.png

```

注意到，若右上角顯示的是"Select kernel"，可以點進去，選擇對應的虛擬環境名稱。

```{figure} ./images/select_kernel.png

```

如果沒有對應虛擬環境名稱的話，代表VScode沒有正確抓到路徑。

可以手動輸入，方法如下：

回到終端機輸入以下（注意必須在虛擬環境啟動的情況下）：

```
which python
```
```{figure} ./images/pyexe_path.png

```

接著把輸出的路徑貼到以下地方：

"python: select interpreter"

```{figure} ./images/python_select.png

```

"enter interpreter path"

```{figure} ./images/enter_path.png

```

輸入路徑，然後按下enter：

```{figure} ./images/path_enter.png

```

## 小結

以後要使用python的步驟就是：

1. 開啟vscode
2. open folder 到專案版本資料夾路徑
3. 打開終端機
4. 輸入：```poetry shell``` （注意當前工作資料夾須在pyproject.toml同一層資料夾）
5. 打開jupyter notebook檔，選擇好kernel(右上角)；或是python檔，選擇好interpreter(下方)。

就可以進行開發了！

本章的內容非常多，而且很多可能大家不熟悉的內容。

卡住的話就先整個關掉甚至關機，然後喝杯水，放空一下。

很多問題可能就迎刃而解了！