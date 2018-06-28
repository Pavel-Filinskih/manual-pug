
### Теги

в Jade(pug) нет закрывающих тегов. Вместо этого Jade использует табуляцию для определения вложености тегов.
Jade компилирует это, рассматривая первое слово в каждой строке в качестве тега, в то время как последующие
слова на этой строке обрабатываются как текст внутри тега.

#### pug

```
  div
    p Hello!
    p World!
```

#### html

```html
  <div>
    <p>Hello!</p>
    <p>World!</p>
  </div>
```
  
> Отступы между "тегами" делать "табуляцией" либо пробелом!


### Атрибуты


Jade предоставляет специальную стенографию для индификаторов и классов, используя знакомые всем обозначения:

#### pug

```
  div(class="movie-card", id="oceans-11")
    h1(class="movie-title") Ocean's 11
    img(src="/img/oceans-11.png", class="movie-poster")
    ul(class="genre-list")
      li Comedy
      li Thriller
```

или

```
  div.movie-card#oceans-11
    h1.movie-title Ocean's 11
    img.movie-poster(src="/img/oceans-11.png")
    ul.genre-list
      li Comedy
      li Thriller
```

#### html

  <div class="movie-card" id="oceans-11">
    <h1 class="movie-title">Ocean's 11</h1>
    <img src="/img/oceans-11.png" class="movie-poster">
    <ul class="genre-list">
      <li>Comedy</li>
      <li>Thriller</li>
    </ul>
  </div>
  

### Блоки текста

```
Давайте представим такую ситуацию: у вас есть тег <р> и вы хотите добавить в него довольно таки большой объём текста.
Но стоп, ведь Jade рассматривает первое слово каждой строки как новый HTML-тег — и как тут быть?
Добавление точки после вашего тега даёт понять компилятору Jade, что всё внутри данного тега является текстом.
```

#### pug

```
  div
    p How are you?
    p.
      I'm fine thank you.
      And you? I heard you fell into a lake?
      That's rather unfortunate. I hate it when my shoes get wet.
```

```
Для полной ясности: в случае, если я не поставил бы точку после тега <р> в примере, то скомпилированный HTML
имел бы в себе открытый тег <і>, разорвав словосочетание “I’m” в начале строки.
```

### Полезные функции

```
Jade реализован на JavaScript, по этому использовать JavaScript в Jade довольно просто. Вот пример:

Что же мы тут сделали?! Начав строку с дефиса, мы указали компилятору Jade, что мыхотим использовать JavaScript,
и всё, оно работает! И вот что мы получим, когда скомпилируем этот код в HTML:
```

#### pug

```
  - var x = 5;
  div
    ul
      - for (var i=1; i<=x; i++) {
        li Hello
      - }
```

#### html

```html
  <div>
    <ul>
      <li>Hello</li>
      <li>Hello</li>
      <li>Hello</li>
      <li>Hello</li>
      <li>Hello</li>
    </ul>
  </div>
```

```
Мы используем дефис, когда код не должен напрямую попадать в поток вывода. В случае, ежели мы хотим использовать JavaScript для вывода чего-либо в Jade, мы используем =. Давайте подправим код выше, чтобы указать нумерацию
элементов в списке:
```

#### pug

```
  - var x = 5;
  div
    ul
      - for (var i=1; i<=x; i++) {
        li= i + ". Hello"
      - }
```

#### html

```html
  <div>
    <ul>
      <li>1. Hello</li>
      <li>2. Hello</li>
      <li>3. Hello</li>
      <li>4. Hello</li>
      <li>5. Hello</li>
    </ul>
  </div>
```

### Циклы

Давайте пройдёмся по элементам массива в цикле:

#### pug

```
  - var droids = ["R2D2", "C3PO", "BB8"];
  div
    h1 Famous Droids from Star Wars
    for name in droids
      div.card
        h2= name
```

#### html

```html
  <div>
    <h1>Famous Droids from Star Wars</h1>
    <div class="card">
      <h2>R2D2</h2>
    </div>
    <div class="card">
      <h2>C3PO</h2>
    </div>
    <div class="card">
      <h2>BB8</h2>
    </div>
  </div>
```

### Интерполирование

Совмещать текст и JavaScript таким образом

#### pug

```
  - var profileName = "Danny Ocean";
  div
    p Hi there, #{profileName}. How are you doing?
```

#### html

```html
  <p>Hi there, Danny Ocean. How are you doing?</p>
```

### Миксины

```
Миксины, они как функции, они принимают параметры в качестве входных данных и генерируют соответствующию разметку. Миксины оглашаются с помощью ключевого слова mixin.
```

#### pug

```
  mixin thumbnail(imageName, caption)
    div.thumbnail
      img(src="/img/#{imageName}.jpg")
      h4.image-caption= caption
```

После того, как миксин был оглашён, вы можете его вызвать с помощью символа +

```
+thumbnail("oceans-eleven", "Danny Ocean makes an elevator pitch.")
+thumbnail("pirates", "Introducing Captain Jack Sparrow!")
```

#### html

```html
  <div class="thumbnail">
    <img src="/img/oceans-eleven.jpg">
    <h4 class="image-caption">
      Danny Ocean makes an elevator pitch.
    </h4>
  </div>

  <div class="thumbnail">
    <img src="/img/pirates.jpg">
    <h4 class="image-caption">
      Introducing Captain Jack Sparrow!
    </h4>
  </div>
```

### Подключение модулей

```
одним из достоинств pug является возможность подключения модулей — одного кода к различным файлам
для подключения блока кода написать - include путь_к_файлу/файл
```

> указывать расширение .pug необязательно!

```
  div.wrapp#main
    include modules/header // выводим шапку
    div.content
      p "Какой-то текст"
      p Lorem
      p Lorem2
    include modules/footer // выводим футер
```

### Наследование шаблонов

```
Мы можем создать отдельный шаблон страницы и в нём определить блоки кода, которые будут определяться или изменяться на странице-наследнике. Для примера создадим шаблон template.pug и разместим его в директории templates. Его содержимое:
```

#### pug

```
  doctype
  html(lang="ru")
    head
      block style
        link(rel="stylesheet" href="css/default-styles.css")
      block title
    body
      block content
```

```
Ключевое слово block обозначает блок кода, который будет вставлен на странице, наследующей данный шаблон.
Можно задать значение по умолчанию, как в случае тега link, подключающего таблицу стилей.
```

#### pug

```
  extends templates/template

  block title
    title Заголовок страницы

  block content
    include modules/header
    h1 Заголовок
    p Текст абзаца

  extends templates/template
   
  block title
    title Заголовок страницы
   
  block content
    include modules/header
    h1 Заголовок
    p Текст абзаца
```

```
Шаблон подключается с помощью конструкции extends путь_к_шаблону (templates/template).
А далее описывается содержимое блоков. Например, в блоке content подключается шапка
и содержится простая разметка в виде заголовка и абзаца с текстом.
Т.е сгенерированная страница будет иметь следующий вид:
```

#### html

```html
  <!DOCTYPE html>
  <html lang="ru">
    <head>
      <link rel="stylesheet" href="css/default-styles.css">
      <title>Заголовок страницы</title>
    </head>
    <body>
      <header>
        <ul class="nav">
          <li class="nav__item"><a class="nav__link" href="#">Item_1</a></li>
          <li class="nav__item"><a class="nav__link" href="#">Item_2</a></li>
          <li class="nav__item"><a class="nav__link" href="#">Item_3</a></li>
        </ul>
      </header>
      <h1>Заголовок</h1>
      <p>Текст абзаца</p>
    </body>
  </html>
```
