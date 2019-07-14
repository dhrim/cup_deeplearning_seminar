# 교육 목표

딥러닝 코딩을 위한 python의 문법과 사용을 익힌다.



# 명령어 python, pip

## python 실행 방법
python 파일을 실행 시키는 방법

```
$ cat my_code.py
print("hello")

$ python my_code.py
hello
```

python 쉘에서 코드를 직접 실행 시키는 방법

```
$ python
Python 3.5.4 |Anaconda custom (64-bit)| (default, Nov  8 2017, 18:11:28)
[GCC 4.2.1 Compatible Clang 4.0.1 (tags/RELEASE_401/final)] on darwin
Type "help", "copyright", "credits" or "license" for more information.

>>> print("hello")
hello
```


## pip로 패키지 설치
pip는 python 설치 시 같이 설치된다.

모듈 설치 방법

```
$ pip install SomePackage            # latest version
$ pip install SomePackage==1.0.4     # specific version
$ pip install 'SomePackage>=1.0.4'     # minimum version
```

# 소개
스트링, 숫자, 변수, print()

string as list

list


- 자료 : https://notebooks.azure.com/Microsoft/projects/samples/html/Introduction%20to%20Python.ipynb
- 퀴즈 : https://www.w3resource.com/python-exercises/python-basic-exercises.php
    - 1, 6, 8, 138, 147, 148


# 데이터 타입
numbers, boolean, string index

tuple, list, sets, dictionay

- 자료 : https://www.w3resource.com/python/python-data-type.php



# 연산자

- 자료 : https://www.w3resource.com/python/python-operators.php



# 조건문
if, elif, else

- 자료 : https://www.w3resource.com/python/python-if-else-statements.php
- 퀴즈 : https://www.w3resource.com/python-exercises/python-conditional-statements-and-loop-exercises.php





# for 루프
for

iteration for data types

- 자료 : https://www.w3resource.com/python/python-for-loop.php
- 퀴즈 : https://www.w3resource.com/python-exercises/python-conditional-statements-and-loop-exercises.php



# List 상세
- 자료 : https://www.w3resource.com/python/python-list.php
- 퀴즈 : list : https://www.w3resource.com/python-exercises/list/



# Dictionary 상세
- 자료 : https://www.w3resource.com/python/python-dictionary.php
- 퀴즈 : dictonary : https://www.w3resource.com/python-exercises/dictionary/



# 내장 함수

int(), float(), str()

type()

len(), type()

list(), dict(), set(), tuple()

range()

- 자료 : https://www.w3resource.com/python/built-in-function/



# comprehensive List

- 자료 : https://github.com/jmportilla/Complete-Python-Bootcamp/blob/master/List%20Comprehensions.ipynb



# file io
- 자료 : https://wikidocs.net/26
- 퀴즈 : https://www.w3resource.com/python-exercises/file/index.php


# 함수
- 자료 : https://www.w3resource.com/python/python-user-defined-functions.php
- 퀴즈 : https://www.w3resource.com/python-exercises/python-functions-exercises.php



# 모듈
- 자료 : https://www.w3resource.com/python/python-module.php



# 특수 변수 __name__
파일로 시작된 경우 주 모듈에는 __name__이라는 변수가 생기고, 값은 ‘__main__’으로 설정된다.

모듈 스스로가 직접 실행되었는 지 혹은 import되었는 지 알 수 있다.



그래서 python 스크립트는 흔히 다음의 모습을 보인다.
```
def main():
  ...

if __name__=="__main__"
  main()
```



# Class
자료 : <<TODO>>


