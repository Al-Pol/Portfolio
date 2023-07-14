# **Музыка больших городов**

<font size = 4> **Описание проекта**
 
Заказчик просит провести сравнение музыкальных предпочтений у пользователей Яндекс.Музыки, проживающих в Москве и Санкт-Петербурге.

<font size = 4> **Цель проекта**
 
Нужно проверить три гипотезы:
1. Активность пользователей зависит от дня недели. Причём в Москве и Петербурге это проявляется по-разному.
2. В понедельник утром в Москве преобладают одни жанры, а в Петербурге — другие. Так же и вечером пятницы преобладают разные жанры — в зависимости от города. 
3. Москва и Петербург предпочитают разные жанры музыки. В Москве чаще слушают поп-музыку, в Петербурге — русский рэп.

<font size = 4> **План проекта**

1. [Изучение данных](#Изучение-данных)  
   1.1 [Импортируем библиотеки](#Импортируем-библиотеки)  
   1.2 [Считываем данные из CSV-файла в датафрейм и сохраняем в переменные](#Считываем-данные-из-CSV-файла-в-датафрейм-и-сохраняем-в-переменные)   
   1.3 [Выводим основную информацию о датафреймах методом info](#Выводим-основную-информацию-о-датафреймах-методом-info)  
2. [Предобработка данных](#Предобработка-данных)  
   2.1 [Преведём заголовки к одному стилю](#Преведём-заголовки-к-одному-стилю)  
   2.2 [Заполним пропущенные значения](#Заполним-пропущенные-значения)  
   2.3 [Найдем и обработаем дубликаты в данных](#Найдем-и-обработаем-дубликаты-в-данных)  
      2.3.1 [Обработаем явные дубликаты](#Обработаем-явные-дубликаты)  
      2.3.2 [Обработаем неявные дубликаты](#Обработаем-неявные-дубликаты)  
3. [Проверка гипотез](#Проверка-гипотез)  
   3.1 [Сравнение поведения пользователей двух столиц](#Сравнение-поведения-пользователей-двух-столиц)  
   3.2 [Музыка в начале и в конце недели](#Музыка-в-начале-и-в-конце-недели)  
   3.3 [Жанровые предпочтения в Москве и Петербурге](#Жанровые-предпочтения-в-Москве-и-Петербурге)  
4. [Итоговый вывод](#Итоговый-вывод)  

## Изучение данных

### Импортируем библиотеки


```python
# импорт библиотеки pandas
import pandas as pd 
```

### Считываем данные из CSV-файла в датафрейм и сохраняем в переменные


```python
# чтение файла с данными и сохранение в df
try: 
    df = pd.read_csv('d:/Data_science/Projects_jupiter/data/yandex_music_project.csv')
except:
    df = pd.read_csv('/datasets/yandex_music_project.csv')
```


```python
# получение первых 10 строк таблицы df
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>userID</th>
      <th>Track</th>
      <th>artist</th>
      <th>genre</th>
      <th>City</th>
      <th>time</th>
      <th>Day</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>FFB692EC</td>
      <td>Kamigata To Boots</td>
      <td>The Mass Missile</td>
      <td>rock</td>
      <td>Saint-Petersburg</td>
      <td>20:28:33</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>1</th>
      <td>55204538</td>
      <td>Delayed Because of Accident</td>
      <td>Andreas Rönnberg</td>
      <td>rock</td>
      <td>Moscow</td>
      <td>14:07:09</td>
      <td>Friday</td>
    </tr>
    <tr>
      <th>2</th>
      <td>20EC38</td>
      <td>Funiculì funiculà</td>
      <td>Mario Lanza</td>
      <td>pop</td>
      <td>Saint-Petersburg</td>
      <td>20:58:07</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>3</th>
      <td>A3DD03C9</td>
      <td>Dragons in the Sunset</td>
      <td>Fire + Ice</td>
      <td>folk</td>
      <td>Saint-Petersburg</td>
      <td>08:37:09</td>
      <td>Monday</td>
    </tr>
    <tr>
      <th>4</th>
      <td>E2DC1FAE</td>
      <td>Soul People</td>
      <td>Space Echo</td>
      <td>dance</td>
      <td>Moscow</td>
      <td>08:34:34</td>
      <td>Monday</td>
    </tr>
    <tr>
      <th>5</th>
      <td>842029A1</td>
      <td>Преданная</td>
      <td>IMPERVTOR</td>
      <td>rusrap</td>
      <td>Saint-Petersburg</td>
      <td>13:09:41</td>
      <td>Friday</td>
    </tr>
    <tr>
      <th>6</th>
      <td>4CB90AA5</td>
      <td>True</td>
      <td>Roman Messer</td>
      <td>dance</td>
      <td>Moscow</td>
      <td>13:00:07</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>7</th>
      <td>F03E1C1F</td>
      <td>Feeling This Way</td>
      <td>Polina Griffith</td>
      <td>dance</td>
      <td>Moscow</td>
      <td>20:47:49</td>
      <td>Wednesday</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8FA1D3BE</td>
      <td>И вновь продолжается бой</td>
      <td>NaN</td>
      <td>ruspop</td>
      <td>Moscow</td>
      <td>09:17:40</td>
      <td>Friday</td>
    </tr>
    <tr>
      <th>9</th>
      <td>E772D5C0</td>
      <td>Pessimist</td>
      <td>NaN</td>
      <td>dance</td>
      <td>Saint-Petersburg</td>
      <td>21:20:49</td>
      <td>Wednesday</td>
    </tr>
  </tbody>
</table>
</div>



**Описание данных:**
* `userID` — идентификатор пользователя;
* `Track` — название трека;  
* `artist` — имя исполнителя;
* `genre` — название жанра;
* `City` — город пользователя;
* `time` — время начала прослушивания;
* `Day` — день недели.

### Выводим основную информацию о датафреймах методом info 


```python
# получение общей информации о данных в таблице df
df.info() 
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 65079 entries, 0 to 65078
    Data columns (total 7 columns):
     #   Column    Non-Null Count  Dtype 
    ---  ------    --------------  ----- 
     0     userID  65079 non-null  object
     1   Track     63848 non-null  object
     2   artist    57876 non-null  object
     3   genre     63881 non-null  object
     4     City    65079 non-null  object
     5   time      65079 non-null  object
     6   Day       65079 non-null  object
    dtypes: object(7)
    memory usage: 3.5+ MB
    

**Вывод:**  

Итак, в таблице семь столбцов. Тип данных во всех столбцах — `object`.  
Количество значений в столбцах различается. Значит, в данных есть пропущенные значения.

Кроме этого, в названиях колонок видны нарушения стиля:
* строчные буквы сочетаются с прописными,
* встречаются пробелы,
* в названии столбца `userID` нет нижнего подчеркивания между словами.

В каждой строке таблицы — данные о прослушанном треке. Часть колонок описывает саму композицию: название, исполнителя и жанр. Остальные данные рассказывают о пользователе: из какого он города, когда он слушал музыку. 

## Предобработка данных

### Преведём заголовки к одному стилю


```python
# перечень названий столбцов таблицы df
df.columns
```




    Index(['  userID', 'Track', 'artist', 'genre', '  City  ', 'time', 'Day'], dtype='object')




```python
# переименование столбцов
df = df.rename(columns={'  userID': 'user_id',
                        'Track': 'track',
                        '  City  ': 'city',
                        'Day': 'day'})
# проверка результатов - перечень названий столбцов
df.columns
```




    Index(['user_id', 'track', 'artist', 'genre', 'city', 'time', 'day'], dtype='object')



### Заполним пропущенные значения


```python
# подсчёт пропусков
df.isna().sum()
```




    user_id       0
    track      1231
    artist     7203
    genre      1198
    city          0
    time          0
    day           0
    dtype: int64



**Замечание:**
Не все пропущенные значения влияют на исследование. Так в `track` и `artist` пропуски не важны для нашей работы. Достаточно заменить их заглушкой.

Но пропуски в `genre` могут помешать сравнению музыкальных вкусов в Москве и Санкт-Петербурге. На практике было бы правильно установить причину пропусков и восстановить данные. Такой возможности нет в данной ситуации. Придётся:
* заполнить и эти пропуски заглушкой;
* оценить, насколько они повредят расчётам. 


```python
# замена пропущенных значений на 'unknown'.
# если выяснится способ заполнения пропусков в столбце "genre",
# можно просто удалить этот столбец из списка "columns_to_replace"
columns_to_replace = ['track', 'artist', 'genre']
for column in columns_to_replace:
    df[column] = df[column].fillna('unknown')
# подсчёт пропусков    
df.isna().sum()
```




    user_id    0
    track      0
    artist     0
    genre      0
    city       0
    time       0
    day        0
    dtype: int64



### Найдем и обработаем дубликаты в данных

#### Обработаем явные дубликаты


```python
# подсчёт явных дубликатов
df.duplicated().sum()
```




    3826




```python
# удаление явных дубликатов
df = df.drop_duplicates().reset_index(drop=True)
# проверка на отсутствие дубликатов
df.duplicated().sum()
```




    0



#### Обработаем неявные дубликаты

Рассмотрим неявные дубликаты в колонке `genre`. Например, название одного и того же жанра может быть записано немного по-разному. Такие ошибки тоже повлияют на результат исследования.


```python
# Просмотр количества уникальных названий жанров
print('Количество уникальных жанров:', pd.Series(df['genre'].unique()).count())
# Просмотр уникальных названий жанров
df['genre'].sort_values(ascending=True).unique()
```

    Количество уникальных жанров: 290
    




    array(['acid', 'acoustic', 'action', 'adult', 'africa', 'afrikaans',
           'alternative', 'alternativepunk', 'ambient', 'americana',
           'animated', 'anime', 'arabesk', 'arabic', 'arena',
           'argentinetango', 'art', 'audiobook', 'author', 'avantgarde',
           'axé', 'baile', 'balkan', 'beats', 'bigroom', 'black', 'bluegrass',
           'blues', 'bollywood', 'bossa', 'brazilian', 'breakbeat', 'breaks',
           'broadway', 'cantautori', 'cantopop', 'canzone', 'caribbean',
           'caucasian', 'celtic', 'chamber', 'chanson', 'children', 'chill',
           'chinese', 'choral', 'christian', 'christmas', 'classical',
           'classicmetal', 'club', 'colombian', 'comedy', 'conjazz',
           'contemporary', 'country', 'cuban', 'dance', 'dancehall',
           'dancepop', 'dark', 'death', 'deep', 'deutschrock', 'deutschspr',
           'dirty', 'disco', 'dnb', 'documentary', 'downbeat', 'downtempo',
           'drum', 'dub', 'dubstep', 'eastern', 'easy', 'electronic',
           'electropop', 'emo', 'entehno', 'epicmetal', 'estrada', 'ethnic',
           'eurofolk', 'european', 'experimental', 'extrememetal', 'fado',
           'fairytail', 'film', 'fitness', 'flamenco', 'folk', 'folklore',
           'folkmetal', 'folkrock', 'folktronica', 'forró', 'frankreich',
           'französisch', 'french', 'funk', 'future', 'gangsta', 'garage',
           'german', 'ghazal', 'gitarre', 'glitch', 'gospel', 'gothic',
           'grime', 'grunge', 'gypsy', 'handsup', "hard'n'heavy", 'hardcore',
           'hardstyle', 'hardtechno', 'hip', 'hip-hop', 'hiphop',
           'historisch', 'holiday', 'hop', 'horror', 'house', 'hymn', 'idm',
           'independent', 'indian', 'indie', 'indipop', 'industrial',
           'inspirational', 'instrumental', 'international', 'irish', 'jam',
           'japanese', 'jazz', 'jewish', 'jpop', 'jungle', 'k-pop',
           'karadeniz', 'karaoke', 'kayokyoku', 'korean', 'laiko', 'latin',
           'latino', 'leftfield', 'local', 'lounge', 'loungeelectronic',
           'lovers', 'malaysian', 'mandopop', 'marschmusik', 'meditative',
           'mediterranean', 'melodic', 'metal', 'metalcore', 'mexican',
           'middle', 'minimal', 'miscellaneous', 'modern', 'mood', 'mpb',
           'muslim', 'native', 'neoklassik', 'neue', 'new', 'newage',
           'newwave', 'nu', 'nujazz', 'numetal', 'oceania', 'old', 'opera',
           'orchestral', 'other', 'piano', 'podcasts', 'pop', 'popdance',
           'popelectronic', 'popeurodance', 'poprussian', 'post',
           'posthardcore', 'postrock', 'power', 'progmetal', 'progressive',
           'psychedelic', 'punjabi', 'punk', 'quebecois', 'ragga', 'ram',
           'rancheras', 'rap', 'rave', 'reggae', 'reggaeton', 'regional',
           'relax', 'religious', 'retro', 'rhythm', 'rnb', 'rnr', 'rock',
           'rockabilly', 'rockalternative', 'rockindie', 'rockother',
           'romance', 'roots', 'ruspop', 'rusrap', 'rusrock', 'russian',
           'salsa', 'samba', 'scenic', 'schlager', 'self', 'sertanejo',
           'shanson', 'shoegazing', 'showtunes', 'singer', 'ska', 'skarock',
           'slow', 'smooth', 'soft', 'soul', 'soulful', 'sound', 'soundtrack',
           'southern', 'specialty', 'speech', 'spiritual', 'sport',
           'stonerrock', 'surf', 'swing', 'synthpop', 'synthrock',
           'sängerportrait', 'tango', 'tanzorchester', 'taraftar', 'tatar',
           'tech', 'techno', 'teen', 'thrash', 'top', 'traditional',
           'tradjazz', 'trance', 'tribal', 'trip', 'triphop', 'tropical',
           'türk', 'türkçe', 'ukrrock', 'unknown', 'urban', 'uzbek',
           'variété', 'vi', 'videogame', 'vocal', 'western', 'world',
           'worldbeat', 'ïîï', 'электроника'], dtype=object)



**Замечание:**  

Мы видим следующие неявные дубликаты:
* *hip*,
* *hop*,
* *hip-hop*.
Заменим эти ззначения на новое - `hiphop`:


```python
# Устранение неявных дубликатов
df = df.replace(['hip', 'hop', 'hip-hop'], 'hiphop')
# Просмотр количества уникальных названий жанров
print('Количество уникальных жанров:', pd.Series(df['genre'].unique()).count())
# Просмотр уникальных названий жанров
df['genre'].sort_values().unique()
```

    Количество уникальных жанров: 287
    




    array(['acid', 'acoustic', 'action', 'adult', 'africa', 'afrikaans',
           'alternative', 'alternativepunk', 'ambient', 'americana',
           'animated', 'anime', 'arabesk', 'arabic', 'arena',
           'argentinetango', 'art', 'audiobook', 'author', 'avantgarde',
           'axé', 'baile', 'balkan', 'beats', 'bigroom', 'black', 'bluegrass',
           'blues', 'bollywood', 'bossa', 'brazilian', 'breakbeat', 'breaks',
           'broadway', 'cantautori', 'cantopop', 'canzone', 'caribbean',
           'caucasian', 'celtic', 'chamber', 'chanson', 'children', 'chill',
           'chinese', 'choral', 'christian', 'christmas', 'classical',
           'classicmetal', 'club', 'colombian', 'comedy', 'conjazz',
           'contemporary', 'country', 'cuban', 'dance', 'dancehall',
           'dancepop', 'dark', 'death', 'deep', 'deutschrock', 'deutschspr',
           'dirty', 'disco', 'dnb', 'documentary', 'downbeat', 'downtempo',
           'drum', 'dub', 'dubstep', 'eastern', 'easy', 'electronic',
           'electropop', 'emo', 'entehno', 'epicmetal', 'estrada', 'ethnic',
           'eurofolk', 'european', 'experimental', 'extrememetal', 'fado',
           'fairytail', 'film', 'fitness', 'flamenco', 'folk', 'folklore',
           'folkmetal', 'folkrock', 'folktronica', 'forró', 'frankreich',
           'französisch', 'french', 'funk', 'future', 'gangsta', 'garage',
           'german', 'ghazal', 'gitarre', 'glitch', 'gospel', 'gothic',
           'grime', 'grunge', 'gypsy', 'handsup', "hard'n'heavy", 'hardcore',
           'hardstyle', 'hardtechno', 'hiphop', 'historisch', 'holiday',
           'horror', 'house', 'hymn', 'idm', 'independent', 'indian', 'indie',
           'indipop', 'industrial', 'inspirational', 'instrumental',
           'international', 'irish', 'jam', 'japanese', 'jazz', 'jewish',
           'jpop', 'jungle', 'k-pop', 'karadeniz', 'karaoke', 'kayokyoku',
           'korean', 'laiko', 'latin', 'latino', 'leftfield', 'local',
           'lounge', 'loungeelectronic', 'lovers', 'malaysian', 'mandopop',
           'marschmusik', 'meditative', 'mediterranean', 'melodic', 'metal',
           'metalcore', 'mexican', 'middle', 'minimal', 'miscellaneous',
           'modern', 'mood', 'mpb', 'muslim', 'native', 'neoklassik', 'neue',
           'new', 'newage', 'newwave', 'nu', 'nujazz', 'numetal', 'oceania',
           'old', 'opera', 'orchestral', 'other', 'piano', 'podcasts', 'pop',
           'popdance', 'popelectronic', 'popeurodance', 'poprussian', 'post',
           'posthardcore', 'postrock', 'power', 'progmetal', 'progressive',
           'psychedelic', 'punjabi', 'punk', 'quebecois', 'ragga', 'ram',
           'rancheras', 'rap', 'rave', 'reggae', 'reggaeton', 'regional',
           'relax', 'religious', 'retro', 'rhythm', 'rnb', 'rnr', 'rock',
           'rockabilly', 'rockalternative', 'rockindie', 'rockother',
           'romance', 'roots', 'ruspop', 'rusrap', 'rusrock', 'russian',
           'salsa', 'samba', 'scenic', 'schlager', 'self', 'sertanejo',
           'shanson', 'shoegazing', 'showtunes', 'singer', 'ska', 'skarock',
           'slow', 'smooth', 'soft', 'soul', 'soulful', 'sound', 'soundtrack',
           'southern', 'specialty', 'speech', 'spiritual', 'sport',
           'stonerrock', 'surf', 'swing', 'synthpop', 'synthrock',
           'sängerportrait', 'tango', 'tanzorchester', 'taraftar', 'tatar',
           'tech', 'techno', 'teen', 'thrash', 'top', 'traditional',
           'tradjazz', 'trance', 'tribal', 'trip', 'triphop', 'tropical',
           'türk', 'türkçe', 'ukrrock', 'unknown', 'urban', 'uzbek',
           'variété', 'vi', 'videogame', 'vocal', 'western', 'world',
           'worldbeat', 'ïîï', 'электроника'], dtype=object)



**Вывод:**  

Мы исправили заголовки, чтобы упростить работу с таблицей. Без дубликатов исследование станет более точным.
Пропущенные значения мы заменили на `'unknown'`.  
Ещё предстоит увидеть, не повредят ли исследованию пропуски в колонке `genre`.

## Проверка гипотез

### Сравнение поведения пользователей двух столиц

Первая гипотеза утверждает, что пользователи по-разному слушают музыку в Москве и Санкт-Петербурге. Проверим это предположение по данным о трёх днях недели — понедельнике, среде и пятнице. Для этого:

* разделим пользователей Москвы и Санкт-Петербурга.
* сравним, сколько треков послушала каждая группа пользователей в понедельник, среду и пятницу.


```python
# Подсчёт прослушиваний в каждом городе
df.groupby('city')['city'].count()
```




    city
    Moscow              42741
    Saint-Petersburg    18512
    Name: city, dtype: int64



**Замечание:** В Москве прослушиваний больше, чем в Петербурге. Из этого не следует, что московские пользователи чаще слушают музыку. Просто самих пользователей в Москве больше.


```python
# Подсчёт прослушиваний в каждый из трёх дней
df.groupby('day')['day'].count()
```




    day
    Friday       21840
    Monday       21354
    Wednesday    18059
    Name: day, dtype: int64



**Замечание:** В среднем пользователи из двух городов менее активны по средам. Но картина может измениться, если рассмотреть каждый город в отдельности.


```python
# Функция для подсчёта прослушиваний для конкретного города и дня.
# С помощью последовательной фильтрации с логической индексацией она 
# сначала получит из исходной таблицы строки с нужным днём,
# затем из результата отфильтрует строки с нужным городом,
# методом count() посчитает количество значений в колонке user_id. 
# Это количество функция вернёт в качестве результата
def number_tracks(day, city):
    track_list = df[df['day'] == day]
    track_list = track_list[track_list['city'] == city]
    track_list_count = track_list['user_id'].count()
    return track_list_count
```


```python
# количество прослушиваний в Москве по понедельникам
mon_moscow = number_tracks('Monday', 'Moscow')
# количество прослушиваний в Санкт-Петербурге по понедельникам
mon_spb = number_tracks('Monday', 'Saint-Petersburg')
# количество прослушиваний в Москве по средам
wed_moscow = number_tracks('Wednesday', 'Moscow')
# количество прослушиваний в Санкт-Петербурге по средам
wed_spb = number_tracks('Wednesday', 'Saint-Petersburg')
# количество прослушиваний в Москве по пятницам
fri_moscow = number_tracks('Friday', 'Moscow')
# количество прослушиваний в Санкт-Петербурге по пятницам
fri_spb = number_tracks('Friday', 'Saint-Petersburg')
```


```python
# создание таблицы с помощью конструктора "pd.DataFrame"
data = [['Moscow', mon_moscow, wed_moscow, fri_moscow],
        ['Saint-Petersburg',mon_spb, wed_spb, fri_spb]]
columns = ['city', 'monday', 'wednesday', 'friday']
# Таблица с результатами
info = pd.DataFrame(data=data, columns=columns)
info
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>monday</th>
      <th>wednesday</th>
      <th>friday</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Moscow</td>
      <td>15740</td>
      <td>11056</td>
      <td>15945</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Saint-Petersburg</td>
      <td>5614</td>
      <td>7003</td>
      <td>5895</td>
    </tr>
  </tbody>
</table>
</div>



**Вывод:**

Данные показывают разницу поведения пользователей:  
- В Москве пик прослушиваний приходится на понедельник и пятницу, а в среду заметен спад.
- В Петербурге, наоборот, больше слушают музыку по средам. Активность в понедельник и пятницу здесь почти в равной мере уступает среде.

Значит, данные говорят в пользу первой гипотезы.

### Музыка в начале и в конце недели

Согласно второй гипотезе, утром в понедельник в Москве преобладают одни жанры, а в Петербурге — другие. Так же и вечером в пятницу преобладают разные жанры — в зависимости от города.


```python
# получение таблицы с данными по Москве
moscow_general = df[df['city'] == 'Moscow']
# получение таблицы с данными по Санкт - Петербургу
spb_general = df[df['city'] == 'Saint-Petersburg']
```


```python
# создание функции genre_weekday() с параметрами table, day, time1, time2,
# которая возвращает информацию о самых популярных 10 жанрах в указанный день
# в заданное время с помощью последовательной фильтрации:

def genre_weekday(df, day, time1, time2):
    genre_df = df[df['day'] == day]
    genre_df = genre_df[genre_df['time'] < time2]
    genre_df = genre_df[genre_df['time'] > time1]
    genre_df_grouped = genre_df.groupby('genre')['genre'].count()
    genre_df_sorted = genre_df_grouped.sort_values(ascending=False)
    return genre_df_sorted[:10]
```

**Задание 25**


Cравните результаты функции `genre_weekday()` для Москвы и Санкт-Петербурга в понедельник утром (с 7:00 до 11:00) и в пятницу вечером (с 17:00 до 23:00):


```python
# вывод самых популярных жанров для Москвы в понедельник утром (с 7:00 до 11:00)
genre_weekday(moscow_general, 'Monday', '07:00', '11:00')
```




    genre
    pop            781
    dance          549
    electronic     480
    rock           474
    hiphop         286
    ruspop         186
    world          181
    rusrap         175
    alternative    164
    unknown        161
    Name: genre, dtype: int64




```python
# вывод самых популярных жанров для Санкт-Петербурга в понедельник утром (с 7:00 до 11:00)
genre_weekday(spb_general, 'Monday', '07:00', '11:00')
```




    genre
    pop            218
    dance          182
    rock           162
    electronic     147
    hiphop          80
    ruspop          64
    alternative     58
    rusrap          55
    jazz            44
    classical       40
    Name: genre, dtype: int64




```python
# вывод самых популярных жанров для Москвы в пятницу вечером (с 17:00 до 23:00)
genre_weekday(moscow_general, 'Friday', '17:00', '23:00')
```




    genre
    pop            713
    rock           517
    dance          495
    electronic     482
    hiphop         273
    world          208
    ruspop         170
    alternative    163
    classical      163
    rusrap         142
    Name: genre, dtype: int64




```python
# вывод самых популярных жанров для Санкт-Петербурга в пятницу вечером (с 17:00 до 23:00)
genre_weekday(spb_general, 'Friday', '17:00', '23:00')
```




    genre
    pop            256
    electronic     216
    rock           216
    dance          210
    hiphop          97
    alternative     63
    jazz            61
    classical       60
    rusrap          59
    world           54
    Name: genre, dtype: int64



**Вывод:**

Если сравнить топ-10 жанров в понедельник утром, можно сделать такие выводы:

1. В Москве и Петербурге слушают похожую музыку. Единственное отличие — в московский рейтинг вошёл жанр “world”, а в петербургский — джаз и классика.

2. В Москве пропущенных значений оказалось так много, что значение `'unknown'` заняло десятое место среди самых популярных жанров. Значит, пропущенные значения занимают существенную долю в данных и угрожают достоверности исследования.

Вечер пятницы не меняет эту картину. Некоторые жанры поднимаются немного выше, другие спускаются, но в целом топ-10 остаётся тем же самым.

Таким образом, вторая гипотеза подтвердилась лишь частично:
* Пользователи слушают похожую музыку в начале недели и в конце.
* Разница между Москвой и Петербургом не слишком выражена. В Москве чаще слушают русскую популярную музыку, в Петербурге — джаз.

Однако пропуски в данных ставят под сомнение этот результат. В Москве их так много, что рейтинг топ-10 мог бы выглядеть иначе, если бы не утерянные  данные о жанрах.

### Жанровые предпочтения в Москве и Петербурге

Гипотеза: Петербург — столица рэпа, музыку этого жанра там слушают чаще, чем в Москве.  А Москва — город контрастов, в котором, тем не менее, преобладает поп-музыка.


```python
# группировка данных по Москве по жанру
moscow_genres = moscow_general.groupby('genre')['genre'].count().sort_values(ascending=False)
# просмотр первых 10 строк moscow_genres
moscow_genres.head(10)
```




    genre
    pop            5892
    dance          4435
    rock           3965
    electronic     3786
    hiphop         2096
    classical      1616
    world          1432
    alternative    1379
    ruspop         1372
    rusrap         1161
    Name: genre, dtype: int64




```python
# группировка данных по Санкт - Петербургу по жанру
spb_genres = spb_general.groupby('genre')['genre'].count().sort_values(ascending=False)
# просмотр первых 10 строк spb_genres
spb_genres[:10]
```




    genre
    pop            2431
    dance          1932
    rock           1879
    electronic     1736
    hiphop          960
    alternative     649
    classical       646
    rusrap          564
    ruspop          538
    world           515
    Name: genre, dtype: int64



**Вывод:**

Гипотеза частично подтвердилась:
* Поп-музыка — самый популярный жанр в Москве, как и предполагала гипотеза. Более того, в топ-10 жанров встречается близкий жанр — русская популярная музыка.
* Вопреки ожиданиям, рэп одинаково популярен в Москве и Петербурге. 


## Итоговый вывод

В данном проекте данных Яндекс Музыки мы проверяли гипотезы и сравнивали поведение пользователей двух столиц.

**Этапы выполнения проекта:**     
1. Изучение данных.  
   Здесь мы установили и импортировали необходимые для работы библиотеки, загрузили и изучили данные. 
      
2. Предобработка данных.  
   На данном этапе мы привели заголовки к одному типу, заполнили пропуски и обработали дубликаты (явные и неявные). 

3. Проверка гипотез.

**В итоге:**  
Мы проверили три гипотезы и установили:
1. День недели по-разному влияет на активность пользователей в Москве и Петербурге.  
   Первая гипотеза полностью подтвердилась.

2. Музыкальные предпочтения не сильно меняются в течение недели — будь то Москва или Петербург. Небольшие различия заметны в начале недели, по понедельникам: в Москве слушают музыку жанра `world`, а в Петербурге — джаз и классику.   
   Таким образом, вторая гипотеза подтвердилась лишь отчасти. Этот результат мог оказаться иным, если бы не пропуски в данных.

3. Во вкусах пользователей Москвы и Петербурга больше общего чем различий. Вопреки ожиданиям, предпочтения жанров в Петербурге напоминают московские.  
   Третья гипотеза не подтвердилась. Если различия в предпочтениях и существуют, на основной массе пользователей они незаметны.
