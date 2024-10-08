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

# Настройки легенд на графиках


Правильно сформированная легенды (подписи меток для различных элементов) графика могут сделать график более понятным для восприятия.
Рассмотрим настройку размещения и внешнего вида легенды в Matplotlib.

Простейшую легенду можно создать с помощью команды `plt.legend()`, которая автоматически создает легенду для любых помеченных элементов графика:

```python
import matplotlib.pyplot as plt
import numpy as np

%matplotlib inline
plt.style.use('default')
```

```python jupyter={"outputs_hidden": false}
x = np.linspace(0, 10, 1000)
fig, ax = plt.subplots()
ax.plot(x, np.sin(x), '-b', label=r'$\sin(x)$')
ax.plot(x, np.cos(x), '--r', label=r'$\cos(x)$')
ax.axis('equal')
leg = ax.legend();
```

Существует множество способов настроить легенду.
Например, можно указать местоположение и отключить рамку:

```python jupyter={"outputs_hidden": false}
ax.legend(loc='upper left', frameon=False)
fig
```

Параметр `ncol` исползуется для указания количества столбцов в легенде:

```python jupyter={"outputs_hidden": false}
ax.legend(frameon=False, loc='lower center', ncol=2)
fig
```

Можно ииспользовать скругленный блок (`fancybox`) или добавить тень, изменить прозрачность (альфа-значение) рамки или изменить отступ вокруг текста:

```python jupyter={"outputs_hidden": false}
ax.legend(fancybox=True, framealpha=1, shadow=True, borderpad=1)
fig
```

Дополнительную информацию о доступных параметрах легенды смотрите в документации `plt.legend`.


## Выбор элементов для легенды

Легенда по умолчанию включает в себя все маркированные элементы.
Если это не то, что требуется сделать, можно точно настроить, какие элементы и метки будут отображаться в легенде, используя объекты, возвращаемые командами построения графиков.
Команда `plt.plot()` способна создавать несколько линий одновременно и возвращает список созданных экземпляров линий.
Для указания, какие элементы использовать, достаточно передать какие-либо из них функции `plt.legend()` вместе с задаваемыми метками:

```python jupyter={"outputs_hidden": false}
y = np.sin(x[:, np.newaxis] + np.pi * np.arange(0, 2, 0.5))
lines = plt.plot(x, y)

# lines — это список экземпляров plt.Line2D
plt.legend(lines[:2], ['первый', 'второй']);
```

На практике обычно понятнее использовать первый метод, применяя метки к элементам графика, которые необходимо отобразить в легенде:

```python jupyter={"outputs_hidden": false}
plt.plot(x, y[:, 0], label='первый')
plt.plot(x, y[:, 1], label='второй')
plt.plot(x, y[:, 2:])
plt.legend(framealpha=1, frameon=True);
```

Обратите внимание, что по умолчанию легенда игнорирует все элементы, для которых не установлен атрибут `label`.


## Легенда для размера точек

Иногда значения легенды по умолчанию недостаточны для данного графика.
Например, в том случае если размер точек используется для обозначения определенных характеристик данных, хотелось бы чтобы легенда отражала это.
Вот пример, в котором будет использоваться размер точек для обозначения численности населения крупнейших городов Московской области.
Для этого потребуется легенда, которая определяет масштаб размеров точек.
Добиться этого можно, нанеся на график некоторые маркированные данные без записей:

```python jupyter={"outputs_hidden": false}
import pandas as pd
cities = pd.read_csv('../../datasets/moscow_cities.csv')

# Извлечение необходимых данных
lat, lon = cities['Широта'], cities['Долгота']
population, area = cities['Население'], cities['Площадь']

# Распределите точки, используя размер и цвет, но без подписи.
plt.scatter(lon, lat, label=None,
            c=np.log10(population), cmap='viridis',
            s=area, linewidth=0, alpha=0.5)
#plt.axis(aspect='equal')
plt.xlabel('Долгота')
plt.ylabel('Широта')
plt.colorbar(label='log$_{10}$(Население)')
plt.clim(3, 7)

# Создаем легенду:
for area in [100, 300, 500]:
    plt.scatter([], [], c='k', alpha=0.3, s=area,
                label=str(area) + ' km$^2$')
plt.legend(scatterpoints=1, frameon=False, labelspacing=1, title='Площадь городов')

plt.title('Крупнейшие города Подмосковья: Численность населения и площадь');
```

Легенда всегда будет ссылаться на какой-либо объект, находящийся на графике, поэтому, если необходимо отобразить объект конкретного вида, необходимо сначала его нарисовать на графике. 
В этом случае нужные объекты (серые кружки) отсутствуют на графике, поэтому они были симитированы, отображая пустые списки.
Обратите внимание, что в легенде перечислены только те элементы графика, для которых указана метка.

Посредством вывода на график пустых списков были созданы маркированные объекты, которые затем собираются в легенде. Теперь легенда дает отражает всю необходимую информацию.
Эта стратегия может быть полезна для создания более сложных визуализаций.

Наконец, обратите внимание, что для таких географических данных было бы удобнее, если можно было бы показать границы городов или другие элементы, характерные географических объектов.
Для этого отличным выбором инструмента станет дополнительный набор инструментов Basemap из Matplotlib, который будет рассмотрен в [Отображение географических данных с помощью Basemap](matplotlib_13_geographic_data_with_basemap.md).


## Несколько легенд

Иногда при проектировании графика возникает необходимость добавить несколько легенд к одним и тем же осям.
К сожалению, Matplotlib не поддреживает такое решение напрямую.
Стандартным способом можно создать только одну легенду для графика.
При попытке создать вторую легенду с помощью `plt.legend()` или `ax.legend()`, она просто перезапишет первую.
Решить эту проблему можно, создав изначально для легенды новый отрисовщик (artist), после чего добавить вручную второй отрисовщик на график с помощью низкоуровневого метода `ax.add_artist()`

```python jupyter={"outputs_hidden": false}
fig, ax = plt.subplots()

lines = []
styles = ['-', '--', '-.', ':']
x = np.linspace(0, 10, 1000)

for i in range(4):
    lines += ax.plot(x, np.sin(x - i * np.pi / 2),
                     styles[i], color='black')
ax.axis('equal')

# Задаем линии и метки первой легенды
ax.legend(lines[:2], ['line A', 'line B'],
          loc='upper right', frameon=False)

# Создаем вторую легенду и добавляем отрисовщика вручную
from matplotlib.legend import Legend
leg = Legend(ax, lines[2:], ['line C', 'line D'],
             loc='lower right', frameon=False)
ax.add_artist(leg);
```
