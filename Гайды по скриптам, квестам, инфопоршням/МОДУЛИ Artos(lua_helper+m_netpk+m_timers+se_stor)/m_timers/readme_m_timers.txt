От себя добавлю, что если использвать таймер с выводом на худ с текстом, то можно получить вылет по отсутствию статика timer_hud_caption, который можно добавить в gamedata\config\ui\ui_custom_msgs.xml

	<timer_hud_caption x="852" y="80" width="157" height="67" light_anim="ui_slow_blinking_alpha" la_cyclic="1" la_texture="0" la_text="1" la_alpha="1">
		<text x="0" y="0" font="graffiti22" r="240" g="217" b="182" a="255" align="c"/>
	</timer_hud_caption>
	
Но я выбрал иной вариант, в m_timers.script в функции
function HudTimer:set_hud() --/ caption: false|string|nil|other	

заменил:
			hud:AddCustomStatic("timer_hud_caption", true)
			local wnd = hud:GetCustomStatic("timer_hud_caption"):wnd() --/ window frame caption
на:
			hud:AddCustomStatic("hud_timer_text", true)
			local wnd = hud:GetCustomStatic("hud_timer_text"):wnd() --/ window frame caption
==============================================================================================
Менеджер универсальных таймеров (SHoC:6/7(118),CS:8(124),SCoP:12(128))
m_timers.script
==============================================================================================
Модуль (скрипт) реализует в игре удобный интерфейс для работы с таймерами различного назначения (реального и игрового времени).
Для работы требуются скрипты "Расширения Lua"(lua_extension) и "Общие полезные функции" (lua_helper).
Для хранения в сэйвах игры табличных данных запущенных таймеров может использоваться модуль "Универсальное хранилище" (se_stor),
в противном случае данные сохраняются в pstor актора (что может вызвать переполнение при интенсивном использовании таймеров).

--/---------------------------------------------------------------------
Подключение:
1. Скопировать в папку скриптов игры скрипт: m_timers.script;
2. Добавить в биндер актора (см. bind_stalker.script) вызовы событий "сохранение игры", "загрузки сохранения" и запуска обновлений модуля;
3. Добавить в _g.script инициализацию дополнительного функционала (lua_extension и lua_helper).
4. (настоятельно рекомендуется) Подключить модуль "Универсальное хранилище" (se_stor).

Примечание:
В комплекте файл '_g_ADD.script является строками, которые требуется добавить в исходный _g.script.
Файл bind_stalker.script от оригинальной версии игры SHoC v.1.0006 дан в качестве рабочего примера.
В файл bind_stalker.script добавляются строки (в три разных метода биндера) отмеченные знаком --/#+# и имеющие подстроки 'm_timers.'

Использование:
1. Запрос существования (запущенного) таймера (по имени):
  timer_exists("my_timer_name") --/ boolean
2. Получение объекта таймера (по имени):
  get_timer("my_timer_name") --/ object from class 'RealTimer|GameTimer'
3. Получение текущего значения таймера:
  get_timer("my_timer_name"):get_time_rest()
4. Удаление запущенного таймера:
  get_timer("my_timer_name"):remove()
5. Таймер на игровом экране (HUD-Timer) допускается в единственном экземпляре:
  get_hud_timer() --/ object 'HUD-Timer' from class 'RealTimer'
6. quick-таймеры не имеют имен и недоступны извне.
7. Имена таймерам выдаются автоматически и эксклюзивны.
При необходимости имя может быть задано при запуске таймера (см.ниже).


8. Запуск таймера производится одной из соответствующих функций start_XXX_timer:
 start_real_timer - таймер реального времени;
 start_game_timer - таймер игрового времени;
 start_quick_timer - таймет реального или игрового времени без возможности сохранения;
 start_hud_timer - таймер реального времени с HUD-дом на игровом экране;
 start_multi_timer - цикличный таймер.
 
- 1-й аргумент - время в секундах;
- 2-й аргумент - функция, которая будет вызвана таймером по окончании времени;
- 3-й аргумент - параметры передаваемые таймеру;
- 4-й аргумент - (опционально):
 -- для 'real' и 'game' таймеров - имя таймера;
 -- для 'hud' - тип hud'а (с текстурой или только текст);
 -- для 'multi' - тип времени (реальное или игровое).        
Имя таймера (кроме 'hud') может быть задано так (заменяется "выданное имя"):
start_real_timer(0.5, "my_file.my_func"):set_name("my_timer_name") --/ 0.5 real-seconds + (re)set name

Функция может быть задана прямым указанием или строкою или таблицей.
При отсутствии указанной функции и наличии 3-го аргумента (параметров) будет вызван внутренний обработчик.
Внутренний обработчик позволяет при соответствующих именах параметров выполнять:
а) выдача инфопоршня или нескольких:
start_game_timer(3*60, nil, {info_id = "my_infoportion"}) --/ 3 game-minutes
б) Вывод сообщения на экран (через news_manager):
start_real_timer(5*60, nil, {tip = {"Text tip_RT"}})
в) Вывод строки в лог:
start_quick_timer(8, nil, {log = "test QT8"})
г) Вывод звука:
start_quick_timer(8, nil, {snd = "device\\pda\\pda_sos"})
и др.

Примеры использования:
start_real_timer(6, "my_file.my_func") --/ 6 real-seconds
start_real_timer(5*60, nil, {tip = {"st_tip", "Text test_RT"}}, "RT_test") --/ 5 real-minutes + set name
start_real_timer(0.5, "my_file.my_func"):set_name("my_timer_name") --/ 0.5 real-seconds + (re)set name
start_game_timer(3*60, nil, {info_id = "my_infoportion"}) --/ 3 game-minutes
start_hud_timer (10*60, nil, {log = "test_HT"}) --/ 10 real-minutes
start_hud_timer (45, nil, {log = "caption_HT"}, "blowout") --/ 45 real-seconds + text caption
start_hud_timer (45, nil, {log = "only_text_HT"}, false) --/ 45 real-seconds + only text-time
start_quick_timer(4, my_file.my_func) --/ 4 real-seconds + call function
start_quick_timer(8, nil, {info_id = "my_infoportion"}, log = "tip QT8"}) --/ 8 real-minutes + log
start_multi_timer(2*60, nil, {log = "tip_MR"}) --/ 120 real-seconds + log + cyclical
start_multi_timer(3*60, nil, {tip = {nil, "tip_MG"}, log = "tip_MG"},true) --/ 120 game-minutes + tip + log + cyclical
--/ для объекта таймера доступны:
timer:id() --/ чтение(получение) идентификатора запущенного таймера (не путать с игровыми идентификаторами объектов!)
timer:name() --/ чтение(получение) имени запущенного таймера
timer:set_name(timer_name) --/ установка/изменение имени таймера
timer:restart(time_sec, action, properties, timer_name) --/ перезапуск работающего таймера (аргументы аналогичгы запуску)
timer:get_time_rest() --/ получение оставшегося времени работающего таймера (real mseconds or game seconds)
timer:get_time_end() --/ получение времени (число mseconds), когда таймер сработает
timer:get_time_end(true) --/ получение времени (строка), когда таймер сработает
timer:get_property(property) --/ чтение значения параметра заданного таймеру
timer:set_property(property,value) --/ установка/замена параметра заданного таймеру
timer:remove() --/ удаление таймера

--/---------------------------------------------------------------------
Artos (22.09.2013)
