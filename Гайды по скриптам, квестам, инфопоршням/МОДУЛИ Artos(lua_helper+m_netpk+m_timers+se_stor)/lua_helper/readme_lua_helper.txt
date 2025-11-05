Модуль(скрипт) различных автономных 'общих' функций (SHoC:6/7(118),CS:8(124),SCoP:12(128))
lua_helper.script
==============================================================================================
Скрипт, добавляет в игру различные часто употребляемые и/или полезные функции которые могут использоваться модмейкерами.
--/---------------------------------------------------------------------
Подключение:
1. Скопировать в папку скриптов игры скрипт: lua_helper.script;
2. Добавить в конец _g.script строку инициализации:
  prefetch("lua_helper") --/#+# подключение модуля 'общих' хелп-функций
3. Внимание! Скрипт ориентирован на работу в в системе "Мод конструктор 'SIGMA'",
поэтому отдельные функции требуют подключенного к игре скрипта "lua_extension".
Для автономного использования скрипта возможно вам будет достаточно выполнить
его инициализацию из биндера актора (bind_stalker.script), добавив в него строку:
function actor_binder:reinit()
  --/ ... все предыдущие строки
  --/#+# -------------------------------------------------------------
  if lua_helper.Init_ActorPStor then lua_helper.Init_ActorPStor() end
  --/< ---------------------------------------------------------------
end

--/---------------------------------------------------------------------
Список функций добавляемых в глобальное пространство _G:
--/-----------------------------------------------------
HasVar - проверка наличия переменной в 'общем' хранилище
DelVar - удаление переменной из 'общего' хранилища
SetVar - запись/изменение переменной в хранилище 'общем'
GetVar - чтение переменной из 'общего' хранилища

has_info - проверка наличия инфопоршня у актора
has_info_portions - проверка наличия инфопоршней у актора
has_any_info_portions - проверка наличия какого-либо из инфопоршней у актора
check_info_portions - проверка наличия/отсутствия инфопоршней у актора
give_info - установка инфопоршня для актора
give_info_portions - установка инфопоршней для актора
disable_info - удаление инфопоршня у актора
disable_info_portions - удаление инфопоршней у актора

Get_CheckedFunc -
GetVarA - проверка наличия переменной в pstor'e актора
GetVarA_Table
SetVarA
DelVarA
GetVarObj - проверка наличия переменной в pstor'e объекта
GetVarObj_Table
SetVarObj
DelVarObj
GetSizeVar - размер переменной в pstor'ах или в хранилище (в байтах)
Get_MobClass
Dlg_AddPhrase
Get_Actor_NPC
Get_Actor
Get_NPC

ReadFromIni
Get_AmmoList
Get_GrenadeList
Get_IniSection
Get_IniSections
Get_Cfg_String
Get_Cfg_Bool
Get_Cfg_Number
Get_Cfg_Num32
Get_Cfg_2Prm
Get_InvName
Get_InvShortName
Get_WeightSection
Get_CostSection
Get_ClassSection
Get_CharName
Get_Community
get_object_community
IsSobjOb
Release_Obj,
Spawn_Obj
Spawn_NPC
create_ammo
SendTip
Send_ReceivedInfo
SpellingBeginning
SpellingTermination
Spelling
Has_Item
Has_Items
Has_AnyItems
SwitchToOffLine
SwitchToOnLine
Clear_Inventory
Spawn_AmmoInInv
Spawn_ItemInInv
Spawn_ItemsInInv
Give_Money
Lost_Money
Relocate_Money
Relocate_Items
Relocate_AnyItems
Reload_Item
Take_Item
Lost_Items
Add_MapSpot
Del_MapSpot
Get_MapIdObj
Get_MapNameObj
VecToStr
Parse_Names
Parse_StrToTbl
Parse_CustomData
Fill_CustomData
Dir2TabRad
Dir2TabDeg
Deg2Rad
Rad2Deg
AngleDiffRad
AngleDiffDeg

Get_StrTime
Get_StrTimeOrDate
set_seconds2ctime
Get_RestSeconds
Set_RestSeconds
Add_RestSeconds
Get_PastSeconds
Get_PastMinutes
Get_PastHours
ms2string
sec2string

Start_PT
Stop_PT
Get_PT
Get_MemUsage
--/---------------------------------------------------------------------
Artos (28.09.2013)
