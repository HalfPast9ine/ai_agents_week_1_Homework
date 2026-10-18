# Шаг 1
- Тема набора
 
Факты 2026 г. из англоязычной Википедии по различным видам спорта.

- Откуда факты
    "2026 in sports",
    "2026 in association football",
    "2026 FIFA World Cup",
    "2026 Winter Olympics",
    "2026 in basketball",
    "2026 in tennis",
    "2026 in Formula One",
    "2026 in motorsport",
    "2026 in cycling",
    "2026 in athletics",
    "2026 in swimming",
    "2026 in ice hockey",
    "2026 in baseball",
    "2026 in American football",
    "2026 in rugby union",
    "2026 in cricket",
    "2026 in golf",
    "2026 in boxing",
    "2026 in mixed martial arts",
    "2026 in figure skating",
    "2026 in chess",
    "2026 in esports",
    "2026 in weightlifting",
    "2026 in bodybuilding",
    "2026 Mr. Olympia",
    "Arnold Classic",
    "2026 Arnold Classic",
    "IFBB Professional League",
    "2026 in combat sports"

в Википедии.


### Запуски скриптов
- $ python check_fresh.py data/fresh.jsonl

записей: 150, проблем: 0

- $ python check_fresh.py data/fresh.jsonl --sonnet

$ python check_fresh.py data/fresh.jsonl --sonnet
записей: 150, проблем: 0

Sonnet без инструментов: 2 из 150 верно, 1%. Потрачено $0.245
критерий свежести пройден
  fresh-20: In the 2026 FIFA World Cup, what format was used for the knockout stage starting June 28? -> The Round of 32 knockout format was used
  fresh-43: Who was sworn in as the Mayor of New York City on January 1, 2026? -> Zohran Mamdani was sworn in as the Mayor

accuracy = 0.01

# Шаг 2
Добавлен tool news отвечающий за поиск по новостям DuckDuckGo. 

# Шаг 3
Таблица репорт


config	model	n	accuracy	cost_per_task	cost_per_correct	avg_steps	avg_seconds
0	дешёвая без инструментов	gpt-4o-mini	150	0.00	0.00005	inf	1.00	2.0
1	дешёвая - поиск + чтение + news	gpt-4o-mini	150	0.31	0.00055	0.00177	3.54	11.6
2	дешёвая - поиск	gpt-4o-mini	150	0.19	0.00064	0.00330	3.19	8.6
3	дешёвая - поиск + чтение	gpt-4o-mini	150	0.40	0.00099	0.00248	5.16	12.7
4	mid без инструментов	claude-haiku-4.5	150	0.02	0.00193	0.09627	1.00	3.7
5	сильная без инструментов	claude-sonnet-4.6	150	0.13	0.00765	0.06036	1.00	8.5


по accuracy выигрывает дешёвая - поиск + чтение	gpt-4o-mini; accuracy =	0.40
интересно то, что добавление инструмента поимка по новостям news уменьшило accuracy до 0.31, однако она лучше по cost_per_task и cost_per_correc

сильная без инструментов дала accuracy = 0.15, при этом оказалась самой дорогой.


**Вывод:** агент на дешевой модели с простейшими тулами лучше и дешевле, чем дорогая модель. 

## Разбор неудачных трейсов:

traces/дешёвая - поиск + чтение/fresh-126.json
ответ верный, но не прошёл проверку: агент написал March 20, 2026, эталон 20 March.

traces/дешёвая - поиск + чтение/fresh-10.json
нашёл не то: web_search открыл взрыв в Бейруте и списки аварий, запросы ушли в 2023 год; page_find по «2026 in China» не вызван, в FINAL угадано 11.




