# Kaggle 문제 풀이

Kaggle의 dogs-vs-cats 문제 풀이

http://kaggle.com/c/dogs-vs-cats

개와 고양이의 영상을 분류하는 문제

일반 CNN으로 분류를 하고 VGG16를 가져와 전이학습 한다.


# 준비

- 게정 생성
- 데이터 사용 준비
- 인증 토근 생성

## 계정 준비

Kaggle에 계정을 생성한다.



## 데이터 사용 동의

다음 페이지에서 dogs-vs-cats 데이터 사용을 위한 동의를 한다.

https://www.kaggle.com/c/dogs-vs-cats/data

각 문제 마다 동의해야 한다.


## 인증 토큰 생성

우측 상단의 프로파일 아이콘을 클릭하고 'My Account'를 클릭.

API 섹션에서 'Create New API Token'클릭하고

kaggle.json이란 파일이 다운로드 된다.


# 데이터 준비

## 파일 다운로드
3개의 파일을 다운로드 받는다.

- train.zip : 학습에 사용될 데이터
- test1.zip : 분류를 실행할 대상
- sampleSubmission.csv : 제출할 파일의 샘플

## 학습 데이터 준비
train.zip을 압축 풀면 다음 형태로 존재한다.
```
train/
  dog.0001.jpg
  dog.0002.jpg
  ...
  cat.0001.jpg
  cat.0002.jpg
```

이를 다음 형태의 폴더 구조로 만든다.
```
dogs_cata/
    train/
        dog/
            dog.0001.jpg
            dog.0002.jpg
            ...
        cat/
            cat.0001.jpg
            cat.0002.jpg
            ...
    valid/
        dog/
            dog.8001.jpg
            dog.8002.jpg
            ...
        cat/
            cat.8001.jpg
            cat.8002.jpg
            ...

```
# 학습 실행

## 일반 CNN

일반 CNN 사용
```
model.add(Conv2D(32, (3, 3), activation='relu', input_shape=(224, 224, 3)))
model.add(MaxPooling2D((2, 2)))
model.add(Conv2D(64, (3, 3), activation='relu'))
model.add(MaxPooling2D((2, 2)))
model.add(Conv2D(64, (3, 3), activation='relu'))

model.add(layers.Flatten())
model.add(layers.Dense(1024, activation='relu'))
model.add(layers.Dropout(0.5))
model.add(layers.Dense(10, activation='softmax'))
```

데이터 준비와 기타 코드는 아래의 VGG16의 것을 사용

## VGG16 사용

[VGG16_classification_and_cumtom_data_training.ipynb](deep_learning/VGG16_classification_and_cumtom_data_training.ipynb)의
'디렉토리 구조의 데이터의 학습 최소 코드' 섹션의 코드를 사용


# 분류 실행


# 제출 파일 생성한다


# 제출

