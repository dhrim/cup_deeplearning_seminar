# 실무과제 - Kaggle 문제 풀이

Kaggle의 dogs-vs-cats 문제 풀이

http://kaggle.com/c/dogs-vs-cats

개와 고양이의 영상을 분류하는 문제

일반 CNN으로 분류를 하고 VGG16를 가져와 전이학습 한다.

실습 자료 : [실무과제_kaggle_문제풀기.ipynb](deep_learning/실무과제_kaggle_문제풀기.ipynb)

<br>


# 준비

- 게정 생성
- 데이터 사용 동의
- 인증 토근 생성

## 계정 준비

Kaggle에 계정을 생성한다.

https://www.kaggle.com

<br>


## 데이터 사용 동의

다음 페이지에서 dogs-vs-cats 데이터 사용을 위한 동의를 한다.

https://www.kaggle.com/c/dogs-vs-cats/data

각 문제 마다 동의해야 한다.

<br>


## 인증 토큰 생성

우측 상단의 프로파일 아이콘을 클릭하고 'My Account'를 클릭.

API 섹션에서 'Create New API Token'클릭하고

kaggle.json이란 파일이 다운로드 된다.

<br>


# 데이터 준비

## 파일 다운로드
3개의 파일을 다운로드 받는다.

- train.zip : 학습에 사용될 데이터
- test1.zip : 분류를 실행할 대상
- sampleSubmission.csv : 제출할 파일의 샘플

colab에서 kaggle의 데이터 다운로드는 다음 순서로 진행한다.

방법은 https://www.kaggle.com/general/74235 문서를 참조함



Kaggle에 계정이 있어야 하고, dogs-vs-cats의 Data 섹션에서 data사용에 대한 동의를 미리 해야 함.


<br>


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

<br>


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

<br>


## VGG16 사용

[VGG16_classification_and_cumtom_data_training.ipynb](deep_learning/VGG16_classification_and_cumtom_data_training.ipynb)의
'디렉토리 구조의 데이터의 학습 최소 코드' 섹션의 코드를 사용

<br>

# 분류 실행

train이나 validation에 사용한 ImageDataGenerator를 사용하려면
test 폴더 밑에 폴더 하나가 더 있어야 한다.

```
test\
  unknown\
    10000.jpg
    10001.jpg
    10002.jpg
```

그리고 ImageDataGenerator를 사용하여 예측을 실행할 수 있다.
```
test_data_generator = data_no_aug_generator.flow_from_directory(
      "test",
      target_size=(224,224),
      class_mode='sparse',
      shuffle=False,
      batch_size=1
)
sample_count = len(test_data_generator.filenames)
predict = model.predict_generator(test_data_generator, steps=sample_count)
```

예측된 결과는 pandas DataFrame에 담는다.
```
import pandas as pd

test_df = pd.DataFrame({
    'filename': test_data_generator.filenames
})
test_df['category'] = np.argmax(predict, axis=-1)
```

test_df는 다음과 같다.
```
	filename	        category
0	unknown/1.jpg	    1
1	unknown/10.jpg	    0
2	unknown/100.jpg	    0
3	unknown/1000.jpg	1
4	unknown/10000.jpg	1
```

# 제출 파일 생성

submissionSample.csv와 같은 포멧의 파일을 생성한다.
```
id,label
1,0
2,0
3,0
4,0
5,0
6,0
7,0
8,0
9,0
```

다음과 같은 코드로 파일을 생성한다.
```
submission_df = test_df.copy()
submission_df['id'] = submission_df['filename'].str.split('/').str[1].str.split('.').str[0].astype(int)
submission_df['label'] = submission_df['category']
submission_df.drop(['filename', 'category'], axis=1, inplace=True)
submission_df = submission_df.sort_values(['id'])
submission_df.to_csv('submission.csv', index=False)

submission_df.head()
```


# 제출

2019/08/05 현재 dogs-vs-cats의 제출은 종료되었다.
