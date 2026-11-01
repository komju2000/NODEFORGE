Portfolio · UE 5.5

# ДаниилDaniil**.**

Blueprint-разработчик в Unreal Engine 5. Ищу стажировку или junior-позицию в инди-студии, удалённо.Unreal Engine 5 Blueprint developer. Looking for a remote internship or junior role at an indie studio.

Собираю геймплейные системы на Blueprints: заказы, машины, переноску предметов, UI. Делаю их на данных и событиях, без Event Tick. Сам рисую простой UI в Figma и моделирую в Blender.I build gameplay systems in Blueprints: orders, machines, item carrying, UI. I keep them data-driven and event-based, with no Event Tick. I also draw simple UI in Figma and model in Blender.

UE5 BlueprintsUMGEnhanced InputData AssetsBlueprint InterfacesFigmaBlenderGit / Diversion

Email

coffidino5@gmail.com

Telegram

@Komju0

Телефон

Phone

+7 991 319-96-14

НавыкиSkills

### Unreal Engine 5

основной инструментmain tool

- Blueprints: события, функции, макросы, наследование
- Primary Data Assets и структуры для контента
- Blueprint Interfaces вместо цепочек Cast
- UMG: виджеты, обновление по событию
- Таймеры вместо Tick, физика, коллизии

- Blueprints: events, functions, macros, inheritance
- Primary Data Assets and structs for content
- Blueprint Interfaces instead of Cast chains
- UMG: widgets updated by events
- Timers instead of Tick, physics, collision

### Blender

средний уровеньintermediate

- Моделирование пропсов и окружения
- Оптимизация моделей под игру: полигонаж, чистая сетка
- Экспорт в Unreal
- Текстурирование: изучаю, пока пробел

- Prop and environment modeling
- Game-ready optimization: polycount, clean topology
- Export to Unreal
- Texturing: currently learning

### Figma

базовый уровеньbasic

- Игровые панели и рамки для HUD
- Иконки предметов
- Перенос в UMG

- Game panels and HUD frames
- Item icons
- Moving designs into UMG

Главный проектMain project

## Industrial Chaos

Хаотичная фабрика от третьего лица. Игрок носит руду к машинам, переплавляет её в слитки, собирает заказ в коробку и сдаёт на точку, пока таймер заказа идёт. Полы со льдом, маслом и лавой мешают. Прототип, сделан один, только на Blueprints.A chaotic third-person factory. The player carries ore to machines, smelts it into ingots, packs an order into a box and delivers it before the order timer runs out. Ice, oil and lava floors get in the way. Solo prototype, Blueprints only.

Engine

UE 5.5

Роль

Role

всё, соло

everything, solo

Код

Code

100% Blueprints

Версия

Version

0.0.2

Industrial Chaos gameplay: character on the factory floor, order panel top left, score counter top center, smelter on the right

Геймплей: панель заказов слева сверху, счёт по центру, плавильня справа. Графика пока прототипная.Gameplay: order panel top left, score top center, smelter on the right. Art is still prototype.

**заказ****order**→ **руда****ore**→ **плавильня****smelter**→ **слиток****ingot**→ **коробка****box**→ **точка сдачи****delivery**→ **очки****score**

### Сдача заказа

### Order delivery

ActorBeginOverlap

Other Actor

Cast To BP_OrderBox

bIsHeld = false

FindMatchingOrder

StoredItems → bFound

CalculateItemsValue

Score +=

RemoveOrderByID

OrderID

Clear + Refresh UI

Box widget, ScoreText

Точка сдачи сравнивает содержимое коробки с активными заказами. Нашёлся заказ: начисляются очки и заказ закрывается. Не нашёлся: коробка очищается.The delivery zone compares the box contents with active orders. If an order matches, score is added and the order closes. If not, the box is emptied.

### Машина принимает предмет

### Machine accepts an item

BeginOverlap

BP_MasterItem

CanAcceptItem

Recipe input?

AddStoredItem

ItemData, +1

SetLifeSpan 0.05

\+ disable collision

CheckAndStartProcessing

Timer → output

Все машины наследуются от BP_MasterMachine. Плавильня отличается только Data Asset с рецептами.All machines inherit from BP_MasterMachine. The smelter differs only by its recipe Data Asset.

### Решения

### Decisions

**Контент в данных****Content as data**

Предметы, рецепты, машины и пулы заказов лежат в Primary Data Assets. Новый слиток или рецепт добавляется без новой логики.

Items, recipes, machines and order pools are Primary Data Assets. A new ingot or recipe needs no new logic.

**Без Event Tick****No Event Tick**

Машины и менеджер заказов работают на таймерах. Счёт и коробка обновляют UI только по событию, без биндингов каждый кадр.

Machines and the order manager run on timers. Score and box UI update only on events, with no per-frame bindings.

**Интерфейсы****Interfaces**

Взаимодействие идёт через BPI_Interact и BPI_OpenMenu. Персонажу не нужно знать класс объекта.

Interaction goes through BPI_Interact and BPI_OpenMenu. The character does not need to know the object's class.

**Баланс в одном месте****Tuning in one place**

Число предметов в заказе, время на предмет и лимит заказов настраиваются в Data Asset уровня.

Items per order, time per item and the active order limit are set in the level's Data Asset.

Разбор багаBug breakdown

**Pending kill при броске.** Брошенный предмет включал коллизию, коробка или машина сразу забирала его и вызывала Destroy Actor. Код броска ещё выполнялся и обращался к удалённым компонентам. Решение: предмет отключает коллизию и исчезает через Set Life Span 0.05. Бросок успевает завершиться, и предмет не засчитывается дважды.

**Pending kill on throw.** A thrown item enabled its collision, and the box or machine grabbed it at once and called Destroy Actor. The throw code was still running and touched destroyed components. Fix: the item disables collision and is removed with Set Life Span 0.05. The throw finishes, and the item cannot be counted twice.

ЕщёMore

в работеin progress

### RTS FFA prototype

Стратегия в духе Warcraft III, режим каждый за себя. Сделана RTS-камера:

A Warcraft III–style strategy, free-for-all mode. The RTS camera is done:

- плавный зум колесом через Lerp
- наклон 45–50° вверху и 30–35° внизу
- Enhanced Input, свой Player Controller и Game Mode

- smooth wheel zoom with Lerp
- pitch 45–50° at the top, 30–35° at the bottom
- Enhanced Input, custom Player Controller and Game Mode

### Сейчас изучаю

### Learning now

- Текстурирование и PBR-материалы
- Репликация в Blueprints для кооператива в Industrial Chaos

- Texturing and PBR materials
- Blueprint replication for Industrial Chaos co-op

ДаниилDaniil · Blueprint DeveloperОткрыт к стажировке и junior-позицииOpen to internship and junior roles