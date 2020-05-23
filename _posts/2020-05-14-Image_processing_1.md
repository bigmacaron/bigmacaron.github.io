---
title: " Image processing 1"
date: 2020-05-21 08:26:28 -0400
categories:  Image processing
---

## numpy 정리

### numpy array

#### list 의 numpy 화
```
list_data = [1,2,3]

array = np.array(list_data)

print(list_data)


print(type(list_data))

```
```np.array(list명)```넘파이 자료형으로 바꿔줌
```
print(array.size)
print(array.dtype)
print(array[1])
```
크기, 타입,인덱스 접근이 가능함   

#### 배열을 쉽게 생성하고 ,특정값으로 채우기

```
array1 = np.arange(4) #0부터 3까지 숫자를 가진 array 생성
print(array1)
```
0부터 3까지 숫자를 가진 array 생성  

```
array2= np.zeros((4,4), dtype=float)
```
4 * 4 의 배열을 가지고, 데이터 타입은 float 이며 모두 0으로 채운 배열생성

```
array3 = np.ones((3,3), dtype=int)
```
3 * 3 의 배열을 가지고, 데이터 타입은 int 이며 모두 1으로 채운 배열생성

```
array4 = np.random.randint(0, 10 , (3,3))
```

3 * 3 의 배열을 가지고, 데이터 타입은 int 이며 랜덤숫자로 채운 배열생성  

#### 배열 합치기   

```concatenate()``` 이용하기  

```
array1 = np.array([1,2,3])
array2 = np.array([4,5,6])
array3 = np.concatenate([array1, array2])

print(array3)
````
array3 의 출력 결과는 [1,2,3,4,5,6] 이 나옴

```reshape()```

```
array1 = np.array([1,2,3,4,5,6])
array2 = array1.reshape((2,3))

print(array2)
```
출력 결과는 [[1 2 3] [4 5 6]] 이나옴  

이를 응용하여   

---
```
array1 = np.array([1,2,3])
array2 = np.array([4,5,6])
array3 = np.concatenate([array1, array2]) # [1,2,3,4,5,6] 으로 합침  

array4 = array3.reshape((2,3)) # 2 * 3 행렬로 만듦  
```
1차원 배열은 이런식으로 합쳐야함  
아래는 2차원 배열임  
```

array1 = np.arange(4).reshape(1,4) # 0부터 3까지 1*4 행렬 생성 [[0,1,2,3]]  
array2 = np.arange(8).reshape(2,4) # 0부터 7까지 2*4 행렬 생성 [[0,1,2,3],[4,5,6,7]]   

array3 = np.concatenate([array1, array2 ], axis=0)  
```

여기서 주의할점이 있음 ```concaterate()``` 를 쓸때 1차원인지    
2차원 배열인지 확인을 해서 axis 를 쓸수있음 바로위와 같은 경우   
axis = 0 을 줘서 row 를 추가할순 있지만 column 을 추가할순 없음(어떻게 합쳐야 할지를 모름)    
다만 아래와 같은 경우는 둘다 쓸수있음    
조건은 둘다 2차원 배열이고, 행열이 같은숫자일때  

```

array1 = np.array([[1,2,3]])
array2 = np.array([[4,5,6]])
#array3 = np.concatenate([array1, array2], axis = 0)
#array3 = np.concatenate([array1, array2], axis = 1)
```
여기에서
  
 ```axis = 0``` 은 
  
```
[[1 2 3]
 [4 5 6]]
```
  
 ```axis = 1```은  
  
 ```
[[1 2 3 4 5 6]]
```
위처럼 차이점을 알고 쓰면 좋을듯함  

#### 배열 나누기  

```
array = np.arange(8).reshpe(2, 4)
left , right = np.split(array , [2] , axis=1)

print(array)
print(left)
print(right)

#[[0 1 2 3]
# [4 5 6 7]]  < array 출력결과

#[[0 1]
#[4 5]] < left 출력결과

#[[2 3]
#[6 7]] < right 출력결과

```

``` np.split(array , [2] , axis=1)``` 에서   
```[2]``` 과 ```axis=1``` 는 아래를 기준으로 나눈다는 뜻임  
[[0 1 ^  2 3]      
 [4 5  ^ 6 7]]   

#### numpy 연산  

기타 연산들이 있지만 나머지들은 직관적이라 생략하고 마스킹 연산만 정리함  

```
array1= np.arange(16).reshape(4,4)  

array2 = array1  < 10   
print(array2) #  불린값이 들어간 것만 array2에 담김  
array1[array2] = 50  

```

array1 에 10미만의 숫자들을 다 50으로 바꿈    
나중에 응용할때 이건 색상이 너무 밝거나 하는 픽셀들만 따로 처리할수있음    

```np.max``` ,```np.min```,```np.sum```,```np.mean``` 등도 쓸수있음  

```
array1 = np.arange(16).reshape(4,4) 

print(array1)  
print("세로열의 합계 :", np.sum(array1 , axis=0))  
print("가로행의 합계 :", np.sum(array1 , axis=1))  
```

이런식으로 가로와 세로의 합계도 구할수 있음  

### numpy 함수   

#### 저장과 불러오기  

```
array = np.arange(0,10)
np.save('saved.npy', array) #saved.npy 라는 파일이 생성됨

result = np.load('saved.npy) #그냥 열수 없음으로 np.load를 사용함
```
단일 파일을 저장하는 방법

```
array1 = np.arange(0, 10)
array2 = np.arange(10 , 20)

np.savez('saved.npz' , array1 = array1, array2 = array2) #savez 를 이용해서 복수의 개체를 저장

data = np.load('saved.npz') #불러오기  
arrayone = data['array1']  
arraytwo = data['array2']  # 각각 인덱스로 접근 가능  

print(arrayone)
print(arraytwo)
```

각각 저장해둔뒤 인덱스값으로 각각 불러올수 있음  

#### 내림차순 오름차순 정렬

```arrayname.sort()``` 과  
```print(arrayname[::-1])``` 를 이용하면 된다  
각각 오름차순 내림 차순 정렬을 시켜준다.   
단,sort() 이후에 [::-1] 을 써야함

#### 각열을 기준으로 정렬

```
array = np.array([[5,9,10,3,1] , [8,3,4,2,5]])

array.sort(axis=0)

print(array)

#[[ 5  3  4  2  1]
# [ 8  9 10  3  5]]
```
위처럼 열을 기준으로 오름차순 정렬이 가능하다.

#### 균일한 간격으로 데이터 생성
```linspace()``

```
array = np.linspace(0,10,5) 

print(array)
```
0부터 10 사이의수를 간격을 두어 5개를 생성함   
즉, [ 0.  , 2.5  , 5.  , 7.5,  10. ] 이 생성됨   

#### 난수의 재연
```
print(np.random.randint(0,100, (2,3)))

# 2*3 행렬을 0에서 100까지의 수를 랜덤으로 집어 넣어서 생성
```
위는 매번 실행시 마다 값이 달라지는것을 알수있다.  
그래서 아래와 같이 실행함  
  

```
np.random.seed(7)
print(np.random.randint(0,100, (2,3)))
```
위처럼 하면 매번 같은 난수를 생성할수 있다 .  

#### 복사
```
array1 = np.arange(0,10)
array2 = array1
array2[0] = 99
print(array1)
```
위는 array1의 값이 바뀐걸 알수있다.(주소값이 같기 때문)
```
array1 = np.arange(0,10)
array2 = array1.copy()  #복사해서 넣음 (주소값이 달라짐)
array2[0] = 99
print(array1)
```
위처럼 하면 array1 에 영향을 끼치지 않음

#### 중복된 원소 제거 

```
array = np.array([1,1,2,2,3,3,3,3,4,4,5])
print(np.unique(array))
```

```np.unique()```를 이용함

  





















