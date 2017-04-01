title: 'Shell scripting in Python3'
author: Naetw
tags:
  - Python
  - shell scripting
categories:
  - note
date: 2016-11-24 23:33:00
---

> 其實一直都沒有好好寫過 Python 來做什麼事，幾乎都只用來寫 exploit 而以，這次想說把 SA 作業拿來用 Python 寫好了，順便練習一下@@

最常用的就是 `subprocess.call()`，可以帶 argument 來執行，會回傳 return code

```python
import subprocess
subprocess.call(args, executable=None, shell=False)
```

* 當 shell == False 時，`args` 會被視為 executable
    * Ex: `subprocess.call('echo hello')` 會被視為 `./echo\ hello`，但是並沒有 echo\ hello 這個 ELF
    * 如果要避免以上問題可以把 shell 開啟，或是把 **cmd** 跟 **argument** 拆開
        * Ex: `subprocess.call(['echo','hello'])`
        * 執行後的 output 會寫到 stdout
        * 當 shell == True 時，`executable` default 會視為 /bin/sh，因此 `args` 會直接丟給 /bin/sh 去執行
        * 當要使用的 cmd 是 built-in command 時，就必須要用 **shell=True**

## Asynchronously

在使用 `subprocess.call()` 時，須等到他呼叫的程式執行完後，控制權才會回到 Python program

這時候我們就必須直接呼叫 `subprocess.call()` 的底層 - `subprocess.Popen()`

其實我們可以將 `subprocess.call()` 視為 Interface，他在取得 `subprocess.Popen()` 的 instance，就呼叫 `Popen.wait()`，到結束後，回傳 return code

Popen() 跟 child process 互動的方法：

* Popen.poll() - 檢查 child process 是否還在執行，如果是則回傳 None，結束的話就回傳 return code
* Popen.wait() - 會等到 child process 結束之後回傳 return code，才把控制權還給 Python
* Popen.communicate(input=None) - 用來取回 stdout & stderr

```python
import subprocess
from subprocess import PIPE

process = subprocess.Popen('id', shell=True, stdout=PIPE, stderr=PIPE)
process.communicate()
```

## Chile process output 的處理:

* 利用 `Popen.communicate()` 回傳的是 bytes literal，可以利用下面方法轉回 string
    * string = byte_literal.decode("utf-8")
    * 在這裡回傳的 bytes literal 最後會有一個 newline，如果單純用 `str.split('\n')`，回傳的 list 會多一個 element，這時可以用 `str.splitlines()`
        * Ex: `backup/dotfiles@2016-11-21-19:08:27\nbackup/dotfiles@2016-11-21-19:33:31\n` 
            * 如果用 `split('\n')` 會變成 `['backup/dotfiles@2016-11-21-19:08:27', 'backup/dotfiles@2016-11-21-19:33:31', '']`
                * 如果改用 `splitlines()`，`['backup/dotfiles@2016-11-21-19:08:27', 'backup/dotfiles@2016-11-21-19:33:31']`

## 在 loop 中改變 iteration 的內容

* 如果要在 loop 中改變 list 的值，使用 `for element in list[:]`，利用 list slice 把要 loop 的 list 複製一份出來，這樣在 loop 中就可以改變 list 的值，而不影響正在走訪中的 list
