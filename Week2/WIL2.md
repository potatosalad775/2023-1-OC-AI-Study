# WIL2 - 2023-03-26 :: matplotlib + seaborn을 통한 데이터 시각화

seaborn은 matplotlib 위에 구축된 라이브러리로, 통계적인 시각화를 중점적으로 기능을 간결화한 것이 특징입니다. 

## matplotlib 사용하기

```python
import matplotlib.pyplot as plt

x = [0, 1, 2, 3, 4, 5]
y = [123, 1, 59, 40, 10, 100]
```

### 제목 삽입

```python
plt.title("Shape Like W")
```

### Line plot

```python
plt.plot(x, y)
plt.show()
```

### Bar plot

```python
plt.bar(x, y)
plt.show()
```

### subplot + figsize

subplot을 통해 여러 개의 그래프를 그릴 수 있다.
첫번째, 두번째 숫자는 각각 그래프가 배치되는 행렬의 갯수를,
세번째 숫자는 해당 그래프의 순서를 지정하는데 사용된다.

```python
plt.figure(figsize=(12,5)) # 그래프의 사이즈 결정

plt.subplot(131) # 1x3 그래프들 중 첫번째
plt.plot(x, y)

plt.subplot(132) # 1x3 그래프들 중 두번째
plt.bar(x, y)

plt.subplot(133) # 1x3 그래프들 중 세번째
plt.scatter(x, y)

plt.show()
```

### pie chart

explode 배열을 통해 몇번째 항목이 중심에서 얼마나 떨어지는지 지정할 수 있다.

```python
cars=['AUDI', 'BMW', 'FORD', 'TESLA', 'JAGUAR']
data=[23, 29, 41, 12, 17]
explode = [0, 0, 0.2, 0, 0]

plt.pie(data, explode=explode, labels=cars, autopct='%.1f%%', shadow=True)
plt.show()
```

## seaborn 사용하기

```python
import matplotlib.pyplot as plt
import seaborn as sns
```

### data set 불러오기

```python
penguin_df = sns.load_dataset("penguins")
```

### Scatter plot

```python
sns.scatterplot(data=penguin_df, x="body_mass_g", y="bill_length_mm")
plt.show()
```

### kde plot

펭귄 몸무게의 분포를 주황색으로 그리고, 그래프 선 아래를 채운다.

```python
sns.kdeplot(data=penguin_df, x="body_mass_g", fill=True, color='orange')
plt.show()
```

### box plot

seaborn을 사용한 box plot에서는 `이상치`가 함께 표현된다.
`이상치`란 특정 그룹에 분류되지 못하는 값으로, 데이터의 범위를 크게 벗어난 아주 작은 값이나 큰 값을 의미한다.

```python
tips=sns.load_dataset('tips')

sns.boxplot(tips, x="size", y="tip")
plt.show()
```

## plt vs ax

matplotlib는 두 가지 방법으로 사용이 가능하다.
- stateless API (객체지향)
- stateful API (state-based)

stateless 방법은 내가 지정한 figure, 내가 지정한 ax에 그림을 그리는 방법이고,
stateful 방법은 현재의 figure, 현재의 ax에 그림을 그리는 방법이다.
* figure : 그래프가 그려지는 공간 / ax : 해당 공간 속 내가 사용할 부분

stateless 방법은 figure과 ax를 지정해야하는 - 즉, 직접 만들어야하므로 객체지향적이라고 할 수 있고, 반면에 stateful 방법은 현재의 figure과 ax이 자동으로 지정됩니다.

정리하자면, 
stateless 방식을 사용할 때 fig, ax 명령어가 사용되고,
stateful 방식을 사용할 때 plt 명령어가 사용된다.

## Decorating

```python
import matplotlib.pyplot as plt

X = ["Tom", "Amy", "Jim", "Bill", "Pegi"]
Y = [8, 23, 52, 37, 64]

fig, ax = plt.subplots() # using Stateless API
```

### bar color

```python
ax.bar(X, Y, color="lightgray", edgecolor="gray")
```

### axis label

```python
ax.set_xlabel("Salesperson", fontsize=14, fontweight="bold", color="gray")
ax.set_ylabel("Revenue (1000$)", fontsize=14, fontweight="bold", color="gray")
```

### adjust axis distance

```python
plt.tight_layout(h_pad=12, w_pad=12) # X, Y축과의 거리는 12
```

### Text annotation

반복문을 사용하여 각각의 bar 위에 height 정보를 annotation으로 추가합니다.

```python
fig, ax = plt.subplots()
bars = ax.bar(X, Y, color="lightgray", edgecolor="gray")

for bar in bars:
    height = bar.get_height()
    ax.annotate(f'{height}', xy=(bar.get_x() + (bar.get_width() / 2), height), va='bottom', ha='center')
```

### 수평선

그래프의 bar가 가장 위로 올라오도록 ax.bar에 zorder=3을 추가합니다.

```python
bars = ax.bar(X, Y, color="lightgray", edgecolor="gray", zorder=3)

plt.axhline(y=45, color='blue', linestyle='--')
plt.axhline(y=15, color='red', linestyle='--')

ax.axhspan(0, 15, facecolor='red', alpha=0.3) # 0~15까지 영역 채우기
```