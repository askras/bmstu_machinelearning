---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.16.7
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

<!-- #region id="HcF9My_FhLuk" editable=true slideshow={"slide_type": ""} -->
# Лабораторная работа 3. Пакет Pandas
<!-- #endregion -->

<!-- #region id="HcF9My_FhLuk" editable=true slideshow={"slide_type": ""} -->
**Цель работы:** ознакомление с методом аппроксимации функций.

Продолжительность работы: - 4 часа.

Мягкий дедлайн (10 баллов): 27.03.2025

Жесткий дедлайн (5 баллов): 10.04.2025
<!-- #endregion -->

<!-- #region id="HcF9My_FhLuk" editable=true slideshow={"slide_type": ""} -->
Оценка за каждое задание указана в комментариях перед заданием.

Задание выполняется самостоятельно, в противном случае все причастные получат 0 баллов :)
Если вы нашли решение любого из заданий (или его части) в открытом источнике, необходимо указать ссылку на этот источник в отдельном блоке в конце своей работы.
В противном случае **работа также будет оценена в 0 баллов**.
<!-- #endregion -->

```python
score = 0
```

```python id="26zbFY25hLuv" outputId="404c22c6-2696-48ab-c71e-7a3ffbc36b18" editable=true slideshow={"slide_type": ""}
import numpy as np
import pandas as pd
```

<!-- #region id="DJQzjjqJhLuu" editable=true slideshow={"slide_type": ""} -->
Pandas — удобная библиотека для работы с табличными данными в Python, если данных не слишком много и они помещаются в оперативную память вашего компьютера. Несмотря на неэффективность реализации и некоторые проблемы, библиотека стала стандартом в анализе данных.

Основной объект в pandas — это DataFrame, представляющий собой таблицу с именованными колонками различных типов, индексом (может быть многоуровневым). DataFrame можно создавать, считывая таблицу из файла или задавая вручную из других объектов.

В этой части потребуется выполнить несколько небольших заданий. Можно пойти двумя путями: сначала изучить материалы, а потом приступить к заданиям, или же разбираться "по ходу". Выбирайте сами.

Материалы:
1. [Pandas за 10 минут из официального руководства](http://pandas.pydata.org/pandas-docs/stable/10min.html)
2. [Документация](http://pandas.pydata.org/pandas-docs/stable/index.html) (стоит обращаться, если не понятно, как вызывать конкретный метод)
3. [Примеры использования функционала](http://nbviewer.jupyter.org/github/justmarkham/pandas-videos/blob/master/pandas.ipynb)

Многие из заданий можно выполнить несколькими способами. Не существуют единственно верного, но попробуйте максимально задействовать арсенал pandas и ориентируйтесь на простоту и понятность вашего кода. Мы не будем подсказывать, что нужно использовать для решения конкретной задачи, попробуйте находить необходимый функционал сами (название метода чаще всего очевидно). В помощь вам документация, поиск и stackoverflow.
<!-- #endregion -->

<!-- #region id="VFs_6IythLux" editable=true slideshow={"slide_type": ""} -->
## Часть 1. Изучение набора данных

В этой части задания использование циклов запрещается и повлечет за собой снижение оценки. Использование <code>vectorize</code> и  <code>apply</code>, <code>apply_along_axis</code> крайне нежелательно.

Для каждой задачи из этого раздела вы должны написать код для получения ответа, а также дать текстовый ответ, если он предполагается.

На некоторые вопросы вы можете получить путём пристального взгляда на таблицу, но это не будет засчитываться. Вы в любом случае должны получить ответ с помощью кода.
<!-- #endregion -->

<!-- #region id="gDD85M7bhLux" editable=true slideshow={"slide_type": ""} -->
Откройте файл ../data/lab01_train.csv с таблицей (не забудьте про её формат). Выведите последние 10 строк.

Посмотрите на данные и скажите, что они из себя представляют, сколько в таблице строк, какие столбцы? (на это не надо отвечать, просто подумайте об этом)
<!-- #endregion -->

```python id="PKulur3LhLuy" editable=true slideshow={"slide_type": ""}
df = pd.read_csv('./data/lab01_train.csv')
df.head()
```

<!-- #region id="Jhk3tVpVhLuy" editable=true slideshow={"slide_type": ""} -->
Сколько было заявок от студентов второго курса, а сколько третьего курса?
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""}
number_of_request_course_2 = None # Здесь должен быть ваш код
number_of_request_course_3 = None # Здесь должен быть ваш код
```

```python editable=true slideshow={"slide_type": ""}
%%capture

assert number_of_request_course_2  == 138, 'Тест не пройден'
assert number_of_request_course_3 == 223, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="Jhk3tVpVhLuy" editable=true slideshow={"slide_type": ""} -->
Есть ли студенты с равными перцентилями?
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""}
is_equal_percentil = None # Здесь должен быть ваш код
```

```python editable=true slideshow={"slide_type": ""}
%%capture

assert is_equal_percentil == True, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="jPiMwyqHhLuy" editable=true slideshow={"slide_type": ""} -->
Есть ли в данных пропуски? В каких колонках? Сколько их в каждой из этих колонок?
<!-- #endregion -->

```python id="edY1RrSOhLuy" editable=true slideshow={"slide_type": ""}
course_2_na = None # Здесь должен быть ваш код
course_3_na = None # Здесь должен быть ваш код
is_mi_student_na = None # Здесь должен быть ваш код
is_ml_student_na = None # Здесь должен быть ваш код
is_first_time_na = None # Здесь должен быть ваш код
blended_na = None # Здесь должен быть ваш код
percentile_na = None # Здесь должен быть ваш код
```

```python editable=true slideshow={"slide_type": ""}
%%capture

assert course_2_na == 223, 'Тест не пройден'
assert course_3_na == 138, 'Тест не пройден'
assert is_mi_student_na == 343, 'Тест не пройден'
assert is_ml_student_na == 304, 'Тест не пройден'
assert blended_na == 223, 'Тест не пройден'
assert is_first_time_na == 0, 'Тест не пройден'
assert percentile_na == 0, 'Тест не пройден'

print("Выполнено")
score += 0.5
```

<!-- #region id="B5ORhP2uhLuz" editable=true slideshow={"slide_type": ""} -->
Заполните пропуски пустой строкой для строковых колонок и нулём для числовых.
<!-- #endregion -->

```python id="TlF_JJ2ghLuz" editable=true slideshow={"slide_type": ""}
# Здесь должен быть ваш код

course_2_na = None # Здесь должен быть ваш код
course_3_na = None # Здесь должен быть ваш код
is_mi_student_na = None # Здесь должен быть ваш код
is_ml_student_na = None # Здесь должен быть ваш код
is_first_time_na = None # Здесь должен быть ваш код
blended_na = None # Здесь должен быть ваш код

df_na = None # Здесь ваш код подсчитывающий общее количество пропусков в отредактированном датафрейме
```

```python editable=true slideshow={"slide_type": ""}
%%capture

assert df_na == 0, 'Тест не пройден'


print("Выполнено")
score += 0.5
```

<!-- #region id="w5ELhkT1hLuz" editable=true slideshow={"slide_type": ""} -->
Посмотрите повнимательнее на колонку 'is_first_time'. 
Есть ли в ней ответы "Нет"? Сколько их?
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""}
number_is_first_time_responses_no = None # Здесь должен быть ваш код
```

```python editable=true slideshow={"slide_type": ""}
%%capture

assert number_is_first_time_responses_no == 52, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="w5ELhkT1hLuz" editable=true slideshow={"slide_type": ""} -->
Если вы найдете повторные обращения студентов, оставьте только самую позднюю версию. 
<i>Обращения со значением "Нет" в <code>is_first_time</code> могут быть как повторными, так и первичными, поскольку поле заполняли сами студенты.</i>
<!-- #endregion -->

```python editable=true slideshow={"slide_type": ""}
# Здесь должен быть ваш код

# Общее количество оставшихся заявлений
total_number_of_request = None # Здесь должен быть ваш код
```

```python editable=true slideshow={"slide_type": ""}
%%capture

assert total_number_of_request == 347, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="IlzHp3HVhLuz" editable=true slideshow={"slide_type": ""} -->
Какие  blended-курсы для второкурсников существуют? 
<!-- #endregion -->

```python
blended_courses_for_second_year_students = {} # Здесь должен быть ваш код
```

```python
%%capture

assert blended_courses_for_second_year_students == {'DevOps',
                                                   'Введение в дифференциальную геометрию',
                                                   'Соревновательный анализ данных'}, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="IlzHp3HVhLuz" editable=true slideshow={"slide_type": ""} -->
На какой blended-курс записалось наибольшее количество студентов? 
<!-- #endregion -->

```python
blended_course_with_max_request = None # Здесь должен быть ваш код
```

```python
%%capture

assert blended_course_with_max_request == 'DevOps', 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="IlzHp3HVhLuz" editable=true slideshow={"slide_type": ""} -->
На каком из курсов собрались студенты с самым высоким средним рейтингом? 
<!-- #endregion -->

```python
course_with_highest_average_rating = None # Здесь должен быть ваш код
```

```python
%%capture

assert course_with_highest_average_rating == 'Введение в дифференциальную геометрию', 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="IU_FLt8HhLu0" editable=true slideshow={"slide_type": ""} -->
Выясните, есть ли в данных студенты с абсолютно одинаковыми предпочтениями по всем курсам (не забудьте учесть blended-курсы для третьекурсников). 
Сколько таких наборов, которые взяли несколько студентов? 
<!-- #endregion -->

```python
number_duplicate_sets_of_courses = None # Здесь должен быть ваш код
```

```python
%%capture

assert number_duplicate_sets_of_courses == 32, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="IU_FLt8HhLu0" editable=true slideshow={"slide_type": ""} -->
Выведите все дублирующиеся наборы курсов, вместе с количеством выбравших их студентов.

<i>Предпочтения двух студентов считаются абсолютно одинаковыми, если выбранные ими дисциплины имеют одинаковый приоритет.</i>
<!-- #endregion -->

```python
duplicate_sets_of_courses_with_number_students = None # Здесь должен быть ваш код
```

```python
%%capture

duplicate_sets_of_courses_with_number_students_answer = pd.read_csv('./data/duplicate_sets_of_courses_with_number_students.csv').fillna(value={'blended': '',})
assert duplicate_sets_of_courses_with_number_students.equals(duplicate_sets_of_courses_with_number_students_answer), 'Тест не пройден'

print("Выполнено")
score += 0.5
```

```python

```

<!-- #region id="spu3r3vchLu0" -->
Найдите курсы по выбору, на которые записывались как студенты второго, так и студенты третьего курса.
<!-- #endregion -->

```python
courses_students_of_2_and_3_year = {} # Здесь должен быть ваш код
```

```python
%%capture

assert courses_students_of_2_and_3_year == {'Численные методы',
                                            'Statistical Learning Theory',
                                            'Безопасность компьютерных систем',
                                            'Сбор и обработка данных с помощью краудсорсинга',
                                            'Высокопроизводительные вычисления',
                                            'Принятие решений в условиях риска и неопределённости',
                                            'Моделирование временных рядов'}, 'Тест не пройден'
print("Выполнено")
score += 0.5
```

<!-- #region id="PYfX-Dr5hLu0" -->
Методом исключения найдите курсы, которые выбрали только студенты второго курса и только студенты третьего курса.
<!-- #endregion -->

```python
courses_only_2_year = {} # Здесь должен быть ваш код
courses_only_3_year = {} # Здесь должен быть ваш код
```

```python
%%capture

assert courses_only_2_year == {'Анализ неструктурированных данных',
                               'Байесовские методы машинного обучения',
                               'Генеративные модели в машинном обучении',
                               'Глубинное обучение в обработке звука',
                               'Компьютерное зрение',
                               'Конфликты и кооперация',
                               'Методы сжатия и передачи медиаданных',
                               'Обучение с подкреплением',
                               'Проектирование и разработка высоконагруженных сервисов',
                               'Символьные вычисления'}, 'Тест не пройден'

assert courses_only_3_year == {'Анализ данных в бизнесе',
                               'Дискретная оптимизация',
                               'Дополнительные главы прикладной статистики',
                               'Компьютерные сети',
                               'Матричные вычисления',
                               'Машинное обучение 2',
                               'Промышленное программирование на языке Java',
                               'Системы баз данных',
                               'Теория баз данных',
                               'Язык SQL'}, 'Тест не пройден'

print("Выполнено")
score += 0.5
```

<!-- #region id="uL45Tg5fhLu1" -->
## Часть 2. Распределение студентов по курсам
<!-- #endregion -->

<!-- #region id="VNqXUpr4hLu3" -->
Теперь вам нужно распределить студентов по осенним курсам по выбору, учитывая их предпочтения.
<!-- #endregion -->

<!-- #region id="5cAR2FgphLu3" -->
Алгоритм распределения студентов по курсам:
1. По умолчанию на каждой дисциплине по выбору у 2 и 3 курсов может учиться 1 группа (до 30 студентов). Исключения описаны ниже. На blended-дисциплинах для третьекурсников количество мест не ограничено.
2. Проводится первая волна отбора. Для каждой дисциплины формируется список тех, кто указал её первым приоритетом (если студент должен выбрать два курса по выбору, то для него дисциплины, которые он указал первым и вторым приоритетом, рассматриваются как дисциплины первого приоритета). Если желающих больше, чем мест, то выбирается топ по перцентилю рейтинга.
3. На дисциплинах, где остались места после первой волны, формируются списки тех, кто выбрал их вторым приоритетом, и места заполняются лучшими по перцентили рейтинга студентами. После этого проводится такая же процедура для дисциплин третьего приоритета.
4. Если студент не попал на необходимое количество курсов по итогам трёх волн, с ним связывается учебный отдел и решает вопрос в индивидуальном порядке.
<!-- #endregion -->

<!-- #region id="BiNKVwS6hLu3" -->
Обращаем ваше внимание на следующие детали:

- По умолчанию студент выбирает один осенний и один весенний курс по выбору, а также третьекурсники выбирают один blended-курс. Студенты групп третьего курса специализаций МОП и ТИ выбирают по 2 осенних и 2 весенних курса по выбору, также студенты групп второго курса специализации МИ выбирают 2 осенних курса. <i>Для студентов, которые выбирают 2 курса (например, осенних) первый приоритет — <code>fall_1</code> и <code>fall_2</code>, второй приоритет — <code>fall_3</code>. Такие студенты участвуют только в двух волнах отбора</i>.

- Студенты специализации МОП не могут выбрать весенним курсом по выбору Машинное обучение 2. <i>Если студент специализации МОП выбрал Машинное обучение 2, то его приоритеты сдвигаются. Из-за совпадений первого и второго курса по выбору двигать приоритеты не надо</i>.

- Blended-курсы не трогайте, по ним не надо распределять, на другие курсы они никак не влияют.

- Постарайтесь воздержаться от использования циклов там, где это возможно. <i>Допустимо итерироваться по <b>курсам</b>, на которые проводится отбор, и по <b>волнам</b> отбора. Если вы придумаете, как обойтись и без этих циклов, то на усмотрение проверяющего могут быть добавлены бонусные баллы. <b>Дублирование кода не признается успешным избавлением от циклов</b></i>

- На выходе ожидается файл res_fall.csv с результатами распределения на осенние курсы по выбору. Файл должен быть следующего формата:

    * три столбца: ID, course1, course2
    
    * Если студент не попал на курс, но должен был, то вместо названия курса в ячейке должна быть строка "???"
    
    * Если студент должен выбрать только один курс, то в колонке course2 для него должна стоять строка "-"
    
    * Если студент должен выбрать два курса по выбору, то порядок в колонках course1 и course2 не важен.
    
    * Формат csv: для сохранения воспользуйтесь df.to_csv('solution.csv', index=None)
    

Для работы вам могут понадобиться следующие данные:

- Результаты опроса (вы уже использовали этот файл в первой части задания)

- Соответствие номеров групп специализациям:

    * 61, 62 - МОП; 63 - ТИ; 64 — АДИС; 65, 66 — РС; 67 — АПР
    
    * У студентов второго курса номера групп соответствуют номерам до распределения по специализациям.

- Ограничения по количеству мест на курсах по выбору:

    * Осенние: везде 30 мест, кроме Statistical Learning Theory (60 мест), Высокопроизводительных вычислений (60 мест), Анализа неструктурированных данных ($\infty$ мест)

    * Весенние: везде 30 мест, кроме Обучения с подкреплением (60 мест), Анализа данных в бизнесе (60 мест).


Кстати, убедитесь, что в данных больше нет пропусков и повторных записей.
<!-- #endregion -->

<!-- #region id="o5t55IcQhLu4" -->
#### Проверка

Для начала давайте убедимся, что вы успешно выполнили задания первой части и проверим ваши данные на наличие пропусков и повторов:
<!-- #endregion -->

```python id="34HWEdVdhLu4"
%%capture
assert df.shape[0] == 347, 'В таблице остались повторы или потеряны данные'

assert df.isna().sum().sum() == 0, 'В таблице остались пропуски'
```

<!-- #region id="_ImUFyG3hLu4" -->
Если вы не получили AssertionError, то можете продолжать.
<!-- #endregion -->

<!-- #region id="LTGSwwvAhLu4" -->
Создайте новый признак, обозначающий, сколько осенних курсов должен выбрать студент
В этом вам может помочь информация о специализации и группе стундента.
<!-- #endregion -->

<!-- #region id="S47aq1sQhLu4" -->
Проверка:
<!-- #endregion -->

```python id="OlOgzDKDhLu5"
%%capture

col_name = 'fall_course_num'

assert(df[df['id'] == '2662600c2c37e11e62f6ee0b88452f22'][col_name] == 2).all(), 'Тест не пройден'
assert(df[df['id'] == 'd555d2805e1d93d4f023e57dc4c8f403'][col_name] == 1).all(), 'Тест не пройден'
assert(df[df['id'] == '8fe79f84f36e3a5d2d6745621321302c'][col_name] == 1).all(), 'Тест не пройден'
assert(df[df['id'] == 'e4caca755ee0bdd711e18fb8084958b5'][col_name] == 2).all(), 'Тест не пройден'

print("Выполнено")
score += 1.5
```

<!-- #region id="VtQYhwuPhLu5" -->
Распределите студентов в соответствии с первым приоритетом
<!-- #endregion -->

Здесь для проверки приведена таблица, в которой есть 2 дополнительные колонки:
    
    1) is_first_place - является ли студент лучшим по перцентили хотя бы на одном из курсов, куда он был зачислен 
    (True / NaN)
    
    2) is_last_place  - является ли студент худшим по перцентили хотя бы на одном из курсов, куда он был зачислен (True / NaN)

```python id="uZTkGEq5hLu5"
check_df = pd.read_csv('./data/lab01_train_check.csv')
```

<!-- #region id="E_fbyB9qhLu5" -->
После распределения студентов в соответствии с первым приоритетом добавьте в свой датафрейм аналогичные признаки и запустите проверку:
<!-- #endregion -->

<!-- #region id="RO6DwNNFhLu6" -->
Проведите все три волны отбора студентов на курсы по выбору
Результат сохраните в файл res_fall.csv.
<!-- #endregion -->

```python id="jn2OOrLjhLu6"
# Здесь должен быть ваш код
```

```python id="GOAzQeYehLu6"
%%capture
assert((df[df['is_first_place'].isna() == False][['id']].sort_values('id').reset_index(drop=True)
        ==
        check_df[check_df['is_first_place'].isna() == False][['id']].sort_values('id').reset_index(drop=True)
       ).id.values).all()


assert((df[df['is_last_place'].isna() == False][['id']].sort_values('id').reset_index(drop=True)
       == 
       check_df[check_df['is_last_place'].isna() == False][['id']].sort_values('id').reset_index(drop=True)
      ).id.values).all()

print("Выполнено")
score += 2
```

<!-- #region id="k8d711qGhLu7" -->
**Дополнительное задание. [2 бонусных балла] Распределите таким же образом студентов еще и на весенние курсы по выбору.**

Если ваш код был хорошо структурирован, то это не составит проблем. 

Если вы выполнили это задание, сдайте среди прочего файл res_spring.csv в таком же формате, как и res_fall.csv.
<!-- #endregion -->

```python id="hwmusOGThLu7"
# Здесь должен быть ваш код
```

```python
# общий балл
score
```
