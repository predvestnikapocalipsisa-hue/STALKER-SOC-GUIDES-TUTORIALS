-- -*- mode: lua; coding: windows-1251-dos -*-
В чате  разбирали как работают схемы в сталкер более углубленно и сделали свою схему собирательства бесхозного лута и обыска трупов!
Имя ей [finder] работает она всегда, поэтому в логике включать не нужно, НО можно выключить, например, так:
[walker]
path_walk = my_way_walk1
no_loot = true
Или в xr_finder.script дописать свой стори айди
local excluded_npc_story_ids = {
    [6] = true,   -- NPC с story_id = 6
    [15] = true,
    [123] = true,
}
Схему можете потестить, но предупреждаю пока всё сыровато.
Подключение:
xr_finder.script - закинуть в \gamedata\scripts
modules.script
load_scheme("xr_finder",      "finder",      stype_stalker)
xr_logic.script
в function enable_generic_schemes(ini, npc, stype, section) добавить

xr_finder.set_scheme(npc, ini, "finder", section)

в function reset_generic_schemes_on_scheme_switch(npc, scheme, section) добавить

xr_finder.reset_finder(npc, scheme, st, section)