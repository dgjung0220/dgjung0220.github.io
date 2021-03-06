---
tags: ["데이터분석","pandas"]
title: '영화관 데이터 살펴보기_1'
date: 2020.05.08
category: '데이터분석'
---



#### 영화 상영관 데이터 살펴보기



##### 사용된 데이터

영진위에서 제공하는 산업통계 데이터의 2015년 전국 영화관 현황 자료를 이용하였다.  [영진위 2015년 전국 영화관 현황](https://www.kofic.or.kr/kofic/business/board/selectBoardDetail.do?boardNumber=2)

```python
file_name='2015_theaters_in_korea.xlsx'
df = pd.read_excel(file_name,
                   sheet_name='서울',
                   usecols='B,C,D,E,F,G,H,P,Q'
                   encoding='utf-8')
```



##### 1. 서울시 총 영화관, 스크린, 좌석 수

서울시의 총 영화관 수는 81개이며, 스크린 수 및 좌석 수는 각각 511개,  89,610개이다. 

```python
np.sum(df.head(81).loc[:,['총 스크린 수']])			# 511
np.sum(df.head(81).loc[:,['총 좌석수']])			# 89,610 
```



##### 2. 가장 스크린이 많은 영화관

가장 스크린이 많은 영화관은 송파구의 **롯데시네마 월드타워점**으로 총 21개이다. 

```python
df.sort_values(by='총 스크린 수', ascending=False).dropna(axis=0).head()
```

![image-20200508155129834](/upload/image-20200508155129834.png)



##### 3. 가장 좌석 수가 많은 영화관

가장 좌석 수가 많은 영화관 역시 **롯데시네마 월드타워점**이다. 대한극장은 스크린수는 11개로 다른영화관보다 비교적 적지만, 총 좌석 수는 2,754개로 4번째다.

```python
df.sort_values(by='총 좌석수', ascending=False).dropna(axis=0).head()
```

![image-20200508155631935](/upload/image-20200508155631935.png)



##### 4. 스크린 & 좌석 수 기준 Top5

```python
df.sort_values(by=['총 스크린 수','총 좌석수'], ascending=False).dropna(axis=0).head()
```

![image-20200508155656492](/upload/image-20200508155656492.png)



##### 5. 가장 오래된 영화관

개관일을 기준으로 오름차순 정렬하였다. 개관일이 정확하지 않은 데이터를 걸러내기 위해 `errors='coerce'` 를 사용하였다. 개관일을 알 수 있는 영화관 중에서 **가장 오래된 영화관은 CGV강변**이다.  서울시 Top 10 내 대부분은 CGV가 차지하고 있다.

```python
df['개관일'] = pd.to_datetime(df['개관일'], errors='coerce')
df.sort_values(by='개관일').head(10)
```

![image-20200508155712740](/upload/image-20200508155712740.png)



##### 6. 롯데는 서울에서 언제부터?

롯데는 CGV보다 늦은 7년 뒤인 2005년에 명동점을 개관했다.

```python
temp_series = pd.Series(df['영화관명'])
df[temp_series.str.contains('롯데')].sort_values(by='개관일').head(5)
```

![image-20200508155749266](/upload/image-20200508155749266.png)



##### 7. 브랜드별 비교

서울시에는 CGV가 27개, 롯데가 23개, 그리고 메가박스가 11개가 있다.

```python
pd.Series(df['홈페이지']).value_counts().head()
```

- 롯데

  영화관 23개, 스크린 수 164개, 좌석 수 29,433개

  ```python
  temp_series = pd.Series(df['영화관명'])
  df_lotte = df[temp_series.str.contains('롯데')]
  len(df_lotte)							# 23
  np.sum(df_lotte['총 스크린 수'])			# 164
  np.sum(df_lotte['총 좌석수'])			 # 29,433
  ```

- CGV

  영화관 27개, 스크린 수 216개, 좌석 수 36,661개

  ```python
  df_cgv = df[temp_series.str.contains('CGV')]
  len(df_cgv)								# 27
  np.sum(df_cgv['총 스크린 수'])			# 216
  np.sum(df_cgv['총 좌석수'])				# 36,661 
  ```

- 메가박스

  영화관 11개, 스크린 수 88개, 좌석 수 14,891개

  ```python
  df_mb = df[temp_series.str.contains('메가박스')]
  len(df_mb)
  np.sum(df_mb['총 스크린 수'])
  np.sum(df_mb['총 좌석수'])
  ```

서울시에는 기타 소규모 영화관을 제외하고 CGV, 롯데, 메가박스가 5 : 3 : 2 의 비율을 차지하고 있다.

 ```python
lotte_num = np.sum(df_lotte['총 좌석수'])
cgv_num = np.sum(df_cgv['총 좌석수'])
mb_num = np.sum(df_mb['총 좌석수'])

total = lotte_num + cgv_num + mb_num

print(cgv_num / total * 100)			# 45.26887695252208
print(lotte_num / total * 100)			# 36.34376736432673
print(mb_num / total * 100)				# 18.387355683151203
 ```



##### 8. 지역구별 비교 

지역구별 영화관 수는 종로구가 총 9개으로 가장 많고, 중구, 강남구가 각각 8개씩 있다.

```python
df['기초단체'] = df['기초단체'].str.strip()
grouped = df.groupby(df['기초단체'])
print(grouped.size().sort_values(ascending=False))
grouped.size().sort_values().plot(kind='bar')
```

![image-20200508160511782](/upload/image-20200508160511782.png)



하지만 스크린 수는 강남구가 48개로 가장 많다. 송파구는 영화관이 2개지만, 월드타워점이 있기 때문에 스크린 수가 28개, 그리고 좌석 수는 4번째로 많은 5,835석으로 강남구,중구의 7,000석 규모를 제외하고 상당히 많은 수준이다.

```python
grouped.sum().sort_values(by=['총 좌석수'], ascending=False).head()
```

![image-20200508160632806](/upload/image-20200508160632806.png)



롯데는 송파구, 강서구, 동대문구순으로 좌석 및 스크린 수가 많았고, 마포구에 3개로 가장 많은 영화관이 위치해있다. 

```python
grouped_lotte = df_lotte.groupby(df['기초단체'])
grouped_lotte_by_size = grouped_lotte.size().to_frame()

# 컬럼명 변경
grouped_lotte_by_size.columns[0]
grouped_lotte_by_size.rename(columns={grouped_lotte_by_size.columns[0]:'개수'}, inplace=True)

# Merge dataframe
lotte_count = pd.merge(grouped_lotte_by_size, grouped_lotte.sum(), left_index=True, right_index=True)
lotte_count.sort_values(by='총 좌석수', ascending=False)
```

![image-20200508160738783](/upload/image-20200508160738783.png)



반면 CGV는 영등포구, 구로구, 강동구순으로 롯데가 상대적으로 적은 지역에 많은 수의 좌석 수를 가지고 있었다.  CGV 또한 마포구에 3개의 영화관이 위치해있고, 좌석 수 또한 롯데와 비슷했다.

```python
grouped_cgv = df_cgv.groupby(df['기초단체'])
grouped_cgv_by_size = grouped_cgv.size().to_frame()

# 컬럼명 변경
grouped_cgv_by_size.columns[0]
grouped_cgv_by_size.rename(columns={grouped_cgv_by_size.columns[0]:'개수'}, inplace=True)

# Merge dataframe
cgv_count = pd.merge(grouped_cgv_by_size, grouped_cgv.sum(), left_index=True, right_index=True)
cgv_count.sort_values(by='총 좌석수', ascending=False)
```

![image-20200508160855888](/upload/image-20200508160855888.png)



메가박스는 스크린 수가 전체적으로 적지만, 강남구에 가장 많은 3,593석을 가지고 있고 서초구 또한 상대적으로 많은 1608석을 가지고 있다. 

```python
grouped_mb = df_mb.groupby(df['기초단체'])
grouped_mb_by_size = grouped_mb.size().to_frame()

# 컬럼명 변경
grouped_mb_by_size.columns[0]
grouped_mb_by_size.rename(columns={grouped_mb_by_size.columns[0]:'개수'}, inplace=True)

# Merge dataframe
mb_count = pd.merge(grouped_mb_by_size, grouped_mb.sum(), left_index=True, right_index=True)
mb_count.sort_values(by='총 좌석수', ascending=False)
```

![image-20200508161023859](/upload/image-20200508161023859.png)

##### 9. 연도별 비교

영화관의 수는 2007년 9개, 2013년 8개로 가장 많이 늘었다. 2005년 롯데가 서울 내에 영화관을 개관한 이후, 매년 3 관 이상씩 영화관이 생겨났다. (영화관 개관일이 표기되지 않은 7개의 영화관 제외)

```python
df['개관일'] = df['개관일'].dt.year
df_sort_by_year = df.sort_values(by='개관일').dropna(axis=0)
grouped_year = df_sort_by_year.groupby(df['개관일'])
grouped_year.size().plot()
```

![image-20200508212640628](/upload/image-20200508212640628.png)







----

###### Tip

- 칼럼 내 문자열 공백 제거

  기초단체별로 그룹화할 때 칼럼 안에 공백이 있어 같은 그룹도 다르게 그룹화되었다. ('강남구', '강남구  ')

  strip() 함수를 이용하여 공백을 제거할 수 있다.

  ```python
  df['기초단체'] = df['기초단체'].str.strip()
  ```

- matplotlib 한글 깨짐 문제

  matplotlibrc 의 font-family를 수정하여 영구적으로 변경할 수도 있고, 아래와 같이 사용할 때 지정해줄 수 있다.

  ```python
  import matplotlib.pyplot as plt
  plt.rc('font', family='Malgun Gothic')
  ```

  
