# WIL1 - 2023-03-18 :: 데이터 분석을 위한 pandas 입문

## iloc / loc / query

데이터프레임의 일부를 추출해내는 방법이라는 점에서 동일하다. 다만,

* iloc
  
    0으로 시작하는 숫자 기반의 인덱스값 지원

    ```python
    # DataFrame의 1~3번째 열 추출
    df.iloc[ : , 0:3] 
    ```

* loc 
  
    칼럼명, 조건식 지원

    ```python
    # 'A'열 및 'D'열 추출
    df.loc[ : , ['A', 'D']] 
    ```

* query

    다양한 연산과 인덱스 검색, 문자열 부분 검색 지원

    ```python
    # '언어'가 '파이썬'인 열 추출
    df.query("언어 == '파이썬'")
    ```

    여러 조건을 함께 사용할 수도 있다.

    ```python
    df.query("(학과 == '컴공') and (언어 == '파이썬') and (직무 == 'AI엔지니어')")
    ```

    Column명에 공백이 포함되어 있을 경우 backtick, (`)으로 감싸 사용한다.

    ```python
    # 'Maple Story' Column 속 'panda'가 포함된 데이터 추출
    df.query("`Maple Story`.str.contains('panda')")
    ```

    reset_index 함수를 통해 index값을 0부터 다시 배치할 수 있다.
    
    ```python
    ps.query("species == 'Gentoo'").reset_index(drop=True)
    ```

## DataFrame 정렬

.sort_values를 사용하여 정렬이 가능하다.

```python
# A열 기준, 내림차순으로, 기존 Index 무시 및 재정렬
df.sort_values(by='A', ascending=False, ignore_index=True)
```

## DataFrame 붙이기

```python
# 위아래로 붙이기
concat0 = pd.concat([df, df1], axis = 0)

# 좌우로 붙이기
concat1 = pd.concat([df, df1], axis = 1)
```

## Groupby

DataFrame에서 각 열마다 평균, 최대, 최소, 중앙값 따위를 얻을 수 있다.

```python
penguinSize.groupby(by=["species"]).mean()
```

하나의 Column에 각각 다른 결과값을 함께 담을 수도 있다.

```python
penguinSize.groupby(by=["island"]).aggregate(['mean', 'max', 'min', 'median'])
```

## Apply Lambda

Column값에 함수를 적용할 수 있다.

```python
# 제곱 함수
def power2(x):
    x *= x
    return x

# 'body_mass_g'의 제곱값을 가진 Column을 맨 뒤에 덧붙임
penguinSize = penguinSize.assign(new_column = penguinSize['body_mass_g'].apply(power2))
```

## Drop

```python
# Column별 공백값 개수
penguinSize.isna().sum()

# 공백값을 가진 데이터 제거
penguinSize.dropna()

# 특정 Column 제거
penguinSize.drop(columns=['island'])
```

## Dictionary

특정 데이터프레임을 사전(Dictionary)화하여 다른 데이터프레임에 해당 값을 적용할 수 있다.

```python
# '이름' Column 값을 기준으로 같은 행에 위치한 데이터들을 배열로 사전화
dict = dfdf2.set_index("이름").T.to_dict('list')

# '이름' 바로 옆 '코드' Column 값을 dfdf1 데이터프레임 내부 값과 대치
dfdf3 = dfdf1.applymap(lambda x : dict[x][0], na_action="ignore")
```

## 외부 데이터를 불러와 가공하는 방법

```python
# Read CSV File
ps = pd.read_csv("penguins_size.csv")
```