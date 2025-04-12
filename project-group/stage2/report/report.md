---
## Front matter
title: "Исследование плавления и затвердевания малых кластеров"
subtitle: "Отчет по 2 этапу проекта"
author: "Лихтенштейн А.А., Рогожина Н.А., Шилоносов Д.В., Гэинэ А."

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

# Цель работы

Целью данного этапа является изучение теоретических основ метода молекулярной динамики и построение модели для исследования процессов плавления и затвердевания малых кластеров с “магическими” числами частиц.

# Задание

1. Изучить теоретические основы метода молекулярной динамики
2. Рассмотреть особенности фазовых переходов в малых кластерах
3. Разработать физическую модель для исследования плавления и затвердевания малых кластеров с “магическими” числами частиц (7, 19, 37)
4. Определить необходимые параметры и алгоритмы для дальнейшего моделирования

# Алгоритмы, используемые в работе

1. **Алгоритм генерации гексагональных кластеров**
	- **Цель**: Создание начальной конфигурации кластера с "магическими" числами частиц (7, 19, 37).
	- **Шаги**:
		1. Центральная частица размещается в начале координат.
		2. Последующие частицы добавляются концентрическими оболочками вокруг центра.
		3. Для каждой оболочки рассчитываются координаты частиц с использованием углов и радиусов, обеспечивающих гексагональную симметрию.
	- **Формула**: Координаты частиц в оболочке `shell`:
	$$angle = \frac{2\pi i}{6 \cdot shell}, positions[i] = [shell \cdot b \cdot \cos(angle), shell \cdot b \cdot \sin(angle)$$

2. **Алгоритм Верле (скоростная форма)**
	- **Цель**: Интегрирование уравнений движения частиц с высокой точностью.
	- **Шаги**:
		1. Обновление скоростей на половину шага:
        $$\vec{r}_i^{n+1/2} = \vec{r}_i^n + \vec{v}_i^n \cdot \frac{\Delta t}{2}$$
		2. Обновление позиций:
        $$\vec{r}_i^{n+1} = \vec{r}_i^n + \vec{v}_i^{n+1/2} \cdot \Delta t$$
		3. Пересчёт ускорений на основе новых позиций.
		4. Завершение обновления скоростей:
        $$\vec{v}_i^{n+1} = \vec{v}_i^{n+1/2} + \vec{a}_i^{n+1} \cdot \frac{\Delta t}{2}$$

3. **Расчёт термодинамических характеристик**
	- **Температура**:
     $$T = \frac{2}{(2N-3)k} \sum_i \frac{m_i(\vec{v}_i-\vec{v}_{cm})^2}{2}$$
     где $\vec{v}_{cm}$ — скорость центра масс кластера, k — постоянная Больцмана.
	- **Флуктуации длины связи**:
     $$\delta = \sqrt{\frac{2}{N(N-1)} \sum_{i<j} \frac{\langle r_{ij}^2 \rangle - \langle r_{ij} \rangle^2}{\langle r_{ij} \rangle}}$$
	- **Теплоемкость**:
     $$C = \frac{dE}{dT}$$
	
4. **Анализ фазовых переходов**
	- **Методы**:
		- **Пик теплоемкости**: Определяется через производную энергии по температуре.
	- **Критерий Линдеманна**: Плавление фиксируется при превышении порога флуктуаций длины связи (обычно 0.1).
	- **Гистерезис**: Сравнение кривых нагрева и охлаждения для выявления различий.

5. **Визуализация данных**
	- **Методы**:
		- Построение графиков зависимостей (температура, энергия, теплоемкость).
		- Анимация движения частиц с отображением связей.
		- Парная корреляционная функция для анализа структуры.

6. **Анализ зависимости температуры плавления от размера кластера**
	- **Формула**:
		$$T_{melt} = T_{bulk} = - \frac{c}{N^{1/3}}$$
	- **Метод**: Линейная регрессия для определения $T_{bulk}$ и $c$.

### Ключевые особенности:
- **Стабильность**: Ограничение максимальных сил и скоростей для предотвращения численных ошибок.
- **Гибкость**: Поддержка различных "магических" чисел и размеров кластеров.
- **Автоматизация**: Интеграция всех этапов (генерация, моделирование, анализ, визуализация) в единый pipeline (`main.py`).

# Выводы

Эти алгоритмы позволяют исследовать уникальные свойства нанокластеров, такие как оболочечное плавление и размерные эффекты, что соответствует целям работы.

# Список литературы{.unnumbered}

::: {#refs}
:::
