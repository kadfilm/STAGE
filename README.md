

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

# STAGE — Show Timecode And Graphic Editor

**Version:** 0.3.4
**Software Type:** Web-based Timecode Editor & Visualizer
**Target Platform:** CS-HALL (FiveM Script)

---

## ENGLISH

### USER GUIDE

#### 1. Overview
STAGE is a dedicated pre-programming environment designed for the creation and synchronization of complex light shows. The software emulates the workflow of professional lighting consoles, providing a comprehensive toolset for fixture visualization (Patch), timeline sequencing, and executable code generation.

#### 2. Viewport
The visualizer operates in a 2D projection with real-time coordinate transformation support.

* **Top View:** Plan view (Z-axis perpendicular to screen).
* **Front View:** Frontal projection.
* **Iso View:** Isometric projection for volumetric assessment.
* **Navigation:** Supports standard Panning and Zooming controls for precise fixture positioning.

#### 3. Patch & Fixtures
Geometry import is performed by parsing external `.lua` map files containing vector data.

**Supported Fixture Types:**
* **Spotlight:** Moving heads / spots (Parameters: RGB, Intensity, Strobe).
* **Smoker:** Smoke machines / Hazers control.
* **Sparkler:** Cold spark fountains control.
* **Speaker/Monitor/Screen:** Multimedia device triggers.

#### 4. Timeline & Sequencer
The primary show control interface. Operates based on absolute Timecode received from the master media player.

* **Tracks:** Automatic track sorting by fixture type or model hash. Supports "Stacked" mode for high-density timeline viewing.
* **Cues (Clips):** Timeline events containing cue data. Each clip stores fixture selection, color attributes, effect type, and timing (In/Out/Duration).
* **Keyframing (Fades):** Implements Linear Interpolation for Fade In/Out parameters and Color Morphing (Gradient Transition).
* **Snapping:** Magnetic snapping to the Playhead and adjacent clip boundaries for frame-accurate sequencing.

#### 5. FX Generator
Automated tool for creating Chases and distributing effects across the fixture matrix.

* **Sorting Patterns:** Algorithms for selection sorting (Left-to-Right, Center-Out, Random, Linear).
* **Phasing (Delay):** Start delay control for creating wave and sweep effects.
* **Block Operations:** `Explode` (converting static blocks into individual steps) and `Merge` (combining steps into a single block) functions.

#### 6. Export (Compiler)
The system compiles the visual timeline representation into Lua code.

* **Architecture:** Utilizes asynchronous thread architecture (`CreateThread`).
* **Optimization:** Lighting cues are grouped by time intervals to minimize CPU load during execution (linear execution logic, no JIT).

---

### TECHNICAL DOCUMENTATION

**Tech Stack:** Vanilla JavaScript (ES6 Modules), HTML5 Canvas API, CSS3.
**Architecture:** State-Driven UI (State modification triggers immediate re-render).

#### File System Structure

**Root Directory:**
* `index.html`: Entry point. Contains the DOM structure, modal windows, and module imports.
* `style.css`: Stylesheets. Defines Layout (Flexbox), color scheme, and the critical Timeline Z-Index layer hierarchy.

**Directory `/js` (Core Logic):**

1.  **`state.js`**
    * **Role:** Global State Store (Single Source of Truth).
    * **Content:** Arrays for `fixtures`, `clips` (timeline events), `groups` (selection groups), viewport settings, and playback flags.

2.  **`config.js` (CFG)**
    * **Role:** Configuration file.
    * **Content:** Static constants (zoom scale, hit radius, snap thresholds).

3.  **`main.js`**
    * **Role:** Application Initialization.
    * **Functions:** Registration of global event handlers (Keyboard/Mouse), render loop startup, module linking.

**Directory `/js/modules` (Modules):**

4.  **`timeline.js`**
    * **Role:** Sequencer Logic.
    * **Functions:**
        * `renderTimeline()`: Clip positioning calculation, track distribution (bin-packing algorithm), DOM element rendering.
        * `handleTimelineDrag()`: Logic for Drag&Drop, Trimming, and Fade adjustments.
        * `checkConflicts()`: Collision detection (overlapping commands for identical fixtures).

5.  **`canvas.js`**
    * **Role:** Visualizer Graphics Engine.
    * **Functions:**
        * `drawCanvas()`: Rendering grid, fixtures, and states on HTML5 Canvas.
        * 3D to 2D Coordinate Projection (handling rotation and zoom matrices).
        * Real-time rendering of active states (Intensity, Color) during playback.

6.  **`video.js`**
    * **Role:** Timecode Synchronization (Timecode Master).
    * **Functions:** Integration with YouTube IFrame API. Transport control (Play/Pause/Seek) and broadcasting current `timestamp` to other modules.

7.  **`ui.js`**
    * **Role:** Interface Management (Inspector / Properties).
    * **Functions:** Data Binding between `clip` objects and property input fields. Panel visibility management.

8.  **`io.js` (Input/Output)**
    * **Role:** File Operations and Parsing.
    * **Functions:**
        * `parseLua()`: Regex extraction of `vector3` coordinates and metadata from Lua tables.
        * `exportData()`: Final Lua script generation. Converting timeline data into `Wait()` loops and Scene API calls.
        * `saveProjectSTG()`: Serialization of project state to JSON.

9.  **`tools.js`**
    * **Role:** Editing Toolkit.
    * **Functions:** Wave Generator logic, `explode` and `merge` algorithms, new event creation.

10. **`history.js`**
    * **Role:** Session Version Control.
    * **Functions:** Undo/Redo stack implementation via Deep Copy of application state.

11. **`layout.js`**
    * **Role:** Window Manager.
    * **Functions:** Resizing panels (Sidebar, Timeline, Video player) with Canvas resolution recalculation.

12. **`colorpicker.js`**
    * **Role:** Custom UI Component.
    * **Functions:** HSV color selection model implementation.

13. **`security.js`**
    * **Role:** Runtime Protection.
    * **Functions:** Blocking context menus and DevTools shortcuts to secure the environment.

---
---

## RUSSIAN

### РУКОВОДСТВО ПОЛЬЗОВАТЕЛЯ

#### 1. Обзор
STAGE представляет собой среду пре-программинга (Pre-programming environment), предназначенную для создания синхронизированных световых шоу. Программное обеспечение эмулирует работу реальных световых консолей, предоставляя инструменты для визуализации расстановки приборов (Patch), секвенсирования по таймлайну (Timeline) и генерации исполняемого кода.

#### 2. Рабочее пространство (Viewport)
Визуализатор работает в двухмерной проекции с поддержкой трансформации координат.

* **Top View:** План сцены сверху (ось Z перпендикулярна экрану).
* **Front View:** Фронтальная проекция.
* **Iso View:** Изометрическая проекция для оценки объема.
* **Навигация:** Поддерживается панорамирование (Pan) и зумирование (Zoom) для точного позиционирования.

#### 3. Патч и Фикстуры (Fixtures)
Импорт геометрии осуществляется через парсинг `.lua` файлов карт.

**Поддерживаемые типы приборов:**
* **Spotlight:** Управляемые головы / прожекторы (RGB, Intensity, Strobe).
* **Smoker:** Генераторы дыма.
* **Sparkler:** Генераторы искр (холодные фонтаны).
* **Speaker/Monitor/Screen:** Мультимедийные устройства.

#### 4. Таймлайн и Секвенсор
Основной инструмент управления шоу. Работает на основе абсолютного времени (Timecode), получаемого от медиа-плеера.

* **Tracks (Треки):** Автоматическая сортировка дорожек по типам приборов или моделям. Поддерживается "Stacked" режим для компактного отображения.
* **Cues (Клипы):** События на таймлайне. Каждый клип хранит данные о выборе приборов, цвете, типе эффекта и временных метках (In/Out/Duration).
* **Keyframing (Фейды):** Реализована линейная интерполяция значений (Linear Interpolation) для параметров Fade In и Fade Out, а также морфинг цвета (Gradient).
* **Snapping:** Магнитная привязка к курсору воспроизведения (Playhead) и границам соседних клипов.

#### 5. Генератор Эффектов (FX Generator)
Инструмент для автоматического создания последовательностей (Chases) и распределения эффектов по матрице приборов.

* **Sorting Patterns:** Алгоритмы сортировки выборки (Left-to-Right, Center-Out, Random и др.).
* **Phasing (Delay):** Управление задержкой старта для создания волновых эффектов.
* **Block Operations:** Функции `Explode` (разбиение статики на шаги) и `Merge` (объединение шагов в блок).

#### 6. Экспорт (Compiler)
Система компилирует визуальное представление в Lua-код.

* **Архитектура:** Используется архитектура асинхронных потоков (`CreateThread`).
* **Оптимизация:** Световые картины группируются по временным интервалам для минимизации нагрузки на CPU (JIT-компиляция не используется, исполнение линейное).

---

### ТЕХНИЧЕСКАЯ ДОКУМЕНТАЦИЯ

**Стек технологий:** Vanilla JavaScript (ES6 Modules), HTML5 Canvas API, CSS3.
**Архитектура:** State-Driven UI (Изменение стейта вызывает ре-рендер).

#### Структура файловой системы

**Корневая директория:**
* `index.html`: Точка входа. Содержит DOM-структуру интерфейса, модальные окна и подключение модулей.
* `style.css`: Таблицы стилей. Определяет Layout (Flexbox), цветовую схему и Z-Index иерархию слоев таймлайна.

**Директория `/js` (Core Logic):**

1.  **`state.js`**
    * **Роль:** Глобальное хранилище состояния (Single Source of Truth).
    * **Содержание:** Массивы `fixtures` (приборы), `clips` (события таймлайна), `groups` (группы выделения), настройки вьюпорта и флаги воспроизведения.

2.  **`config.js` (CFG)**
    * **Роль:** Конфигурационный файл.
    * **Содержание:** Статические константы (масштаб зума, радиус хитбоксов, пороги прилипания).

3.  **`main.js`**
    * **Роль:** Инициализация приложения.
    * **Функции:** Регистрация глобальных обработчиков событий (Keyboard/Mouse), запуск цикла рендеринга, связка модулей.

**Директория `/js/modules` (Modules):**

4.  **`timeline.js`**
    * **Роль:** Логика секвенсора.
    * **Функции:**
        * `renderTimeline()`: Расчет позиций клипов, распределение по трекам (алгоритм упаковки прямоугольников), отрисовка DOM-элементов.
        * `handleTimelineDrag()`: Логика Drag&Drop, изменения длительности (Trimming) и настройки фейдов.
        * `checkConflicts()`: Детекция коллизий (перекрытия команд для одних и тех же приборов).

5.  **`canvas.js`**
    * **Роль:** Графический движок визуализатора.
    * **Функции:**
        * `drawCanvas()`: Отрисовка сетки, приборов и их состояний на HTML5 Canvas.
        * Проекция координат 3D -> 2D (учет вращения и зума).
        * Отрисовка активных состояний (интенсивность, цвет) в реальном времени.

6.  **`video.js`**
    * **Роль:** Синхронизация таймкода (Timecode Master).
    * **Функции:** Интеграция с YouTube IFrame API. Управление транспортом (Play/Pause/Seek) и передача текущего `timestamp` в остальные модули.

7.  **`ui.js`**
    * **Роль:** Управление интерфейсом (Inspector / Properties).
    * **Функции:** Data Binding (двусторонняя связь) между объектами `clip` и полями ввода свойств. Управление видимостью панелей.

8.  **`io.js` (Input/Output)**
    * **Role:** Файловые операции и парсинг.
    * **Functions:**
        * `parseLua()`: Регулярные выражения для извлечения `vector3` координат и метаданных из Lua-таблиц.
        * `exportData()`: Генерация итогового Lua-скрипта. Преобразование данных таймлайна в циклы `Wait()` и вызовы API сцены.
        * `saveProjectSTG()`: Сериализация состояния проекта в JSON.

9.  **`tools.js`**
    * **Role:** Инструментарий редактирования.
    * **Functions:** Логика генерации волн (Wave Generator), алгоритмы `explode` и `merge`, создание новых событий.

10. **`history.js`**
    * **Role:** Система контроля версий сессии.
    * **Functions:** Реализация стека Undo/Redo через создание глубоких копий (Deep Copy) состояния приложения.

11. **`layout.js`**
    * **Role:** Управление оконным менеджером.
    * **Functions:** Ресайз панелей (Sidebar, Timeline, Video player) с пересчетом размеров Canvas.

12. **`colorpicker.js`**
    * **Role:** Кастомный UI компонент.
    * **Functions:** Реализация HSV модели выбора цвета.

13. **`security.js`**
    * **Role:** Защита среды исполнения.
    * **Functions:** Блокировка контекстного меню и горячих клавиш DevTools.
