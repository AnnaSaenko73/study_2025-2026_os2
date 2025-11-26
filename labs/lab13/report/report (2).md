---
## Front matter
title: "Отчёт по лабораторной работе №13"
subtitle: "Фильтр пакетов"
author: "Анна Саенко"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true
toc-depth: 2
lof: true
lot: true
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
    - spelling=modern
    - babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: IBM Plex Serif
romanfont: IBM Plex Serif
sansfont: IBM Plex Sans
monofont: IBM Plex Mono
mathfont: STIX Two Math
mainfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
romanfontoptions: Ligatures=Common,Ligatures=TeX,Scale=0.94
sansfontoptions: Ligatures=Common,Ligatures=TeX,Scale=MatchLowercase,Scale=0.94
monofontoptions: Scale=MatchLowercase,Scale=0.94,FakeStretch=0.9
mathfontoptions:
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
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float}
  - \floatplacement{figure}{H}
---

# Цель работы  

Получить навыки настройки пакетного фильтра в Linux.

# Ход выполнения работы  

## Управление брандмауэром с помощью firewall-cmd

Для начала работы я получила права суперпользователя и определила параметры текущей зоны.  
Я узнала, какая зона используется по умолчанию, какие зоны доступны в системе, а также просмотрела список всех поддерживаемых сервисов.  
На скриншоте видно вывод команд с информацией о зоне и доступных службах.

![Получение информации о зонах и сервисах](Screenshot_1.png){ #fig:001 width=80% }

### Просмотр текущей конфигурации

Я проверила, какие службы разрешены в активной зоне.  
Затем сравнила вывод конфигурации при просмотре стандартной зоны и при указании зоны *public*.  
Поскольку зона *public* является активной по умолчанию, результаты совпали.

![Просмотр активной зоны](Screenshot_2.png){ #fig:002 width=80% }

### Добавление сервиса VNC

Я добавила службу `vnc-server` в конфигурацию времени выполнения.  
Сервис появился в списке.

![Добавление службы vnc-server](Screenshot_3.png){ #fig:003 width=80% }

После перезапуска службы firewalld изменения исчезли.  
Это произошло потому, что служба была добавлена только во временную конфигурацию (runtime), которая теряется при перезагрузке службы.

### Добавление сервиса VNC в постоянную конфигурацию

Я повторно добавила службу `vnc-server`, но на этот раз в постоянную конфигурацию.  
Сервис не появился сразу в runtime, так как постоянные изменения не применяются автоматически.  
После перезагрузки конфигурации изменения вступили в силу.

![Добавление в постоянную конфигурацию](Screenshot_4.png){ #fig:004 width=80% }

### Добавление порта 2022

Я добавила порт 2022/TCP в постоянную конфигурацию и перезагрузила настройки.  
После обновления конфигурации порт появился в списке.

![Добавление порта 2022](Screenshot_5.png){ #fig:005 width=80% }

## Управление брандмауэром через firewall-config (GUI)

Я запустила графическую утилиту и переключила режим на *Permanent*.  
В зоне *public* были включены службы ftp, http и https.

![Добавление служб через GUI](Screenshot_6.png){ #fig:006 width=80% }

На вкладке *Ports* я добавила порт 2022/udp.

![Добавление порта через GUI](Screenshot_7.png){ #fig:007 width=80% }

После выхода из утилиты изменения ещё не были активны.  
Загрузка конфигурации применила их к runtime, и новые параметры стали видны.

![Финальная конфигурация](Screenshot_8.png){ #fig:008 width=80% }

## Самостоятельная работа

Я настроила доступ к службам telnet, imap, pop3 и smtp:

– telnet добавлен через командную строку;  
– imap, pop3 и smtp включены через графический интерфейс `firewall-config`.

Изменения сохранены как постоянные, и будут активны после перезагрузки системы.

![Финальная конфигурация](Screenshot_9.png){ #fig:009 width=80% }


# Контрольные вопросы

1. **Какая служба должна быть запущена перед началом работы с менеджером конфигурации брандмауэра firewall-config?**  
   Перед запуском `firewall-config` должна быть запущена служба `firewalld`.

2. **Какая команда позволяет добавить UDP-порт 2355 в конфигурацию брандмауэра в зоне по умолчанию?**  
   Для добавления порта используется команда:  
   `firewall-cmd --add-port=2355/udp --permanent`

3. **Какая команда позволяет показать всю конфигурацию брандмауэра во всех зонах?**  
   Для отображения полной конфигурации используется команда:  
   `firewall-cmd --list-all-zones`

4. **Какая команда позволяет удалить службу vnc-server из текущей конфигурации брандмауэра?**  
   Удаление службы выполняется командой:  
   `firewall-cmd --remove-service=vnc-server`

5. **Какая команда firewall-cmd позволяет активировать новую конфигурацию, добавленную опцией --permanent?**  
   Чтобы применить постоянные изменения, используется команда:  
   `firewall-cmd --reload`

6. **Какой параметр firewall-cmd позволяет проверить, что новая конфигурация была добавлена в текущую зону и теперь активна?**  
   Проверка активной конфигурации выполняется командой:  
   `firewall-cmd --list-all`

7. **Какая команда позволяет добавить интерфейс eno1 в зону public?**  
   Добавление интерфейса в зону выполняется командой:  
   `firewall-cmd --zone=public --add-interface=eno1 --permanent`

8. **Если добавить новый интерфейс в конфигурацию брандмауэра, пока не указана зона, в какую зону он будет добавлен?**  
   Если зона не указана, интерфейс будет добавлен в **зону по умолчанию** (default zone), обычно это `public`.

# Заключение

В результате выполнения лабораторной работы я получила практические навыки управления брандмауэром Linux с помощью утилит `firewall-cmd` и `firewall-config`.  
Были выполнены следующие действия:

- определение активной зоны и просмотр доступных зон и служб;
- сравнение конфигурации зоны по умолчанию с явным указанием зоны;
- добавление сервиса в конфигурацию времени выполнения и в постоянную конфигурацию;
- объяснение различий между runtime- и permanent-настройками;
- добавление сетевых портов в брандмауэр;
- применение изменений и проверка итоговой конфигурации;
- использование графического интерфейса `firewall-config` для управления службами и портами.
