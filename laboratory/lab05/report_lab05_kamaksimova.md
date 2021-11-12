---
# Front matter
title: "Отчёт по лабораторной работе 5"
subtitle: "Дискреционное  разграничение прав в Linux. Исследование влияния дополнительных атрибутов"
author: "Максимова Ксения НБИбд-02-18"

# Generic otions
lang: ru-RU
toc-title: "Содержание"

# Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

# Pdf output format
toc: true # Table of contents
toc_depth: 2
lof: true # List of figures
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
### Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Misc options
indent: true
header-includes:
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.
  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.
  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen
  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen
  - \binoppenalty=700 # the penalty for breaking a line at a binary operator
  - \relpenalty=500 # the penalty for breaking a line at a relation
  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph
  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph
  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math
  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line
  - \predisplaypenalty=10000 # penalty for breaking before a display
  - \postdisplaypenalty=0 # penalty for breaking after a display
  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)
  - \raggedbottom # or \flushbottom
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Изучение механизмов изменения идентификаторов, применения
SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма
смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов.

# Задание

Изучить механизм изменения идентификаторов

# Теоретическое введение

Unix отслеживает не символьные имена владельцев и групп, а их идентификаторы (UID - для пользователей и GID для групп). 
Эти идентификаторы хранятся в файлах "/etc/passwd и "/etc/group соответственно. Символьные эквиваленты идентификаторов используются только для удобства, 
например, при использовании команды ls, идентификаторы заменяются соответствующими символьными обозначениями[[4]].

SUID (Set User ID, Бит смены владельца) - это разрешение файловой системы Linux, которое позволяет запустить исполняемый файл от имени его владельца[[1]].
Другими словами, использование этого бита позволяет нам поднять привилегии пользователя в случае, если это необходимо. 
Классический пример использования этого бита в операционной системе это команда sudo[[2]].
Не смотря на то, что функция setuid очень полезна во многих случаях, ее неправильное использование может представлять угрозу безопасности [[2]], 
если атрибут setuid назначен исполняемым программам, которые не были тщательно разработаны. 
Из-за потенциальных проблем с безопасностью многие операционные системы игнорируют атрибут setuid при применении к исполняемым сценариям оболочки[[3]].

SGID (Set Group ID) - файл будет запускаться пользователем от имени группы, которая владеет файлом[[2]].

Флаги setuid и setgid имеют различные эффекты в зависимости от того, применяются ли они к файлу, каталогу или двоичному исполняемому или недвоичному исполняемому файлу. 
Флаги setuid и setgid влияют только на двоичные исполняемые файлы, а не на скрипты (например, Bash, Perl, Python).[[3]]

Sticky bit - это флаг права доступа пользователя, который может быть назначен файлам и каталогам в Unix-подобных системах.
Существует два определения: одно для файлов, другое для каталогов.

Для файлов, особенно исполняемых файлов, суперпользователь может пометить их как сохраняемые в основной памяти, 
даже когда их потребность заканчивается, чтобы свести к минимуму замену, которая может произойти, 
когда возникнет другая необходимость, и теперь файл необходимо перезагрузить из относительно медленной вторичной памяти. 
Эта функция устарела из-за оптимизации подкачки.

Для каталогов, когда установлен бит привязки каталога, файловая система обрабатывает файлы в таких каталогах особым образом, 
чтобы только владелец файла, владелец каталога или пользователь root могли переименовать или удалить файл. 
Без набора бит привязки любой пользователь с разрешениями на запись и выполнение для каталога может переименовывать или удалять содержащиеся файлы независимо от владельца файла. 
Обычно это устанавливается в каталоге "/tmp, чтобы обычные пользователи не могли удалять или перемещать файлы других пользователей[[3]].
# Выполнение лабораторной работы

Создание программы

1. Войдите в систему от имени пользователя guest.

![Рис 1.Вход в систему](image/1.png){ #fig:001 width=70% }

[Рисунок 1](image/1.png)

2. Создайте программу simpleid.c:

![Рис 2.Программа simpleid.c](image/2.png){ #fig:002 width=70% }

[Рисунок 2](image/2.png)

3. Скомплилируйте программу и убедитесь, что файл программы создан:

![Рис 3.Компиляция программы](image/3.png){ #fig:003 width=70% }

[Рисунок 3](image/3.png)

![Рис 4.Файл программы](image/4.png){ #fig:004 width=70% }

[Рисунок 4](image/4.png)

4. Выполните программу simpleid:

![Рис 5.Выполнение программы](image/5.png){ #fig:005 width=70% }

[Рисунок 5](image/5.png)

5. Выполните системную программу id и сравните полученный вами результат с данными предыдущего пункта задания.

![Рис 6.Команда id](image/6.png){ #fig:006 width=70% }

[Рисунок 6](image/6.png)

Вывод программы simpleid и вывод системной программы id совпадает

6. Усложните программу, добавив вывод действительных идентификаторов:

Получившуюся программу назовите simpleid2.c.

![Рис 7.Программа simpleid2.c](image/7.png){ #fig:007 width=70% }

[Рисунок 7](image/7.png)

7. Скомпилируйте и запустите simpleid2.c:

![Рис 8.Компеляция simpleid2.c](image/8.png){ #fig:008 width=70% }

[Рисунок 8](image/8.png)

8. От имени суперпользователя выполните команды:

chown root:guest "/home/guest/simpleid2
chmod u+s "/home/guest/simpleid2

![Рис 9.Команды chown и chmod](image/9.png){ #fig:009 width=70% }

[Рисунок 9](image/9.png)

9. Используйте sudo или повысьте временно свои права с помощью su.
Поясните, что делают эти команды.
Команда: chown root:guest "/home/guest/simpleid2 меняет владельца файла simpleid2
Команда: chmod u+s "/home/guest/simpleid2 меняет атрибуты на файле simpleid2

10. Выполните проверку правильности установки новых атрибутов и смены
владельца файла simpleid2:

![Рис 10.Проверка правильности установки новых атрибутов](image/10.png){ #fig:010 width=70%}

[Рисунок 10](image/10.png)

11. Запустите simpleid2 и id:
Сравните результаты.

![Рис 11.Результаты simpleid2 и id](image/11.png){ #fig:011 width=70%}

[Рисунок 11](image/11.png)

Результаты вывода программ simpleid2 и id совпадают

12. Проделайте тоже самое относительно SetGID-бита.
13. Создайте программу readfile.c:

![Рис 12.Программа readfile.c](image/12.png){ #fig:012 width=70%}

[Рисунок 12](image/12.png)

14. Откомпилируйте её.

![Рис 13.Компиляция readfile.c](image/13.png){ #fig:013 width=70%}

[Рисунок 13](image/13.png)

15. Смените владельца у файла readfile.c и измените права так, чтобы только суперпользователь
(root) мог прочитать его, a guest не мог.

![Рис 14.Изменение владельца файла readfile.c и изменение прав доступа](image/14.png){ #fig:014 width=70%}

[Рисунок 14](image/14.png)

16. Проверьте, что пользователь guest не может прочитать файл readfile.c.

![Рис 15.Проверка доступа](image/15.png){ #fig:015 width=70%}

[Рисунок 15](image/15.png)

17. Смените у программы readfile владельца и установите SetU’D-бит.
18. Проверьте, может ли программа readfile прочитать файл readfile.c

![Рис 16.Проверка](image/16.png){ #fig:016 width=70%}

[Рисунок 16](image/16.png)

19. Проверьте, может ли программа readfile прочитать файл "/etc/shadow

![Рис 17.Проверка](image/17.png){ #fig:017 width=70%}

[Рисунок 17](image/17.png)


Исследование Sticky-бита

1. Выясните, установлен ли атрибут Sticky на директории "/tmp

![Рис 18.Проверка наличия атрибута](image/18.png){ #fig:018 width=70%}

[Рисунок 18](image/18.png)

2. От имени пользователя guest создайте файл file01.txt в директории "/tmp со словом test:

![Рис 19.Создание файла file01.txt](image/19.png){ #fig:019 width=70%}

[Рисунок 19](image/19.png)

3. Просмотрите атрибуты у только что созданного файла и разрешите чтение и запись для категории пользователей «все остальные»:

![Рис 20.Атрибуты файла file01.txt](image/20.png){ #fig:020 width=70%}

[Рисунок 20](image/20.png)

![Рис 21.Атрибуты файла file01.txt](image/21.png){ #fig:021 width=70%}

[Рисунок 21](image/21.png)

4. От пользователя guest2 (не являющегося владельцем) попробуйте прочитать файл "/tmp/file01.txt:

![Рис 22.Чтение файла file01.txt](image/22.png){ #fig:022 width=70%}

[Рисунок 22](image/22.png)

5. От пользователя guest2 попробуйте дозаписать в файл "/tmp/file01.txt слово test2 

![Рис 23.Запись в файл file01.txt](image/23.png){ #fig:023 width=70%}

[Рисунок 23](image/23.png)

6. Проверьте содержимое файла

![Рис 24.Проверка содержимого](image/24.png){ #fig:024 width=70%}

[Рисунок 24](image/24.png) 

9. От пользователя guest2 попробуйте удалить файл "/tmp/file01.txt 

![Рис 25.Проверка возможности удаления](image/25.png){ #fig:025 width=70%}

[Рисунок 25](image/25.png)

10. Повысьте свои права до суперпользователя и выполните после этого команду, снимающую атрибут t (Sticky-бит) с директории "/tmp:

![Рис 26.Вход от имени суперпользователя](image/26.png){ #fig:026 width=70%}

[Рисунок 26](image/26.png)

![Рис 27.Снимаем атрибут t](image/27.png){ #fig:027 width=70%}

[Рисунок 27](image/27.png)

11. Покиньте режим суперпользователя

![Рис 28.Покидаем режим суперпользователя](image/28.png){ #fig:028 width=70%}

[Рисунок 28](image/28.png)

12. От пользователя guest2 проверьте, что атрибута t у директории "/tmp нет:

![Рис 29.Проверяем наличие атрибута t](image/29.png){ #fig:029 width=70%}

[Рисунок 29](image/29.png)

13. Повторите предыдущие шаги.

![Рис 30.Повторяем предыдущие шаги](image/30.png){ #fig:030 width=70%}

[Рисунок 30](image/30.png)

14. Удалось ли вам удалить файл от имени пользователя, не являющегося его владельцем? 
Нет, файл удалить не удалось

15. Повысьте свои права до суперпользователя и верните атрибут t на директорию "/tmp:

![Рис 31.Возвращаем атрибут](image/31.png){ #fig:031 width=70%}

[Рисунок 31](image/31.png)

# Выводы

Получены практические навыки работы в консоли с дополнительными атрибутами. Рассмотрены работы механизма
смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

# Список литературы{.unnumbered}
[1. Повышение привилегий в ОС Linux через SUID/SGID](https://habr.com/ru/company/jetinfosystems/blog/506750/)
[2. Использование SETUID, SETGID и Sticky bit для расширенной настройки прав доступа в операционных системах Linux](https://ruvds.com/ru/helpcenter/suid-sgid-sticky-bit-linux/)
[3. Setuid](https://en.wikipedia.org/wiki/Setuid)
[4. Права доступа Unix, SUID, SGID, Sticky биты](https://help.ubuntu.ru/wiki/стандартные_права_unix)

::: {#refs}
:::
