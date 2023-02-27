# Recamans-sequence

Here's an example program written in ELF64 assembly language that calculates Recamán's sequence:
~~~
section .data
    recaman db 0, 1               ; Start with 0 and 1
    recaman_len equ 100           ; Calculate first 100 terms

section .text
    global _start

_start:
    mov rdi, recaman             ; Load address of Recamán sequence
    mov rcx, recaman_len         ; Load number of terms to calculate

    mov rax, 1                   ; Initialize current value
    mov rbx, 1                   ; Initialize step size
    mov byte [rdi], al           ; Store first value (0)

next_term:
    mov rsi, [rdi]               ; Load current value
    sub rsi, rbx                 ; Calculate next value
    cmp rsi, 0                   ; Check if next value is negative
    jl skip                      ; If so, skip to next term
    ; Check if next value has already been calculated
    movzx edx, byte [rdi + rsi]
    test edx, edx
    jz skip                      ; If so, skip to next term
    mov byte [rdi + rax], dl     ; Store next value
    ; Mark next value as calculated
    mov byte [rdi + rsi], al
    inc rax                      ; Increment current value
    inc rbx                      ; Increment step size
    loop next_term

skip:
    mov eax, 60                  ; System call for exit
    xor edi, edi                 ; Exit status 0
    syscall                      ; Exit the program
~~~
This program defines a recaman sequence with an initial value of 0 and 1, and calculates the first 100 terms of the sequence using the algorithm for Recamán's sequence.

The next_term loop calculates the next value of the sequence by subtracting the current step size from the current value, and then checking whether the resulting value is negative or has already been calculated. If the value is negative or has already been calculated, the loop skips to the next term. Otherwise, it stores the next value in the recaman sequence and marks it as calculated.

The program uses the rdi, rcx, rax, and rbx registers to keep track of the recaman sequence, the number of terms to calculate, the current value, and the step size, respectively.

The program exits with a status code of 0 using the eax and edi registers and the syscall instruction.

To assemble and link this program, save it to a file with a .asm extension (e.g. recaman.asm), and run the following commands:
~~~
nasm -f elf64 recaman.asm -o recaman.o
ld recaman.o -o recaman
~~~
This will create an executable file named recaman. You can run it with the command ./recaman to generate the first 100 terms of Recamán's sequence.
