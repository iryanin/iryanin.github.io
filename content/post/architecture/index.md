+++
author = "Ryan"
title = "Miscellaneous of architecture"
date = "2023-02-23"
description = "About implementation details of arm architecture"
tags = [
    "Arm assembly",
    "Architecture",
]
categories = [
    "Programming Language",
    "Architecture"
]
image = "cover.jpg"
+++

## IEEE754 Floating Point Representation

### Rounding rules
1. Round to nearest, ties to even
    * Larger than 0.5 : Add 1
    * Smaller than 0.5 : Do nothing
    * Equal to 0.5 : 
    * 0 if last digit is 0
    * 1 if last digit is 1
2. Round to nearest, ties away from zero
3. Round toward 0
4. Round toward $\+\infty$ (Ceiling)
5. Round toward $\-\infty$ (Floor)
