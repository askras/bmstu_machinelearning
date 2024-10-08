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

# Основы работы с массивами NumPy


Работа с данными в Python практически всегда сводится к манипулированию массивами NumPy: даже более новые инструменты, такие как Pandas, построены на основе массива NumPy.
В этом разделе будут представлены основые приемы работы с массивами NumPy для доступа к данным и подмассивам, а также для разделения, изменения формы и объединения массивов.
Хотя типы операций, показанные здесь, могут показаться немного сухими и педантичными, они являются основой многих других примеров, используемых в практике обработки данных.
Познакомьтесь с ними поближе!

Итак, рассмотрим несколько категорий основных манипуляций с массивами:

- *атрибуты массивов*: Определение размера, формы, потребления памяти и типов данных массивов;
- *индексация массивов*: Получение и задание значений отдельных элементов массива;
- *разделение массивов*: получение и задание значение меньших подмассивов внутри большего массива;
- *изменение формы массивов*: изменение формы заданного массива;
- *объединение и разделение массивов*: объединение нескольких массивов в один и разделение одного массива на несколько.


## Атрибуты массивов NumPy


Вначала обсудим некоторые полезные атрибуты массива.
Начнем с определения трех случайных массивов: одномерного, двумерного и трехмерного.
Для этого воспользуемся генератором (псевдо)случайных чисел NumPy, задав для него начальное значение, чтобы гарантировать генерацию одних и тех же случайных массивов при каждом запуске:

```python
import numpy as np
np.random.seed(42)  # начальное значение для целей воспроизводимости

x1 = np.random.randint(10, size=6)  # одномерный массив
x2 = np.random.randint(10, size=(3, 4))  # двумерный массив
x3 = np.random.randint(10, size=(3, 4, 5))  # трёхмерный массив
```

Каждый массив имеет атрибуты `ndim` (количество измерений), `shape` (размер каждого измерения) и `size` (общий размер массива):

```python
print("x3 ndim: ", x3.ndim)
print("x3 shape:", x3.shape)
print("x3 size: ", x3.size)
```

Еще один полезный атрибут &mdash; `dtype`, тип данных массива (который был описан ранее в [Типы данных в Python](numpy_00_introduction_to_numpy.md)):

```python
print("dtype:", x3.dtype)
```

Другие атрибуты включают `itemsize`, который указывает размер (в байтах) каждого элемента массива, и `nbytes`, который указывает общий размер (в байтах) массива:

```python jupyter={"outputs_hidden": false}
print("itemsize:", x3.itemsize, "bytes")
print("nbytes:", x3.nbytes, "bytes")
```

В общем случае значение атрубута `nbytes` должно быть равно `itemsize`, умноженному на `size`.


## Индексация массива: доступ к отдельным элементам


Индексация массива NumPy практически идентична стандартной индексации списков Python.
В одномерном массиве доступ к значению $i$-го (начиная с нуля) элемента можно получить, указав нужный индекс в квадратных скобках, как и в списках Python:

```python jupyter={"outputs_hidden": false}
x1
```

```python jupyter={"outputs_hidden": false}
x1[0]
```

```python jupyter={"outputs_hidden": false}
x1[4]
```

Для индексации с конца массива можно использовать отрицательные индексы:

```python jupyter={"outputs_hidden": false}
x1[-1]
```

```python jupyter={"outputs_hidden": false}
x1[-2]
```

В многомерном массиве доступ к элементам можно получить с помощью кортежа индексов, разделенных запятыми. При этом первый индекс уакзывает номер строки, второй &mdash; номер столбца:

```python jupyter={"outputs_hidden": false}
x2[0, 0]
```

```python jupyter={"outputs_hidden": false}
x2[2, 0]
```

```python jupyter={"outputs_hidden": false}
x2[2, -1]
```

Значения также можно изменять с помощью любого из приведенных выше индексных обозначений:

```python jupyter={"outputs_hidden": false}
x2[0, 0] = 12
x2
```

Следует помнить, что в отличие от списков Python, массивы NumPy имеют фиксированный тип.
Это означает, например, то, что при попытки вставить вещественное число значение в целочисленный массив его значение будет обрезано без каких бы то ни было сообщений. Не будьте застигнуты врасплох таким поведением!

```python jupyter={"outputs_hidden": false}
x1[0] = 3.14159  # это значение будет усечено!
x1
```

## Срезы массивов: доступ к подмассивам


Аналогично доступу к отдельным элементам массива, можно использовать квадратные скобки для доступа к подмассивам с помощью *срезов* (*slice*), обозначаемой символом двоеточия (`:`).
Синтаксис срезов NumPy соответствует синтаксису стандартного списка Python; чтобы получить доступ к срезу массива `x`, используйте это:

    x[start:stop:step]

Если какие-либо из этих значений не указаны, по умолчанию используются значения `start=0`, `stop=размер соответсвующего измерения массива`, `step=1`.
Рассмотрим доступ к подмассивам в одном и в нескольких измерениях.


### Одномерные подмассивы

```python jupyter={"outputs_hidden": false}
x = np.arange(10)
x
```

```python jupyter={"outputs_hidden": false}
x[:5]  # первые пять элементов
```

```python jupyter={"outputs_hidden": false}
x[5:]  # элементы после индекса 5
```

```python jupyter={"outputs_hidden": false}
x[4:7]  # средний подмассив
```

```python jupyter={"outputs_hidden": false}
x[::2]  # каждый второй элемент
```

```python jupyter={"outputs_hidden": false}
x[1::2]  # каждый второй элемент, начиная с индекса 1
```

Потенциально к путанице может привести случай когда значение `step` отрицательное.
В этом случае значения по умолчанию для `start` и `stop` меняются местами.
Это становится удобным способом перевернуть массив:

```python jupyter={"outputs_hidden": false}
x[::-1]  # все элементы в обратном порядке
```

```python jupyter={"outputs_hidden": false}
x[5::-2]  # каждый второй элемент в обратном порядуе, начиная с индекса 5
```

### Многомерные подмассивы

Многомерные срезы работают таким же образом, при этом несколько срезов разделяются запятыми.
Например:

```python jupyter={"outputs_hidden": false}
x2
```

```python jupyter={"outputs_hidden": false}
x2[:2, :3]  # две строки, три столбца
```

```python jupyter={"outputs_hidden": false}
x2[:3, ::2]  # все строки, каждый второй столбец
```

Наконец, измерения подмассива можно даже поменять местами:

```python jupyter={"outputs_hidden": false}
x2[::-1, ::-1]
```

#### Доступ к строкам и столбцам массива

Одной из часто используемых процедур является доступ к отдельным строкам или столбцам массива.
Это можно сделать, объединив индексацию и срез, используя пустой срез, отмеченный одним двоеточием (`:`):

```python jupyter={"outputs_hidden": false}
print(x2[:, 0])  # первый столбец x2
```

```python jupyter={"outputs_hidden": false}
print(x2[0, :])  # первая строка x2
```

В случае доступа к строке пустой срез можно опустить для более компактного синтаксиса:

```python jupyter={"outputs_hidden": false}
print(x2[0])  # эквивалентно x2[0, :]
```

### Подмассивы как представления без копирования

Одна важная и чрезвычайно полезная вещь, которую следует знать о срезах массива, заключается в том, что они возвращают *представления*, а не *копии* данных массива.
Это одна из особенностей, в которой срез массива NumPy отличается от среза списка Python: в списках срезы будут копиями.
Рассмотрим наш двумерный массив, полученный ранее:

```python jupyter={"outputs_hidden": false}
print(x2)
```

Давайте извлечем из этого подмассив размером $2 \times 2$:

```python jupyter={"outputs_hidden": false}
x2_sub = x2[:2, :2]
print(x2_sub)
```

Теперь, если мы изменим этот подмассив, мы увидим, что исходный массив изменился! Обратите внимание:

```python jupyter={"outputs_hidden": false}
x2_sub[0, 0] = 99
print(x2_sub)
```

```python jupyter={"outputs_hidden": false}
print(x2)
```

Такое поведение по умолчанию на самом деле весьма полезно: оно означает, что при работе с большими наборами данных мы можем получать доступ к частям этих наборов данных и обрабатывать их без необходимости копировать базовый буфер данных.


### Создание копий массивов

Несмотря на все замечательные особенности представлений массивов, иногда бывает полезно явно копировать данные из массива или подмассива. 
Это проще всего сделать с помощью метода `copy()`:

```python jupyter={"outputs_hidden": false}
x2_sub_copy = x2[:2, :2].copy()
print(x2_sub_copy)
```

Если теперь мы изменим этот подмассив, исходный массив не будет затронут:

```python jupyter={"outputs_hidden": false}
x2_sub_copy[0, 0] = 42
print(x2_sub_copy)
```

```python jupyter={"outputs_hidden": false}
print(x2)
```

## Изменение формы массивов

Еще один полезный тип операций &mdash; изменение формы массивов.
Наиболее гибкий способ сделать это &mdash; использовать метод `reshape`.
Например, если необходимо разместить числа от 1 до 9 в сетке размером $3 \times 3$, можно сделать следующее:

```python jupyter={"outputs_hidden": false}
grid = np.arange(1, 10).reshape((3, 3))
print(grid)
```

Обратите внимание: чтобы это сработало, размер исходного массива должен совпадать с размером преобразованного массива.
Там, где это возможно, метод `reshape` будет использовать представление исходного массива без копирования, но в случае отсутсвия смежных буферов памяти это не всегда удается.

Другим распространенным шаблоном преобразования является преобразование одномерного массива в двумерную матрицу-строку или матрицу-столбец.
Это можно сделать с помощью метода `reshape` или, что еще проще, воспользовавшись ключевым словом `newaxis` в операции среза:

```python jupyter={"outputs_hidden": false}
x = np.array([1, 2, 3])
```

```python jupyter={"outputs_hidden": false}
# получение вектора строки с помощью изменения формы
x.reshape((1, 3))
```

```python jupyter={"outputs_hidden": false}
# получение вектора строки с помощью newaxis
x[np.newaxis, :]
```

```python jupyter={"outputs_hidden": false}
# получение вектора столбца с помощью изменения формы
x.reshape((3, 1))
```

```python jupyter={"outputs_hidden": false}
# получение вектора столбца с помощью newaxis
x[:, np.newaxis]
```

Такие преобразования довольно часто приходится делать на практике


## Объединение и разбиение массивов

Все предыдущие операции работали с отдельными массивами. 
Но можно объединить несколько массивов в один, или наоборот разделить один массив на несколько массивов. 


### Объединение массивов

Конкатенация, или объединение двух массивов в NumPy, выполняется с помощью процедур `np.concatenate`, `np.vstack` и `np.hstack`.

`np.concatenate` принимает  в качестве первого аргумента кортеж или список массивов:

```python jupyter={"outputs_hidden": false}
x = np.array([1, 2, 3])
y = np.array([3, 2, 1])
np.concatenate([x, y])
```

Можно объединять более двух массивов одновременно:

```python jupyter={"outputs_hidden": false}
z = [99, 99, 99]
np.concatenate([x, y, z])
```

Объединять можно даже двумерные массивы:

```python jupyter={"outputs_hidden": false}
grid = np.array([[1, 2, 3],
                 [4, 5, 6]])
```

```python jupyter={"outputs_hidden": false}
# объединение вдоль стоблцов по *строкам* (нулевая ось)
np.concatenate([grid, grid], axis=0)
```

```python
# объединение вдоль строк по *столбцам* (первая ось)
np.concatenate([grid, grid], axis=1) 
```

```python jupyter={"outputs_hidden": false}
# по умолчанию axis=0
np.concatenate([grid, grid])
```

Способ указания оси может оказаться запутанным для пользователей, использующих другие языки.
Ключевое слово `axis` указывает *ось массива, вдоль которой будет проводиться объединение*.
Таким образом, указание `axis=0` означает, что объединение массивов будет проводиться вдоль оси с индексом 0 (строки).
Аналогично, при указание `axis=1` объединение будет проводиться вдоль оси с индексом 1 (столбцы). 


![Индуксация массивов](./img/axis.png)


Для работы с массивами с различающимися размерностями может быть более понятным использование функций `np.vstack` (вертикальное объединение) и `np.hstack` (горизонтальное объединение):

```python jupyter={"outputs_hidden": false}
x = np.array([1, 2, 3])
grid = np.array([[9, 8, 7],
                 [6, 5, 4]])

# вертикальное объединение массивов
np.vstack([x, grid])
```

```python jupyter={"outputs_hidden": false}
# горизонтальное объединение массивов
y = np.array([[99],
              [99]])
np.hstack([grid, y])
```

Аналогично, `np.dstack` будет объединять массивы вдоль третьей оси.


### Разбиение массивов

Противоположностью конкатенации является разбиение, реализуемое функциями `np.split`, `np.hsplit` и `np.vsplit`. 
Каждой из них в качестве второго аргумента необходимо передать список индексов, задающих точки разделения:

```python jupyter={"outputs_hidden": false}
x = [1, 2, 3, 99, 99, 3, 2, 1]
x1, x2, x3 = np.split(x, [3, 5])
print(x1, x2, x3)
```

Обратите внимание, что $N$ точек разделения приводят к $N + 1$ подмассивам.


Связанные функции `np.hsplit` и `np.vsplit` аналогичны:

```python jupyter={"outputs_hidden": false}
grid = np.arange(16).reshape((4, 4))
grid
```

```python jupyter={"outputs_hidden": false}
upper, lower = np.vsplit(grid, [2])
print(upper)
print(lower)
```

```python jupyter={"outputs_hidden": false}
left, right = np.hsplit(grid, [2])
print(left)
print(right)
```

Аналогично `np.dsplit` разделит массивы по третьей оси.
