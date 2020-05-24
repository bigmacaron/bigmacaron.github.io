---
title: " Image processing 3"
date: 2020-05-23 08:26:28 -0400
categories:  Image processing
---

https://076923.github.io/posts/Python-opencv-8/   

img resize에 관한 글


## opneCv 정리

### openCv 사용법

#### 이미지 합치기

```
image = cv2.imread('cat/cat1.jpg')
img = cv2.imread('cat/cat2.jpg')

# height, width = image.shape[:2]
# print(image.shape[:2])

resize_img=cv2.resize(img , dsize=(478, 512) , interpolation=cv2.INTER_AREA )

result1 = cv2.add(image, resize1)
# print(image.shape)
# print(resize1.shape)

#result2 = image + resize_img


cv2.imshow('this is cat' , result1)
cv2.waitKey(0) 
```

```result1``` 는 openCv 에서 제공하는 saturation 연산  
255 이상인값은 255로 표기
```result2``` 는 numpy에서 제공하는 modulo 연산
255 이상의 값은 255로 나눈 나머지값으로 표기


#### 임계점 처리하기

```
cv2.threshold(image, thresh, max_value,type)
#임계값을 기준으로 흑/백을 분류 하는 함수
```

- image : 처리할 Gray Scale 이미지  
- thresh : 임계 값  
- max_value : 임계 값을 넘었을 때 적용할 값  
- type : 임계점을 처리하는 방식  
+ THRESH_BINARY : 임계 값 보다 크면 max_value, 작으면 0  
+ THRESH_BINARY_INV : 임계 값 보다 작으면 max_value , 크면 0
+ THRESH_TRUNC :  임계 값 보다 크면 임계 값, 작으면 그대로
+ THRESH_TOZERO: 임계 값 보다 크면 그대로 , 작으면 임계 값
+ THRESH_TOZERO_INV : 임계 값 보다 크면 0, 작으면 그대로

```
cv2.adaptiveThreshold(image, max_value, adaptive_method, type, block_size, C)
#적응 임계점 처리 함수
```

- max_value : 임계 값을 넘었을 때 적용할 값    
- adaptive_method : 임계 값을 결정하는 계산 방법    
+ ADAPTIVE_THRESH_MEAN_C  : 주변 영역의 평균값으로 결정  
+ ADAPTIVE_THRESH_GAUSSIAN_C :  가우시안 분포 필터를 이용  
- type :  임계점을 처리하는 방식  
- bolck_size : 임계 값을 적용할 영역의 크기  
- C : 평균이나 가중 평균에서 차감할 값

#### Tracker

```
cv2.createTrackbar(track_bar_name , window_name, value , count , on_change )
#tracker 생성 함수
```

- value : 초기값
- count : Max 값 (min : 0 )
- on_change : 값이 변경될 때 호출 되는 callBack 함수



```
import cv2
import numpy as np

def change_color(x):
  r = cv2.getTrackbarPos("R", "Image") 
  # R 이라는 트랙바 이름과 변경할 이미지 이름
  g = cv2.getTrackbarPos("G", "Image")
  b = cv2.getTrackbarPos("B", "Image")
  image[:] = [b, g, r]
  #전체 이미지에서 모든 영역의 값을 b g r 로 넣어줌
  cv2.imshow('Image', image) #값이 바뀔때마다 적용 해야함으로 
  
  
image = np.zeros((600, 400, 3), np.uint8)
# 600 * 400 의 크기, 3개의 색을 담을수 있는 image를 만듬
# 데이터 타입에 관한 설명은 아래 링크 달아둠
cv2.namedWindow("Image")

cv2.createTrackbar("R", "Image", 0, 255, change_color)
#트랙바
cv2.createTrackbar("G", "Image", 0, 255, change_color)
cv2.createTrackbar("B", "Image", 0, 255, change_color)

cv2.imshow('Image', image)
cv2.waitKey(0)
```
트랙바 예제  
데이터 타입 설명  
https://kongdols-room.tistory.com/53


#### openCv 도형그리기
---
##### 직선
```
cv2.line(image, start, end , color, thickness)
#하나의 직선을 그리는 함수
```

- start : 시작 좌표(2차원)
- end : 종료 좌표 (2차원)
- thickness : 선 두께

```
import cv2
import numpy as np

image = np.full((512, 512, 3), 255, np.uint8)  
image = cv2.line(image, (0, 0), (255, 255), (255, 0, 0), 2)  
#(0,0) 에서 (255,255) 까지 (0,0,255) 의 색으로 두께 2  
#cv2 는 BGR  임 빨간색이 그어짐  

	
````
0,0 의 좌표 에서 255,255 까지 흰색으로 채운 이미지에       
빨간색 직선을 그림  
    
##### 사각형
```
cv2.rectangle(image, start, end , color, thickness)
#사각형 그리는 함수
```
- start : 시작 좌표(2차원)  
- end : 종료 좌표 (2차원)  
- thickness : 선 두께 (-1 의 값은 채우기)  

```
import cv2  
import numpy as np  
  

image = np.full((512, 512, 3), 255, np.uint8)  
#흰색 이미지 생성
image = cv2.rectangle(image, (20, 20), (255, 255), (255, 0, 0), 3)
#(20, 20) 에서 (255, 255) 까지 (255, 0, 0) 파란색 선두께 3
#image = cv2.rectangle(image, (20, 20), (255, 255), (255, 0, 0), -1)
#위에껀 파란색으로 채움

cv2.imshow('Image', image)
cv2.waitKey(0)
```
20,20 의 좌표 에서 255,255 까지 흰색으로 채운 이미지에        
파란색 (안을 채우지 않은) 사각형을 그림     

##### 원

```
cv2.circle(image, center, radian , color, thickness)
# 원 그리는 함수
```

- center : 원의 중심  
- radian : 반지름  
- thickness : 선 두께 (-1 의 값은 채우기)  

```

import cv2
import numpy as np

image = np.full((512, 512, 3), 255, np.uint8)
image = cv2.circle(image, (255, 255), 60, (0, 0,0), 2)

cv2.imshow('Image', image)
cv2.waitKey(0)
```

중심점이 255,255 인 곳에서 반지름 60인 두께2 의 선으로 그림  

##### 다각형 그리기  

```
cv2.polylines(image, points ,is_closed , color, thickness)
#다각형 그리는 함수
```

- points : 꼭지점들
- is_closed : 닫힌 도형 여부 (True ,False)
- thickness : 선 두께 (-1 의 값은 채우기)  

```
import cv2
import numpy as np


image = np.full((512, 512, 3), 255, np.uint8)
points = np.array([[5, 5], [128, 218],[373, 333] ,[300, 200], [300, 100]])
image = cv2.polylines(image, [points], True, (0, 0, 0), 3)


cv2.imshow("img", image)
cv2.waitKey(0)
```
다격형 예제

##### 텍스트 그리기

```
cv2.putText(image, text ,position, font_type , font_scale, color)
#텍스트 그리는 함수
```

- position : 텍스트 출력 위치
- font_type : 글씨체
- font_scale : 글씨 크기 가중치

```
import cv2
import numpy as np

image = np.full((512, 512, 3), 255, np.uint8)
image = cv2.putText(image, 'Hello Python', (70, 255), cv2.FONT_ITALIC, 2, (0, 0, 0))


cv2.imshow("img", image)
cv2.waitKey(0)
```
텍스트 그리기 예제  
Hello Python!  

#### 이미지 활용
---

##### Contours (외각선 찾고, 그리기)

```
cv2.findContours(image, mode, method)
#이미지에서 외각선을 찾는 함수
```
- mode : 외각선을 찾는 방법  
+ RETR_EXTERNAL : 바깥쪽 Line 만 찾기  
+ RETR_LIST : 모든 Line 을 찾지만 Hierarchy 구성 안함  
+ RETR_TREE : 모든 Line 을 찾지만 Hierarchy 구성 함  
- method : 외각선을 찾는 근사치 방법  
CHAIN_APPROX_NONE : 모든 외각선 포인트 저장  
CHAIN_APPROX_SIMPLE : 외각선 라인을 그릴수 있는 포인트만 저장  
  
입력 이미지는 그레이 스케일 전처리 과정이 필요

```
cv2.drawContours(image, contours, contour_index, color, thickness)
#외각선을 그리는 함수
```
- contour_index: 그리고자 하는 Contours Line (전체: -1)

```
import cv2
import numpy as np

# img=cv2.imread("cat/cat1.jpg" ,cv2.IMREAD_GRAYSCALE)
# 위처럼 불러올수도 있음

image =cv2.imread("cat/cat1.jpg")
image_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
ret, thresh = cv2.threshold(image_gray, 220, 255, 0)
#cv2.threshold 를 하면 ret 에 임계치가 들어감(지금은 필요없는 값)
# 임계치 보다 크면 255, 적으면 0으로 설정함
#  위에 0 의 값이 cv2.THRESH_BINARY 와 같음


cv2.imshow("this is tile", thresh) 
cv2.waitKey(0)
# cv2.imwrite('cat_gray.jpg', image_gray)

contours = cv2.findContours(thresh, cv2.RETR_TREE, cv2.CHAIN_APPROX_SIMPLE)[0]
#위에 흑백으로 변환한 사진을 가지고 외각선을 따옴
#위처럼 ret 을 이용해도 되고 인덱스값 0만 들고와도 됨
img = cv2.drawContours(image, contours, -1, (0, 255, 0), 4)

# cv2.imwrite('cat_contour.jpg', img)
cv2.imshow("this is tile", img) 
cv2.waitKey(0)
```
[cat_gray]('bigmacaron.github.io/img/cat_gray.jpg')
[cat_contour]('bigmacaron.github.io/img/cat_contour.jpg')
