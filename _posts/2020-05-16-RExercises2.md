---
title: "R exercises"
date: 2020-05-16 08:26:28 -0400
categories: exercises
---

## arrange()

arrange()와 filter() 는 행을 선택하는 대신 순서를 변경한다는 점 을 제외하고 는 비슷하게 작동

```
arrange(flights, dep_time, year , month, day )
```

지연시간 -> 년,월,일 순서로 정렬 

```
arrange(flights, desc(dep_delay))
```

내림차순으로 정렬 할수 있음

tibble을 써서 NA  값에 대해 알아보면

```
df <- tibble(x = c(5, 2, NA))
arrange(df, x)
#> # A tibble: 3 x 1
#>       x
#>   <dbl>
#> 1     2
#> 2     5
#> 3    NA
arrange(df, desc(x))
#> # A tibble: 3 x 1
#>       x
#>   <dbl>
#> 1     5
#> 2     2
#> 3    NA
```

처럼 나옴 말인 즉슨, NA값은 항상 마지막으로 정렬됨

## select()

select() 변수 이름을 기반으로 검색하여 선택 할수 있는 기능

```
# Select columns by name
select(flights, year, month, day)
#> # A tibble: 336,776 x 3
#>    year month   day
#>   <int> <int> <int>
#> 1  2013     1     1
#> 2  2013     1     1
#> 3  2013     1     1
#> 4  2013     1     1
#> 5  2013     1     1
#> 6  2013     1     1
#> # … with 336,770 more rows
# Select all columns between year and day (inclusive)
```

위처럼 나옴 전체 테이블에서 원하는걸 자른후  
메모리 위에 가상 테이블을 올리는듯한 느낌으로 생성됨

```
select(flights, year:day)
select(flights, -(year:day))
```
이런것들도 가능  
year부터 day 까지 표기됨  
year부터 day 까지 빼고 표기됨  



```
select(flights, starts_with("yea"))
```
yea로 시작하는 컬럼
```
select(flights, ends_with("ay"))
```
ay 로 끝나는 컬럼
```
select(flights, contains("de"))
```
de가 포함된 컬럼
```
select(flights, matches("(.)\\1"))
```
정규 표현식 사용가능

```
select(flights, num_range("x", 1:3))
```
x1 ~ x3 

?select 를 참조해볼수 있음

```
select(flights, tail_num = tailnum)
rename(flights, tail_num = tailnum)
```
위는 둘다 컬럼명을 변경할수 있지만  
select 는 하나의 컬럼을 선택하고
rename은 컬럼명 변경후 모든 컬럼을 보여줌


everything() 이란것도 있는데 
```
select(flights, tail_num = tailnum, everything())
select(flights, time_hour, air_time, everything())
```
이런식으로 everything() 을  사용할수도 있음  
rename 과 같게 쓸수도 있고, 컬럼위치를 앞으로 당길수도 있음

## mutate()

새로운 컬럼을 생성 하는 기능


```
flights_sml <- select(flights, 
                      year:day, 
                      ends_with("delay"), 
                      distance, 
                      air_time
)

mutate(flights_sml,
       gain = dep_delay - arr_delay,
       speed = distance / air_time * 60
)

```

연습을 위해 flights_sml 로 작은 테이블을 생성했고,  
flights_sml 에서 거리 시간,지연 시간등을 적절히 사용하여   
speed와  gain 을 만듦 

```
mutate(flights_sml,
  gain = dep_delay - arr_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

방금 만든 컬럼들을 조합하여 바로 쓸수도 있음  
 gain만들고  hours만든뒤  
 gain / hours 을 이용할수있음  

```
transmute(flights,
  gain = dep_delay - arr_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```
기존 결과는  dep_delay ,arr_delay 등과 gain ,hours 를 모두  
표기 하였다면 위의 코드는 gain_per_hour, hours, gain 만 표시함

### Tip

산술연산자 등을 사용할수도 있음

```
transmute(flights,
  dep_time,
  hour = dep_time %/% 100,
  minute = dep_time %% 100
)
```
여기서 dep_time은 예를들어 516이라면  
05시 16분 인데 위의 코드를 사용하면  
hour = 5,  minute = 16 을 넣어서 컬럼을 생성함  
여기서 %/% 는 정수 나누기 %%는 나머지를 의미함  











 
