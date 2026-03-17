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
	
	
изменения коснувшиеся smart_terrain.script

local team_smtr_assigned = {} -- добавим эту строку перед

function se_smart_terrain:enabled(obj)

добавим 
  if xr_companion and xr_companion.obj_id_in_team(obj.id) then
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
  if xr_companion and xr_companion.obj_id_in_team(obj.id) then
    -- Наш человек
    xr_companion.update_level_names()
    local dest=xr_companion.get_offline_dest()
--    mylog("Setting task " .. dest .. " for " .. obj:name())
    return CALifeSmartTerrainTask( dest )
  end
  
  
Очистка пстор при смерти нпс, и функции влияющие на перемещение нпс

xr_motivator.script

function motivator_binder:death_callback(victim, who)

в конце добавим
  if xr_companion and xr_companion.obj_id_in_team(self.object:id()) then
          local npc_id = self.object:id()
        --log1("Отключение схемы companion для NPC ID: " .. tostring(npc_id))
        xr_companion.remove_from_actor_pstor(npc_id)
        --log1("Схема companion для NPC ID: " .. tostring(npc_id) .. " отключена")
    return false
  end


xr_camper.script

function evaluator_end:evaluate()

добавим
  if xr_companion and xr_companion.obj_id_in_team(self.object:id()) then
  -- Чтобы не пытался ходить по патрульным путям с другого уровня
    return true
  end


xr_sleeper.script

function evaluator_need_sleeper:evaluate ()

добавим
  if xr_companion and xr_companion.obj_id_in_team(self.object:id()) then
  -- Чтобы не пытался ходить по патрульным путям с другого уровня
    return false
  end


xr_walker.script

function evaluator_need_walker:evaluate()

добавим
  if xr_companion and xr_companion.obj_id_in_team(self.object:id()) then
  -- Чтобы не пытался ходить по патрульным путям с другого уровня
    return false
  end	