-- -*- mode: lua; coding: windows-1251-dos -*-
ОРИГИНАЛЬНАЯ СХЕМА КАРАВАН ТЧ 1.0006. Стыбзил схему karavan из ОП2. Схема каравана простая - гг ведет чудиков

Такую логику можно дать неограниченному количеству нпс.

Логика нпс:
[smart_terrains]
none = true

[logic]
active = karavan

[karavan]
close_dist = 2
near_dist = 10
faraway_dist = 31
close_state = guard
near_state = rush
faraway_state = sprint
wait_state = rush
look_on_actor = false 
radius = 1 
combat_ignore_cond = {=fighting_dist_ge(40)} always


Подключение:
gamedata\scripts\modules.script

добавим строку в:
----------------------------------------------------------------------
-- Загрузка модулей сталкеров:
----------------------------------------------------------------------

load_scheme("rx_karavan",     "karavan",     stype_stalker)