# Регулярные выражения
## Что такое регулярки и с чем их едят
***Регулярные выражения*** (их еще называют **regexp**, или **regex**) — это механизм для поиска и замены текста. В строке, файле, нескольких файлах... Их используют разработчики в коде приложения, тестировщики в автотестах, да просто при работе в командной строке!
Чем это лучше простого поиска? Тем, что позволяет задать шаблон.
Регулярные выражения позволяют задавать и гораздо более сложные шаблоны поиска или замены.

Например, при помощи регулярных выражений можно задать шаблоны, позволяющие:

1. найти все последовательности символов «кот» в любом контексте, как то: «кот», «котлета», «терракотовый»;
2. найти отдельно стоящее слово «кот» и заменить его на «кошка»;
3. найти слово «кот», которому предшествует слово «персидский» или «чеширский»;
4. убрать из текста все предложения, в которых упоминается слово кот или кошка.

Результатом работы с регулярным выражением может быть:

1. проверка наличия искомого образца в заданном тексте;
2. определение подстроки текста, которая сопоставляется образцу;
3. определение групп символов, соответствующих отдельным частям образца.
Если регулярное выражение используется для замены текста, то результатом работы будет новая текстовая строка, представляющая из себя исходный текст, из которого удалены найденные подстроки (сопоставленные образцу), а вместо них подставлены строки замены (возможно, модифицированные запомненными при разборе группами символов из исходного текста). Частным случаем модификации текста является удаление всех вхождений найденного образца — для чего строка замены указывается пустой.

## Основные моменты и замечания
- регулярки регистрозависимые, но при желании это можно исправить
- специальные символы: [ ] \ / ^ $ . | ? * + ( ) { }
- спец. символы нужно заэкранировать (\ перед ними) если необходимо найти их: \.txt
- точка (.) найдёт любой символ (один!!!) (А.я найдет Аня, Аля)
- [] - перечисление символов или указание диапазона (абв, 0-9 и тд)
- в наборе символов: [abcd] - возможные символы (без пробелов и запятых!!!!), [a-z] - диапазон
- [] для одного символа
- | для перечисления возможных слов: кот|котик|котенька|котёнок
- () - группа символов: A[нл]я 
- существую метасимволы, заменяющие конкретный диапазон значений:
  - \d -> [0-9]  - цифровой символ 
  - \D -> [^0-9] - нецифровой символ
  - \s -> [ \f\n\r\t\v] - пробельный символ
     - \r -> возврат каретки
     - \n -> перевод строки
     - \t -> табуляция
     - \v -> вертикальная табуляция
     - \f -> конец страницы
     - [\b] -> возврат на 1 символ
  - \S -> [^ \f\n\r\t\v] - непробельный символ
  - \w -> [[:word:]] - буквенный/цифровой символ/знак подчеркивания
  - \W -> [^[:word:]] - любой символ, кроме буквенного/цифрового/знака подчёркивания
  - . (точка) -> вообще любой символ
- \* - любой символ ноль или более раз
- {} - количество повторений
  - {n} -> ровно n раз
  - {m, n}  -> от m до n включительно
  - {m,} -> не менее m
  - {,n} -> не более n
 - квантификатор(ограничитель) применяется к последнему символу, или к группе символов, если они стоят в ()
 - \b - граница слова: если \bёжик\b найдется именно ёжик; можно ограничить только спереди или сзади
 - \B -НЕ-граница слова
 - ^kakaja-to stroka& - для поиска конкретной фразы

## Регулярки в С++
В Java исходным представлением этого шаблона всегда является строка, то есть объект класса String. 
```с++
String regex=”java”; // шаблон строки ”java”;
String regex=”\\d{3}”; // шаблон строки из трех цифровых символов;
```
Работа с регулярными выражениями в любой Java-программе начинается с создания объекта класса Pattern. Для этого необходимо вызвать один из двух имеющихся в классе статических методов compile. Первый метод принимает один аргумент – строковый литерал регулярного выражения, а второй – плюс еще параметр, включающий режим сравнения шаблона с текстом:
```
public static Pattern compile (String literal)
public static Pattern compile (String literal, int flags)
```
Список возможных значений параметра flags определен в классе Pattern и доступен нам как статические переменные класса.
По сути, класс Pattern — это конструктор регулярных выражений. Под «капотом» метод compile вызывает закрытый конструктор класса Pattern для создания скомпилированного представления. Такой способ создания экземпляра шаблона реализован с целью создания его в виде неизменяемого объекта. При создании производится синтаксическая проверка регулярного выражения. При наличии ошибок в строке – генерируется исключение PatternSyntaxException.

## Ключевые классы
Java RegExp обеспечиваются пакетом java.util.regex. Здесь ключевыми являются три класса:

- Matcher — выполняет операцию сопоставления в результате интерпретации шаблона.
- Pattern — предоставляет скомпилированное представление регулярного выражения.
- PatternSyntaxException — предоставляет непроверенное исключение, что указывает на синтаксическую ошибку, допущенную в шаблоне RegEx.
Также есть интерфейс MatchResult, который представляет результат операции сопоставления.
## Пример из чат-бота
```Java
pattern = Pattern.compile(entry.getKey());          //ищем нужный нам шаблон
matcher = pattern.matcher(message);
if (matcher.find())                //если нашли такой шаблон
{
  //условия выполняются в зависимости от шаблонов
}
```
## Другие примеры кода
### Есть ли в строке подстрока? //пример с Pattern
```Java
String text = "http://social.zabgu.ru/login";
String regex = ".*http://.*";
boolean matches = Pattern.matches(regex, text);
System.out.println("matches = " + matches);
```
### Количество вхождений в строку //пример с Matchers
Сначала мы создаем объект Pattern, вызывая его статический метод компиляции и передавая ему шаблон, который мы хотим использовать.
Затем мы создаем объект Matcher, вызывая метод matcher объекта Pattern и передавая ему текст, который мы хотим проверить на совпадения.
После этого вызываем метод find в объекте Matcher.
Метод find продолжает продвигаться по входному тексту и возвращает true для каждого совпадения, поэтому мы можем использовать его для поиска количества совпадений:
```Java
public static int runTest(String regex, String text) 
{
    Pattern pattern = Pattern.compile(regex);     // Pattern хранит регуоярное выражение
    Matcher matcher = pattern.matcher(text);      // Matcher умеет искать подствоки согласно регулярному выражению
    int matches = 0;
    while (matcher.find())
    {
        matches++;                                // ???
        //чтобы определить позиции, на которых нашлись совпадения, пишем следующее:
        int start=matcher.start();
        int end=matcher.end();
        System.out.println("Найдено совпадение " + text.substring(start,end) + " с "+ start + " по " + (end-1) + " позицию");
    }
    return matches;
}

// TODO: как определить часть строки, которую нашёл matcher?
```

Полезная инфа:
- [Крутая статья об использовании регулярок](https://habr.com/ru/post/545150/)
- [Регулярки в С++](https://habr.com/ru/company/otus/blog/532056/)
- [Регулярки в Java(коротко)](https://javarush.ru/groups/posts/regulyarnye-vyrazheniya-v-java)
- [Тоже немного про Java](https://tproger.ru/articles/java-regex-ispolzovanie-reguljarnyh-vyrazhenij-na-praktike/)
- [Тоже по регуляркам в Java, но много](https://translated.turbopages.org/proxy_u/en-ru.ru.93222ca1-627ce1a5-60345635-74722d776562/https/www.baeldung.com/regular-expressions-java)