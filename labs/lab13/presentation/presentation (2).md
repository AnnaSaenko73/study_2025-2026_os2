---
## Front matter
lang: ru-RU
title: Презентация по лабораторной работе №13
subtitle: Настройка пакетного фильтра (firewalld)
author:
  - Анна Саенко
institute:
  - Российский университет дружбы народов, Москва, Россия
date: \today

## i18n babel
babel-lang: russian
babel-otherlangs: english

## Formatting pdf
toc: false
slide_level: 2
aspectratio: 169
section-titles: true
theme: metropolis
header-includes:
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
---

# Цели и задачи работы

## Цель лабораторной работы

Получить практические навыки работы с брандмауэром Linux и конфигурацией пакетного фильтра с использованием `firewall-cmd` и `firewall-config`.

## Задачи лабораторной работы

1 Определить активную зону и доступные службы  
2 Добавить сервис в runtime и permanent конфигурации  
3 Добавить порт TCP/UDP в зону  
4 Изменить конфигурацию через GUI (`firewall-config`)  
5 Проверить применение изменений

# Ход выполнения работы

## Определение активной зоны и доступных служб

![Получение информации о зонах и сервисах](Screenshot_1.png){ #fig:001 width=70% }

## Просмотр текущей конфигурации зоны

![Просмотр активной зоны](Screenshot_2.png){ #fig:002 width=70% }

## Добавление сервиса vnc-server (runtime)

![Добавление службы vnc-server](Screenshot_3.png){ #fig:003 width=70% }

## Добавление сервиса vnc-server (permanent)

![Добавление в постоянную конфигурацию](Screenshot_4.png){ #fig:004 width=70% }

## Добавление порта 2022/TCP

![Добавление порта 2022](Screenshot_5.png){ #fig:005 width=70% }

## Включение служб (ftp, http, https)

![Добавление служб через GUI](Screenshot_6.png){ #fig:006 width=70% }

## Добавление порта 2022/udp

![Добавление порта через GUI](Screenshot_7.png){ #fig:007 width=70% }

## Настройка служб telnet, imap, pop3, smtp

![Финальная конфигурация](Screenshot_9.png){ #fig:008 width=70% }

# Выводы

## В ходе лабораторной работы были выполнены:

- просмотр и анализ текущей зоны и служб;
- добавление сервиса в runtime и permanent конфигурации;
- объяснение различий между временными и постоянными настройками;
- добавление сетевых портов;
- использование GUI `firewall-config` для управления службами.
