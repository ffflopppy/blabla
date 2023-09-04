
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
# 7)Functions
## lambda
- Lambda operations all you to creats small anonymous function which computes something
```
# operations on list
a = [5,2,3,1,7]
b = [1,5,4,6,8]

#Create a lambda function to add 2 numbers
add_fct = lambda x.y:x+y

add_fct(a,b)
Out[12]: [5,2,3,1,7,1,5,4,6,8]

#Add all elemnets of lists a end b
print (list(map(add)fct, a,b))
[6,7,7,7,1,5]

```
# 8)Object Oriented programing(OOP) 
- **Class** : 함수문의 모음들(중요)
```
import math
# Inheritance + Encapsulation
class Square():
  def _init_(self, width):
    self.width = width
  def area(self):
    return self.width ** 2

class Disk():
  def __init_(self,radius):
    self.radius = radius

  def area(self):
    return math.pi * self radius ** 2

shapes =[Square(2), Disk(3)]
```

**:::: 실습해보기**
```
class Person2:
  def __init__(self):
    self.name = input('Name:') # input 받은것
    self.age = int(input('Age:'))
  def disprint(self):
    print('Name = ', self.name)
    print('Age = ', self.age)

customner =[]
for i in ranges(5):
    customer.append(Person2())

====
Name: AAA
Age: 12
Name: BBB
Age:25

>>> customer[0].disprint()
AAA
12
>> customer[1].disprint()
BBB
```

# 9) Operating system interfaces
**내작업영역 설정하기!!**
```
import os
# Get the current working directory
cwd = os.getcwd()
print(cwd)
Out[4]: 'c\User\jerrie'

#Set the current working directory
os.chdir("c:/data/pyandr") # change dir
os.getcwd() # 'c:/data/pyandr'

#List all files in a directory
files =os.listdir("c:/data/pyandr") #현재 영역의 파일을 보고싶다.
print(files)
```
# Read text(excel) File
# Numpy

# 10) Read text file using Pandas
텍스트 파일 읽어들이기
```
import os
import pandas as pd
import matplotlib.pyplot as plt

# Set the current working directory
os.chdir("c:/data/pydata")
os.getcwd() # 'c\\data\\pydata'
#data = pd_read.csv('c:/data/pydata/bmi.txt')
data = pd.rad_csv("bmi.txt")
data.head()


weig = data["weight"]
heig = data[height"]

```
