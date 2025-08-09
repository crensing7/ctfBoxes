## set-register
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov rdi, 0x1337
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## set-multiple-registers
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           mov rax, 0x1337
           mov r12, 0xcafed00d1337beef
           mov rsp, 0x31337
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## add-to-register
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           add rdi, 0x331337
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## linear-equation-registers
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           imul rdi, rsi
           add rdi, rdx
           mov rax, rdi
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## integer-division
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           mov rax, rdi
           div rsi
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## modulo-operation
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           mov rax, rdi
           div rsi
           mov rax, rdx
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## set-upper-byte
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           mov ah, 0x42
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## efficient-modulo
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           mov al, dil
           mov bx, si
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## byte-extraction
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           shr rdi, 32
           mov al, dil
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## bitwise-and
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           and rax, 0
           and rdi, rsi
           or rax, rdi
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## check-even
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           and rdi, 1
           and rax, 0
           xor rdi, 1
           or rax, rdi
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## memory-read
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov rax, [0x404000]
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## memory-write
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov [0x404000], rax
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## memory-increment
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
   mov rax, [0x404000]
   mov rbx, 0x1337
   add [0x404000], rbx
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## byte-access
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov al, [0x404000]
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## memory-size-access
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           mov al, [0x404000]
           mov bx, [0x404000]
           mov ecx, [0x404000]
           mov rdx, [0x404000]
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## little-endian-write
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov rax, 0xdeadbeef00001337
    mov [rdi],  rax
    mov rax, 0xc0ffee0000
    mov [rsi],  rax
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## memory-sum
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov rax, [rdi]
    mov rbx, [rdi+8]
    add rax, rbx
    mov [rsi], rax
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## stack-subtraction
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           pop rax
           sub rax, rdi
           push rax
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## swap-stack-values
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
           push rdi
           push rsi
           pop rdi
           pop rsi
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## average-stack-values
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
   mov rax, [rsp]
   add rax, [rsp+8]
   add rax, [rsp+16]
   add rax, [rsp+24]
   mov rsi, 4
   div rsi
   push rax         
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## absolute-jump
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov rax, 0x403000
    jmp rax
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## relative-jump
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    jmp target
    .rept 0x51
    nop
    .endr
target:
    mov rax, 0x1
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## jump-trampoline
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''

    jmp target
    .rept 0x51
    nop
    .endr
target:
    pop rdi
    mov rbx, 0x403000
    jmp rbx

''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## conditional-jump
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''
    mov ebx, [edi]
    cmp ebx, 0x7f454c46
    jne check1
    mov eax, [edi+4]
    add eax, [edi+8]
    add eax, [edi+12]
    jmp done
check1:
    cmp ebx, 0x00005a4d
    jne check2
    mov eax, [edi+4]
    sub eax, [edi+8]
    sub eax, [edi+12]
    jmp done
check2:
    mov eax, [edi+4]
    imul eax, [edi+8]
    imul eax, [edi+12]
done:
''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## indirect-jump
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''

    cmp rdi, 3
    jg default
    imul rdi, 8
    jmp done
default:
    mov rdi, 3
done:
    jmp [rsi+rdi]

''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## average-loop
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''

   mov rax, [rdi] 
   mov rcx, 1
_loop:
   cmp rsi, rcx
   je _done
   add rax, [rdi+rcx*8]
   add rcx, 1
   jmp _loop
_done:
   div rsi

''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## count-non-zero
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''

    mov rax, 0
    cmp rdi, 0
    je _done
_loop:
    mov bl, [rdi+rax]
    cmp bl, 0
    je _done
    add rax, 1
    jmp _loop
_done:


''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## string-lower
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''

        mov rbx, 0           
        mov rsi, 0

        cmp rdi, 0
        je _done

_loop:
        mov dl, [rdi+rsi]
        cmp dl, 0x00
        je _done
        
        cmp dl, 0x5a
        jg _inc

        mov r9, rdi
        xor rdi, rdi
        mov dil, dl
        mov r10, 0x403000
        call r10
        mov [r9+rsi], al
        add rbx, 1
        mov rdi, r9

_inc:
        add rsi, 1
        jmp _loop

_done:
        mov rax, rbx
        ret

''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
## most-common-byte
```
from pwn import *

context.arch = 'amd64'
context.bits = 64

ssh_conn = ssh(host='pwn.college', user='hacker', keyfile='/home/god/.ssh/pwnCollege')
p = ssh_conn.process('/challenge/run')

code = asm('''

TRY AGAIN LATER

''')

p.send(code)
output = p.recvall()
print(output.decode(errors='ignore'))

p.close()
ssh_conn.close()
```
