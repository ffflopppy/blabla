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

~~~
xm[:,2] # 3번째열
xm[0:2, :]
~~~
<img width="169" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/e158427f-76ec-4074-952f-198c77266cb8">

R에서 행렬은 matrix 함수로 만들수 있다.
~~~
xm <- matrix(1:9, nrow =3, byrow=T, dimnames=list(c('r1','r2','r3'), c('col1','col2','col3')))
~~~
<img width="712" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/99586bba-2e3f-464f-8f39-ba294993e953">

// matrix(data=NA, nrow=1,ncol=1, byrow=FALSE, dimnames=NULL)
- dat: 행렬의 원소에 해당하는 값들을 설정한다
- nrow, ncol : 행의 수와 열의 수를 설정하는데 둘중 하나의 값만 설정하면 나머지 값은 data에서 설정한 변수의 값의 길에 의해서 자동으로 결정된다.
- byrow: 기본 값은 FALSE로, data의 값으로 첫 번째 열, 두 번째 열 등등의 순서로 값을 지정하게 되며 이 값이 TRUE이면 첫 번째 행, 두 번째 행 등등의 순서로 값을 지정한다.
- dimnames: 행렬의 행 이름과 열 이름을 따로 설정할 수 있다. 대개 숫자 계산을 위한 행렬에서는 행과 열의 이름이 필요 없기 때문에 dimnames는 설정하지 않는다.

~~~
xm <- matrix(1:9, nrow =3, byrow=T)
colnames(xm) <- c('col1','col2','col3')
rownames(xm) <- c('r1','r2','r3')
~~~
<img width="294" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/4e3d8627-25d6-400a-a9b6-0d01349f70d8">
dimnames를 후에 지정할수 있다.


>> xm[,1] # 첫번째 열

<img width="120" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/a98954a9-a750-4023-be63-86b9efeba9e6">

>> xm[0:2, :] # 첫번째 두번째 행
<img width="178" alt="image" src="https://github.com/ffflopppy/blabla/assets/58408873/e008442f-6abe-4d69-8337-744bb8a487f8">


# 리스트
# 데이터프레임

