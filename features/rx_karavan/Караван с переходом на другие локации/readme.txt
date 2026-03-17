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


но потребуются изменения:
изменения коснувшиеся smart_terrain.script
local team_smtr_assigned = {} -- добавим эту строку перед

function se_smart_terrain:enabled(obj)
добавим 

  if rx_karavan and rx_karavan.obj_id_in_team(obj.id) then
  -- Если NPC находится на одном уровне с ГГ или дан приказ оставаться на месте, выкидываем его из смтр
  -- иначе назначаем NPС из команды первый попавшийся смарт-террейн
    local npc_level = alife():level_name(game_graph():vertex(obj.m_game_vertex_id):level_id())
    if npc_level==level.name() then
      team_smtr_assigned[obj.id]=nil
      return false
    else
      if team_smtr_assigned[obj.id]==self.id then
        return true
      elseif team_smtr_assigned[obj.id] then
        return false
      else
        -- mylog("assigning ".. self:name() .. " to " .. obj:name() .. " on " .. npc_level)
        team_smtr_assigned[obj.id]=self.id
        return true
      end
    end
  end


function se_smart_terrain:suitable( obj )

добавим
  if team_smtr_assigned[obj.id]==self.id then
  -- Наш человек
    return 100000
  end


function se_smart_terrain:task( obj )

добавим
  if rx_karavan and rx_karavan.obj_id_in_team(obj.id) then
    -- Наш человек
    rx_karavan.update_level_names()
    local dest=rx_karavan.get_offline_dest()
--    mylog("Setting task " .. dest .. " for " .. obj:name())
    return CALifeSmartTerrainTask( dest )
  end
  
  
Важный параметр: очистка пстор при смерти нпс:

xr_motivator.script

function motivator_binder:death_callback(victim, who)

в конце добавим
  if rx_karavan and rx_karavan.obj_id_in_team(self.object:id()) then
          local npc_id = self.object:id()
        --log1("Отключение схемы karavan для NPC ID: " .. tostring(npc_id))
        rx_karavan.remove_from_actor_pstor(npc_id)
        --log1("Схема karavan для NPC ID: " .. tostring(npc_id) .. " отключена")
    return false
  end
  
Нужно для корректного перехода нпс:


xr_camper.script

function evaluator_end:evaluate()

добавим
  if rx_karavan and rx_karavan.obj_id_in_team(self.object:id()) then
  -- Чтобы не пытался ходить по патрульным путям с другого уровня
    return true
  end


xr_sleeper.script

function evaluator_need_sleeper:evaluate ()

добавим
  if rx_karavan and rx_karavan.obj_id_in_team(self.object:id()) then
  -- Чтобы не пытался ходить по патрульным путям с другого уровня
    return false
  end


xr_walker.script
function evaluator_need_walker:evaluate()

добавим
  if rx_karavan and rx_karavan.obj_id_in_team(self.object:id()) then
  -- Чтобы не пытался ходить по патрульным путям с другого уровня
    return false
  end