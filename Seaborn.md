# 1. Basic

## 1.1 Definition

Seaborn은 기본적으로 matplotlib를 베이스(밑단)으로 삼고 최상위 단에서 연산을 하는 모양이며, pandas 데이터 구조를 사용하여 입출력을 받아 연산을 한다.

matplotlib을 기반으로 했다는 건 Seaborn이 matplotlib에서 '추가적으로 사용가능한 확장판' 정도로 이해하면 된다. (matplotlib을 기반으로 다양한 색 테마, 차트 기능을 추가한 라이브러리)

<img width="301" alt="1" src="https://user-images.githubusercontent.com/68315507/104022472-ad408b80-5203-11eb-968b-09ae137d77de.png">
<img width="741" alt="2" src="https://user-images.githubusercontent.com/68315507/104022476-af0a4f00-5203-11eb-8561-f3133a1ea978.png">

## 1.2 Package Loading

```python
# seaborn import
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# for jupyter notebook
%matplotlib inline
```

# 2. Guide

## 2.1 Lineplot & option

```python
# 1. 기본
# hue = 'col'    # 범주형 데이터를 yes or no 인식하여 그래프에 표기 (scatter에서 유용할 듯)
# estimator = sum    # seaborn은 기본적으로 mean을 채택, sum으로 변경해주기
# ci = None    # confiden interval 잡지 않아서 cpu 덜 잡아먹음

sns.barplot(data=jeju_all, x="ym", y="totalspent", ax=ax1,
            ci = None, estimator = sum)

sns.lineplot()
```

## 2.2 relplot

```python
tips = sns.load_dataset("tips")
sns.relplot(x="total_bill", y="tip", hue="smoker", style="smoker",
            data=tips)
```

<img width="713" alt="3" src="https://user-images.githubusercontent.com/68315507/104022481-b03b7c00-5203-11eb-8818-5078c3c6103d.png">

## 2.3 Catplot

```python
sns.catplot(x="day", y="total_bill", hue="smoker",
            col="time", aspect=.6,
            kind="swarm", data=tips)
```

<img width="715" alt="4" src="https://user-images.githubusercontent.com/68315507/104022486-b0d41280-5203-11eb-83e5-00b351f68235.png">

## 2.4 Pairplot

```python
iris = sns.load_dataset("iris")
sns.pairplot(iris)
```

<img width="714" alt="5" src="https://user-images.githubusercontent.com/68315507/104022493-b16ca900-5203-11eb-9f82-fc8fcaeade17.png">

## 2.5 Heatmap

```python
flights = sns.load_dataset("flights")
flights = flights.pivot("month", "year", "passengers")
plt.figure(figsize=(10, 10))
ax = sns.heatmap(flights, annot=True, fmt="d")
```

<img width="717" alt="6" src="https://user-images.githubusercontent.com/68315507/104022503-b3366c80-5203-11eb-97b7-115831d5e2d8.png">

# 3. Application

## 3.1 Scatter + reg

scatterplot 위에 regplot 그려서 시각화 효과 주기

<img width="414" alt="7" src="https://user-images.githubusercontent.com/68315507/104022504-b3cf0300-5203-11eb-9b1b-3691bc714f97.png">
