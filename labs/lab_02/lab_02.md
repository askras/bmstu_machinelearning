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

<!-- #region id="7247b8d4" -->
# Лабораторная работа 2. Пакет Matplotlib

Оценка за каждое задание указана в комментариях перед заданием.

Задание выполняется самостоятельно, в противном случае все причастные получат 0 баллов :)
Если вы нашли решение любого из заданий (или его части) в открытом источнике, необходимо указать ссылку на этот источник в отдельном блоке в конце своей работы. 
В противном случае **работа также будет оценена в 0 баллов**.
<!-- #endregion -->

### Задание 1 (1 балл)

Сгенерируйте массив нормально распределенных значений размерности 2 из 100 точек (выберите среднее значение $\mu$ и среднее квадратическое отклонение $\sigma$ по своему выбору). 
Проверьте [правило трех сигм](https://ru.wikipedia.org/wiki/%D0%A1%D1%80%D0%B5%D0%B4%D0%BD%D0%B5%D0%BA%D0%B2%D0%B0%D0%B4%D1%80%D0%B0%D1%82%D0%B8%D1%87%D0%B5%D1%81%D0%BA%D0%BE%D0%B5_%D0%BE%D1%82%D0%BA%D0%BB%D0%BE%D0%BD%D0%B5%D0%BD%D0%B8%D0%B5#%D0%9F%D1%80%D0%B0%D0%B2%D0%B8%D0%BB%D0%BE_%D1%82%D1%80%D1%91%D1%85_%D1%81%D0%B8%D0%B3%D0%BC): нарисуйте окружность с центром в точке $\mu$ с таким радиусом, чтобы на нее приходилось 0,99 всех точек, а также окружность радиусом 3 сигмы. 
Выделите точку $\mu$ отдельным цветом.

```python
# ваш код должен быть тут
```

## Задание 2 (1 балл)

Используйте вспомогательный график, чтобы нарисовать гистограммы с 10 сегментами для каждого измерения и построить график плотности вдоль гистограммы для данных из первого задания.

```python executionInfo={"elapsed": 316, "status": "ok", "timestamp": 1694439806964, "user": {"displayName": "Sergey Korpachev", "userId": "09181340988160569540"}, "user_tz": -180} id="8f1ca529"
# ваш код должен быть тут
```

<!-- #region id="6e8398b0" -->
### Task 3 (2 балла)

Загрузите набор данных ["Ирисы Фишера"](https://ru.wikipedia.org/wiki/Ирисы_Фишера). 
Создайте тепловую карту с корреляциями между объектами, строки и столбцы которой должны быть подписаны названиями объектов. 
Важно использовать matplotlib. 
Прямая корреляция должна отображаться зеленым цветом, обратная &mdash; красным, а отсутствие корреляции &mdash; белым. 
Сделайте график достаточно крупным.

**Подсказка:** используйте `plt.xticks`, `plt.yticks`, `plt.imshow`, `plt.colorbar`

Also build the same heatmap using seaborn.heatmap
<!-- #endregion -->

```python id="b6414734"
# ваш код должен быть тут
```

## Задание 4 (1 балл)

Cоздайте такую же тепловую карту, используя `seaborn.heatmap`

```python executionInfo={"elapsed": 293, "status": "ok", "timestamp": 1694439821851, "user": {"displayName": "Sergey Korpachev", "userId": "09181340988160569540"}, "user_tz": -180} id="47f49efd"
# ваш код должен быть тут
```