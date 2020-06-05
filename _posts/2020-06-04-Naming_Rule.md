---
title: "Naming Rule "
date: 2020-06-04 08:26:28 -0400
categories:  Naming Rule
---

## 표준 Naming Rule
### 명명 규칙 표준

시스템을 개발하는데 있어 표준 Naming Rule을 적용하여 개발자 및 
운영자가 분석 및 코딩하는데 있어 좀더 쉽게 접근할 수 있도록 표준 Naming Rule을 적용한다

#### PEP8의 기준의 몇가지 예시를 따름

#### whitespace

- 한 줄의 문자 길이가 79자 이하여야 한다.
- 함수와 클래스는 빈 줄 두개로 구분한다.
- 클래스에서 메서드는 빈 줄 하나로 구분한다.
- 변수 할당 앞 뒤에 스페이스를 하나만 사용한다.
- 리스트 인덱스, 함수 호출, 키워드 인수 할당에는 스페이스를 사용하지 않는다.
- 불필요한 공백을 피함
+ [] 와 ()
+ 쉼표 (,) 의 앞과 쌍점 (:) , 쌍반점(;) 앞

#### Comments

- 주석을 달때 신중하게 달아야함
- 코드와 모순되는 주석은 없는것만 못하므로, 꼭 코드 갱신시 같이 갱신 해야함
- 주석을 한줄로 달때는 신중히 작성(모두가 이해할수 있도록)
- 코드 다음에 주석을 달아줌

#### naming


- 모듈의 이름은 소문자로만

```
(good) import modulename
(bad) import ModuleName
(bad) import module_name
```

- 클래스는 CamelCase

```
(good) class CamelCase(object):
(bad) class mixedCase(object):
(bad) class snake_case(object):
```

- 함수의 이름은 snake_case
+ 내부적으로 쓰이면 밑줄을 앞에 붙임

```
(good) def python_function_name(arg1, arg2):
(bad) def PythonFunctionName(arg1, arg2):
(bad) def pythonFunctionName(arg1, arg2):
```

- 소문자 L 대문자 O 대문자 I 는 변수명으로 사용금지
- 모듈(Module) 명은 짧은 소문자로 구성되며 필요하다면 밑줄로 나눔
+ 모듈은 파이썬 파일(.py)에 대응하기 때문에 파일 시스템의 영향을 받으니 주의
+ C/C++ 확장 모듈은 밑줄로 시작함


https://soojin.ro/blog/naming-boolean-variables
