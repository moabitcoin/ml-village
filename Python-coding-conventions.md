# Introduction

Hi there ! Glad you could make time for this topic. We know this is an overhead we could avoid discussing but we have found it to save us many hours on merge request, discussing formatting & ensue code quality. Within python :snake: environment, we ([follow](https://i.imgur.com/W4oNHsD.gif)) [PEP8](https://www.python.org/dev/peps/pep-0008/) coding conventions. Easiest way to enforce this into your favourite text editor is to install a PEP8 [package/plugins](https://pypi.org/project/pycodestyle/). 

# Table of Contents
1. [Tabs v Spaces](#tabs-v-spaces)
2. [Maximum line length](#maximum-line-length)
3. [Module imports](#module-imports)
4. [Function indent](#function-indent)
5. [Blank lines](#blank-lines)
6. [Whitespaces in expressions](https://www.python.org/dev/peps/pep-0008/#whitespace-in-expressions-and-statements)
7. [Function & variable names](https://www.python.org/dev/peps/pep-0008/#function-and-variable-names)
8. [Constants](https://www.python.org/dev/peps/pep-0008/#constants)
9. [Class Names](https://www.python.org/dev/peps/pep-0008/#id41)
10. [Avoid names with](https://www.python.org/dev/peps/pep-0008/#names-to-avoid)
11. [Naming style](https://www.python.org/dev/peps/pep-0008/#descriptive-naming-styles)
12. [In-line comments](#in-line-comments)

## Tabs v Spaces
**Tabs are 2 spaces**. After much deliberation we decided something everyone disagrees on. For compatibility please change your editor settings to follow this convention. 

`Good`
```
def enter_club(club='berghain'):
  
  # try
  return denied

```
`No Good`
```
def enter_club(club='berghain'):
  
    # try
    return denied

```
## Maximum line length
**Maximum line length is set to 80**. This allows wed editors/local editors to compare files side-by-side when doing to MR/PR. Also recommended in [PEP8](https://www.python.org/dev/peps/pep-0008/#maximum-line-length). 

`Good`
```
def never_gonna(give_you='up', let_you='down', run='around', desert='you',
                make_you='cry', say='goodbye'):
  return troll

```
`No Good`
```
def never_gonna(give_you='up', let_you='down', run='around', desert='you', make_you='cry', say='goodbye'):
  return troll

```

## Imports 
**All imports are [absolute](https://www.python.org/dev/peps/pep-0008/#imports)**. Organise imports in the following order at the top your python file. 
1. Standard library imports.
2. Related third party imports.
3. Local application/library specific imports.

`Good`
```
import os
import sys
import collections 

import tqdm

from planet import Earth as home
from planet.Earth import dogs as friends 
```

`NO Good`
```
import os, sys, collections

import tqdm
from planet import Earth as home
from planet.Earth import dogs as friends 
```
## Hanging indent
It's clean and it's recommended to follow, separating function name and input variables. This holds for function definition and function calls. 

`Good`
```
wimbaway = in_the_jungle(the_quite, jungle,
                         the_lion, sleeps_tonight)
```
`NO Good`
```
wimbaway = in_the_jungle(the_quite, jungle,
           the_lion, sleeps_tonight)
```

## Blank lines
Surround top-level function and class definitions with two blank lines. Method definitions inside a class are surrounded by a single blank line. Use blank lines in functions, sparingly, to indicate logical sections.

```
def bless_the_rains(down_in_africa=True):

  # Black as a pit from pole to pole


def wake_me_up(when_september_ends=True):

  # of August

```

```
class Humans(object):

  def __init__(self, no_super_powers=True):

    self. no_super_powers = no_super_powers


class XMen(object):

  def __init__(self, prof_xavier=None):

    self.xavier = prof_xavier
    self.magneto = magneto

  def _is_wolverine_alive(self):

    return True

  def _are_mutants_banned(self):

    return True

```
## In-line comments

`Good`
```
# This is a hack 
w -= (gradient_f(x, w) * learning_rate)
```
`No good`
```
w -= (gradient_f(x, w) * learning_rate)  # This is a hack 
```
Unless it helps (still limiting to 80 chars max length)
```
w -= (gradient_f(x, w) * learning_rate)  # comp grads and update weights
```