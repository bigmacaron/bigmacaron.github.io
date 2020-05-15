---
title: "R exercises"
date: 2020-05-15 08:26:28 -0400
categories: exercises
---









# R 연습 및 재정리 

```
install.packages("nycflights13")	
library(nycflights13)
library(tidyverse)
flights
rm(list = ls()) 	#리스트 삭제
```
입력하고 시작

## 1. filter()  사용법

### 1.filter()

```
new_year <- filter(flights, month == 1, day == 1)
```

 flights 에서 월, 일이 1인 값을 필터링 하여 new_year 에 담음
 dplyr 함수는 입력을 수정하지 않으므로 결과를 저장하려면 대입 연산자를 사용해야함

```
(christmas <- filter(flights, month == 12, day == 25))
```

위와 똑같지만 한번에 수행

```
nov_dec <- filter(flights, month == 11 | month == 12)
```

11 월 또는 12 월에 출발 한 모든 항공편 일반적인 논리 연산자와 같음

```
nov_dec2 <- filter(flights, month %in% c(11, 12))
```

위와 같은 내용 임 month column 에서 11과 12의 값을 가지는걸 필터링함

```
filter(flights, !(arr_delay > 120 | dep_delay > 120))
filter(flights, arr_delay <= 120, dep_delay <= 120)
```

비슷한 내용임 다른 언어와 비슷하게 ! (not) 연산자를  
쓸수 있고 이상 이하 같은 연산자를 사용가능  


## 2.missing values
```
NA > 5
#> [1] NA 라고나옴
10 == NA
#> [1] NA 라고나옴
NA + 10
#> [1] NA 라고나옴
NA / 2
#> [1] NA 라고나옴
```

여기서 NA 는 값이 없음을 의미함  
예를 들어 

```
# Let x be Mary's age. We don't know how old she is.
x <- NA

# Let y be John's age. We don't know how old he is.
y <- NA

# Are John and Mary the same age?
x == y
#> [1] NA
# We don't know!
```


마리와 존의 나이를 모름으로 NA 를 넣고 비교를 하면 둘다 NA 가 나오는게   
직관적으로 보면 당연한 결과지만 헷갈릴수도 있음

is.na() 함수

```
is.na(x)
#> [1] TRUE
```

값이  있는지 없는지 보르면  is.na() 을 사용하면됨

```
df <- tibble(x = c(1, NA, 3), y=c(1, 2, TRUE))

filter(df, x > 1)
```

 x ,y column에 tibble 을 이용해 생성 TRUE는 bool 이 아닌 1로 들어감    
 NA 값은 제외됨 

## 3.연습문제

*모든 항공편 찾기    
  -도착 지연 시간이 2시간 이상이었음  
  -휴스턴으로 날아갔다(IAH 또는 HOU)  
  -United, American 또는 Delta에서 운영  
  -여름에 출발 (7 월, 8 월, 9 월)  
  -늦게 출발 하지 않았으나 2시간 늦게 도착  
  -최소 1시간 이상 지연되었지만 비행 중 30분 이상 지연됨  
  -자정과 오전 6시 사이에 출발 (포함)  

```

```
