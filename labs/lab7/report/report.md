---
## Front matter
title: "Отчёт по лабораторной работе №7"
subtitle: "Исследование эффективности рекламы"
author: "Надежда Александровна Рогожина"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: true # List of tables
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
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Задание

29 января в городе открылся новый салон красоты. Полагаем, что на момент открытия о салоне знали N 0
потенциальных клиентов. По маркетинговым исследованиям известно, что в районе проживают N потенциальных клиентов салона. Поэтому после открытия салона руководитель запускает активную рекламную компанию. После этого скорость изменения числа знающих о салоне пропорциональна как числу знающих о нем, так и числу не знаю о нем.

Постройте график распространения рекламы, математическая модель которой описывается
следующим уравнением:

1. $\frac{dn}{dt} = (0.55 + 0.0001 n(t))(N-n(t))$

2. $\frac{dn}{dt} = (0.00005 + 0.2 n(t))(N-n(t))$

3. $\frac{dn}{dt} = (0.5 sin(t) + 0.3 cos(t) n(t))(N-n(t))$

При этом объем аудитории N=500, в начальный момент о товаре знает 5 человек. Для случая 2 определите в какой момент времени скорость распространения рекламы будет иметь максимальное значение.

# Теоретическое введение

Организуется рекламная кампания нового товара или услуги. Необходимо, чтобы прибыль будущих продаж с избытком покрывала издержки на рекламу. Вначале расходы могут превышать прибыль, поскольку лишь малая часть потенциальных покупателей будет информирована о новинке. Затем, при увеличении числа продаж, возрастает и прибыль, и, наконец, наступит момент, когда рынок насытиться, и рекламировать товар станет бесполезным.

Предположим, что торговыми учреждениями реализуется некоторая продукция, о которой в момент времени t из числа потенциальных покупателей N знает лишь n покупателей. Для ускорения сбыта продукции запускается реклама по радио, телевидению и других средств массовой информации. После запуска рекламной кампании информация о продукции начнет распространяться среди потенциальных покупателей путем общения друг с другом. Таким образом, после запуска рекламных объявлений скорость изменения числа знающих о продукции людей пропорциональна как числу знающих о товаре покупателей, так и числу покупателей о нем не знающих.

# Выполнение лабораторной работы

Используя этот код:
```
using DifferentialEquations, Plots


N = 500
n0 = 5
f(n,p,t) = (p[1]+p[2]*n)*(N - n)
ff(n,p,t) = (p[1]*sin(t)+p[2]*cos(t)*n)*(N-n)
p1 = [0.55, 0.0001]
tspan1 = (0.0, 10.0)
prob1 = ODEProblem(f, n0, tspan1, p1)

sol1 = solve(prob1, Tsit5(), saveat=0.01)
plot(sol1, markersize =:15, yaxis="N(t)",  label="N(t)")
```
и его немного измененные версии (где изменялись коэффициенты), получилось построить 3 графика распространения рекламы, включая отмеченную точку наибольшей скорости распространения рекламы (через нахождение значения скорости в определенной точке времени), которая была найдена на моменте времени t=460 (рис. [-@fig:001], рис. [-@fig:002], рис. [-@fig:003]).

![(0.55 + 0.0001 n(t))(N-n(t))](image/fig1.png){#fig:001 width=70%}

![(0.00005 + 0.2 n(t))(N-n(t))](image/fig2.png){#fig:002 width=70%}

![(0.5 sin(t) + 0.3 cos(t) n(t))(N-n(t))](image/fig3.png){#fig:003 width=70%}

Также, была смоделирована та же модель с помощью `OpenModelica`.

Первое уравнение (рис. [-@fig:004], рис. [-@fig:005]):

![Код](image/1.png){#fig:004 width=70%}

![(0.55 + 0.0001 n(t))(N-n(t))](image/2.png){#fig:005 width=70%}

Второе уравнение (рис. [-@fig:006], рис. [-@fig:007], рис. [-@fig:008]):

![Код](image/3.png){#fig:006 width=70%}

![(0.00005 + 0.2 n(t))(N-n(t))](image/4.png){#fig:007 width=70%}

![Точка наибольшей скороски](image/5.png){#fig:008 width=70%}

Третье уравнение (рис. [-@fig:009], рис. [-@fig:010]):

![Код](image/6.png){#fig:009 width=70%}

![(0.5 sin(t) + 0.3 cos(t) n(t))(N-n(t))](image/7.png){#fig:010 width=70%}


# Выводы

В ходе лабораторной работы было смоделировано поведение рекламы с помощью 2-х средств: `Julia` и `OpenModelica`.

# Список литературы{.unnumbered}

::: {#refs}
:::
