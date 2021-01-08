# visualization_study
python visualization study log

## 1. Basic - Figure, Subplot

```python
# 1. 개념
# - figure : 그래프가 그려질 전체 창 (도화지 개념)
# - subplot : 실제 그림이 그려질 공간 (분할 영역)
# - figure와 subplot에 이름 부여 가능
# - 기본적으로 하나의 figure와 하나의 subplot이 생성

# [ 참고 : 각 창에서 시각화모드(pylab) 전환 방법 ]
# 1) cmd
# 1-1) anaconda prompt(ipython)
# ipython           # cmd에서 실행
# $matplotlib qt    # anaconda prompt

# 1-2) pylab 아나콘드 모드 직접 전환 
# ipython -- pylab           # cmd에서 실행

# 2) spyder tool
# Preferences > Ipython Console > Graphics > Graphics Backend > Automatic > 재실행

# 2. 생성
# 2.1 figure와 subplot 생성
import matplotlib.pyplot as plt

fig1 = plt.figure()    # figure 생성
ax1 = fig1.add_subplot(2,    # figure 분할 행의 수
                       2,    # figure 분할 컬럼의 수
                       1)    # 분할된 subplot의 위치 (1부터 시작)

ax2 = fig1.add_subplot(2,    # figure 분할 행의 수
                       2,    # figure 분할 컬럼의 수
                       1) 

s1 = Series([1, 10, 2, 25, 4, 3])
ax2.plot(s1)    # ax2 subplot에 직접 plot 도표 전달 => 특정 그래프에 데이터 전달 가능 (순서 상관없이 위치전달 가능)

# 2.2 figure와 subplot 동시 생성
# - plt.subplots로 하나의 figure와 여러 개의 subplot 동시 생성 
# - 이름 부여 시 figure 이름과 subplot 대표 이름 각각 지정
# - subplot 위치 지정은 색인
plt.subplots(nrows = 1,          # figure 분할 행의 수
             ncols = 1,          # figure 분할 컬럼의 수
             sharex = False,     # 분할된 subplot의 x축 공유 여부
             sharey = False)     # 분할된 subplot의 y축 공유 여부

fig2, ax = plt.subplots(2, 2)
ax[1, 1].plot(s1)     

# 2.3 응용
figure, (ax1,ax2, ax3, ax4) = plt.subplots(nrows=4, ncols=1)
figure.set_size_inches(30, 40)

df1.plot(ax = ax1)
df2.plot(ax = ax2)
```

## 2. 그래프 종류 및 사용법

### 2.1 선그래프 및 옵션

```python
     
# 1. 선 그래프 그리기
# 1) Series 전달
ax[0, 1].plot(s1)        # 특정 figure, subplot에 그리는 방법
s1.plot()                # 가장 마지막 figure 혹은 subplot에 전달, 새로 생성

# 2) DataFrame 전달
# - 컬럼별 서로 다른 선 그래프 출력
# - 컬럼이름값이 자동으로 범례 생성 (위치 가장 좋은 자리)
# - 인덱스이름값이 자동으로 x축 생성

# [ 예제 - 선그래프 그리기 ]
# - fruits.csv 파일을 읽고 과일별 판매량 증감 추이 시각화
fruits = pd.read_csv('fruits.csv')
fruits2 = fruits.pivot('year', 'name', 'qty')

fruits2.plot()

# 2. 그래프 옵션 전달
# 1) plot 메서드 내부 옵션 전달 방식 (상세 옵션 전달 불가)
# legend의 위치, 글씨 옵션
fruits2.plot(xticks,        # x축 눈금
             ylim,          # y축 범위
             fontsize,      # 글자 크기
             rot,           # (x축 이름) 글자 회전방향
             color,         # 선 색
             linestyle,     # 선 스타일
             marker,        # 선 모양
             style,         # (종합) 선 스타일
             title,         # 그래프 이름
             kind)          # 그래프 종류 (default = 선 그래프)
             
fruits2.plot(xticks = fruits2.index,
             style = '--')             
             
# [ 참고 : 선 스타일 종류 및 전달 ]
# 'r--' : 붉은 대시선
# 'k-'  : 검은색 실선
# 'b.'  : 파란색 점선
# 'ko--': 검은색 점모양 대시선

# marker size
# ex) markersize = '10.0'

s1.plot(style = 'b.')
s1.plot(color = 'b', linestyle = '--', marker = 'o')

s1.index = ['월', '화', '수', '목', '금', '토']
s1.plot()    # x축 이름이 깨짐 (한글)

plt.rc('font', family = 'AppleGothic')    # 글씨체 변경방법

# 2) 광 범위 옵션 전달 방식 : plt.옵션함수명
# 종류 확인 : dir(plt)
# 2-1) x축, y축 이름
# labelpad : 여백 지정
# fontdict
font1 = {'family': 'serif',
         'color': 'b',
         'weight': 'bold',
         'size': 14
         }

font2 = {'family': 'fantasy',
         'color': 'deeppink',
         'weight': 'normal',
         'size': 'xx-large'
         }

plt.xlabel('발생년도', labelpad = 15, fontdict = font1)
plt.ylabel('검거율', labelpad = 20, fontdict = font2)

# 2-2) 그래프 제목
plt.title('구별 년도별 검거율 변화')

# 2-3) x축, y축 범위
plt.ylim([0, 130])

# 2-4) x축, y축 눈금
plt.xticks(cctv2.index)

# 2-5) legend ***
# loc : 위치 지정 (best, upper right, lower left, right, center left)
# ncol : 범례의 열 개수 지정
plt.legend(fontsize = 6,
					 ncol = 2,
           loc = 'upper right',    
           title = '구 이름')

# [ 참고 : 선그래프 옵션 확인 방법 ]
plt.plot(s1)    # 선 스타일, 마커 스타일 확인 가능

# [ 참고 : plt.rc로 global 옵션 전달 방식 ]
plt.rcParams.keys()    # 각 옵션그룹별 세부 옵션 정리

plt.rc(group,        # 파라미터 그룹
       **kwargs)     # 상세 옵션
       
plt.rc('font', family = 'AppleGothic')       
```

- fontdict

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae4cc494-46ff-4c7c-869a-8a2d1f55810f/Screen_Shot_2020-12-31_at_2.32.36_AM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ae4cc494-46ff-4c7c-869a-8a2d1f55810f/Screen_Shot_2020-12-31_at_2.32.36_AM.png)

- 색상

```python
# 1) 포맷 문자열 사용
plt.plot([1, 2, 3, 4], [2.0, 2.5, 3.3, 4.5], 'r')
plt.plot([1, 2, 3, 4], [2.0, 2.8, 4.3, 6.5], 'g')
plt.plot([1, 2, 3, 4], [2.0, 3.0, 5.0, 10.0], 'b')

# 2) color 키워드 이낮 사용
plt.plot([1, 2, 3, 4], [2.0, 2.5, 3.3, 4.5], color='springgreen')
plt.plot([1, 2, 3, 4], [2.0, 2.8, 4.3, 6.5], color='violet')
plt.plot([1, 2, 3, 4], [2.0, 3.0, 5.0, 10.0], color='dodgerblue')

# 3) Matplotlib 색상 이름
color = 'C0'
color = 'C1'
...

```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25070cfa-d748-463d-ba9e-235e00e2cce4/Screen_Shot_2020-12-31_at_2.00.17_AM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/25070cfa-d748-463d-ba9e-235e00e2cce4/Screen_Shot_2020-12-31_at_2.00.17_AM.png)

CSS 색상

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a9d3ebb-b5a2-41df-ad72-3ef5b5072b6e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/6a9d3ebb-b5a2-41df-ad72-3ef5b5072b6e/Untitled.png)

### 2.2 바그래프

```python
# 1. barplot 그리기
# - 행별로 서로 다른 그룹
# - 각 컬럼의 데이터들이 서로 다른 막대로 출력 기본 (R에서는 beside = T)
# - 각 컬럼의 데이터들이 하나의 막대로 출력(stacked = True)
# - 컬럼 이름이 자동으로 범례 이름으로 전달
# - 인덱스 이름이 자동으로 x축 이름으로 전달

fruits2.plot(kind = 'bar')
fruits2.plot.bar()

plt.xticks(rotation = 0)    # x축 눈금 회전

# 2. 심화 - 바그래프에 %를 적기 위한 코드
# patches : 각 그래프를 의미하는 값
ax2.patches.get_x()    # 해당 그래프의 위치값 x
ax2.patches.get_y()    # 해당 그래프의 위치값 y
ax2.get_height()    # 해당 그래프의 높이 => 여기에 100 곱하면 %가 됨.

for p in ax2.patches:
    percentage = '{:.1f}%'.format(100 * p.get_height())
    x = p.get_x()
    y = p.get_y() + p.get_height()
    ax2.annotate(percentage, (x, y))
ax2 = jeju_p.plot.bar(x='ym', y='0', rot=0, color='#fbb4ae')
for p in ax2.patches:
    percentage = '{:.1f}%'.format(100 * p.get_height())
    x = p.get_x()
    y = p.get_y() + p.get_height()
    ax2.annotate(percentage, (x, y))
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/454975f4-bb9a-4d0e-87a8-81d36b711a1f/Screen_Shot_2021-01-02_at_7.25.46_AM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/454975f4-bb9a-4d0e-87a8-81d36b711a1f/Screen_Shot_2021-01-02_at_7.25.46_AM.png)

### 2.3 히스토그램

```python
# 1. 히스토그램
cctv['CCTV수'].hist(bins=10)                     # 막대의 개수
cctv['CCTV수'].hist(bins=[0,100,200,500,1000])   # 막대의 범위

cctv['CCTV수'].plot(kind='kde')                  # 커널 밀도 함수(누적분포)
```

### 2.4 파이차트

```python
# 1. 기본
ratio = [34, 32, 16, 18]
labels = ['Apple', 'Banana', 'Melon', 'Grapes']

plt.pie(ratio, labels=labels, autopct='%.1f%%')
plt.show()

# 2. 시작각도와 방향설정 by startangle, counterclock
plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False)
plt.show()

# 3. 중심에서 벗어나는 정도 실현 by explode
explode = [0, 0.10, 0, 0.10]

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, explode=explode)
plt.show()

# 4. 색상 지정 by color
colors = ['silver', 'gold', 'whitesmoke', 'lightgray']

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, explode=explode, shadow=True, colors=colors)
plt.show()

# 5. 부채꼴 스타일 지정 by wedgeprops
wedgeprops={'width': 0.7, 'edgecolor': 'w', 'linewidth': 5}

plt.pie(ratio, labels=labels, autopct='%.1f%%', startangle=260, counterclock=False, colors=colors, wedgeprops=wedgeprops)
plt.show()
```

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9af188bf-958f-43f0-b0b7-e681f452fc06/Screen_Shot_2020-12-31_at_2.13.51_AM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9af188bf-958f-43f0-b0b7-e681f452fc06/Screen_Shot_2020-12-31_at_2.13.51_AM.png)

## 3. 함수

### 3.1 변수중요도 가로막대그래프

```python
# *) 시각화
plt.plot([0.001, 0.01, 0.1, 0.5, 1], vscore_tr, label = 'train_score')
plt.plot([0.001, 0.01, 0.1, 0.5, 1], vscore_te, label = 'test_score', color = 'red')
plt.legend()    # 낮은 learning rate에서도 충분한 예측력 나옴

# 7-2) feature importance check => 특성 중요도 튜닝 (설명변수 중요도)
def plot_feature_importances(model, data) : 
    n_features = data.data.shape[1]    # 컬럼 사이즈
    plt.barh(range(n_features), model.feature_importances_, align='center')
    plt.yticks(np.arange(n_features), data.feature_names)    # y 눈금
    plt.xlabel("특성 중요도")    # x축 이름
    plt.ylabel("특성")         # y축 이름
    plt.ylim(-1, n_features)

plot_feature_importances(m_gb, df_iris)
```
