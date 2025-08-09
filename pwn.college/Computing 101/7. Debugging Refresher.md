## level1
```
// basic gdb usage
```
## level2
```
// basic gdb usage
```
## level3
```
// basic gdb usage
```
## level4
```
ni until read@plt > $rsi
x/gx $rsi
```
## level5
```
start
break *main+709
commands
        silent
        set $variable = *(unsigned long long*)($rsi)
        printf "%llx\n", $variable
        continue
end
continue
```
## level6
```
// multiple ways to do this level
run
break *main+625
commands
        silent
        set $rip = $rip + 5
        continue
end

break *main+584
commands
        silent
        set {char[20]}$rsp = "college\n"
        set $rdi = $rsp
        continue
end

break *main+601
commands
        silent
        set {char[20]}$rsp = "everyone\n"
        set $rdi = $rsp
        continue
end

break *main+649
commands
        silent
        set {char[20]}$rsp = "loves\n"
        set $rdi = $rsp
        continue
end

break *main+673
commands
        silent
        set {char[20]}$rsp = "pwn\n"
        set $rdi = $rsp
        continue
end

break *main+686
commands
        silent
        set $rdx = $rax
        continue
end
```
## level7
```
call win()
```
## level8
```
set $rip = win
break *win+253
n
```