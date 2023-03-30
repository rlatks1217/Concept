
- numpy는 다차원 배열이기 때문에 엑셀처럼 테이블 모양으로 사용할 수 없음 => 불편함
- 이것을 해소해주는 numpy를 기반으로 한 Pandas라는 mudule을 사용함

### Pandas 이용 목적
- 데이터 분석(EDA : 탐색적 데이터 분석(ex)많은 사람 중 가장 인기있는 연예인 찾기))
- 데이터 전처리
- 데이터 분석이 잘못되었다면 AI에게 잘못된 데이터를 학습시키게 되는 것임(망함) => 그래서 학습시킬 수 있는 적당한 형태로 데이터를 바꿔줘야 함(데이터 전처리라고 함)

### Pandas
- Series(1차원, 1차원 ndarray, 사용자 지정 index 이용 가능)
- DataFrame(2차원, Series의 집합) => 데이터 분석할 때는 결국 이것을 사용함

### DataFrame
- dictionary를 사용하여 만드는 것이 편함
- column index와 row index(내가 임의로 지정하더라도 default로 정해져있는 숫자 인덱스를 사용할 수 있음)가 있음(index가 2개임)

conda env list => 가상환경의 이름 list를 알려주는 코드
conda remove 가상환경 이름 => 가상환경을 지우는 코드(이 코드 작성 후 실제 경로의 폴더도 지워줘야 함)
```
df.loc[['one', 'four']] #Fancy index를 지정 인덱스를 통해 하고 싶은 경우 loc를 써야 함
df[['이름','학과']] 
#loc 쓰지 않아도 컬럼은 Fancy index를 할 수 있음
#loc를 쓰게 될 경우 df[특정행이나 행의 대한 조건(boolean_mask),[컬럼이름들]] 라고 작성해야 함


df.loc[df['학점']> 2.0, ['이름','학점']]
#boolean_mask는 행이나 열에 대해서 필터 역할을 해줄 수 있음
df.loc[df['학점']>= 3.0, ['등급']] = 'A' 
df



#학점이 1.5이상 2.5이하인 학생의 학과 이름 학점을 출력하세요!
df.loc[(df['학점'] >= 1.5) & (df['학점'] <= 2.5), '이름':'학점']
#python &이 and연산자

#신사임당의 학과와 학년정보를 알고 싶어요!
df.loc[(df['이름'] == '신사임당'), '학년':'학과']



#새로운 학생을 추가하고 싶어요(row를 추가하겠다는 의미)

df.loc['six',:] = [1, '원숭이', '유아교육', 3.6, 'B']
#six라는 인덱스가 없기 때문에 우측의 데이터들을 가진 row가 하나 추가될 것



import numpy as np
import pandas as pd

df = pd.read_csv('./data/auto-mpg.csv', header=None)
#header=None ==> 상위 몇 개만 뽑을지 지정하지 않겠다.

#컬럼이름 정해주기
df.columns =['mpg', 'cylinders', 'displacement', 'horsepower',
              'weight', 'acceleration', 'model year', 'origin', 'name']

df



#DataFrame이 제공하는 함수
display(df.head(3)) #상위 3개만 보여줘라(default는 5개)
print(df.shape)

print(df.count()) 
#각 컬럼의 데이터가 몇 개인지 세어줌
#잘못된 데이터나 비어있는 데이터는 세지 않음(당연히 Nan과 같은 건 안 셈)
#왜 많이 사용되는가? 머신러닝에 사용되는 데이터는 결측치가 있으면 안 되기 때문에 확인용으로 많이 사용

#4. value_counts()
# Series에 적용하는 것(고유값을 셀 때 사용함)
# df['origin'].value_counts() #1이 몇개고 2가 몇개 등등의 결과값을 반환함

#5. unique()
# Series에 적용하는 것(중복을 제거할 때 사용함)
# df['model year'].unique()

#6. isin()
# df['origin'].isin([1, 2]) 
#해당 컬럼의 데이터 중 1이나 2가 값으로 있는지 확인
#boolean값으로 반환함 -> boolean_Mask임을 알 수 있음
df.loc[df['origin'].isin([1, 2]), :] #유럽과 미국에서 만든 차들만 출력



#가장 많이 사용하는 함수 중 하나인 ..sort()
#numpy할 때 list를 이용해서 ndarray를 만들었음
arr = np.array([1,2,3,4,5])
arr = np.arange(10)

#ndarray를 만들 때 난수를 이용해서 만들 수 있음
np.random.randint(0, 10, (6,4)) 
#randint() : int형의 난수를 발생시킴/0~9까지중에서(마지막 숫자(10) 포함 안함) 
#마지막에 나온 튜플 형태의 인자는 shape임(지금은 2차원 shape을 준 것이지)

#원래 random을 이용해서 난수를 행성할 때는 함수에 특정 인자가 들어가야 되는 구조임
#(randInt()와 같은 함수에 아무 인자도 주지 않을 경우 내부적으로 아무 숫자나 인자로 집어넣음
#그래서 실행할 때마다 매번 다른 인자를 넣기 때문에 계속 난수값이 바뀌어서 나오는 것임)
#즉, random처럼 보이지만 사실은 내부적으로 알아서 아무 숫자나 인자로 넣고 특정 수들(반환값)을 반환하는 로직을 실행하는 구조임
#이 때 들어가는 인자를 seed라고 함
#그래서 seed()를 통해 특정 숫자를 지정해주면 그 case에 해당하는 수들만 반환하게 됨 
#결론적으로 난수값이 변하지 않음

#시드를 고정하면 난수값이 안 변함
np.random.seed(100) #이 값이 변하지 않는 이상 난수가 변하지 않음
np.random.randint(0, 10, (6,4))



#이 난수로 만든 2차원 데이터를 이용해서 DataFrame을 만들자
df = pd.DataFrame(np.random.randint(0, 10, (6,4)))
df.columns = ['A','B','C','D']

df.index = pd.date_range('20230101', periods=6) 
#날짜 범위를 생성해줌(일단위로 생성/periods는 갯수(기간))
#그 날짜를 df의 index로 지정(여기서의 index는 행 인덱스를 지칭함)

random_date = np.random.permutation(df.index)
#해당 인덱스를 섞어줌

display(df)

#index를 다시 설정(row 인덱스와 column 인덱스 둘 다 다시 설정 가능)
df2 = df.reindex(index=random_date, columns=['B','A','D','C'])

display(df2)



#어느 인덱스를 정렬할 지 축(axis)으로 정해줌
#ascending=True 오름차순 정렬을 의미

#열 방향
df2.sort_index(axis=1, ascending=True, inplace= True) 
#컬럼의 순서가 바뀌므로 당연히 컬럼값들의 위치도 컬럼을 따라감
#여기서도 당연히 inplace= True이 가능

#행 방향
df2.sort_index(axis=0, ascending=False)

#특정 컬럼의 값들을 기준으로 행을 정렬
df2.sort_values(by='A') #B컬럼 기준으로 정렬

#컬럼값이 같은 경우에는 이렇게 처리(SQL order by와 똑같음)
df2.sort_values(by=['A','B'])



# 통계 중 기술통계(describe)부분이 있음
# 평균, 합, 편차, 분산, 표준편차, 사분위, 공분산, 상관관계 등이 여기 해당
# 즉, 수치적인 계산이 나오는 것들이 바로 여기에 해당됨

# Pandas가 제공하는 함수를 이용해서 계산하는 것
# sum(), mean() : 대표적인 기술통계함수

#python의 list를 만들어요 그런데 2차원으로 만들것이기 때문에 중첩리스트 형태로 만들 것임

data = [[2, np.nan],
        [7, -3],
        [np.nan, np.nan],
       [1,-2]]

# 이걸 가지고 DataFrame을 만들어 보아요(dictionary를 가지고 만들 수도 있음)
df = pd.DataFrame(data, columns= ['one','two'], index=['a','b','c','d'])

display(df)

# numpy가 아니라 DataFrame을 하고 있음
# 이 둘은 집계함수를 쓸 때 차이가 있음

print(df.sum()) 
#numpy에서는 2차원 배열 안에 있는 데이터를 모두 더했었음
#df안의 값을 모두 더하는 게 아님
#sum 안에 인자가 없으면 default로 axis =0(행방향)으로 설정되어 있음



# print(df.sum(axis=1)) # 결과가 4개로 나올 것
# print(df.mean()) 
#이 역시 axis의 default는 행방향(0)이고 nan이 있을 경우 그건 빼고 연산하는 것이 default

# print(df.mean(axis=0, skipna=False)) 
#skipna=False라고 하면 nan을 무시하지 않음
#그런데 nan을 무시하지 않으면 숫자랑 nan은 연산할 수 없으므로 결과가 nan으로 떨어진다.



#NaN 처리 방법
#1. 삭제 => 가장 좋은 방법임
# 근데 만약 애초에 데이터가 충분치 않을 경우 거기서 삭제까지 하면 데이터가 더 적어지게 됨
# 데이터분석에서 데이터는 많은 게 좋다고 함
# 일반적으로는 10만건 정도 있으면 충분한 데이터라고 함(하지만 사실은 그때그때 다름)

#2. NaN을 적절하게 수정해서 사용해야 함(Interpolation - 보간이라고 함)
#일반적으로 NaN은 데이터들의 평균값으로 수정해서 사용하는 경우가 있음
#주변 값이랑 비슷한 값을 NaN자리에 넣어서 사용하는 경우도 있음
#이런 여러 방법을 이용해서 값을 수정함

mean_one = df['one'].mean()
mean_two = df['two'].mean()

#함수를 이용하여 NaN을 다른 값으로 수정할 수 있음
# df['one'] = df['one'].fillna(value=mean_one) #원본 열에 변화된 열 넣음
df['one'].fillna(value=mean_one, inplace=True) 
#여기서도 inplace=True 사용가능
print(df['one'])
#NaN을 찾아서 mean_one의 값을 넣어라
#mean_one이 실수라면 나머지 데이터의 형태도 실수형태로 바뀜



#DataFrame 연결 혹은 겳합
#그룹핑
df1 = pd.DataFrame({
    'a' : ['a0','a1','a2','a3'],
    'b' : [1,2,3,4],
    'c' : ['c0','c1','c2','c3']
})

display(df1)


df2 = pd.DataFrame({
    'b' : ['b0','b1','b2','b3'],
    'c' : ['c0','c1','c2','c3'],
    'd' : ['d0','d1','d2','d3'],
    'e' : ['e0','e1','e2','e3']
},index=[2,3,4,5])

display(df1)

#1. 위, 아래 방향으로 이어 붙여요
result1 = pd.concat([df1, df2], axis = 0) #연결할 DataFrame들을 list로 묶어서 인자로 넣음, 붙을 방향(위아래(행방향) or 양옆(열방향))
display(result1) #보면서 이해
#각 DataFrame에 있던 행 인덱스 역시 고대로 붙게 됨(인덱스가 중복됨)

result2 = pd.concat([df1, df2], axis = 0, ignore_index=True)
#ignore_index=True하면 각 DataFrame에 있던 인덱스를 무시하고 
#새로 합쳐진 DataFrame에 대한 새로운 인덱스를 생성함
display(result2)

#DataFrame을 가로로 붙여요
#붙을 때 행 인덱스가 같은 것끼리 붙음
result2 = pd.concat([df1, df2], axis = 1, ignore_index=True)



import numpy as np
import pandas as pd

data1 = { "학번" : [1,2,3,4],
          "이름" : ["홍길동","신사임당","강감찬","이순신"],
          "학년" : [2,4,1,3]}

data2 = { "학번" : [1,2,4,5],
          "학과" : ["CS","MATH","MATH","CS"],
          "학점" : [3.4,2.9,4.5,1.2]}



df1 = pd.DataFrame(data1)
df2 = pd.DataFrame(data2)

display(df1, df2)

#inner join 해보기 merge() 사용
pd.merge(df1, df2, on='학번', how='inner') #on : 기준 컬럼 / join 기준

pd.merge(df1, df2, on='학번', how='outer') #on : 기준 컬럼 / join 기준



data1 = { "학번" : [1,2,3,4],
          "이름" : ["홍길동","신사임당","강감찬","이순신"],
          "학년" : [2,4,1,3]}

data2 = { "학생학번" : [1,2,4,5],
          "학과" : ["CS","MATH","MATH","CS"],
          "학점" : [3.4,2.9,4.5,1.2]}

df1 = pd.DataFrame(data1)
df2 = pd.DataFrame(data2)

#하지만 두 DataFrame이 동일한 종류의 데이터를 가지고 있더라도 컬럼이름은 다를 수 있음
#그래서 아래처럼 처리한다.
pd.merge(df1, df2, left_on='학번',right_on='학생학번', how='outer')



#Grouping
#Group을 하기 전에.. 함수매핑부터 알아보아요
import numpy as np
import pandas as pd
import seaborn as sns

# seaborn이 가지고 있는 data-set을 사용하기 위해 설치한 것임
# seaborn이 가지고 있는 data-set 중 titanic이라는 이름의 데이터를 사용해 볼 것
titanic = sns.load_dataset('titanic') #데이터를 pandas의 DataFrame 형태로 바로 바꿔줌
display(titanic)



# df = titanic[['age','fare']]

#위처럼도 되지만 행과 열을 추출하기 위해서는 일단 loc를 써야 한다고 알자
df = titanic.loc[:,['age','fare']]
display(df)

#사용자 정의 함수
def add_10(n) :
    return n +10

def myFunc(a,b) :
    return a + b

# age열의 모든 행에 대해서 함수를 적용하고 싶어요!
# 즉, Series에 대해서 함수를 적용하려고 하는 것임 => 함수매핑!
# apply()라는 함수를 쓰면 됨 => 결과값은 처리가된 Series가 바로 리턴됨
# Series의 각 요소마다 함수를 적용시키는 역할을 함

#기본적으로 Series의 각 요소들이 해당 함수의 인자로 들어가게 될 것임
result1 = df['age'].apply(add_10)
print(result1)

#위와는 다르게 함수의 인자가 2개다 기본적으로 Series의 각 요소가 첫째 인자로 들어갈 것이고 
#나머지 하나를 더 집어넣어줘야 하므로 이런식으로 임의로 하나를 더 넣어준다
result2 = df['age'].apply(myFunc, b=20)
print(result1)

#람다로 처리
result3 = df['age'].apply(lambda x: x+30)
print(result3)



# 위에서 설명한 건 Series에 대해서 apply()함수를 적용할 수 있다는 내용이었음
# 그렇다면 DataFrame에 함수를 적용하려면 어떻게 해야 할까?
# applymap()을 이용하면 됨

df = titanic.loc[:,['age','fare']]
display(df)

def add_10(n) :
    return n + 10

result4 = df.applymap(add_10)
display(result4.head(4))



#진짜 group by

df = titanic.loc[:, ['age','sex','class','fare','survived']]
display(df.head())

# 그룹으로 묶어보자
grouped = df.groupby(['class']) #[컬럼명]을 인자를 넣어서 묶음
print(grouped) # 묶은 데이터라는 정보만 나오고 어떻게 묶였는지 눈으로 확인할 수 없음

# 총 981개의 데이터가 class를 기준으로 'First', 'Second','Third'
# 3개의 그룹으로 묶여요! 확인하려면 for문을 이용하면 됨

#key : 몇번째 그룹인지 / group : 실제 한 그룹으로 묶인 데이터들의 DataFrame형태
for key, group in grouped : # (key, group)는 튜플이기 때문에 key, group으로 괄호를 생략한 형태로 쓸 수 있음
    print('key : {}'.format(key))
    print('key : {}'.format(len(group)))
    display(group.head())
    
avg = grouped.mean()
display(avg)

#한 그룹만 뽑아보자
group3 = grouped.get_group('Third')
display(group3)



#사실 [컬럼명]라는 list형태로 인자를 주고 있음 
#이것은 [컬럼명1, 컬럼명2]처럼 여러 개로 grouping을 할 수도 있단 얘기임 
grouped = df.groupby(['class','sex'])
avg2 = grouped.mean()
display(avg)



#그룹 안에 그룹을 뽑고 싶은 경우 튜플을 인자로 주면 됨
group4 = grouped.get_group(('Third','female'))
display(group4)
```

