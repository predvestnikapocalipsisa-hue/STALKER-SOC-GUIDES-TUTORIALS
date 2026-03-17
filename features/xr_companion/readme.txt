-- -*- mode: lua; coding: windows-1251-dos -*-
Схема Компаньон, сделана в чате, на основе схем: Караван(Rulix aka Bak), Напарники(Red75).

НПС могут переходить на другие локации вместе с актором.
Работает на оригинале 1.0006 и ОГСР

Схему можно применять через диалог:
function main322(actir, obj)
  xr_companion.add_to_actor_pstor(obj:id())
end

Отключить схему в диалоге:
function main32(actor, obj)
  xr_companion.remove_from_actor_pstor(obj:id())
end

Подключение:
xr_companion.script кидаем к себе с заменой


gamedata\scripts\modules.script

раскомментируем или добавим строку в:
----------------------------------------------------------------------
-- Загрузка модулей сталкеров:
----------------------------------------------------------------------
load_scheme("xr_companion",   "companion",   stype_stalker)


gamedata\scripts\xr_logic.script
function enable_generic_schemes(ini, npc, stype, section)
добавим строку:
    xr_companion.set_scheme(npc, ini, "companion", section)