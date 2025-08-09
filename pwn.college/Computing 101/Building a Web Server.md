## exit
```
.intel_syntax noprefix
.global _start
.section .text

_start:
        mov rdi, 0
        mov rax, 60
        syscall

.section .data
```
## socket
```
.intel_syntax noprefix
.global _start

.section .text

_start:

        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 0x29
        syscall

        mov rdi, 0
        mov rax, 0x3c
        syscall

.section .data
```
## bind
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8

.section .text
.global _start
_start:

        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        mov rdi, rax
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall


        mov rdi, 0
        mov rax, 60
        syscall
```
## listen
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8

.section .text
.global _start
_start:

        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        mov rdi, rax
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        mov rsi, 0
        mov rax, 50
        syscall

        mov rdi, 0
        mov rax, 60
        syscall
```
## accept
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8

.section .text
.global _start
_start:

        // socket()
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, rax
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rsi, 0
        mov rax, 50
        syscall

        // accept()
        mov rdx, 0
        mov rax, 43
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
## static-response
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8
msg:
        .ascii "HTTP/1.0 200 OK\r\n\r\n"

.section .bss
request: .skip 256


.section .text
.global _start
_start:

        // socket()
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, rax
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rsi, 0
        mov rax, 50
        syscall

        // accept()
        mov rdx, 0
        mov rax, 43
        syscall

        // read()
        mov rdi, rax
        lea rsi, [rip + request]
        mov rdx, 256
        mov rax, 0
        syscall

        // write()
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // close()
        mov rax, 3
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
## dynamic-response
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8
msg:
        .ascii "HTTP/1.0 200 OK\r\n\r\n"

.section .bss
request: .skip 256
path: .skip 128
flag: .skip 256


.section .text
.global _start
_start:

        // socket() fd:3
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, 3
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rdi, 3
        mov rsi, 0
        mov rax, 50
        syscall

        // accept() fd:4
        mov rdi, 3
        mov rsi, 0
        mov rdx, 0
        mov rax, 43
        syscall

        // read() CHANGE MOV RDI, RAX
        mov rdi, 4
        lea rsi, [rip + request]
        mov rdx, 256
        mov rax, 0
        syscall

get:    // GET parse
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je getDone
        inc rsi
        jmp get

getDone:
        inc rsi
        lea rdi, [rip + path]

file:   // filename parse
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je fileDone
        cmp al, 0x0A
        je fileDone
        mov byte ptr [rdi], al
        inc rsi
        inc rdi
        jmp file

fileDone:
        // open() fd:5
        lea rdi, [rip + path]
        mov rsi, 0
        mov rax, 2
        syscall

        // read()
        mov rdi, 5
        lea rsi, [rip + flag]
        mov rdx, 256
        mov rax, 0
        syscall

        mov r10, rax

        // close()
        mov rdi, 5
        mov rax, 3
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + flag]
        mov rdx, r10
        mov rax, 1
        syscall

        // close()
        mov rdi, 4
        mov rax, 3
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
## iterative GET server
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8
msg:
        .ascii "HTTP/1.0 200 OK\r\n\r\n"

.section .bss
request: .skip 256
path: .skip 128
flag: .skip 256


.section .text
.global _start
_start:

        // socket() fd:3
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, 3
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rdi, 3
        mov rsi, 0
        mov rax, 50
        syscall

_iter:
        // accept() fd:4
        mov rdi, 3
        mov rsi, 0
        mov rdx, 0
        mov rax, 43
        syscall

        // read() CHANGE MOV RDI, RAX
        mov rdi, 4
        lea rsi, [rip + request]
        mov rdx, 256
        mov rax, 0
        syscall

get:    // GET parse
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je getDone
        inc rsi
        jmp get

getDone:
        inc rsi
        lea rdi, [rip + path]

file:   // filename parse
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je fileDone
        cmp al, 0x0A
        je fileDone
        mov byte ptr [rdi], al
        inc rsi
        inc rdi
        jmp file

fileDone:
        // open() fd:5
        lea rdi, [rip + path]
        mov rsi, 0
        mov rax, 2
        syscall

        // read()
        mov rdi, 5
        lea rsi, [rip + flag]
        mov rdx, 256
        mov rax, 0
        syscall

        mov r10, rax

        // close()
        mov rdi, 5
        mov rax, 3
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + flag]
        mov rdx, r10
        mov rax, 1
        syscall

        // close()
        mov rdi, 4
        mov rax, 3
        syscall

		jmp _iter
		
        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
## concurrent GET server
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8
msg:
        .ascii "HTTP/1.0 200 OK\r\n\r\n"

.section .bss
request: .skip 256
path: .skip 128
flag: .skip 256


.section .text
.global _start
_start:

        // socket() fd:3
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, 3
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rdi, 3
        mov rsi, 0
        mov rax, 50
        syscall

_iter:
        // accept() fd:4
        mov rdi, 3
        mov rsi, 0
        mov rdx, 0
        mov rax, 43
        syscall

        mov rax, 57
        syscall

        cmp rax, 0
        je _child

_parent:
        // close() fd:4
        mov rdi, 4
        mov rax, 3
        syscall

        jmp _iter

_child:

        // close() fd:3
        mov rdi, 3
        mov rax, 3
        syscall

        // read() fd:4
        mov rdi, 4
        lea rsi, [rip + request]
        mov rdx, 256
        mov rax, 0
        syscall

get:    // GET parse
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je getDone
        inc rsi
        jmp get

getDone:
        inc rsi
        lea rdi, [rip + path]

file:   // filename parse
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je fileDone
        cmp al, 0x0A
        je fileDone
        mov byte ptr [rdi], al
        inc rsi
        inc rdi
        jmp file

fileDone:
        // open() fd:3
        lea rdi, [rip + path]
        mov rsi, 0
        mov rax, 2
        syscall

        // read()
        mov rdi, 3
        lea rsi, [rip + flag]
        mov rdx, 256
        mov rax, 0
        syscall

        mov r10, rax

        // close()
        mov rdi, 3
        mov rax, 3
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + flag]
        mov rdx, r10
        mov rax, 1
        syscall

        // close()
        mov rdi, 4
        mov rax, 3
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
## concurrent POST server
```
.intel_syntax noprefix
.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8
msg:
        .ascii "HTTP/1.0 200 OK\r\n\r\n"

.section .bss
request: .skip 512
path: .skip 128
contentReverse: .skip 256
content: .skip 256


.section .text
.global _start
_start:

        // socket() fd:3
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, 3
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rdi, 3
        mov rsi, 0
        mov rax, 50
        syscall

_iter:
        // accept() fd:4
        mov rdi, 3
        mov rsi, 0
        mov rdx, 0
        mov rax, 43
        syscall

        mov rax, 57
        syscall

        cmp rax, 0
        je _child

_parent:
        // close() fd:4
        mov rdi, 4
        mov rax, 3
        syscall

        jmp _iter

_child:

        // close() fd:3
        mov rdi, 3
        mov rax, 3
        syscall

        // read() fd:4 SWITCH BACK TO 4
        mov rdi, 4
        lea rsi, [rip + request]
        mov rdx, 512
        mov rax, 0
        syscall

        // save request length
        mov r10, rax

        // POST parse
postParse:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je postDone
        inc rsi
        jmp postParse

postDone:
        inc rsi
        lea rdi, [rip + path]

        // filename parse
fileParse:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je fileDone
        cmp al, 0x0A
        je fileDone
        mov byte ptr [rdi], al
        inc rsi
        inc rdi
        jmp fileParse

fileDone:
        lea rdi, [rip + contentReverse]
        lea rsi, [rip + request]
        add rsi, r10
        sub rsi, 1
        xor rcx, rcx

        // content parse
contentParse:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je contentDone
        cmp al, 0x0A
        je contentDone
        mov byte ptr [rdi], al
        inc rdi
        sub rsi, 1
        inc rcx
        jmp contentParse

contentDone:
        lea rsi, [rip + contentReverse]
        lea rdi, [rip + content]
        add rdi, rcx
        sub rdi, 1
        xor rcx, rcx

reverseContent:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je done
        cmp al, 0x0A
        je done
        cmp al, 0x00
        je done
        mov byte ptr [rdi], al
        inc rsi
        sub rdi, 1
        inc rcx
        jmp reverseContent

done:
        mov r10, rcx

        // open() fd:3
        lea rdi, [rip + path]
        mov rsi, 65
        mov rdx, 0777
        mov rax, 2
        syscall

        // write()
        mov rdi, 3
        lea rsi, [rip + content]
        mov rdx, r10
        mov rax, 1
        syscall

        // close()
        mov rdi, 3
        mov rax, 3
        syscall

        // write()
        mov rdi, 4
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
## web server
```
.intel_syntax noprefix

.section .data
sockaddr_in:
        .word 0x0002
        .word 0x5000
        .long 0x00000000
        .zero 8
msg:
        .ascii "HTTP/1.0 200 OK\r\n\r\n"


.section .bss
request:
        .skip 512
path:
        .skip 128
content:
        .skip 512
contentReverse:
        .skip 512


.section .text
.global _start
_start:
        // socket(fd:3)
        mov rdi, 2
        mov rsi, 1
        mov rdx, 0
        mov rax, 41
        syscall

        // bind()
        mov rdi, 3
        lea rsi, [rip + sockaddr_in]
        mov rdx, 16
        mov rax, 49
        syscall

        // listen()
        mov rdi, 3
        mov rsi, 0
        mov rax, 50
        syscall

_forkLoop:
        // accept(fd:4)
        mov rdi, 3
        mov rsi, 0
        mov rdx, 0
        mov rax, 43
        syscall

        // fork()
        mov rax, 57
        syscall
        cmp rax, 0
        je _childRead

        // close(fd:4)
        mov rdi, 4
        mov rax, 3
        syscall

        jmp _forkLoop

_childRead:
        // close(fd:3)
        mov rdi, 3
        mov rax, 3
        syscall

        // read() request from socket(fd:4) CHANGE 0/4 TESTING
        mov rdi, 4
        lea rsi, [rip + request]
        mov rdx, 512
        mov rax, 0
        syscall

        push rax

_skipType:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je _skipDone
        inc rsi
        jmp _skipType

_skipDone:
        lea rdi, [rip + path]
        inc rsi

_getFilename:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je _typeHandle
        cmp al, 0x0A
        je _typeHandle
        mov byte ptr [rdi], al
        inc rsi
        inc rdi
        jmp _getFilename

_typeHandle:
        mov al, byte ptr [rip + request]
        cmp al, 0x50
        je _postHandle

_getHandle:
        // open(fd:3)
        lea rdi, [rip + path]
        mov rsi, 0
        mov rax, 2
        syscall

        // read(fd:3)
        mov rdi, 3
        lea rsi, [rip + content]
        mov rdx, 512
        mov rax, 0
        syscall

        mov r10, rax

        // close(fd:3)
        mov rdi, 3
        mov rax, 3
        syscall

        // write(fd:4) CHANGE 0/4 TESTING
        mov rdi, 4
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // write(fd:4) CHANGE 0/4 TESTING
        mov rdi, 4
        lea rsi, [rip + content]
        mov rdx, r10
        mov rax, 1
        syscall

        // close(fd:4)
        mov rdi, 4
        mov rax, 3
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall

_postHandle:
        lea rdi, [rip + contentReverse]
        lea rsi, [rip + request]
        pop rbx
        add rsi, rbx
        sub rsi, 1
        xor rcx, rcx

_requestToReverse:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je _reverseDone
        cmp al, 0x0A
        je _reverseDone
        mov byte ptr [rdi], al
        inc rdi
        sub rsi, 1
        inc rcx
        jmp _requestToReverse

_reverseDone:
        lea rdi, [rip + content]
        lea rsi, [rip + contentReverse]
        add rdi, rcx
        sub rdi, 1
        push rcx

_reverseToContent:
        mov al, byte ptr [rsi]
        cmp al, 0x20
        je _contentDone
        cmp al, 0x0A
        je _contentDone
        cmp al, 0x00
        je _contentDone
        mov byte ptr [rdi], al
        inc rsi
        sub rdi, 1
        jmp _reverseToContent

_contentDone:
        // open(fd:3)
        lea rdi, [rip + path]
        mov rsi, 65
        mov rdx, 0777
        mov rax, 2
        syscall

        // write(fd:3)
        mov rdi, 3
        lea rsi, [rip + content]
        pop rdx
        mov rax, 1
        syscall

        // close(fd:3)
        mov rdi, 3
        mov rax, 3
        syscall

        // write(fd:4)
        mov rdi, 4
        lea rsi, [rip + msg]
        mov rdx, 19
        mov rax, 1
        syscall

        // exit()
        mov rdi, 0
        mov rax, 60
        syscall
```
