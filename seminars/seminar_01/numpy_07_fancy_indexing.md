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

# Списочная индексация


В предыдущих разделах было показано как получать доступ к частям массивов и изменять их с помощью простых индексов (например, `arr[0]`)), срезов (например, `arr[5:10]`) и булевых масок (например, `arr[arr < 0]`).
В этом разделе рассмотрим другой стиль индексации массивов, известный как *списочная индексация* (*fancy indexing*).
Списочная индексация похож на простую индексацию, с той лишь разницей, что вместо отдельных скаляров, в качестве индексов, передаются массивы индексов.
Это позволяет очень быстро получать доступ к сложным подмножествам значений массива и, при необходимости, изменять их.


## Особенности списочной индексации

Списочная индексация концептуально проста: она заключается в передачу массива индексов с целью одновременного доступа к нескольким элементам массива.
В качестве примера рассмотрим следующий массив:

```python jupyter={"outputs_hidden": false}
import numpy as np
rand = np.random.RandomState(42)

x = rand.randint(100, size=10)
print(x)
```

Если нужно получить доступ к трем разным элементам, то это можно сделать так:

```python jupyter={"outputs_hidden": false}
[x[2], x[5], x[4]]
```

Но еще проще получить тот же результат, просто используя единый список индексов:

```python jupyter={"outputs_hidden": false}
indices = [2, 5, 4]
x[indices]
```

При использовании списочной индексации форма результата отражает форму *массива индексов* (*штвуч фккфны*), а не форму *индексируемого массива*:

```python jupyter={"outputs_hidden": false}
indices = np.array([[3, 7],
                [4, 5]])
x[indices]
```

Списочная индексация позволяет работать и с многомерными массивами. 
Рассмотрим следующий массив:

```python jupyter={"outputs_hidden": false}
X = np.arange(12).reshape((3, 4))
X
```

При использовании списочной индексации, как и в случае стандартной, первый индекс относится к строке, а второй &mdash; к столбцу:

```python jupyter={"outputs_hidden": false}
row = np.array([0, 1, 2])
col = np.array([2, 1, 3])
X[row, col]
```

Обратите внимание, что первое значение в результате &mdash; `X[0, 2]`, второе &mdash; `X[1, 1]`, а третье &mdash; `X[2, 3]`.
Составление пар индексов при списочной индексации следует всем правилам транслирования, упомянутым в [Операции над массивами: транслирование](numpy_05_computation_on_arrays_broadcasting).
Так, например, при объединении индексных вектор-столбца и вектор-строки получится двумерный результат:

```python jupyter={"outputs_hidden": false}
X[row[:, np.newaxis], col]
```

Здесь каждое значение строки сопоставляется с каждым вектором столбца, точно так же, как при транслировании арифметических операций.
Например:

```python jupyter={"outputs_hidden": false}
row[:, np.newaxis] * col
```

При использовании списочной индексации всегда важно помнить, что возвращаемое значение отражает *транслируемую форму индексов*, а не форму индексируемого массива.


## Комбинированная индексация

Для реализации еще более сложных операций списочную индексацию можно комбинировать с другими схемами индексации:

```python jupyter={"outputs_hidden": false}
print(X)
```

Можно комбинировать списочные и простые индексы:

```python jupyter={"outputs_hidden": false}
X[2, [2, 0, 1]]
```

или списочные индексы и срезы:

```python jupyter={"outputs_hidden": false}
X[1:, [2, 0, 1]]
```

и даже совместить списочную индексацию с маскированием:

```python jupyter={"outputs_hidden": false}
mask = np.array([1, 0, 1, 0], dtype=bool)
X[row[:, np.newaxis], mask]
```

Все эти варианты индексации в совокупности приводят к очень гибкому набору операций для доступа к значениям массива и их изменению.


## Пример: Выборка случайных точек

Одним из распространенных вариантов использования списочной индексации является выбор подмножеств строк из матрицы.
Например, пусть имеется матрица размером $N$ на $D$, представляющая $N$ точек в $D$ измерениях, например, следующие точки, взятые из двумерного нормального распределения:

```python jupyter={"outputs_hidden": false}
mean = [0, 0]
cov = [[1, 2],
       [2, 5]]
X = rand.multivariate_normal(mean, cov, 100)
X.shape
```

Используя Matplotlib визуализируем эти точки в виде диаграммы рассеяния:

```python jupyter={"outputs_hidden": false}
%matplotlib inline
import matplotlib.pyplot as plt

plt.scatter(X[:, 0], X[:, 1]);
```

Используя списочную индексацию, выбереи 20 случайных точек:

```python jupyter={"outputs_hidden": false}
indices = np.random.choice(X.shape[0], 20, replace=False)
indices
```

```python jupyter={"outputs_hidden": false}
selection = X[indices]
selection.shape
```

Теперь, чтобы увидеть, какие точки были выбраны, нарисуем большие красные круги в местах расположения выбранных точек:

```python jupyter={"outputs_hidden": false}
plt.scatter(X[:, 0], X[:, 1], alpha=0.3)
plt.scatter(selection[:, 0], selection[:, 1],
            facecolor='none', s=200, color='red');
```

Такого рода стратегия часто используется для быстрого разделения наборов данных, что часто требуется при разделении на обучающие и тестовые выборки для проверки моделей машиного обучения.


## Изменение значений элементов с помощью списочной индексации

Списочную индексацию можно использовать не только для доспупа к элементам массива, но и для изменения их.
Например, если есть массив индексов, и нужно присвоить соответствующим элементам массива некоторое значение, то можно поступить так:

```python jupyter={"outputs_hidden": false}
x = np.arange(10)
indices = np.array([2, 1, 7, 5])
x[indices] = 99
print(x)
```

Вообще, можно использовать любой оператор типа присваивания. 
Например:

```python jupyter={"outputs_hidden": false}
x[indices] -= 10
print(x)
```

Однако, следует помнить, что при наличии повторяющихся индексов, в таких случаях можно получить неожиданные результатам. 
Рассмотрим пример:

```python jupyter={"outputs_hidden": false}
x = np.zeros(10)
x[[0, 0]] = [4, 6]
print(x)
```

Куда делась 4? 
Рассмотрим эту операцию подробнее.
Вначале выполняется присвоение `x[0] = 4`, а затем `x[0] = 6`.
В результате чего `x[0]` конечно же содержит значение 6.
Это вполне очевидно.

Но теперь рассмотрим следующий случай:

```python jupyter={"outputs_hidden": false}
indices = [2, 3, 3, 4, 4, 4]
x[indices] += 1
x
```

Можно было бы ожидать, что `x[3]` в результате будет содержать значение 2, а `x[4]` будет содержать значение 3, поскольку именно столько раз повторяется каждый индекс. 
Почему это не так?
Концептуально это происходит потому, что `x[i] += 1` является сокращением от `x[i] = x[i] + 1`. 
То есть вычисляется значение `x[i] + 1` , а затем результат присваивается по соответствующим индексам элементам в массиве `x`.
Получается, что это не выполняемый несколько раз инкеремент, а присваивание, приводящее к интуитивно не очевидным результатам.


Если же необходимо другое поведение, когда операция повторяется? 
Тогдп можно использовать метод `at()` соответсвующей универсальной функции и сделать следующее:

```python jupyter={"outputs_hidden": false}
x = np.zeros(10)
np.add.at(x, indices, 1)
print(x)
```

Метод `at()` применяет соответствующий оператор к элеметам с заданными индексами (здесь `indeces`) с использованием заданного значения (здесь 1).
Другим похожим методом, является метод универсальных функций `reduceat()`, о котором можно узнать из документации NumPy.


## Пример: Группировка данных

Рассмотрим как можно использовать эти идеи для эффективной группировки данных и создания гистограммы вручную.
Например, пусть имеется 1000 значений и необходимо быстро выяснить, как они распределены по массиву интервалов.
Это можно сделать с помощью универсальных функций следующим образом:

```python jupyter={"outputs_hidden": false}
np.random.seed(42)
x = np.random.randn(100)

# Построение гисторгаммы вручную
bins = np.linspace(-5, 5, 20)
counts = np.zeros_like(bins)

# Нахождение подходящего интервала для каждого x
i = np.searchsorted(bins, x)

# Добавление 1 к каждому из этих интервалов
np.add.at(counts, i, 1)
counts.shape, bins.shape
```

Теперь значения элементов массива `counts` содержат количество точек в каждом интервале &mdash; другими словами, гистограмму:

```python jupyter={"outputs_hidden": false}
plt.step(bins, counts);
```

Конечно, было бы нерационально поступать так каждый раз, когда необходимо построить гистограмму.
Вот почему Matplotlib предоставляет процедуру `plt.hist()`, которая делает то же самое одной строкой кода:

```python
plt.hist(x, bins, histtype='step');
```

Для расчета разбиения на интервалы `matplotlib` использует функцию `np.histogram`, выполняющую вычисления, очень похожие на те &laquo;ручные&raquo;, которые были представлены выше. 
Давайте сравним их здесь:

```python jupyter={"outputs_hidden": false}
print("Реализация с помощью NumPy:")
%timeit counts, edges = np.histogram(x, bins)

print("Ручная реализация:")
%timeit np.add.at(counts, np.searchsorted(bins, x), 1)
```

Оказывается, реализованный здесь однострочный алгоритм в несколько раз быстрее оптимизированного алгоритма в NumPy! 
Как такое может быть?
Если покопаться в исходном коде `np.histogram` (это можно сделать это в IPython, набрав `np.histogram??`), можно увидеть, что он намного сложнее, чем простой поиск и подсчет, который приведен здесь.
Дело в том, что алгоритм NumPy более гибкий и, в частности, разработан с ориентацией на большую производительность при значительном увеличении количесва точек:

```python jupyter={"outputs_hidden": false}
x = np.random.randn(1000000)
print("Реализация с помощью NumPy:")
%timeit counts, edges = np.histogram(x, bins)

print("Ручная реализация:")
%timeit np.add.at(counts, np.searchsorted(bins, x), 1)
```

Это сравнение показывает, что алгоритмическая эффективность почти никогда не является простым вопросом. 
Алгоритм, эффективный для больших наборов данных, не всегда будет лучшим выбором для маленьких наборов данных, и наоборот.
Ключом к эффективному использованию Python в приложениях ориентированных на обработку больших объемов данных, заключается в умении применять реализованные общие процедуры, такие как `np.histogram`, и знании сферы их использования.
Кроме того, нужно понимать когда стоит реализовывать некоторую функциональность самостоятельно.
