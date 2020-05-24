---
title: " Image processing 2"
date: 2020-05-22 08:26:28 -0400
categories:  Image processing
---
## opneCv 정리

### openCv 사용법

#### 이미지 읽기

```
cv2.imread('file_name' , flag)
```
- flag 의 종류  
+ IMREAD_COLOR : 이미지를 컬러로 읽고, 투명한 부분은 무시함   
+ IMREAD_GRAYSCALE : 이미지를 흑백으로 읽음   
+ IMREAD_UNCHANGED : 이미지를 그대로 읽어옴(투명한부분도 읽음)  

openCv의 주의점은 RBG가 아니라 BGR로 

#### 읽은뒤 열기
```
cv2.imshow('title', image)
```
- title : 윈도우로 열렸을때의 창의 이름
- image: 출력할 이미지 객체 변수 이름


#### 처리후 저장
```
cv2.imwrite('file_name', image)
```
- file_name = 저장할 파일 이름
- image : 저장할 이미지 객체 변수 이름

#### 창 유지시간
```
c2.waitKey(time)
```
창을 열고 대기 할시간을 입력함 
- time : 입력 대기 시간 (0은 무한 대기 )

#### 화면의 모든 윈도우 닫기
```
cv2.destroyAllWindows()
```

#### 종합 정리

```
img_basic = cv2.imread('cat/cat1.jpg' , cv2.IMREAD_COLOR) #cat 폴더에 있는 'cat1.jpg' 를 컬러로 열기
cv2.imshow('this is cat' , img_basic) # 고양이라는 이름으로 'cat.jpg' 파일을 윈도우창으로 열기
cv2.waitKey(0) #창이 열려있는 시간 
cv2.imwrite('result1.png', img_basic) # 처리후 저장
```
컬러로 연후에 컨버터 할수 있음  
내용은 아래와 같음  

```
img_basic = cv2.imread('cat/cat1.jpg' , cv2.IMREAD_COLOR)
cv2.imshow('this is cat' , img_basic) 
cv2.waitKey(0) 

cv2.destroyAllWindows() # 닫힌후 실행이지만 vscode 에서 실행했을땐 차이점을 못느낌

img_gray = cv2.cvtColor(img_basic, cv2.COLOR_BGR2GRAY)
cv2.imshow('this is cat' , img_gray )
cv2.waitKey(0)
```

```cv2.COLOR_BGR2GRAY``` 에서 2는 to와 같음
컬러사진이 뜬후 닫으면  
흑백사진이 나옴  


### 이미지 연산

#### openCv를 활용한 이미지 크기 및 픽셀 정보 확인

```
import cv2

img = cv2.imread('cat/cat1.jpg')


print(img.shape) # 픽셀 크기 
print(img.size) # 이미지 사이즈

px = img[250,250] # 흑백은 BGR 로 나오지 않음

print(px[2]) # BGR 중 R 값만 출력도 가능


```

#### 특정범위 픽셀 변경

```
img = cv2.imread('cat/cat1.jpg' )
# for i in range(0,100):
#     for j in range(0, 100):
#         img[i,j] = [0, 0, 0] 
#for 문을 이용할수도 있지만
# cv2.imshow('this is cat' , img) 
# cv2.waitKey(0) 

img[0:100 , 0:100] = [255 , 0, 255]
#여기서 각각 0:100 은 세로범위 가로범위 임
#슬라이싱 범위를 사용하는게 훨씬 빠름
cv2.imshow('this is cat' , img) 
cv2.waitKey(0) 
```
0부터 99까지의 픽셀을 색을 바꿔줌

#### ROI(Region of interest : 관심영역)추출

```
image = cv2.imread('cat/cat1.jpg')

# Numpy Slicing: ROI 처리 가능
roi = image[200:350, 50:200]

# ROI 단위로 이미지 복사하기 범위가 다르면 오류뜸
image[0:150, 0:150] = roi

cv2.imshow('this is cat' , image)
cv2.waitKey(0) 

```

#### 픽셀별로 색다루기

```
image = cv2.imread('cat/cat1.jpg')

#image[:, :, 2] = 0 # 모든 픽셀에 대해서 index 2번값을 지움 
image[:, :, 1] = 0 # 모든 픽셀에 대해서 index 1번값을 지움 
cv2.imshow('this is cat' , image)
cv2.waitKey(0) 
```

B G R 순서인데 index 2 는 빨간색임으로 빨간색 삭제
index 1 은 초록색임으로 모든 초록색 삭제

### 이미지 변형

#### 이미지 크기 조절

```
cv2.resize(img , dsize , fx , fy , interpolation )

```
- dsize : 메뉴얼 사이즈
- fx : 가로 비율
- fy : 세로 비율
- interpolation : 보간법(사이즈가 변할 때 픽셀 사이의 값을 조절 하는 방법)
   + INTER_CUBIC : 사이즈를 크게 할때 주로 사용
   + INTER_AREA : 사이즈를 작게 할때 주로 사용

```
image = cv2.imread('cat/cat1.jpg')

expand = cv2.resize(image, None, fx=2.0, fy=2.0, interpolation=cv2.INTER_CUBIC)

# shrink = cv2.resize(image, None, fx=0.8, fy=0.8, interpolation=cv2.INTER_AREA)

cv2.imshow('this is cat' , expand)
cv2.waitKey(0) 

```
#### 이미지 위치 바꾸기

```
image = cv2.imread('cat/cat1.jpg')

height, width = image.shape[:2] # 0과 1의 인덱스 값(2개)를 가지고 left 와 right 에 넣음
#행의 갯수가 높이 열의 갯수가 너비임

move = np.float32([[1, 0, 50], [0, 1, 10]]) 
dst = cv2.warpAffine(image, move, (width, height))

cv2.imshow('this is cat' , dst)
cv2.waitKey(0) 
```

여기서 세로의 길이 가로의 길이 등을 헷갈리지 않게 잘 구분 해야함  
배열에서의 행의 갯수가 높이가 되고 
```cv2.warpAffine``` 에서는 너비 높이 즉, 가로세로를 입력해야함

#### 이미지 회전
```
c2.getRotationMatrix2D(center , angle, scale)
```
- center : 회전 중심
- angle : 회전 각도
- scale : 크기 비율

```
image = cv2.imread('cat/cat1.jpg')

height, width = image.shape[:2]
print(image.shape[:2])

move = cv2.getRotationMatrix2D((width / 2, height / 2), 90, 0.8)
#너비의 절반, 높이의 절반 == 중앙 을 기준으로 잡고 90도 시계반대 방향으로 회전뒤 0.8의 크기로 출력
dst = cv2.warpAffine(image, move, (width, height))

cv2.imshow('this is cat' , dst)
cv2.waitKey(0) 
```
























