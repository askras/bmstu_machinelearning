---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.4
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Операции над массивами: транслирование


универсальные функции NumPy можно использовать для *векторизации* операций и тем самым устранения медленных циклов Python.
Другим способом векторизации операций является использование *трансляции* (*broadcasting*) NumPy.
Транслирование &mdash; это просто набор правил для применения бинарных универсальных функций (например, сложения, вычитания, умножения и т. д.) к массивам разных размеров.


## Знакомство с транслированием

Напомним, что для массивов одинакового размера бинарные операции выполняются поэлементно:

```python jupyter={"outputs_hidden": false}
import numpy as np
```

```python jupyter={"outputs_hidden": false}
a = np.array([0, 1, 2])
b = np.array([5, 5, 5])
a + b
```

Транслирование позволяет выполнять подобные операции над массивами разных размеров &mdash; например, можно так же легко добавить скаляр (рассматривая его как нульмерный массив) к массиву:

```python jupyter={"outputs_hidden": false}
a + 5
```

Эту операцию можно рассматривать как операцию, которая растягивает (дублирует) значение `5` в массив `[5, 5, 5]`, а уже после этого складывающую полученный массив с массивом `a`.
Дублирования значений на самом деле не происходит, это лишь лишь удобная для понимания модель, описывающая суть операции транслирования.

Аналогичным образом это работает с массивами более высокой размерности. 
Посмотрим на результат сложения одномерного и двумерного массивов:

```python jupyter={"outputs_hidden": false}
M = np.ones((3, 3))
M
```

```python jupyter={"outputs_hidden": false}
M + a
```

Здесь одномерный массив `a` растягивается вдоль оси 0 (по строкам), чтобы соответствовать форме `M`.


Эти примеры относительно просты для понимания.
Более сложные случаи могут включать транслирование обоих массивов. 
Рассмотрим следующий пример:

```python jupyter={"outputs_hidden": false}
a = np.arange(3)
b = np.arange(3)[:, np.newaxis]

print(a)
print(b)
```

```python jupyter={"outputs_hidden": false}
a + b
```

В данном случае транслированию (растяжению) подверглись оба массива, чтобы они соответствовали общей форме, и в результате получился двумерный массив!


## Правила транслирования

Процесс транслирование в NumPy подчиняется строгому набору правил, определяющих взаимодействие между двумя массивами:

- Правило 1: Если два массива различаются по количеству измерений, то форма массива с меньшим количеством измерений *дополняется* единицей с левой стороны.
- Правило 2: Если форма двух массивов не совпадает ни в одном измерении, массив с формой, равной 1 в этом измерении, растягивается до соответствия форме другого массива.
- Правило 3: Если в каком-либо измерении размеры не совпадают и ни один из них не равен 1, возникает ошибка.

Визуально данные правила можно изобразить так:


![Транслирование массивов](img/broadcasting.png)


Полупрозрачные кубики отображают транслируемые значения: дополнительная память при этом не выделяется, но концептуально может быть полезно представить, что это происходит.


Для лучшего понимания правил подробно рассмотрим несколько примеров.


### Пример транслирования

Рассмотрим сложение двумерного и одномерного массивов:

```python jupyter={"outputs_hidden": false}
M = np.ones((2, 3))
a = np.arange(3)
```

Давайте рассмотрим операцию над этими двумя массивами. Форма массивов:

- `M.shape = (2, 3)`
- `a.shape = (3,)`

Массив `a` имеет меньше измерений, поэтому, по правилу 1, дополняем его слева единицами:

- `M.shape -> (2, 3)`
- `a.shape -> (1, 3)`

Теперь, первое измерение не согласуется, поэтому, по правилу 2, растягиваем это измерение, чтобы оно совпало:

- `M.shape -> (2, 3)`
- `a.shape -> (2, 3)`

Теперь формы совпадают. 
Окончательная форма будет `(2, 3)`:

```python jupyter={"outputs_hidden": false}
M + a
```

### Пример транслирования 2

Теперь рассмотрим пример, в котором необходимо транслировать оба массива:

```python jupyter={"outputs_hidden": false}
a = np.arange(3).reshape((3, 1))
b = np.arange(3)
```

Опять же, начнем с записи формы массивов:

- `a.shape = (3, 1)`
- `b.shape = (3,)`

Правило 1 гласит, что нгеобходимо дополнить форму `b`:

- `a.shape -> (3, 1)`
- `b.shape -> (1, 3)`

А правило 2 говорит о том, что необхдимо изменить размер каждого массива, чтобы он соответствовал размеру другого массива:

- `a.shape -> (3, 3)`
- `b.shape -> (3, 3)`

Поскольку результат совпадает, эти формы совместимы. 

```python jupyter={"outputs_hidden": false}
a + b
```

### Пример транслирования 3

Наконец рассмотрим пример, в котором массивы несовместимы:

```python jupyter={"outputs_hidden": false}
M = np.ones((3, 2))
a = np.arange(3)
```

Форма массивов

- `M.shape = (3, 2)`
- `a.shape = (3,)`

По правилу 1 необходимо дополнить форму `a`:

- `M.shape -> (3, 2)`
- `a.shape -> (1, 3)`

По правилу 2 первое измерение `a` растягивается до соответствия первому измерению `M`:

- `M.shape -> (3, 2)`
- `a.shape -> (3, 3)`

Теперь, в соответствии с правилом 3, конечные формы не совпадают, поэтому эти два массива несовместимы, в чем мы можем убедиться, попытавшись выполнить эту операцию:

```python jupyter={"outputs_hidden": false}
%%capture
M + a
```

Обратите внимание на возможную путаницу: если представить, что можно сделать `a` и `M` совместимыми, скажем, дополнив форму `a` единицей справа, а не слева.
Но правила транслирования это запрещают!
Подобная гибкость могла бы быть полезной в некоторых случаях, но она может привести к возникновению областей неоднозначности.
Если требуется применить правостороннее транслирование, можно сделать это явно, изменив форму массива (например, используя ключевое слово `np.newaxis`, рассмотренное в  [Основы работы с массивами NumPy](numpy_02_the_basics_of_numpy_arrays.md)):

```python jupyter={"outputs_hidden": false}
a[:, np.newaxis].shape
```

```python jupyter={"outputs_hidden": false}
M + a[:, np.newaxis]
```

Обратите внимание, что эти правила транслирования применяются к *любой* бинарной универсалоной функции.
Например, вот что произойдет при использовании функции `logaddexp(a, b)`, которая вычисляет `log(exp(a) + exp(b))` с большей точностью, чем стандартном подходе:

```python jupyter={"outputs_hidden": false}
np.logaddexp(M, a[:, np.newaxis])
```

## Транслирование на практике


Рассмотрим несколько простых примеров того, где могут быть полезны операции транслирования.


### Центрирование массива


Универсальные функции избавляют пользователя NumPy от необходимости явно писать медленные циклы Python. 
Транслирование расширяет эту возможность.
Одним из распространенных примеров этого является центрирование массива данных.
Итак, пусть имеется массив из 10 наблюдений, каждое из которых состоит из 3 значений.
Сохраним ити данные в массиве размером $10 \times 3$:

```python jupyter={"outputs_hidden": false}
X = np.random.random((10, 3))
```

Вычислить среднее значение каждого признака можно, используя фурнкцию агрегирования 'np.mean' вдоль оси 0 (вдоль строк):

```python jupyter={"outputs_hidden": false}
Xmean = X.mean(0)
Xmean
```

Теперь можно центрировать массив `X`, вычитая среднее значение из исходного массивава (это операция транслирования):

```python jupyter={"outputs_hidden": false}
X_centered = X - Xmean
```

Для проверки того, что все сделано правильно, можно проверить, что центрированный массив имеет близкое к нулю среднее значение:

```python jupyter={"outputs_hidden": false}
X_centered.mean(axis=0)
```

С точностью до машинного значения среднее значение теперь действительно равно нулю.


### Построение графика тдвумерной функции


Одной из областей, где транслироване очень полезно, является визуализация изображений на основе двумерных функций.
Если нужно определить функцию $z = f(x, y)$, можно использовать транслирование для вычисления функции на всей координатной сетке:

```python jupyter={"outputs_hidden": false}
# x и y содержат по 50 значений от 0 до 5
x = np.linspace(0, 5, 50)
y = np.linspace(0, 5, 50)[:, np.newaxis]

z = np.sin(x) ** 10 + np.cos(10 + y * x) * np.cos(x)
```

Для построения двумерного графика данной функции воспользуемся Matplotlib:

```python jupyter={"outputs_hidden": false}
%matplotlib inline
import matplotlib.pyplot as plt
```

```python jupyter={"outputs_hidden": false}
plt.imshow(z, origin='lower', extent=[0, 5, 0, 5],
           cmap='viridis')
plt.colorbar();
```
