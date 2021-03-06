---
## Front matter
lang: ru-RU
title: Дискреционное  разграничение прав в Linux. Исследование влияния дополнительных атрибутов
author: |
	 Максимова Ксения НБИбд-02-18\inst{1}

institute: |
	\inst{1}Российский Университет Дружбы Народов

date: 12 ноября, 2021, Москва, Россия

## Formatting
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
toc: false
slide_level: 2
theme: metropolis
header-includes: 
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true

---

## Цель лабораторной работы 

Изучение механизмов изменения идентификаторов, применения
SetUID- и Sticky-битов. Получение практических навыков работы в консоли с дополнительными атрибутами. Рассмотрение работы механизма
смены идентификатора процессов пользователей, а также влияние бита
Sticky на запись и удаление файлов.

## Введение

Unix отслеживает не символьные имена владельцев и групп, а их идентификаторы (UID - для пользователей и GID для групп). 
Эти идентификаторы хранятся в файлах "/etc/passwd и "/etc/group соответственно. Символьные эквиваленты идентификаторов используются только для удобства, 
например, при использовании команды ls, идентификаторы заменяются соответствующими символьными обозначениями

## SUID - Set User ID 

SUID (Set User ID, Бит смены владельца) - это разрешение файловой системы Linux, которое позволяет запустить исполняемый файл от имени его владельца.
Другими словами, использование этого бита позволяет нам поднять привилегии пользователя в случае, если это необходимо.

s - установка ID группы

## SGID - Set Group ID 

SGID (Set Group ID) - файл будет запускаться пользователем от имени группы, которая владеет файлом

## Sticky bit 

Sticky bit - это флаг права доступа пользователя, который может быть назначен файлам и каталогам в Unix-подобных системах.

t – атрибут сохранности текста. Данный атрибут устанавливает ограничения на действия с директорией: пользователь может удалить или модифицировать только те файлы и директории, 
владельцем которых он является или имеет права записи для них.


## Результат лабораторной работы

Получены практические навыки работы в консоли с дополнительными атрибутами. Рассмотрены работы механизма
смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

 




