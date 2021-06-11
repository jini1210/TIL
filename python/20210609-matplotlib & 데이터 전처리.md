# matplotlib & 데이터 전처리

```python
import numpy as np  #넘파이호출
import pandas as pd   #판다스호출
import matplotlib as mp   #matplotlib 호출
import matplotlib.pyplot as plt   #matplotlib.pyplot 호출
import seaborn as sns  #seaborn(시각화 라이브러리) 호출
```

```python
fm = mp.font_manager.FontManager()
#한글 지원하는 폰트명으로 재할당
plt.rcParams['font.family'] = 'Malgun Gothic'
#plt.rc('font', family='Malgun Gothic')
```



### 자료 : KOSIS 국가통계포털

https://kosis.kr/index/index.do



```python
df_kosis=pd.read_csv('pdsample/population_kosis_1997_2019.csv',encoding='cp949')   #우리나라 파일을 불러오려면 'cp949'를 사용해서 불러온다.
```

![image-20210609211738710](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609211738710.png)

```python
df_kosis.shape  #데이터 전체의 행열 갯수를 출력해줌
(19, 829)
```

```python
#모든 컬럼들이 보이도록 설정한다.
pd.options.display.max_columns=829

df_kosis.head(3)   #3개의 가장 윗줄을 출력한다.
```

![image-20210609212200283](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609212200283.png)


```python
df=df_kosis.melt(id_vars='시군구별')
식별자 열은 '시군구별'으로 지정되고 variable및 value열은 원래 데이터 프레임에서 추출 된 값과 함께 그 옆에 있습니다.
```

![image-20210609210332377](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609210332377.png)



```python
df['시군구별'].unique()    #'시군구별'에 해당하는 index를 모두출력한다.
array(['시군구별', '전국', '서울특별시', '부산광역시', '대구광역시', '인천광역시', '광주광역시', '대전광역시',
       '울산광역시', '세종특별자치시', '경기도', '강원도', '충청북도', '충청남도', '전라북도', '전라남도',
       '경상북도', '경상남도', '제주특별자치도'], dtype=object)

df[df['시군구별']=='시군구별']   #'시군구별' 데이터만 가져와라
```

![image-20210609210559486](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609210559486.png)

```python
#'시군구별'이 아닌 데이터만 뽑아온다.
df=df[df['시군구별']!='시군구별'].copy()

df['시군구별'].unique()   #위의 코드를 실행했기 때문에, '시군구별' 데이터를 제외한 모든 데이터를 출력한다.
array(['전국', '서울특별시', '부산광역시', '대구광역시', '인천광역시', '광주광역시', '대전광역시',
       '울산광역시', '세종특별자치시', '경기도', '강원도', '충청북도', '충청남도', '전라북도', '전라남도',
       '경상북도', '경상남도', '제주특별자치도'], dtype=object)
```

```python
df.head()   #전체 데이터의 맨 윗줄 5개만 출력한다.
df.tail()   #전체 데이터의 맨 아랫줄 5개만 출력한다.
```

![image-20210609211151549](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609211151549.png)

![image-20210609211211800](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609211211800.png)



```python
df.info()   #datatype과 형태 등 현재 데이터에 대한 정보를 확인한다.

<class 'pandas.core.frame.DataFrame'>
Int64Index: 14904 entries, 1 to 15731
Data columns (total 3 columns):
 #   Column    Non-Null Count  Dtype 
---  ------    --------------  ----- 
 0   시군구별      14904 non-null  object
 1   variable  14904 non-null  object
 2   value     14904 non-null  object
dtypes: object(3)
memory usage: 465.8+ KB
```

```python
df['variable'].str.split('.')
print(df['variable'].str.split('.'))  #하나의 행, 하나의 열은 Series로 리턴된다.

1           [1997,  01]
2           [1997,  01]
3           [1997,  01]
4           [1997,  01]
5           [1997,  01]
              ...      
15727    [2019,  12, 2]
15728    [2019,  12, 2]
15729    [2019,  12, 2]
15730    [2019,  12, 2]
15731    [2019,  12, 2]
Name: variable, Length: 14904, dtype: object
```

```python
type(df['variable'].str.split('.'))   #위의 데이터의 타입을 확인한다.

pandas.core.series.Series
```

```python
df['연도']=df['variable'].str.split('.',expand=True)[0]
df['월']=df['variable'].str.split('.',expand=True)[1]
df['성별']=df['variable'].str.split('.',expand=True)[2]
#print(df['variable'].str.split('.'))
#연도, 월, 성별이 같은것끼리 차례대로 정렬한다.
```

```python
df.head()
```

![image-20210609212832318](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609212832318.png)

```python
df.tail()
```

![image-20210609212905909](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609212905909.png)

```python
#Series로 리턴
print(type(df['variable'].str.split('.')))
#print(df['variable'].str.split('.'))
print(df['variable'].str.split('.')[1][0])

#DataFrame으로 리턴
print(type(df['variable'].str.split('.',expand=True)))   #expand의 default는 'False'이므로 True로 값을 지정해주어야 한다.
df['variable'].str.split('.',expand=True).head()

<class 'pandas.core.series.Series'>
1997
<class 'pandas.core.frame.DataFrame'>
```

![image-20210609213004747](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609213004747.png)

```python
#성별에 'None'이 포함되어 있다.
df['성별'].unique()

array([None, '1', '2'], dtype=object)

#None이 아닌 unique갯수만 반환한다.
df['성별'].nunique()

2

#성별에 None인 것을 '전체'로 채워줘라 
df['성별']=df['성별'].fillna('전체')   
df['성별'].unique()

array(['전체', '1', '2'], dtype=object)

#'성별'에서 1을 남, 2를 여로 변경한다.
df['성별']=df['성별'].replace('1','남').replace('2','여')
df['성별'].unique()

array(['전체', '남', '여'], dtype=object)

#Series에만 사용할 수 있다.
#빈도수를 계산한다.
df['성별'].value_counts()

전체    4968
남     4968
여     4968
Name: 성별, dtype: int64
        
df=df.rename(columns={'variable':'기간','value':'출생아수'})  #variable을 기간으로, value를 출생아수로 바꾼다.
```

![image-20210609213438235](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609213438235.png)

```python
df.sample(10)   #데이터 전체 중 무작위로 10개만 출력하라
```

![image-20210609213659222](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609213659222.png)

```python
df['출생아수']=df['출생아수'].replace('-',np.nan)   #넘파이에서 nan이라는 값으로 설정할 수 있도록 제공됨
df.sample(10)
```

![image-20210609213745002](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609213745002.png)

```python
#출생아수의 데이터 타입을 확인한다.

df.info()

<class 'pandas.core.frame.DataFrame'>
Int64Index: 14904 entries, 1 to 15731
Data columns (total 6 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   시군구별    14904 non-null  object
 1   기간      14904 non-null  object
 2   출생아수    14364 non-null  object
 3   연도      14904 non-null  object
 4   월       14904 non-null  object
 5   성별      14904 non-null  object
dtypes: object(6)
memory usage: 1.3+ MB
```

```python
#ValueError: cannot convert float NaN to integer
#Nan을 float으로 인식해서 바꿀수가 없다.
df['출생아수'].astype(int)   

---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-65-9b5ee33b496e> in <module>
----> 1 df['출생아수'].astype(int)

~\anaconda3\lib\site-packages\pandas\core\generic.py in astype(self, dtype, copy, errors)
   5875         else:
   5876             # else, only a single dtype is given
-> 5877             new_data = self._mgr.astype(dtype=dtype, copy=copy, errors=errors)
   5878             return self._constructor(new_data).__finalize__(self, method="astype")
   5879 

~\anaconda3\lib\site-packages\pandas\core\internals\managers.py in astype(self, dtype, copy, errors)
    629         self, dtype, copy: bool = False, errors: str = "raise"
    630     ) -> "BlockManager":
--> 631         return self.apply("astype", dtype=dtype, copy=copy, errors=errors)
    632 
    633     def convert(

~\anaconda3\lib\site-packages\pandas\core\internals\managers.py in apply(self, f, align_keys, ignore_failures, **kwargs)
    425                     applied = b.apply(f, **kwargs)
    426                 else:
--> 427                     applied = getattr(b, f)(**kwargs)
    428             except (TypeError, NotImplementedError):
    429                 if not ignore_failures:

~\anaconda3\lib\site-packages\pandas\core\internals\blocks.py in astype(self, dtype, copy, errors)
    671             vals1d = values.ravel()
    672             try:
--> 673                 values = astype_nansafe(vals1d, dtype, copy=True)
    674             except (ValueError, TypeError):
    675                 # e.g. astype_nansafe can fail on object-dtype of strings

~\anaconda3\lib\site-packages\pandas\core\dtypes\cast.py in astype_nansafe(arr, dtype, copy, skipna)
   1072         # work around NumPy brokenness, #1987
   1073         if np.issubdtype(dtype.type, np.integer):
-> 1074             return lib.astype_intsafe(arr.ravel(), dtype).reshape(arr.shape)
   1075 
   1076         # if we have a datetime/timedelta array of objects

pandas\_libs\lib.pyx in pandas._libs.lib.astype_intsafe()

ValueError: cannot convert float NaN to integer
```

```python
#'출생아수'의 Nan때문에 int로 바꿀수 없어 float로 자료형을 변경하고 자료형 정보를 출력한 화면이다.
df['출생아수']=df['출생아수'].astype(float)   
df.info()

<class 'pandas.core.frame.DataFrame'>
Int64Index: 14904 entries, 1 to 15731
Data columns (total 6 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   시군구별    14904 non-null  object 
 1   기간      14904 non-null  object 
 2   출생아수    14364 non-null  float64
 3   연도      14904 non-null  object 
 4   월       14904 non-null  object 
 5   성별      14904 non-null  object 
dtypes: float64(1), object(5)
memory usage: 1.3+ MB
```

```python
#10022	세종특별자치시	2011. 08.2	NaN	2011	08	여
df.iloc[10022]  #10022 는 index이기 때문에 값이 엉뚱하게 출력된다.

시군구별          전라남도
기간      2012. 06.1
출생아수         686.0
연도            2012
월               06
성별               남
Name: 10579, dtype: object

df.loc[10022]

시군구별       세종특별자치시
기간      2011. 08.2
출생아수           NaN
연도            2011
월               08
성별               여
Name: 10022, dtype: object
        
df['출생아수'].describe()

count    14364.000000
mean      3072.798942
std       6683.035581
min         30.000000
25%        638.000000
50%       1045.000000
75%       1947.000000
max      63268.000000
Name: 출생아수, dtype: float64
```

### <시군구별 : 전국, 성별 : 전체> 만 출력하기

```python
df_all=df[(df['시군구별']=='전국')& (df['성별']=='전체')]   
#isin사용은 비교대상이 다르기때문에 안된다.
print(df_all['시군구별'].unique())
print(df_all['성별'].unique())
#'시군구별'은 '전국', '성별'은 '전체'인 데이터만 출력한다.

['전국']
['전체']

df_all.head()
df_all.tail()
```

![image-20210609214839220](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609214839220.png)

![image-20210609214849861](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609214849861.png)

### pandas을 통한 시각화

```python
df_all.set_index(['연도','월']).plot(figsize=(15,4))

<AxesSubplot:xlabel='연도,월'>
```

![image-20210609214954623](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609214954623.png)

```python
#막대그래프는 x축에 연도, 월을 모두 표현한다.
#df_all.set_index(['연도','월']).plot.bar(figsize=(15,4))
df_all.set_index(['연도','월']).plot(kind='bar',figsize=(15,4))

<AxesSubplot:xlabel='연도,월'>
```

![image-20210609215021823](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215021823.png)

```python
#최근 24개만 가져와서 그래프로 그린다.
df_all[-24:].set_index(['연도','월']).plot(kind='bar',figsize=(15,4))

<AxesSubplot:xlabel='연도,월'>
```

![image-20210609215106495](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215106495.png)

```python
df_all[-24]    #[-24]는 컬럼이다.

---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
~\anaconda3\lib\site-packages\pandas\core\indexes\base.py in get_loc(self, key, method, tolerance)
   3079             try:
-> 3080                 return self._engine.get_loc(casted_key)
   3081             except KeyError as err:

pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()

pandas\_libs\index.pyx in pandas._libs.index.IndexEngine.get_loc()

pandas\_libs\hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()

pandas\_libs\hashtable_class_helper.pxi in pandas._libs.hashtable.PyObjectHashTable.get_item()

KeyError: -24

The above exception was the direct cause of the following exception:

KeyError                                  Traceback (most recent call last)
<ipython-input-87-ccf6c75101c4> in <module>
----> 1 df_all[-24]    #[-24]는 컬럼이다.

~\anaconda3\lib\site-packages\pandas\core\frame.py in __getitem__(self, key)
   3022             if self.columns.nlevels > 1:
   3023                 return self._getitem_multilevel(key)
-> 3024             indexer = self.columns.get_loc(key)
   3025             if is_integer(indexer):
   3026                 indexer = [indexer]

~\anaconda3\lib\site-packages\pandas\core\indexes\base.py in get_loc(self, key, method, tolerance)
   3080                 return self._engine.get_loc(casted_key)
   3081             except KeyError as err:
-> 3082                 raise KeyError(key) from err
   3083 
   3084         if tolerance is not None:

KeyError: -24
```

```python
df_all[1:20]
```

![image-20210609215220162](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215220162.png)

### seaborn을 통한 시각화

```python
plt.figure(figsize=(15,4))
sns.lineplot(data=df_all,x='연도',y='출생아수')

plt.figure(figsize=(15,4))
sns.lineplot(data=df_all,x='연도',y='출생아수',ci=None)

plt.figure(figsize=(15,4))
sns.lineplot(data=df_all,x='연도',y='출생아수',ci=None,hue='월')

<AxesSubplot:xlabel='연도', ylabel='출생아수'>
```

![image-20210609215418452](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215418452.png)

```python
plt.figure(figsize=(15,4))
sns.barplot(data=df_all,x='연도',y='출생아수')

plt.figure(figsize=(15,4))
sns.barplot(data=df_all,x='연도',y='출생아수',ci=None)

plt.figure(figsize=(15,4))
sns.barplot(data=df_all,x='연도',y='출생아수',ci=None,hue='월')

<AxesSubplot:xlabel='연도', ylabel='출생아수'>
```

![image-20210609215539672](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215539672.png)

### 지역별 출생아수

```python
df_local=df[df['시군구별']!='전국'].copy()
df_local.head()
```

![image-20210609215714589](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215714589.png)

```python
df_local['시군구별'].unique()

array(['서울특별시', '부산광역시', '대구광역시', '인천광역시', '광주광역시', '대전광역시', '울산광역시',
       '세종특별자치시', '경기도', '강원도', '충청북도', '충청남도', '전라북도', '전라남도', '경상북도',
       '경상남도', '제주특별자치도'], dtype=object)
```

```python
plt.figure(figsize=(15,4))
sns.pointplot(data=df_local,x='연도',y='출생아수',hue='성별')

<AxesSubplot:xlabel='연도', ylabel='출생아수'>
```

![image-20210609215818310](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215818310.png)

```python
df_local_all=df_local[df_local['성별']=='전체']
df_local_all.head()
```

![image-20210609215903929](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609215903929.png)

```python
df_local_all['성별'].unique()

array(['전체'], dtype=object)

plt.figure(figsize=(15,4))
sns.pointplot(data=df_local_all,x='연도',y='출생아수')

plt.figure(figsize=(15,4))
sns.pointplot(data=df_local_all,x='연도',y='출생아수',hue='시군구별')

#bbox_to_anchor=(x,y,width,height)  -> x,y만 지정되어있어서 기본값으로 출력됨
#ncol=1
plt.legend(loc='center right',bbox_to_anchor=(1.17,0.5),ncol=1)

<matplotlib.legend.Legend at 0x28ad10cc0d0>
```

![image-20210609220037656](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609220037656.png)

```python
df_local_2=df_local_all[df_local_all['시군구별'].isin(['서울특별시','경기도','세종특별자치시'])]
df_local_2['시군구별'].unique()

array(['서울특별시', '세종특별자치시', '경기도'], dtype=object)
```

```python
plt.figure(figsize=(15,4))
sns.pointplot(data=df_local_2,x='연도',y='출생아수',hue='시군구별')

<AxesSubplot:xlabel='연도', ylabel='출생아수'>
```

![image-20210609220135107](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609220135107.png)

```python
df_local_3=df_local_all[df_local_all['시군구별']=='세종특별자치시']
df_local_3['시군구별'].unique()

array(['세종특별자치시'], dtype=object)
```

```python
plt.figure(figsize=(15,4))
sns.pointplot(data=df_local_3,x='연도',y='출생아수',hue='시군구별')

<AxesSubplot:xlabel='연도', ylabel='출생아수'>
```

![image-20210609220234348](C:\Users\jini8\AppData\Roaming\Typora\typora-user-images\image-20210609220234348.png)

