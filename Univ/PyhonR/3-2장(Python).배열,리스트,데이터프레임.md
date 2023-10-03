# 배열
~~~
import numpy as np
xv = np.array([1,2,3,4,5,6,7,8,9])
xv2 = np.array(range(1,10))
print(xv)
print(xv2)

# [1 2 3 4 5 6 7 8 9]
# [1 2 3 4 5 6 7 8 9]
~~~
~~~
xm = np.array([[1,2,3],[4,5,6],[7,8,9]])
print(xm)
xm2 = np.arange(1,10)
xm2 = xm2.reshape(3,3)
print(xm2)

## [[1 2 3]
##  [4 5 6]
## [7 8 9]]

## [[1 2 3]
## [4 5 6]
## [7 8 9]]
~~~

> xm[,1] # 첫번째 열
> 
> <img width="160" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/e76fdcc9-5955-4f8f-b836-b3d3b3c50624">

> xm[0:2, :] # 첫번째 두번째 행
> 
> <img width="178" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/e008442f-6abe-4d69-8337-744bb8a487f8">

> xm[0:2, 0]
> 
> <img width="136" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/2bbf2e5b-29f4-4826-a009-0c126015e682">


# 리스트[], 튜플(), 딕셔너리{}


### 1. list
리스트는 값의 수정이 가능하지만, 튜플은 값의 수정이 불가능하다
~~~
a = [1,2,3]
print(a[1]) ##2
~~~

> 파이썬에서 a 리스트 값을 b에 대입하여 복사한 경우 b의 값을 별도로 저장하지 않고 a 값의 주소값을 저장한다(call by reference).
> 
> 값을 카피하고싶은 경우 deepcopy(a) 값을 저장할수 있다(call by value).
> ~~~
> import copy
> b = copy.deepcopy(a)
> ~~~
### 2. tuple
~~~
tpl = ('AA','BB','CC')
print(tpl[1]) ## BB
~~~
### 3. dictionary
딕셔너리는 key와 값을 값는 자료구조{key:value}
~~~
country_code ={'korea':82, 'us':1, 'china':86}
print(country_code['korea'] ## 82
~~~


~~~
xv = np.arange(1,10)
xv[1:4] ## array([2,3,4])
~~~

# 데이터프레임

