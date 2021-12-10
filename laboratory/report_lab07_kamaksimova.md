---
# Front matter
title: "Отчёт по лабораторной работе 7"
subtitle: "Элементы криптографии. Однократное гаммирование"
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

Освоить на практике применение режима однократного гаммирования.

# Задание

Разработать приложение, позволяющее шифровать и дешифровать данные в режиме однократного гаммирования.

# Теоретическое введение

Смысл однократного гаммирования заключается в наложении (снятии) на открытые (зашифрованные) данные
последовательности элементов других данных,полученной с помощью некоторого криптографического алгоритма, 
для получения зашифрованных (открытых) данных. Иными словами, наложение
гаммы — это сложение её элементов с элементами открытого (закрытого)
текста по некоторому фиксированному модулю, 
значение которого представляет собой известную часть алгоритма шифрования [[1]](https://esystem.rudn.ru/pluginfile.php/1198312/mod_resource/content/2/007-lab_crypto-gamma.pdf).

Преимущества однократного гаммирования[[1]](https://esystem.rudn.ru/pluginfile.php/1198312/mod_resource/content/2/007-lab_crypto-gamma.pdf):
1. Абсолютная стойкость шифра в случае, когда однократно используемый ключ, длиной, равной длине исходного сообщения,
является фрагментом истинно случайной двоичной последовательности с равномерным законом распределения. 
2. Криптоалгоритм не даёт никакой информации об открытом тексте: при известном зашифрованном сообщении
все различные ключевые последовательности возможны и равновероятны

При всех очевидных приемуществах, есть один весомый недостаток, который сразу бросается в глаза, - это необходимость иметь огромные объемы данных, 
которые можно было бы использовать в качестве гаммы. Для этих целей обычно пользуются датчиками настоящих случайных чисел[[2]](https://bugtraq.ru/library/books/crypto/chapter7/). 
 
Необходимые и достаточные условия абсолютной стойкости шифра[[1]](https://esystem.rudn.ru/pluginfile.php/1198312/mod_resource/content/2/007-lab_crypto-gamma.pdf):
- полная случайность ключа;
- равенство длин ключа и открытого текста;
- однократное использование ключа

# Выполнение лабораторной работы

Программа, позволяющая шифровать и дешифровать данные в режиме однократного гаммирования.

![Рис 1.Код программы](image/1.png){ #fig:001 width=70% }

[Рисунок 1](image/1.png)

# Выводы

Разработано приложение, позволяющее шифровать и дешифровать данные в режиме однократного гаммирования.

# Список литературы{.unnumbered}

[1. Элементы криптографии. Однократное гаммирование](https://esystem.rudn.ru/pluginfile.php/1198312/mod_resource/content/2/007-lab_crypto-gamma.pdf)

[2. Прикладные задачи шифрования](https://bugtraq.ru/library/books/crypto/chapter7/)

::: {#refs}
:::
