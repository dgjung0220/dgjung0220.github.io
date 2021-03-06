---
tags: ["파이썬","sklearn","앙상블"]
title: '앙상블 적용해보기'
date: 2020.05.19
category: '데이터분석'
---

### sklearn 앙상블 적용 

​	모형 결합(model combining) 또는 앙상블 방법론(ensemble methods) 라고 한다. 특정한 하나의 예측 모형을 사용하는 것이 아니라 복수의 예측 모형을 결합하여 더 나은 성능을 예측하려는 시도. 일반적으로 계산량은 증가하지만, 단일 모형 모델 대비 성능 분산 감소 (과최적화 방지)의 효과가 있고, 보통 단일 대비 성능이 향상된다.

​	앙상블 방법론은 크게 취합(Aggregation) 과 부스팅 방법론으로 나눌 수 있다. Aggregation의 경우, 사용할 모형의 집합이 이미 결정되어 있지만, 부스팅의 경우 사용할 모델을 점진적으로 늘려간다.[내용 참고](https://datascienceschool.net/view-notebook/766fe73c5c46424ca65329a9557d0918/)



#### Aggregation - 다수결 방법론

​	sklearn에서는 VotingClassifier 를 이용하여 이 앙상블 방법을 구현한다. 입력 인수는 다음과 같다.

- estimators : 개별 모형 목록, 리스트나 named parameter 형식

- voting : 문자열 {hard , soft},  **default : hard**

- weights : 사용자 가중치 리스트

  

##### 사용된 모형

쓸 수 있는 것은 다 섞어본다.

- Linear Discriminative Analysis

- Logistic regression

- RandomForest Classifier

- GradientBoosting Classifier

- ExtraTrees Classifier

- AdaBoost Classifier

- Decision Tree Classifier

- SVM (Support Vector Machine)

- neighbors (K-NN; K-Nearest Neighbors)

- MLP Classifier (Multi-Layer Perceptron)

- XGB Classifier (XGBoost)

  

##### 라이브러리 임포트

```python
from sklearn.linear_model import LogisticRegression
from subprocess import check_output
from sklearn.ensemble import RandomForestClassifier, VotingClassifier, GradientBoostingClassifier, ExtraTreesClassifier, AdaBoostClassifier
from sklearn.tree import DecisionTreeClassifier
from sklearn import svm, neighbors
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
from sklearn.neural_network import MLPClassifier
from xgboost import XGBClassifier
```

##### 모델 선언 및 적용

```python
model1 = svm.LinearSVC
model2 = neighbors.KNeighborsClassifier()
model3 = RandomForestClassifier()
model4 = LogisticRegression()
model5 = LinearDiscriminantAnalysis()
model6 = DecisionTreeClassifier()
model7 = MLPClassifier()
model8 = ExtraTreesClassifier()
model9 = AdaBoostClassifier()
model10 = GradientBoostingClassifier()
model11 = XGBClassifier(Eta=0.2)

clf = VotingClassifier(estimators=[ ('svm',model1),
                        ('knn', model2),
                        ('rfor', model3),
                        ('lo-r', model4),
                        ('li-dr', model5),
                        ('dtc', model6),
                        ('mlpc', model7),
                        ('etc', model8),
                        ('abc', model9),
                        ('gbc', model10),
                        ('XGBC', model11)])

clf.fit(X_tr, y_tr)
```

##### 모델 예측 및 평가

```python
confidence = clf.score(X_vld, y_vld)
print('accuracy : ' , confidence)
predictions = clf.predict(X_test)
# output : accuracy :  0.7988826815642458

# Kaggle 제출을 위해 submission에 prediction 내용 추가
sumission['Survived'] = predictions
submission.to_csv('my_ensemble_submission.csv', index = False)
```



#### Aggregation - 배깅(Bagging)

​	sklearn에서는 BaggingClassifier 를 이용하여 이 앙상블 방법을 구현한다. 입력 인수는 다음과 같다.

- best_estimator : 기본 모형
- n_estimatros : 모형 갯수, **default 10**
- bootstrap : 데이터 중복 사용 여부, **default True**
- max_samples : 데이터 샘플 중 선택할 샘플의 수 혹은 비율, **default 1.0**
- bootstrap_features : 특징 차원의 중복 사용 여부, **default False**
- max_features : 다차원 독립 변수 중 선택할 차원의 수 혹은 비율, **default 1.0**

```python
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import BaggingClassifier

model1 = DecisionTreeClassifier(random_state=1).fit(X_tr, y_tr)
ensemble_1 = BaggingClassifier(base_estimator=DecisionTreeClassifier(),).fit(X_tr, y_tr)

print('단일 모델 : ', model1.score(X_vld, y_vld))
print('ensemble_1 : ', ensemble_1.score(X_vld, y_vld))
```

```shell
단일 모델 :  0.7541899441340782
ensemble_1 :  0.7821229050279329
```

