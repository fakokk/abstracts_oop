# Введение (слайд 2)
Внедрение SQL-кода — один из распространённых способов взлома сайтов и программ, работающих с базами данных, основанный на внедрении в запрос произвольного SQL-кода.

SQL (structured query language)— декларативный язык программирования, применяемый для создания, модификации и управления данными в реляционной базе данных, управляемой соответствующей системой управления базами данных.

Внедрение SQL, в зависимости от типа используемой СУБД и условий внедрения, может дать возможность атакующему выполнить произвольный запрос к базе данных (например, прочитать содержимое любых таблиц, удалить, изменить или добавить данные), получить возможность чтения и/или записи локальных файлов и выполнения произвольных команд на атакуемом сервере.

# Уязвимости (слайд 3)

Уязвимости типа SQL- инъекции позволяют нарушителю выполнять несанкционированные операции над содержимым баз данных SQL-серверов путем вставки дополнительных команд в SQL-запросы

Уязвимость SQL-injection заключается в отсутствии корректности данных, поступающих на вход программе, что потенциально может позволить нарушителю составить входные данные таким образом, что приведет к искажению искомого SQL-запроса к субд

## Уязвимые точки
Уязвимые точки для атаки находятся в местах, где формируется запрос к базе: форма аутентификации, поисковая строка, каталог, REST-запросы и непосредственно URL.

# Защита от SQLi (слайд 4)
- Для каждого сервера и фреймворка есть свои тонкости и лучшие практики, но суть всегда одинакова.
- Нельзя вставлять данные в запрос напрямую. Всегда обрабатывайте ввод отдельно и формируйте запрос исключительно из безопасных значений.
- Создавайте белые списки: их значительно труднее обойти, чем черные. Все названия таблиц, полей и баз должны быть заданы конкретными значениями в вашей программе. Это касается и операторов.
- Естественно, не забывайте про ограничение прав доступа к базе.
- Тем не менее, кибербезопасность – это тот случай, когда понимание принципов нападения – лучший способ защиты.
- Большинство взломов через SQL происходят по причине нахождения в строках «необезвреженных» кавычек, апострофов и других специальных символов. Для такого обезвреживания нужно использовать функцию addslashes($str);, которая возвращает строку $str с добавленным обратным слешем (\) перед каждым специальным символом. 

# Типы SQLi (слайд 5)
Существует 5 основных типов SQL инъекций:

- ***Классическая ***(In-Band или Union-based). Самая опасная и редко встречающаяся сегодня атака. Позволяет сразу получать любые данные из базы.
- ***Error-based***. Позволяет получать информацию о базе, таблицах и данных на основе выводимого текста ошибки СУБД.
- Boolean-based. Вместо получения всех данных, атакующий может поштучно их перебирать, ориентируясь на простой ответ типа true/false.
- ***Time-based***. Похожа на предыдущую атаку принципом перебора, манипулируя временем отклика базы.
- ***Out-of-Band***.. Очень редкие и специфические типы атак, основанные на индивидуальных особенностях баз данных.

# Пример атаки (слайд 6)
Допустим, серверное ПО, получив входной параметр id, использует его для создания SQL-запроса. Рассмотрим следующий PHP-скрипт:
*1 картинка*

Если на сервер передан параметр id, равный 5 (например так: http://example.org/script.php?id=5), то выполнится следующий SQL-запрос:
*2 картинка*

Но если злоумышленник передаст в качестве параметра id строку -1 OR 1=1 (например, так: http://example.org/script.php?id=-1+OR+1=1), то выполнится запрос:
*3 картинка*

Таким образом, изменение входных параметров путём добавления в них конструкций языка SQL вызывает изменение в логике выполнения SQL-запроса (в данном примере вместо новости с заданным идентификатором будут выбраны все имеющиеся в базе новости, поскольку выражение 1=1 всегда истинно — вычисления происходят по кратчайшему контуру в схеме).

# Классическая атака(слайд 7)
Использование однострочных комментариев позволяет игнорировать часть запроса, идущую после вашей инъекции. Например, ввод в уязвимое поле Username запроса admin'-- позволит зайти на ресурс под администратором, потому что поверка пароля будет закомментирована. Конечно, сейчас такой тип уязвимости встречается очень редко, но помнить о ней стоит.

![](.pastes\2022-04-11-23-18-12.png)
## И еще пример 
Мы можем ввести в строку поиска вредоносный код, который поможет вытащить данные из базы данных.
Как узнать, уязвим сайт или нет?
представим, что есть сайт блалблалала
*1 картинка*

Возьмём тот же самый поиск и тот же сайт, только в этот раз вместо "игрушечная машинка", мы введем вредоносный код:
*2 картинка*

Этот код искусственно замедляет sql запрос, таким образом сайт будет посылать запрос
*3 картинка*

Если вы заметили, что сайт стал загружаться медленнее, то он уязвим к таким инъекциям.

## Словарь (слайд 8)
Есть стандартный словарь, содержащий в себе основные запросы, для обхода уязвимой формы аутентификации. Впервые его опубликовали лет 10 назад и регулярно дополняют.

# Ломаем забгу (слайд 9)
Попробуйте найти страницы, которые запрашивают у вас данные, например страница поиска, обсуждений, и т.д. Иногда html страницы используют метод POST, чтобы послать команды другой Web странице. В этом случае вы не увидите параметры в URL. Однако в этом случае вы можете искать тэг "FORM" в исходном коде HTML страниц. Вы найдете, что-то типа такого:

<FORM action=Search/search.asp method=post>
<input type=hidden name=A value=C>
</FORM>

Все параметры между <FORM> и </FORM> потенциально могут быть уязвимы к введению SQL кода.

