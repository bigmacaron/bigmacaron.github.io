---
title: "R exercises"
date: 2020-05-17 08:26:28 -0400
categories: exercises
---
## summarise()

summarise()는 데이터 프레임을 요약 축소 하는 기능임   

```
summarise(flights, delay = mean(dep_delay, na.rm = TRUE))
```

delay  는 dep_delay에서 NA 값은 없이   평균값을 냄 이라는 뜻   

```
by_day <- group_by(flights, year, month, day)
summarise(by_day, delay = mean(dep_delay, na.rm = TRUE))
```

group_by 를 이용해 그룹화 시킴 (그룹화는 되지만 by_day 출력하면 전부다 표기됨)   
하지만 ``` summarise(by_day, delay = mean(dep_delay, na.rm = TRUE))```   
를 이용하여 사용하면 그룹화 된 것 만 묶어서 함수에서 쓸수있음   


```
by_dest <- group_by(flights, dest)  	 #도착지를 그룹화 시키고
delay <- summarise(by_dest,  		 #그룹화 시긴걸 요약화 시키고
  count = n(),                             		#count 를 이용해 카운트 
  dist = mean(distance, na.rm = TRUE),      	#평균거리
  delay = mean(arr_delay, na.rm = TRUE)  #평균 딜레이
)
```
를 한뒤 ggplot 을 이용해서

```
ggplot(data = delay, mapping = aes(x = dist, y = delay)) +	#x 축과 y축 설정
  geom_point(aes(size = count), alpha = 1/3) + 		#그래프 종류 alpha = 1/3 는 투명도
  geom_smooth(se = FALSE)				#회귀선 표기 False 는 표준오차 표기안함
```
위와 같이 그래프로 시각화 할수 있음    
   
   
1. 목적지별로 그룹 항공편   
   
2. 거리, 평균 지연 및 비행 횟수를 계산하기 위해 요약   
   
3. 시끄러운 지점과 호놀룰루 공항을 제거하기 위해 필터링   
   

```
delays <- flights %>% 
  group_by(dest) %>% 	#목적지별로 그룹 항공편을 만든뒤
  summarise(
    count = n(), 			#거리, 평균 지연 및 비행 횟수를 계산하기 위해 요약
    dist = mean(distance, na.rm = TRUE),    
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>% 
  filter(count > 20, dest != "HNL")	#시끄러운 지점과 호놀룰루 공항을 제거하기 위해 필터링함
 #count >20 이면 시끄럽다고 판단
```

목적지별로 그룹 항공편을 만든뒤   
거리, 평균 지연 및 비행 횟수를 계산하기 위해 요약   
시끄러운 지점과 호놀룰루 공항을 제거하기 위해 필터링함  
count >20 이면 시끄럽다고 판단    


## NA 값   
   
위에서 한번 다뤘지만 다시 한번더    

```
flights %>% 
  group_by(year, month, day) %>% 
  summarise(mean = mean(dep_delay))
#> # A tibble: 365 x 4
#> # Groups:   year, month [12]
#>    year month   day  mean
#>   <int> <int> <int> <dbl>
#> 1  2013     1     1    NA
#> 2  2013     1     2    NA
#> 3  2013     1     3    NA
#> 4  2013     1     4    NA
#> 5  2013     1     5    NA
#> 6  2013     1     6    NA
#> # … with 359 more rows
```
이것과   
```
flights %>% 
  group_by(year, month, day) %>% 
  summarise(mean = mean(dep_delay, na.rm = TRUE))
#> # A tibble: 365 x 4
#> # Groups:   year, month [12]
#>    year month   day  mean
#>   <int> <int> <int> <dbl>
#> 1  2013     1     1 11.5 
#> 2  2013     1     2 13.9 
#> 3  2013     1     3 11.0 
#> 4  2013     1     4  8.95
#> 5  2013     1     5  5.73
#> 6  2013     1     6  7.15
#> # … with 359 more rows
```
차이가 매우큼 NA값을 없애줘야함   
   
여기서 NA값을 미리 없애줄수 있음   

```
not_cancelled <- flights %>% 
  filter(!is.na(dep_delay), !is.na(arr_delay))

not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(mean = mean(dep_delay))
```

위에서 dep_delay 와 arr_delay 에서 필터를 걸어서 NA값을 없애주고  
not_cancelled 변수에 담음

```
delays <- not_cancelled %>% 
  group_by(tailnum) %>% 
  summarise(
    delay = mean(arr_delay)
  )

ggplot(data = delays, mapping = aes(x = delay)) + 
  geom_freqpoly(binwidth = 10)
```
tailnum 으로 그룹바이 한다음 (항공기 식별 번호)  
평균 지연 시간을 freqpoly 그래프를 만듬  
여기서 5시간 이상 평균 지연된 항공기 있음  

```
delays <- not_cancelled %>% 
  group_by(tailnum) %>% 
  summarise(
    delay = mean(arr_delay, na.rm = TRUE),
    n = n()
  )

ggplot(data = delays, mapping = aes(x = n, y = delay)) + 
  geom_point(alpha = 1/10)
```
위와 비슷한 내용이지만 항공기 갯수에 따른 지연시간을 분석해봄   
tailnum 으로 그룹바이 한다음 (항공기 식별 번호)   
평균 지연 시간을 point 그래프를 만듬  여기서 x 축을 카운트를 줌  
항공편이 거의없는 경우 평균 지연에 훨씬 더 큰 변화가 있음을 알수있음
  
## 유용한 함수들
```
not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(
    avg_delay1 = mean(arr_delay),
    avg_delay2 = mean(arr_delay[arr_delay > 0]) # the average positive delay
  )
```
평균은 합을 길이로 나눈 값 중앙값
```
not_cancelled %>% 
  group_by(dest) %>% 
  summarise(distance_sd = sd(distance)) %>% 
  arrange(desc(distance_sd))
```
확산값
```
not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(
    first = min(dep_time),
    last = max(dep_time)
  )
```
 25 %보다 크고 나머지 75 %보다 작은 값을 찾음
```
not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(
    first_dep = first(dep_time), 
    last_dep = last(dep_time)
  )
```
위치측정
매일 첫 출발과 마지막 출발을 찾을 수 있음
```
not_cancelled %>% 
  group_by(dest) %>% 
  summarise(carriers = n_distinct(carrier)) %>% 
  arrange(desc(carriers))
```
카운트  
```sum(!is.na(x))``` 를 사용하여 NA가 없는 값을 찾을수도 있음

```
not_cancelled %>% 
  count(dest)
```
dplyr은 원하는 개수 만 있으면 간단한 함수 제공
```
not_cancelled %>% 
  count(tailnum, wt = distance)
```
비행기별 총 비행거리 계산할수 있음

```
not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(n_early = sum(dep_time < 500))
```
```
not_cancelled %>% 
  group_by(year, month, day) %>% 
  summarise(hour_prop = mean(arr_delay > 60))
```

불린값 이용해서 비율을 계산 할수 있음


## 여러 변수들의 그룹화  
  

```
daily <- group_by(flights, year, month, day)
(per_day   <- summarise(daily, flights = n()))
#> # A tibble: 365 x 4
#> # Groups:   year, month [12]
#>    year month   day flights
#>   <int> <int> <int>   <int>
#> 1  2013     1     1     842
#> 2  2013     1     2     943
#> 3  2013     1     3     914
#> 4  2013     1     4     915
#> 5  2013     1     5     720
#> 6  2013     1     6     832
#> # … with 359 more rows
(per_month <- summarise(per_day, flights = sum(flights)))
#> # A tibble: 12 x 3
#> # Groups:   year [1]
#>    year month flights
#>   <int> <int>   <int>
#> 1  2013     1   27004
#> 2  2013     2   24951
#> 3  2013     3   28834
#> 4  2013     4   28330
#> 5  2013     5   28796
#> 6  2013     6   28243
#> # … with 6 more rows
(per_year  <- summarise(per_month, flights = sum(flights)))
#> # A tibble: 1 x 2
#>    year flights
#>   <int>   <int>
#> 1  2013  336776
```  

점진적인 롤업이 가능 하지만 주의할점은  
계와 개수에 대해서는 괜찮지 만 가중치 평균과   
분산에 대해 생각해야하며 중앙값과 같은 순위 기반 통계에 대해서는 
정확하게 계산할 수 없음  
즉, 그룹 별 합계의 합계는 전체 합계이지만 그룹 별 중앙값의 중앙값은 전체 중앙값이 아님  

## ungroup() 그룹해제  
  

```
daily %>% 
  ungroup() %>%               # 날짜별로 그룹화 하지 않음
  summarise(flights = n())  # 모든 항공편
```

## summarise() , mutate() , filter() 응용 


```
flights_sml %>% 
  group_by(year, month, day) %>%
  filter(rank(desc(arr_delay)) < 10)
```
```
popular_dests <- flights %>% 
  group_by(dest) %>% 
  filter(n() > 365)
popular_dests
```
```
popular_dests %>% 
  filter(arr_delay > 0) %>% 
  mutate(prop_delay = arr_delay / sum(arr_delay)) %>% 
  select(year:day, dest, arr_delay, prop_delay)
```

위는 공부가 조금더 필요할듯함
