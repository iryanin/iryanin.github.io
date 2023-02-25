+++
author = "Ryan"
title = "Arm Assembly Syntax"
date = "2023-02-23"
description = "Connection between assembly and C programming language."
tags = [
    "Arm assembly",
]
categories = [
    "Programming Language",
]
image = "cover.jpeg"
+++


## Original C code

```C
float y = 9;
int main() { 
    float x = 5;
}
```


## Assembly code in arm architecture

```Assembly
	.section	__TEXT,__text,regular,pure_instructions
	.build_version macos, 13, 0	sdk_version 13, 1
	.globl	_main                           ; -- Begin function main
	.p2align	2
_main:                                  ; @main
	.cfi_startproc
; %bb.0:
	sub	sp, sp, #16
	.cfi_def_cfa_offset 16
	fmov	s0, #5.00000000
	str	s0, [sp, #12]
	mov	w0, #0
	add	sp, sp, #16
	ret
	.cfi_endproc
                                        ; -- End function
	.section	__DATA,__data
	.globl	_y                              ; @y
	.p2align	2
_y:
	.long	0x41100000                      ; float 9

.subsections_via_symbols
```

## Fib in Arm Assembly

```Assembly

    .text
    .p2align 2
    .globl _main
fib:
    sub sp, sp, #32
    stp x29, x30, [sp, #16]
    add x29, sp, #16
    str x0, [x29, #-8]
    cmp x0, #1
    b.le BASE 
RECURSION:
    sub x0, x0, #2
    bl fib
    mov x9, x0
    ldr x0, [sp, #8]
    sub x0, x0, #2
    bl fib
    add x0, x0, x9
    ldp x29, x30, [sp, #16]
    add sp, sp, #32
    ret

BASE:
    ldp x29, x30, [sp, #16]
    add sp, sp, #32
    ret

_main:
    sub	sp, sp, #16
    stp	x29, x30, [sp]
    mov	x29, sp
    mov w0, #20
    bl fib
    ldp x29, x30, [sp]
    add sp, sp, #16
    ret

```

## Fib in C using stack
```C
#include <stdio.h>

struct item {
  int lr;
  int arg;
  int local;
} stack[10000];

int fib(int n) {
  int sp = 10000;

  // Return value can't be saved to stack frame.
  int r0 = n;

  sp--;
  stack[sp].arg = n;
  stack[sp].lr = 0;

  // In nature, the whole while loop adheres to the model of automata

  while (sp < 10000) {
    int lr = stack[sp].lr;
    if (lr == 0) {
      stack[sp].arg = r0;
      if (stack[sp].arg <= 1) {
        sp++;
        continue;
      }
      stack[sp].lr = 1;
      r0--;
      sp--;
      stack[sp].lr = 0;
    } else if (lr == 1) {
      stack[sp].local = r0;
      r0 = stack[sp].arg - 2;
      stack[sp].lr = 2;
      sp--;
      stack[sp].lr = 0;
    } else if (lr == 2) {
      r0 += stack[sp].local;
      sp++;
    }
  }
  return r0;
}

int main() {
  printf("%d\n", fib(9));
  return 0;
}

```

```Assembly

#include <stdio.h>

struct item {
  int lr;
  int arg;
  int local;
} stack[10000];

int fib(int n) {
  int sp = 10000;
  int rv;
  sp--;
  stack[sp].arg = n;
  stack[sp].lr = 0;
  while (sp < 10000) {
    int lr = stack[sp].lr;
    if (lr == 0) {
      if (stack[sp].arg <= 1) {
        rv = stack[sp].arg;
        sp++;
        continue;
      }
      stack[sp].lr = 1;
      int arg = stack[sp].arg;
      sp--;
      stack[sp].lr = 0;
      stack[sp].arg = arg - 1;
    } else if (lr == 1) {
      stack[sp].local = rv;
      stack[sp].lr = 2;
      int arg = stack[sp].arg;
      sp--;
      stack[sp].lr = 0;
      stack[sp].arg = arg - 2;
    } else if (lr == 2) {
      rv += stack[sp].local;
      sp++;
    }
  }
  return rv;
}

int main() {
  printf("%d\n", fib(45));
  return 0;
}

```

## Analysis

> 	.long	0x41100000                      ; float 9
>
>   ;0100 0001 0001 0000 0000 0000 0000 0000

Real number = $(-1)^{0}$ $\times$ 1.0010 0000 0000 0000 0000 000 $\times$ $2^{130 - 127}$

Obviously, using float to save small integer won't lost accuracy.


