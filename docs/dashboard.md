# Dashboard (kiosk timetable widget) — спецификация

Документ — исходный контракт для написания пользовательской/партнёрской документации.  
Описывает **фактическое поведение** страницы dashboard в `torrow-apps` (ветка `timetable-widget`), а не желаемое.

Если формулировка помечена **(caveat)**, это ограничение реализации: в документации его нужно либо явно описать, либо не обещать обратного.

**Код:**

| Слой | Путь |
|------|------|
| Страница | `libs/mobile/mind-map/src/lib/dashboard/` |
| URL-контракт | `libs/mobile/core/src/lib/torrow-url-map.config.ts` (`DashboardNodeParams`) |
| URI | `libs/mobile/core/src/lib/uri/dashboard-uri.service.ts` |
| Константы | `libs/config/src/lib/app.config.ts` |
| Цвета | `libs/domain-models/src/lib/timetable-colors.model.ts` |
| Timegrid | `libs/mobile/shared/src/lib/timegrid/timegrid.component.ts` (FullCalendar) |

---

## 1. Назначение

Dashboard — **kiosk-страница расписания** для одной или нескольких сущностей (`id` / `ids`): вид **день** (FullCalendar `timeGridDay` при одной колонке, `resourceTimeGridDay` при нескольких — §3.0) или **скользящее окно** без `date` (§3.3).

Цели:

- показать занятость (заказы/слоты) на экране без деталей клиента;
- опционально показать «рабочие» окна (жёлтый фон) и «нерабочие» (серый фон);
- автоматически обновлять картинку по таймеру (киоск / TV);
- при нескольких id — отдельные столбцы (swimlanes) в одном дне.

Это **не** полноценный timetable менеджера: нет клика по событию, нет смены вида (неделя/месяц), нет фильтра статусов.

---

## 2. Доступ и URL

### 2.1. Маршрут

Абсолютный путь (Angular matrix params на сегменте `dashboard`):

```
https://{host}/app/tabs/tab-search/dashboard;ids={id1}[,{id2}][;title={text}][;busyLabel={text}][;date={date}][;fromMin={n}][;toMin={n}][;refreshMin={n}][;resfreshMin={n}][;visibility={value}][?sideMenuHidden=true&tabBarHidden=true[&lang={code}][&timezone={IANA}][&securityToken={token}]]
```

- Префикс приложения: сегмент `app`.
- Вкладка: `tabs/tab-search` (`TabSearchNode`).
- Страница: `dashboard`.
- Параметры dashboard — **matrix** (`;key=value`) на сегменте `dashboard`.
- Параметры **query** (`?key=value`) — общие для приложения; dashboard читает их через глобальные сервисы / store, не через `DashboardFacade`.
- Query chrome: `sideMenuHidden`, `tabBarHidden`. Query локали/TZ: `lang`, `timezone` (§3.6). Query доступа: `securityToken` (§3.7).
- `host` в проде: `torrow.net` (или `WEBAPP_URL` / origin приложения).

`MainGuard.canActivate()` пропускает всех в `/app/tabs`. На маршруте `dashboard` **нет** auth-guard.  
`SearchTabGuard` висит только на корне search (`tab-search` list) и `search-sublist`; **не** на `dashboard`.

Следствие: **прямая ссылка на dashboard открывается без логина** (роль `Unauthorized`). Редirect на `/login` при открытии dashboard **не** происходит.  
Редirect на auth бывает при заходе на **главную search-вкладку** без токена (`SearchTabGuard` → `authPhone`).

Доступ к **данным** (item, timetable, workload) решает **бэкенд** по `publicityType` и правам участника. Клиент dashboard **не проверяет** publicity до рендера.

### 2.2. Обязательность `id` / `ids` и тип объекта

Нужен хотя бы один валидный id после merge `id` + `ids` (§3). Если итог пуст — facade бросает:

`DashboardFacade; parseSnapshotParams; empty id`

Тип каждого id: Torrow object id. Практические сценарии kiosk:

| Тип объекта | Фон (working hours) | Занятость |
|-------------|---------------------|-----------|
| **Resource** | да (см. §6.2) | timetable cases этого ресурса |
| **Service** с session duration ≠ None | да, через `getWorkloadPeriod` | timetable cases сервиса |
| Service без session duration / прочий item | нет фона | только timetable cases, если API их отдал |
| GET item упал / `undefined` | нет фона этой колонки | timetable cases всё равно запрашиваются |

Для типичного публичного киоска **`id` / `ids` = Resource**, не Service. У Resource в anonymous GET поле `schedule` часто пустое — это учтено fallback-ом (§6.3).

Несколько id → отдельные столбцы FC; тип колонки определяется ответом GET item (как для одиночного `id`).

### 2.3. Копирование ссылки

Меню страницы: единственный пункт **Copy link**.

Копируется URL из **снимка параметров на момент создания facade**, через `DashboardUriService.getFullUrl`. Matrix `date` копируется **исходным токеном** из адреса (`YYYY-MM-DD` / `now`), не `Date.toString()` распарсенного значения.

**(caveat)** Сдвиг даты в UI (prev/next/datepicker) **не** попадает в скопированную ссылку. URL остаётся с исходным `date` из адреса. **Все** query-параметры (`sideMenuHidden`, `tabBarHidden`, `lang`, `timezone`, `securityToken`, …) тоже не копируются (§3.5).

### 2.4. Аутентификация, `publicityType` и доступ к данным

Dashboard **не** имеет отдельных URL-параметров «режима для гостя». Параметры §3 (`date`, `visibility`, …) одинаковы для всех.  
Различие анонима и залогиненного — в **ответах API** и **user settings**, не в логике парсинга URL.

#### 2.4.1. Роли клиента (`UserRolesEnum`)

| Роль | Токен | Типичный сценарий dashboard |
|------|-------|------------------------------|
| `Unauthorized` | нет | Kiosk/TV по прямой ссылке, без логина |
| `NotRegisteredUser` | есть, регистрация не завершена | Редко; API как у частично авторизованного |
| `User` / `Administrator` | полный | Менеджер открыл ссылку в приложении |

HTTP: `InterceptorAuth` шлёт `Authorization: Bearer <token>` или **пустую** строку, если токена нет. Проверка прав — на API Gateway / сервисах.

#### 2.4.2. `publicityType` объекта (не путать с `visibility` URL)

Свойство item: `TorrowItem.publicityType` (`PublicityType`). Задаётся в карточке объекта («Тип доступности»).  
Dashboard **не читает** `publicityType` — только последствия на API.

| `publicityType` | UI (ru) | Поиск | Аноним по ссылке (ожидание для kiosk) |
|-----------------|---------|-------|----------------------------------------|
| `Link` | По ссылке | нет | **да** — основной режим kiosk; нужен переход по URL/QR |
| `PublicAvailable` | Открытый | да | **да** — публичный просмотр |
| `PublicWithRequest` | Открытый по запросу | да | **нет** без одобренного доступа |
| `Personal` | Персональный | нет | **нет** для постороннего анонима |
| `Private` | — | нет | **нет** |

Клиентская эвристика (не dashboard, но тот же домен): `isCheckInWithoutRegistrationAvailable()` = `Link` \| `PublicAvailable`.

При успешном анонимном GET item часто `personalInfo.participantType === PublicReader` (`TorrowItemHelper.isPublicReader`).

**Для документации kiosk:** Resource/Service должны быть **`Link` или `PublicAvailable`**, иначе GET item / `getTimetableFromServerOnly` вернут ошибку → пустой title, нет фона, пустая сетка.

#### 2.4.3. Resource vs Service для анонима

| | **Resource** (`id` = resource) | **Service** (`id` = service) |
|---|-------------------------------|------------------------------|
| Типичный kiosk | **да** (экран зала, кресло, кабинет) | реже (расписание услуги целиком) |
| `GET item` аноним | обычно **да** при Link/PublicAvailable; **`schedule` часто пустой** | **да** при Link/PublicAvailable; session duration в ответе |
| Фон | клиент: FreeTime на всё окно без schedule (§6.3); с schedule — NotWorking + FreeTime | `session duration ≠ None` → `getWorkloadPeriod` (§6.1); ошибка/пустой ответ на рабочем дне → fallback schedule (§6.2A), иначе `[]`. **`session duration = None` + schedule** → §6.2 (NotWorking + FreeTime), workload не вызывается. Day-off по schedule → NotWorkingTime (§6.2B); skip workload на day-off — **только static** |
| Занятость | `getTimetableFromServerOnly(resourceId, …, Time)` | `getTimetableFromServerOnly(serviceId, …, Time)` |
| Публичный доступ | ссылка + Link/PublicAvailable на **ресурсе** | ссылка + Link/PublicAvailable на **услуге** |

Dashboard **не** подставляет service id вместо resource id и **не** ходит в `GET /resources/{id}/workload`.

#### 2.4.4. Неаутентифицированный vs аутентифицированный — что меняется на странице

Параметры URL (`date`, `fromMin`, `visibility`, …) работают **одинаково**. Меняется наполнение:

| Аспект | Неаутентифицированный (`Unauthorized`) | Аутентифицированный (`User` / `Administrator`) |
|--------|------------------------------------------|-----------------------------------------------|
| Открытие `/dashboard;ids=…` | да, без login | да |
| `GET item` | ok при Link/PublicAvailable; иначе fail → `""` title | ok при правах; **Manager** часто видит полный item |
| Resource `schedule` в GET | **часто пусто** → fallback FreeTime (§6.3) | **может быть** → NotWorking + слоты (§6.2A) |
| Service workload фон | часто **нет** (401/403); relative **всегда** вызывает `getWorkloadPeriod`; при ошибке/пустом ответе + `schedule` со слотами в `timePeriod` → fallback §6.2A; без schedule → `[]` | чаще **есть**; relative **всегда** вызывает workload; при ошибке/пустом ответе + `schedule` со слотами в `timePeriod` → fallback §6.2A |
| `getTimetableFromServerOnly` + `visibility=Time` | серые блоки с подписью **Занято** (или `busyLabel`) (§7.2), без названия заказа / ФИО | то же; **TimetableCase store не трогается** (§3.1) |
| `visibility=View` | названия заказов, если API отдал | то же; риск PII на экране |
| Timezone | `?timezone=` → static day / события / Service-фон / ось FC / date header; **relative окно** — Luxon local устройства (§3.3) | `?timezone=` или user settings `place.timeZone`; relative окно — local устройства |
| Chrome `?sideMenuHidden&tabBarHidden` | одинаково | одинаково; у auth может быть виден back |

**Relative vs static (`date`)** — не зависят от auth (§3.2–3.3).

#### 2.4.5. Отказы API (общие для auth / anon)

| Сбой | Поведение dashboard |
|------|---------------------|
| `GET item` reject / 403 / 404 | `connectItem` → `undefined`; фон колонки `[]`; timetable **всё равно** запрашивается; ion-title `""` только если ни у одной загруженной колонки нет непустого `name` (multi: fallback на другую колонку) |
| `getTimetableFromServerOnly` error / offline | `[]` заказов; фон может остаться; **без** local SQL fallback |
| `getWorkloadPeriod` error (Service) | при непустом `schedule` и слотах schedule **внутри `timePeriod`** → fallback NotWorking + FreeTime (§6.2A); иначе NotWorking only или `[]`; заказы остаются |

Страница **не** показывает отдельное сообщение «нет доступа» — пользователь видит пустой/частичный timetable.

---

## 3. Параметры URL

Интерфейс matrix: `DashboardNodeParams`. Query — глобальные (§3.5–3.7).

**Сводка:**

| Параметр | Где | Dashboard-контракт |
|----------|-----|-------------------|
| `id`, `ids`, `title`, `busyLabel` | matrix | §3 / §3.0 |
| `date`, `fromMin`, `toMin`, `refreshMin`, `resfreshMin`, `visibility` | matrix | §3.1–3.4 |
| `sideMenuHidden`, `tabBarHidden` | query | §3.5 |
| `lang`, `timezone` | query | §3.6 |
| `securityToken` | query | §3.7 |

Парсинг matrix: `ActivatedRoute.snapshot.params` (строки). Числа — `deserializeStringToNumber` (`+str`, `NaN` → `undefined`). Дата — `DateTime.fromISO(..., { zone: dashboardTZ })`; невалидный ISO / `now` / пусто → `new Date()`.

| Параметр | Обязателен | Тип | Default | Смысл |
|----------|------------|-----|---------|--------|
| `id` | нет* | string (Torrow id) | — | Первый источник колонок; merge с `ids` |
| `ids` | нет* | string, comma-separated | — | Доп. колонки; `id1,id2,id3` |
| `title` | нет | string | fallback на первый непустой loaded `name` | Текст `ion-title` |
| `busyLabel` | нет | string | i18n `DASHBOARD.BUSY_TIME_LABEL` | Подпись busy-блока при Time |
| `date` | нет | ISO (`YYYY-MM-DD`); также `now` / любой непарсящийся токен | нет ключа → relative; ключ есть и parse fail → `new Date()` (сегодня, static) | Фиксированный календарный день (static mode) |
| `fromMin` | нет | number, минуты | `120` (`DEFAULT_DASHBOARD_RELATIVE_FROM_MIN`) | Только relative mode: сколько минут **назад** от «сейчас», затем `startOf("hour")` |
| `toMin` | нет | number, минуты | `480` (`DEFAULT_DASHBOARD_RELATIVE_TO_MIN`) | Только relative mode: сколько минут **вперёд** от «сейчас», затем `endOf("hour")` |
| `refreshMin` | нет | number, минуты | `3` (`DEFAULT_DASHBOARD_REFRESH_INTERVAL_MIN`) | Интервал `timer(0, refreshMin)`: перезапрос timetable / пересчёт окна |
| `resfreshMin` | нет | number, минуты | alias | **Deprecated typo.** Используется только если `refreshMin` отсутствует |
| `visibility` | нет | `"View"` \| `"Time"` \| иное | отсутствует / не `View` → **Time** | Уровень детализации заказов |

\* Обязателен непустой `columnIds` после merge `id`+`ids`.

### 3.0. `ids`, `title`, столбцы

```
parseList(s) = split by ",", trim, drop empty
columnIds = uniqueStable([...parseList(id), ...parseList(ids)])  // id first; first occurrence wins
```

| Вход | `columnIds` |
|------|-------------|
| `ids=A` | `[A]` |
| `ids=A,B` | `[A,B]` |
| `ids=A,A,B` | `[A,B]` |
| нет ключей / только запятые | throw `empty id` |

View: `columnIds.length === 1` → `timeGridDay`; иначе `resourceTimeGridDay` (столбцы = Resource/Service). Каждая колонка: `resources[].title = item.name \|\| columnId` (FC использует `resources` только на resource-view).

| Место | Источник |
|-------|----------|
| `ion-title` (`serviceName$`) | `title` после trim, если непустой; иначе первый непустой `name` среди **загруженных** колонок (иначе `""`) |
| Заголовок столбца FC | имя item колонки (только multi / `resourceTimeGridDay`) |

**(caveat)** В matrix `title` / `busyLabel` нельзя без encode символы `;` `?` `#`. Пробелы/кириллица — через URL-encode.

**(caveat)** N колонок → N× `getTimetableFromServerOnly` / GET item / фон на refresh. Ошибка одной колонки → пустые события этой колонки, остальные живут. Медленный/hung `connectItem` **не** блокирует timetable соседних колонок (`startWith(undefined)` на multi `connectItems` и на per-column `item$` в event pipeline).

**(caveat)** Static ось FC — **union** окон только по **загруженным** колонкам (`item !== undefined`). `undefined` (loading / hung / 403) **не** участвует и **не** форсит full-day. **single:** ждём первый ответ connect; `undefined` → civil-day axis; hang → нет эмита / FC defaults. **multi:** `startWith(undefined)` на `connectItems`; пока все `undefined` — нет эмита (FC defaults `00:00`/`24:00`); partial — union known. Day-off или без schedule у **загруженной** колонки → полные сутки. Partial load: сначала ось по уже known колонкам, позже может расшириться (например day-off соседа → full-day). Чанки прочих колонок сбрасываются при смене границ `timePeriod` (`periodKey` = `from:to` scan): static day shift **и** relative, когда hour-snapped окно реально сдвинулось; relative refresh с теми же hour-bounds чанки не сбрасывает.

Hard-limit: `DEFAULT_DASHBOARD_MAX_COLUMNS` (**20**) — после merge/dedupe `columnIds` обрезаются silent truncate (`slice`); лишние id не грузятся. Raw matrix `id`/`ids` в copy-link не переписываются (при открытии снова truncate). Рекомендация — не более 8 колонок.

### 3.1. Правила `visibility`

Enum `TimetableDetailsVisibility`: `Time` | `View`.

Парсер **признаёт только** `visibility=View`. Любое другое значение (в т.ч. `Time`, опечатка, пустая строка) → `undefined` → в runtime **Time**.

Итог:

| URL | Режим |
|-----|--------|
| нет `visibility` | Time (kiosk default) |
| `;visibility=Time` | Time |
| `;visibility=View` | View |
| `;visibility=foo` | Time |

`View` передаётся в `getTimetableFromServerOnly(..., View)` и события рисуются как обычные заказы (заголовок, цвета кейса).  
`Time` передаётся в `getTimetableFromServerOnly(..., Time)` **и** клиент дополнительно маскирует события (§7).

**(caveat)** Dashboard грузит заказы через `getTimetableFromServerOnly`: полный paginated ответ API, **без** upsert/delete в local TimetableCase store. Offline / network error → `[]` заказов (нет fallback на local SQL); фон колонки может остаться.

### 3.2. Правила `date` и навигации

Наличие ключа проверяется так: `paramMap.has("date")` → `hasDateParam`.

| Ситуация | `hasDateParam` | Режим окна | Навигация по датам в UI |
|----------|----------------|------------|-------------------------|
| нет `;date` | false | **relative** | скрыта; `setSelectedDate` / `shiftSelectedDate` no-op |
| `;date=2020-01-09` | true | **static** гражданские сутки этой даты в dashboard TZ | показан `tt-select-timetable-date-block` |
| `;date=now` | true | static, дата = `new Date()` (сегодня) | показана |
| `;date=` (ключ есть, значение пустое) | true | static, дата = `new Date()` | показана |
| `;date=not-a-date` | true | static, дата = `new Date()` | показана |

`date=now` **не** специальный токен в парсере: `fromISO("now")` невалиден, срабатывает fallback `new Date()`. На практике это канонический kiosk-паттерн «сегодня + стрелки дат». Отличается от relative (нет `date`): там нет date bar и окно скользит вокруг now.

Static окно: `[startOf(day, TZ), endOf(day, TZ)]`.  
`fromMin` / `toMin` в static **игнорируются**.

**Ось FC в static mode + schedule:** если у item непустой `schedule.scheduleRanges` и на выбранный день есть рабочие слоты, вертикальная ось сужается до `[earliestWorking − SCHEDULE_AXIS_PADDING_HOURS, latestWorking + SCHEDULE_AXIS_PADDING_HOURS]` (1h каждая сторона; clamp к гражданским суткам dashboard TZ). `timePeriod$` для API и фона остаётся полным днём. Если слотов на день нет (day-off) → ось **полные сутки**; фон — **только** NotWorkingTime (§6). Relative mode (нет `date`) schedule для оси **не** использует.

Сдвиг: ±1 гражданский день в dashboard TZ (Luxon plus/minus days, не getNewSelectedDate), затем `startOf("day")` — override всегда полночь суток, как `parseStaticParamDate` / `toDashboardCivilDate` (даже с `;date=now`, где исходный инстант — `new Date()`). В Material datepicker уходит `pickerDate$`: `Date` с браузерными Y/M/D = гражданский день dashboard (TorrowDateAdapter читает `getFullYear`/`getMonth`/`getDate`). Datepicker шлёт браузерную полночь выбранного дня; facade перед override восстанавливает этот календарный день в dashboard TZ (тот же инстант, что `date=YYYY-MM-DD`). После первого override `dateSig.isStatic` остаётся `true` (сутки выбранного дня).

**(caveat)** Навигация меняет только in-memory `selectedDateOverride$`. URL не обновляется.

### 3.3. Relative window (нет `date`)

```
from = now - fromMin (default 120) → startOf("hour")   // Luxon local, без getDashboardTimeZone
to   = now + toMin   (default 480) → endOf("hour")
```

Реализация: `getRelativeTimePeriod` → `DateTime.fromJSDate(date)` **без** IANA из `?timezone`.  
Query `timezone` **не** сдвигает relative-окно; влияет на static day, map событий и Service-фон (§5.1).

Пример default: сейчас 14:37 → окно примерно **13:00 … 22:59:59** (зависит от TZ браузера).

Каждый `refreshMin` `now` берётся заново → окно «едет» вперёд. Это режим «живой киоск сейчас».

### 3.4. `refreshMin`

Всегда тикает, и в static, и в relative:

- relative: новое `now` → новое окно → новый timetable fetch;
- static: то же календарное окно, но **повторный** timetable fetch (свежие заказы).

`timer(0, ms)` — первый тик сразу при открытии.

`0` / отрицательное / нечисло (`Infinity`, `NaN`) → как будто параметра нет → default `3`. Это **не** «выключить refresh».

### 3.5. Query: скрыть chrome (киоск / TV)

Читаются в `AppComponent` / `MainComponent`, **не** в `DashboardFacade`.

| Query | Значение | Эффект |
|-------|----------|--------|
| `sideMenuHidden` | `true` | скрыть боковое меню |
| `tabBarHidden` | `true` | скрыть нижний tab bar |

Любое значение ≠ `"true"` → chrome виден. Для киоска на TV/планшете всегда оба.

Copy link **не** включает query-параметры: `getFullUrl(this.params)` без `queryParams` (chrome, `lang`, `timezone`, `securityToken`).

### 3.6. Query: язык и часовой пояс

**Не** входят в `DashboardNodeParams`. Обрабатываются на уровне приложения / store.

#### `lang` — язык интерфейса

| | |
|---|---|
| Формат | query `?lang={code}` |
| Коды | `ru`, `en`, `es`, `zh` (`isSupportedLanguage` в `i18n-loader.ts`) |
| Где читается | `AppComponent.useEffectLanguageChanges` — `activatedRoute.queryParams.lang` |
| Эффект app-wide | загрузка i18n-бандла, `translateService.use(language)` |
| Эффект на dashboard | **частичный (caveat)** |

Dashboard прокидывает в date block и timegrid **`SessionQuery.getLanguage`**, а не query `lang` напрямую:

- источник: user settings language → session language (default `ru`, при первом запуске — язык устройства);
- `?lang=en` меняет ngx-translate глобально, но **не** диспатчит `SessionActions.LanguageChange` автоматически.

Практика для kiosk:

| Что | От `?lang=` | От session / user settings |
|-----|-------------|----------------------------|
| FC `locale`, подписи дней в date block | только если session language совпал | да |
| `translateService.instant` в timegrid (`allDayText`) | да | fallback |
| `Accept-Language` HTTP (`InterceptorAuth`) | **нет** — берётся `SessionQuery.getLanguage` | да |

**Вывод для доки:** для стабильного EN-киоска надёжнее session/device language или явная смена языка в приложении; `?lang=` — вспомогательный, не полный контракт dashboard-locale.

#### `timezone` — часовой пояс расчётов

| | |
|---|---|
| Формат | query `?timezone={IANA}` — например `Europe/Moscow`, `Asia/Yekaterinburg` |
| Где читается | `CurrentUserSettingsQuery.getTimeZone` — **приоритет URL** над user settings |
| Цепочка dashboard | `getTimeZone` → `getUserTimeZone` → `getDashboardTimeZone()` |
| Fallback | `DateTime.local().toFormat("z")` — IANA зона браузера/устройства |

Используется в dashboard для ( **не** relative `fromMin`/`toMin`, §3.3):

- static `startOf` / `endOf` календарного дня;
- `slotMinTime` / `slotMaxTime` (wall-clock в dashboard TZ);
- `slotMaxTime` fallback `endOf(from, day)`;
- позиционирование заказов (`mapToEventInput`);
- фон Service (`mapWorkloadPeriodToBackgroundEvents`);
- ось FullCalendar (`tt-timegrid` `timeZone`) и date header.

Без `?timezone=` — `userSettings.settings.place.timeZone` (если есть в store, в т.ч. `UNAUTHENTICATED_USER`).

Пример kiosk (Москва, EN-подписи где сработает translate, fullscreen):

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={resourceId};date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true&lang=en&timezone=Europe/Moscow
```

### 3.7. Query: `securityToken` (доступ к закрытому объекту)

| | |
|---|---|
| Формат | `?securityToken={token}` |
| Где читается | `SecurityTokenProvider.getSecurityToken(itemId)` — token должен содержать `id` объекта в `ois` |
| Эффект на dashboard | `GET item` через `GenericItemBusinessService` → `base-item-operations.getItem` подставляет token в API |

**Только GET item.** `getTimetableFromServerOnly` / `getWorkloadPeriod` token из URL **не** получают — доступ к заказам и Service-фону по-прежнему через publicity / auth API.

**Не** kiosk-by-default: для публичного экрана достаточно `publicityType` = `Link` / `PublicAvailable` (§2.4.2).

`securityToken` нужен, когда объект **не** публичный, но выдан персональный token (share-with-rights, case links). Dashboard **не парсит** token сам — только наследует поведение item-слоя.

Связанные query на других страницах (`caseParticipantId`, `askAboutAcceptElement`) dashboard **не использует**.

Copy link token **не** включает.

---

## 4. UI chrome

### 4.1. Шапка

- Back button (`ttBackButton`).
- Title (`ion-title` / `serviceName$`): trim(`title`) из matrix, если непустой; иначе первый непустой `name` среди **загруженных** колонок; иначе `""` (§3.0).
- Overflow menu: `MenuItemType.CopyLink` (иконка ellipsis). Тот же пункт в right-side menu приложения.

### 4.2. Date block

Рендерится **только** при `hasDateParam`.

Компонент: `tt-select-timetable-date-block` с `timetableType = TimetableViewType.Day`.

Поведение: prev / next / выбор даты. Input `timeZone` = `getDashboardTimeZone()` — дни/стрелки и подпись смещения в IANA-зоне dashboard.

`selectedDate` — zoned instant полуночи гражданского дня (тот же, что у timegrid / запросов). Подпись `tt-format-date` читает его в dashboard TZ. Material `[value]` / `[startAt]` берут отдельный `pickerDate` — локальный `Date` гражданских Y/M/D dashboard. Один zoned `Date` в пикер нельзя: при Auckland-киоске на LA-устройстве полночь 9 января — это 8 января по локальным полям, пикер подсветит не тот день.

Язык: `SessionQuery.getLanguage` → `language` input (не query `lang` напрямую, §3.6).

### 4.3. Timegrid

`tt-timegrid` (FullCalendar):

| Свойство | Значение на dashboard |
|----------|------------------------|
| Вид | `timeGridDay` если `columnIds.length === 1`, иначе `resourceTimeGridDay` (§3.0) |
| `headerToolbar` | пустой (даты/кнопки не от FullCalendar) |
| `slotDuration` | `01:00:00` |
| `slotLabelInterval` | `00:20:00` |
| `nowIndicator` | да |
| `eventDisplay` | `block` (для обычных событий) |
| Клик по дате/событию | **не подписан** — ничего не происходит |
| `resources` | всегда из `resources$`; FC использует на resource-view |
| `timeZone` input | `getDashboardTimeZone()` (IANA; FC игнорирует, если совпала с зоной устройства) |

Видимый вертикальный диапазон:

- источник — `visibleTimeRange$` (может отличаться от `timePeriod$` только в static mode при непустом schedule с рабочими слотами на день);
- `slotMinTime` = `from` окна в dashboard TZ, формат Luxon `"TT"` → `HH:mm:ss`;
- `slotMaxTime` = `to` окна, тот же формат.

**(caveat)** Service + session duration: фон на **рабочих** днях идёт из `getWorkloadPeriod`, а ось FC в static mode — из `item.schedule`. Зоны workload и schedule могут не совпадать; заказы вне schedule±1h на оси не видны, но грузятся за полный `timePeriod$`.

Исключение: если `from` и `to` **не в одном календарном дне dashboard TZ** (relative окно через полночь) **или** `from > to`, `slotMaxTime` = `endOf(from, day, dashboardTZ)` — сетка не рисует «хвост» следующего дня.

Default FC, если слоты не пришли: `00:00:00` … `24:00:00`.

### 4.4. Lifecycle (valve)

Потоки данных обёрнуты в `pausable(valve$)`:

- `ionViewDidEnter` → valve on (подписки живые, timer тикает);
- `ionViewWillLeave` → valve off (`NEVER`) — опрос останавливается.

`pickerDate$` **не** зовёт `pausable` сам: это `selectedDate$.pipe(map(toPickerLocalDate), distinctUntilChanged)`. Valve и refresh-тики те же, что у `selectedDate$`. `distinctUntilChanged` глушит новые `Date(y,m,d)` на каждом тике `refreshMin`, пока гражданский день не сменился.

Повторный вход включает заново. `shareReplay({ refCount: true })` на `item$` при полном unsubscribe сбросит кеш → новый GET item.

---

## 5. Часовой пояс и локаль

### 5.1. Часовой пояс (`getDashboardTimeZone`)

Приоритет (`CurrentUserSettingsQuery.getTimeZone` → `getUserTimeZone` → `getDashboardTimeZone`):

1. query `?timezone={IANA}`;
2. `userSettings.settings.place.timeZone` (auth user или `UNAUTHENTICATED_USER` в store, если есть);
3. IANA зона устройства (`DateTime.local().toFormat("z")`).

Dashboard всегда берёт IANA из `getUserTimeZone` (URL / settings / device). `getShiftedTimeZone` скрывает зону в UI на **других** экранах, если она совпала с устройством; dashboard этим селектором не пользуется. `tt-timegrid` по-прежнему не передаёт TZ в FC, если строка совпала с `DateTime.local().toFormat("z")` — это поведение timegrid, не селектора.

**Где применяется** `getDashboardTimeZone` (relative окно **вне** списка, §3.3):

- static `startOf`/`endOf` дня;
- `slotMinTime` / `slotMaxTime` (формат `"TT"` в dashboard TZ);
- `slotMaxTime` fallback `endOf(from, day)`;
- `mapToEventInput` заказов;
- `mapWorkloadPeriodToBackgroundEvents` (Service);
- `tt-timegrid` и `tt-select-timetable-date-block` (`timeZone$`);
- `shiftSelectedDate` (±1 гражданский день, затем `startOf("day")`);
- `toPickerLocalDate` (локальные Y/M/D для Material datepicker).

Relative `fromMin`/`toMin` — Luxon/`Date` **local** браузера, без `?timezone`. Same-day check в `connectSlotMaxTime` и format `"TT"` — `getDashboardTimeZone()`.

FullCalendar: input `timeZone` = dashboard TZ. Если зона совпала с устройством, `tt-timegrid` не передаёт её в FC (ось = local). Query `timezone` переключает и расчёты, и ось FC.

### 5.2. Язык / локаль

| Компонент | Источник |
|-----------|----------|
| `tt-select-timetable-date-block` `[language]` | `SessionQuery.getLanguage` |
| `tt-timegrid` `[locale]` | то же |
| ngx-translate строки (редко на dashboard) | `translateService` ← merge session + `?lang=` |

Поддерживаемые коды: `ru`, `en`, `es`, `zh`. Query `?lang=` — см. §3.6 (caveat про рассинхрон с FC locale).

---

## 6. Слои отрисовки

События склеиваются так:

```
[ ...backgroundEvents, ...timetableCaseEvents ]
```

Полный порядок в массиве = порядок передачи в FullCalendar. Background (`display: "background"`) рисуется **под** блоками заказов.

FC красит background events с opacity ~0.5 — это причина, почему Time-mask **не** использует `display: background` (§7).

### 6.1. Service + session duration

Условие: item is `ServiceItem` **и** `sessionDurationProperties.sessionDurationType` задан и ≠ `SessionDurationNone`.

Фон: `serviceItemService.getWorkloadPeriod(id, { timePeriod, type: CheckInScheduleType.AsIs })` → `mapWorkloadPeriodToBackgroundEvents`.

Цвета по `WorkloadPeriodStatus` (`TIMETABLE_COLORS`):

| Status | RGB | Hex | Типичный смысл |
|--------|-----|-----|----------------|
| `FreeTime` | `rgb(255, 243, 204)` | `#FFF3CC` | жёлтый, рабочее/свободное |
| `NotWorkingTime` | `rgb(244, 244, 245)` | `#F4F4F5` | светло-серый, вне графика |
| `BusyTime` / `NotAvailableTime` / `MyBusyTime` / `NotSuitableTime` / `Unknown` | `rgb(234, 234, 236)` | `#EAEAEC` | серый busy |

Free slots короче 25 минут растягиваются до 25 мин для читаемости (`minRenderDurationForFreeWorkloadMinutes`). Overlap'ы снимаются `WorkloadPeriodHelper.removeOverlaps`.

**(caveat)** `getWorkloadPeriod` — API сервиса; для анонима / Resource **не** используется. Если вызов падает → при непустом `item.schedule` и слотах schedule **внутри текущего `timePeriod`** — fallback на schedule background (NotWorking + FreeTime, §6.2A); иначе NotWorking only или фон пустой, заказы остаются.  
**(caveat)** **Static day-off** (`isStatic` + schedule есть, слотов в `timePeriod` нет): workload **не** вызывается → NotWorkingTime на весь `timePeriod` (§6.2B). **Relative:** workload **всегда** вызывается для Service+session; при пустом/ошибке fallback §6.2A только если слоты schedule попадают в `timePeriod`, иначе NotWorking only.  
**(caveat)** Workload **не** режется `caseStateList` киоска (§7.4). Отменённый заказ может остаться на фоне услуги, если так отдаёт API.

### 6.2. Item с `schedule.scheduleRanges` (Resource или Service без workload-path)

Условие: `getItemSchedule(item)` непустой **и** item **не** на ветке Service + session duration (§6.1), **или** day-off / fallback workload.

#### A. Рабочие слоты на период

1. На весь видимый `timePeriod` — один background **NotWorkingTime** (`#F4F4F5`).
2. Слоты из `schedule` разворачиваются как виртуальные `TimetableCase` (`extendTimetableCaseArrayWithVirtual` на периоде окна).
3. `divideTimetableCaseListAsBackgroundAndRegular` → background cases.
4. `mapTimetableCaseToBackgroundEvents(..., isColorReversed=true)` → слоты **FreeTime** (`#FFF3CC`) поверх not-working.

Итог: серая «нерабочая» подложка на весь день/окно + жёлтые рабочие интервалы.

#### B. Day-off (schedule есть, слотов на период нет)

Весь видимый `timePeriod` = **только** NotWorkingTime (серый). **Не** FreeTime fallback. Ось FC в static mode — полные сутки.

**(caveat)** Z-order background в FullCalendar зависит от версии/порядка. Ожидание: later FreeTime перекрывает NotWorkingTime. Нужна визуальная проверка на staging, автотеста нет.

### 6.3. Resource без schedule

Весь видимый `timePeriod` = один background **FreeTime** (жёлтый).  
Белого «пустого» дня нет: сетка жёлтая, занятость — серые блоки сверху.

Это **намеренный fallback** kiosk: бэкенд часто не отдаёт resource.schedule без авторизации.

### 6.4. Нет item / не Service / не Resource

Background = `[]`. Сетка без жёлтого/серого рабочего слоя (дефолтный белый FC).

---

## 7. Заказы (timetable cases)

Источник: `timetableCaseService.getTimetableFromServerOnly(id, { from, to, caseStateList }, visibility, pageSize)`.

Перед маппингом: `extendTimetableCaseArrayWithVirtual` — repeating / `UnknownTimeType` + `scheduleRanges` режутся на конкретные интервалы внутри окна.

Маппинг: `TimetableContentHelper.mapToEventInput(timeZone, expanded, [], false, false)`.

После маппинга каждому событию выставляется `resourceId = columnId` (столбец на `resourceTimeGridDay`; на одиночном `timeGridDay` FC поле игнорирует). Маска Time (§7.2) сохраняет `resourceId` через spread.

Ошибка / offline `getTimetableFromServerOnly` → `[]` (пустая сетка заказов, фон может остаться; без local SQL fallback).

### 7.1. Visibility = View

События как в обычном timetable: title, цвета кейса, время в блоке (если не скрыто CSS-классами helper'а).

### 7.2. Visibility = Time (default kiosk)

После `mapToEventInput` каждое событие переписывается:

| Поле | Значение |
|------|----------|
| `backgroundColor` | `TIMETABLE_COLORS.BusyTime` `#EAEAEC` |
| `borderColor` | то же |
| `textColor` | `var(--tt-color-step-300)` |
| `title` | trim(`busyLabel`) если непустой, иначе `DASHBOARD.BUSY_TIME_LABEL` («Занято») |
| `classNames` | `["one-line-event", "time-hidden", "dashboard-busy-label"]` |
| `display` | **не** `background` — обычный непрозрачный блок |

CSS `.time-hidden .fc-event-time { display: none }` — время в блоке скрыто. `.dashboard-busy-label .fc-event-title { font-style: italic }` — подпись курсивом.

Смысл для киоска: видно **когда занято**, не видно **кто/что**. Непрозрачный серый, чтобы не смешиваться с жёлтым FreeTime (opacity 0.5 у FC background дал бы грязный жёлто-серый).

### 7.3. Слои Time + Resource без schedule

Снизу вверх:

1. Жёлтый FreeTime на весь видимый период.
2. Непрозрачные серые BusyTime-блоки заказов.

Это целевой вид публичного resource-киоска.

### 7.4. Действующие заказы

Киоск не должен показывать отмену как занятость (иначе посетитель не запишется на свободный слот).

Фильтр **на сервер** (API `getTimetable` под `getTimetableFromServerOnly`): `caseStateList` = все `CaseState`, **кроме** `Canceled` и `Rejected`. Клиент повторно статусы не фильтрует.

| `caseState` | На сетке |
|-------------|---------|
| `UnknownCaseState` | **да** — считаем действующим |
| `Verifying` / `Waiting` / `Processing` / `Preparing` / `TimeControlled` / `TaskNotCompleted` | да |
| `Completed` / `Closed` / `TaskCompleted` | да — слот был занят |
| `Canceled` / `Rejected` | **нет** |

`getNotCancelledCaseStateList()` **не** используется: он ещё выкидывает `UnknownCaseState`. URL-параметра фильтра статусов нет.

---

## 8. Данные и запросы

На каждую колонку (N = `columnIds.length`, ≤ `DEFAULT_DASHBOARD_MAX_COLUMNS`):

- свой `genericItemBusinessService.connect({ id })` с `shareReplay(1, refCount)` — ветвление Service/Resource/фон и имя колонки;
- свой `getTimetableFromServerOnly` на каждый тик `timePeriod$` (refresh / смена даты).

`connect` делает local GET (`WaitTill.local`), затем подписан на store: когда сервер приносит более новый item (schedule, name, session duration), dashboard перескладывает фон/имя колонки. Одноразовый `get({ id })` этого не видит — Promise резолвится с кэшем.

Ошибка connect одной колонки → `undefined`: фон этой колонки пустой, timetable этой колонки всё равно грузится; остальные колонки живут. ion-title: param `title`, иначе первый непустой `name` среди загруженных колонок, иначе `""`.

`getTimetableFromServerOnly` filter: `{ from, to, caseStateList }` (§7.4).

Под капотом `getTimetableFromServerForTimePeriod` пагинирует до конца. Размер страницы: `DEFAULT_DASHBOARD_TIMETABLE_PAGE_SIZE` (**100**), не глобальный `DEFAULT_PAGE_SIZE` (10). Это **не кап**: если заказов >100, следующие страницы догружаются по 100. Остальной `getTimetable` в приложении по-прежнему 10/страница и может синхронизировать store.

Service-фон: дополнительно `getWorkloadPeriod` (auth-зависимо) **на колонку**.

Нет: `GET /resources/{id}/workload` для Resource.

---

## 9. Константы (документируемые defaults)

| Имя | Значение | Где |
|-----|----------|-----|
| `DEFAULT_DASHBOARD_REFRESH_INTERVAL_MIN` | 3 | refresh, если нет param |
| `DEFAULT_DASHBOARD_RELATIVE_FROM_MIN` | 120 | relative `from` |
| `DEFAULT_DASHBOARD_RELATIVE_TO_MIN` | 480 | relative `to` |
| `DEFAULT_DASHBOARD_TIMETABLE_PAGE_SIZE` | 100 | размер страницы `getTimetableFromServerOnly` на dashboard (не лимит) |
| `DEFAULT_DASHBOARD_MAX_COLUMNS` | 20 | hard-limit колонок после merge/dedupe (§3.0) |
| FreeTime | `#FFF3CC` | рабочее |
| NotWorkingTime | `#F4F4F5` | вне графика |
| BusyTime (и прочие busy-статусы) | `#EAEAEC` | занятость / Time-mask |

---

## 10. Примеры URL (для документации)

Host в проде: `https://torrow.net`.  
`id` копируется из карточки **ресурса** или **услуги**. Тип объекта определяет фон (§6), не путь.

Канонический kiosk (TV/планшет): `date=now` + refresh + скрытый chrome. `visibility` не задан → Time (серые блоки без ФИО).

### 10.1. Ресурс

**Предусловие для анонимного kiosk:** у ресурса `publicityType` = `Link` или `PublicAvailable`.

Фон: FreeTime на всё окно, если в GET нет `schedule`; иначе NotWorkingTime + жёлтые слоты графика. Сверху — занятость ресурса.

**Киоск на сегодня, refresh 5 мин, без меню и tab bar:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids=aae6203f1c864c88bc6bf3592d836ad29;date=now;resfreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

То же с каноническим именем параметра (`refreshMin` предпочтительнее `resfreshMin`):

```
https://torrow.net/app/tabs/tab-search/dashboard;ids=aae6203f1c864c88bc6bf3592d836ad29;date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

**Шаблон — подставить свой resource id:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={resourceId};date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

**Несколько ресурсов в столбцах + свой заголовок шапки:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={resourceId1},{resourceId2};title=Зал%201;date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

**Своя подпись занятого времени (`busyLabel`):**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={resourceId};date=now;busyLabel=%D0%A0%D0%B5%D0%B7%D0%B5%D1%80%D0%B2;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

**Relative «сейчас» (нет date bar, окно ±2ч / +8ч):**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids=aae6203f1c864c88bc6bf3592d836ad29?sideMenuHidden=true&tabBarHidden=true
```

**Фиксированный календарный день:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids=aae6203f1c864c88bc6bf3592d836ad29;date=2026-08-18;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

**Видны названия заказов (не вешать в зале с клиентами):**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids=aae6203f1c864c88bc6bf3592d836ad29;date=now;visibility=View;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

### 10.2. Услуга (Service)

**Предусловие для анонимного kiosk:** у услуги `publicityType` = `Link` или `PublicAvailable`.

Фон: `getWorkloadPeriod` (жёлтый/серый по статусам), если `sessionDurationType ≠ None` **и** API доступен (у анонима часто **нет** → только заказы). Иначе только заказы без рабочего фона.

**Киоск услуги на сегодня:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={serviceId};date=now;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

Пример id услуги (подставить свой из карточки сервиса):

```
https://torrow.net/app/tabs/tab-search/dashboard;ids=103ec111110040024888810101003;date=now;resfreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

**Relative окно услуги:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={serviceId}?sideMenuHidden=true&tabBarHidden=true
```

**Узкое окно (час назад — два вперёд), только relative, без `date`:**

```
https://torrow.net/app/tabs/tab-search/dashboard;ids={serviceId};fromMin=60;toMin=120;refreshMin=5?sideMenuHidden=true&tabBarHidden=true
```

`fromMin`/`toMin` вместе с `date` игнорируются.

### 10.3. Что менять в ссылке

| Кусок | Ресурс | Услуга |
|-------|--------|--------|
| `ids=` | id карточки Resource | id карточки Service |
| путь | одинаковый: `/app/tabs/tab-search/dashboard` | то же |
| фон | §6.2 / §6.3 | §6.1 (нужен session duration + workload API) |
| `date=now` | сегодня + стрелки дат | то же |
| без `date` | скользящее окно вокруг now | то же |
| `?sideMenuHidden=true&tabBarHidden=true` | fullscreen киоск | то же |
| `?lang=ru\|en\|es\|zh` | частично (§3.6) | то же |
| `?timezone=Europe/Moscow` | границы дня / события (§3.6) | то же или user settings TZ |
| `publicityType` item | `Link` / `PublicAvailable` для анонима | то же (§2.4.2) |

---


## 11. Что страница **не** делает

Не обещать в документации:

- неделю/месяц, список, timeline (day resource columns — да, §3.0);
- клик по слоту/заказу (переход в карточку, запись);
- UI-фильтр статусов заказов и `caseStateList` в URL; скрытие `Canceled`/`Rejected` — фиксированный серверный фильтр киоска (§7.4);
- редактирование расписания;
- синхронизацию выбранной даты с URL;
- гарантированный `schedule` ресурса у анонима (есть только FreeTime-fallback);
- workload API для Resource;
- гарантированную смену FC locale только через `?lang=` без session language (§3.6);
- проверку `publicityType` на клиенте (только ответ API);
- сообщение «нет доступа» при 403 (пустой UI / пустая колонка).

---

## 12. Приёмка (чеклист для доки и QA)

1. Resource с `Link`/`PublicAvailable`: аноним открывает URL без `visibility` → жёлтый фон, серые блоки с подписью **Занято** (курсив), время в блоке скрыто.
2. Resource с `Personal`/`Private`: аноним → пустой title, нет фона или пустая сетка (403 на GET).
3. Тот же публичный Resource с `visibility=View` → видны названия/цвета заказов.
4. `;date=YYYY-MM-DD` или `;date=now` → date bar, prev/next, refresh не сдвигает календарный день.
5. Без `date` → нет date bar, окно вокруг now, refresh сдвигает окно.
6. `refreshMin` / `resfreshMin` меняют период опроса.
7. Copy link не включает сдвиг даты и **любые** query-параметры.
8. Service + session duration: **auth** manager → workload-фон; **anon** → часто только заказы.
9. Resource **auth** manager с schedule в GET → NotWorking + FreeTime слоты; **anon** → FreeTime fallback.
10. Падение GET item / timetable fetch → страница не падает, частичный UI.
11. `?timezone=Europe/Moscow` + `;date=now` → границы суток по Москве (API filter), FC axis и date header = Moscow.
12. `?lang=en` → translate-строки; FC locale меняется только если session language = `en`.
13. `Canceled` / `Rejected` не рисуются серым блоком; заказ с `UnknownCaseState` рисуется.
14. `;date=YYYY-MM-DD` + `?timezone=` на устройстве в другой зоне → тот же календарный день venue (не local midnight устройства).
15. UTC-киоск + `?timezone=America/Los_Angeles` + `;date=2020-11-01` → Next открывает 2 ноября (не тот же день).
16. UTC-киоск + `?timezone=Pacific/Auckland` + `;date=2020-01-09` → datepicker выделяет 9 января (не 8); подпись date bar тоже 9 января.
17. Static + item с schedule + рабочий день → ось FC сужена до schedule±1h (clamp к суткам); timetable fetch — за полный день.
18. Static + schedule + day-off → серый NotWorkingTime на весь день, ось 00–24; **не** жёлтый FreeTime (Resource и Service).
19. Service + session duration + schedule + day-off **(static)** → workload не вызывается; фон = schedule NotWorkingTime.
20. Relative + Service + session duration + schedule → `getWorkloadPeriod` **вызывается** (даже если окно не пересекается со schedule).
21. `;ids=A` → `timeGridDay`; `;ids=A,B` → два столбца `resourceTimeGridDay`; шапка = имя A (без `title`); события с `resourceId` A/B.
22. `;ids=A,A,B` → столбцы A,B (dedupe).
23. `;title=X` → шапка X; столбцы по-прежнему имена item.
24. `;title=` / пробелы → fallback на первый непустой `name` загруженной колонки.
25. 403 на одном id при multi → пустая колонка, остальные живы; шапка может взять `name` surviving колонки; ось = bounds/union surviving **загруженных** колонок (403 не форсит full-day).
26. Медленный/hung `connectItem` на первом id при multi → timetable события второй колонки всё равно появляются; ось по schedule второй, если она loaded со schedule.
27. `;busyLabel=X` → подпись X на серых busy-блоках при Time; `busyLabel=` / пробелы → i18n «Занято».

---

## 13. Глоссарий для автора документации

| Термин в коде | Как писать людям |
|---------------|------------------|
| Dashboard / kiosk | Публичное расписание / экран зала |
| Static mode (`date`) | Режим «выбранный день» (`date=now` = сегодня) |
| Relative mode | Режим «сейчас» (скользящее окно, нет `date`) |
| FreeTime | Свободное / рабочее время (жёлтый) |
| NotWorkingTime | Нерабочее время (светло-серый фон) |
| BusyTime / Time visibility | Занято, без деталей (серый блок) |
| View visibility | Полные карточки записей |
| Matrix params | Параметры после `;` в пути |
| Query chrome | `?sideMenuHidden=true&tabBarHidden=true` — скрыть меню и tab bar |
| `lang` (query) | Язык i18n: `ru` / `en` / `es` / `zh`; FC locale — через session (§3.6) |
| `timezone` (query) | IANA TZ для расчётов и оси FC dashboard; приоритет над user settings |
| `securityToken` (query) | Token доступа к item; подставляется в GET item автоматически |
| `publicityType` | Доступность объекта: `Link` / `PublicAvailable` / … (не параметр URL) |
| `visibility` (URL) | Маска заказов на dashboard: `Time` vs `View` (не publicity) |
| `resfreshMin` | Устаревший синоним `refreshMin`, в старых ссылках ещё встречается |

