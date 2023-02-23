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
series = ["Themes Guide"]
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

## Analysis

> 	.long	0x41100000                      ; float 9
>
>   ;0100 0001 0001 0000 0000 0000 0000 0000

Real number = $(-1)^{0}$ $\times$ 1.0010 0000 0000 0000 0000 000 $\times$ $2^{130 - 127}$

Obviously, using float to save small integer won't lost accuracy.


