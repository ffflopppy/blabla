
# 설치


# Import Libraries
파이썬의 모든 모듈은 라이브러리로 되어 있다.
> import와 from 의 차이 알아보기

sqrt(스퀘어루트)

'generic import' of math module
```
import math
math.sqrt(25)
```

import a function **from** math
```
from math import sqrt
sqrt(25)
```

import multiple functions at once from math

```
from math import cos, floor
import os  #데이터 영역 설정 하는 라이브러리 -> 파일의 위치 설정
os.getcwd() #현재 나의 디렉토리 아는 방법
os.chdir("User/ffflopppy") # 디렉토리 바꾸는 chdir
```

define an alias
```
import numpy **as** np
```

show all functions in math module
파이썬은 dir 이 중요.
help기능이 약하다. 함수의 기능이 궁금하다면 dir!!
```
dir(math)
```
## Commnets: 주석
```
# 한줄

"""
여
러
줄
"""
```

## help
help("modules") : 설치된 모듈 목록 보기

해당 모둘 커멘드 직접 보기
```
import pandas as pd
help(len)
hepl(dict)
```

## pip @@ 공부하기
:Python 패키지를 설치하고 관리하는 프로그램
예) 패키지 mglearn, graphviz 설치하기 
Doc 창에서
```
pip install mglearn
pip instal graphviz
```
참고1 : python -m pip install -upgrade pop
참고2 : Path 설정하기 -> 패스를 걸어두는게 좋다.



---------------------------------1강
# Basic operations
## Number
```
**   # 제곱
/    # 나누기
%    # 나머지
//   # 몫
```


## Boolean : comparisons (these return true)
```
>
>=
!=
==
```

# Data types
### determine the type of an object
```
type(2)       # return 'int'
type(2.0)     # return 'float'
type('two')   # return 'str'
type(True)    # return 'bool'
type(None)    # return 'NoneType' 
```
### check if an onject is of a given type
## List[]
## Tuples ()
## Dictionary {key:value}

# Execution control statements
# If
# For
# Functions
# Object Oriented programing(OOP)
# Read text(excel) File
# Numpy
# Pandas
