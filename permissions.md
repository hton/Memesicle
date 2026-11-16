# Data Safety form answers & Permissions justification — Memesicle (com.pdv.memesicle.app)

Last updated: 16.11.2026

## Data Safety (Play Console → App content → Data safety)

| Question | Answer |
|---|---|
| Does your app collect or share any of the required user data types? | **No** — the app never transmits user data to a server. All imported files, tags and kaomoji stay on the device. Foreground-app detection (usage access) and the Premium status are processed on the device only and never leave it. |
| Is all user data encrypted in transit? | **N/A or Yes** — the only network traffic is HTTPS downloads of content: a file from a link the user shares/pastes themselves, and the public starter pack archive from GitHub. Neither sends user data. |
| Do you provide a way for users to request their data be deleted? | **Yes, via app uninstall** — data is never stored anywhere but the device; uninstalling removes it completely. |
| Data types (if the form still requires a category) | **Photos and videos**: mark as collected but **not shared**, processed on device only, purpose = *App functionality*, optional, user-initiated via Share/import. |
| In-app purchase | Payments are handled entirely by Google Play Billing; the app never sees payment details. It only queries Google Play for whether Premium was purchased and stores that flag locally. |

## Permissions in the final (merged) manifest and why

### SYSTEM_ALERT_WINDOW
Draws the floating sticker/GIF/kaomoji picker panel and the floating quick-access bubble on top of other apps. This is the app's core feature — without it the panel cannot be summoned from inside another app (e.g. a chat). Only appears on explicit user action (tapping the bubble or the in-app button).

### FOREGROUND_SERVICE / FOREGROUND_SERVICE_SPECIAL_USE
Two services (`OverlayService`, `BubbleService`) draw the overlay windows. Android requires a foreground service to show an overlay from the background. `OverlayService` stops itself immediately after the panel closes; `BubbleService` runs only while the user has explicitly enabled the floating bubble via the toggle in settings.

### INTERNET
Used only for user-initiated downloads:
- a file from a link the user shares to the app or pastes themselves;
- the starter pack (`github.com/hton/Memesicle/releases/download/starter-pack/starter-pack.zip`) when the user taps "Download starter pack".

No background or automatic network activity, no user data uploaded.

### PACKAGE_USAGE_STATS (optional, granted by the user in system settings)
Lets the floating bubble check which app is currently in the foreground (package name only), so it is shown only in apps the user selected (e.g. Telegram, WhatsApp). Granted manually on the system "Usage access" screen, never requested via a runtime dialog. Without it the bubble simply shows everywhere. The information is used on the device at that moment and is not stored or transmitted.

### com.android.vending.BILLING
Added by the Google Play Billing Library. Needed for the one-time "Memesicle Premium" in-app purchase (lifts the 50 GIF/sticker limit of the free version) and for restoring it.

### ACCESS_NETWORK_STATE
Added by the Google Play Billing Library to check connectivity before contacting Google Play.

### com.pdv.memesicle.app.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION
Added automatically by AndroidX (core) for Android 13+ compatibility. App-private signature permission, not visible to the user.

### No storage permissions
The app declares no READ_MEDIA_* / READ_EXTERNAL_STORAGE permissions. Files come in only through the system Share sheet or the system file picker, which grant access to the single chosen file.

## What this build deliberately does NOT have

This Play-distribution build has **no `BIND_ACCESSIBILITY_SERVICE`**. The sideload build's auto-paste feature (which used an Accessibility Service to synthesize a paste action) was intentionally removed for this build, since Google reviews Accessibility API usage outside its intended purpose very strictly. Instead, picking an item copies it to the clipboard and the user pastes manually (long-press → Paste).

## Accessibility API Declaration form (if Play Console still prompts for it)

Answer: **"My app does not use the Accessibility API"** — true for this build (`com.pdv.memesicle.app`).

## Category & age rating

- **Category**: Communication.
- **Expected IARC age rating**: 3+ / PEGI 3 / Everyone — no adult content, violence, gambling, or user-generated-content moderation concerns beyond what the user imports privately for themselves. Final rating is assigned by the IARC questionnaire in Play Console.

---

# Ответы для формы Data Safety и обоснование разрешений — Memesicle (com.pdv.memesicle.app)

Обновлено: 16.11.2026

## Data Safety (Play Console → Содержание приложения → Безопасность данных)

| Вопрос | Ответ |
|---|---|
| Собирает или передаёт ли приложение какие-либо из обязательных типов пользовательских данных? | **Нет** — приложение никогда не передаёт данные пользователя на сервер. Все импортированные файлы, теги и каомодзи остаются на устройстве. Определение открытого приложения (доступ к данным об использовании) и статус Премиума обрабатываются только на устройстве и никуда не передаются. |
| Все пользовательские данные зашифрованы при передаче? | **N/A или Да** — единственный сетевой трафик — HTTPS-загрузка контента: файла по ссылке, которой пользователь сам поделился или которую вставил, и публичного архива стартового набора с GitHub. Ни то, ни другое не передаёт данные пользователя. |
| Предоставляете ли вы способ запросить удаление данных пользователя? | **Да, через удаление приложения** — данные никогда не хранятся где-либо кроме устройства; удаление приложения полностью их стирает. |
| Типы данных (если форма всё же требует категорию) | **Фото и видео**: отметить как собираемые, но **не передаваемые**, обрабатываются только на устройстве, цель = *Функциональность приложения*, опционально, инициируется пользователем через «Поделиться»/импорт. |
| Встроенная покупка | Платежи полностью обрабатывает Google Play Billing; приложение не видит платёжных данных. Оно только спрашивает у Google Play, куплен ли Премиум, и хранит этот признак локально. |

## Разрешения в итоговом (объединённом) манифесте и зачем они нужны

### SYSTEM_ALERT_WINDOW
Отрисовывает плавающую панель выбора стикеров/GIF/каомодзи и плавающую кнопку быстрого доступа над другими приложениями. Это основная функция приложения — без этого разрешения панель невозможно вызвать из другого приложения (например, из чата). Появляется только по явному действию пользователя (нажатие на плавающую кнопку или на кнопку в самом приложении).

### FOREGROUND_SERVICE / FOREGROUND_SERVICE_SPECIAL_USE
Два сервиса (`OverlayService`, `BubbleService`) отрисовывают оверлейные окна. Android требует foreground-сервис для показа оверлея из фона. `OverlayService` останавливает себя сразу после закрытия панели; `BubbleService` работает только пока пользователь явно включил плавающую кнопку переключателем в настройках.

### INTERNET
Используется только для загрузок, которые запускает сам пользователь:
- файла по ссылке, которой пользователь поделился с приложением или которую вставил сам;
- стартового набора (`github.com/hton/Memesicle/releases/download/starter-pack/starter-pack.zip`) по нажатию «Скачать стартовый набор».

Никакой фоновой или автоматической сетевой активности, данные пользователя не отправляются.

### PACKAGE_USAGE_STATS (опционально, выдаётся пользователем в системных настройках)
Позволяет плавающей кнопке узнать, какое приложение сейчас открыто (только имя пакета), чтобы показываться только в выбранных пользователем приложениях (например, Telegram, WhatsApp). Выдаётся вручную на системном экране «Доступ к данным об использовании», никогда не запрашивается всплывающим диалогом. Без него кнопка просто показывается везде. Информация используется на устройстве в момент проверки, не сохраняется и не передаётся.

### com.android.vending.BILLING
Добавляется библиотекой Google Play Billing. Нужно для разовой встроенной покупки «Memesicle Премиум» (снимает лимит бесплатной версии в 50 GIF/стикеров) и её восстановления.

### ACCESS_NETWORK_STATE
Добавляется библиотекой Google Play Billing для проверки подключения перед обращением к Google Play.

### com.pdv.memesicle.app.DYNAMIC_RECEIVER_NOT_EXPORTED_PERMISSION
Добавляется автоматически библиотекой AndroidX (core) для совместимости с Android 13+. Внутреннее signature-разрешение приложения, пользователю не показывается.

### Разрешений на доступ к хранилищу нет
Приложение не объявляет READ_MEDIA_* / READ_EXTERNAL_STORAGE. Файлы попадают в него только через системное меню «Поделиться» или системный выбор файлов, которые дают доступ только к одному выбранному файлу.

## Чего в этой сборке намеренно нет

В этой Play-сборке **нет `BIND_ACCESSIBILITY_SERVICE`**. Функция авто-вставки из sideload-сборки (которая использовала Accessibility Service для симуляции вставки) намеренно убрана, поскольку Google очень строго проверяет использование Accessibility API не по назначению. Вместо этого выбор элемента копирует его в буфер обмена, и пользователь вставляет вручную (долгое нажатие → Вставить).

## Форма декларации Accessibility API (если Play Console всё же запросит)

Ответ: **«Моё приложение не использует Accessibility API»** — верно для этой сборки (`com.pdv.memesicle.app`).

## Категория и возрастной рейтинг

- **Категория**: Общение (Communication).
- **Ожидаемый рейтинг IARC**: 3+ / PEGI 3 / Для всех — нет контента для взрослых, насилия, азартных игр или проблем модерации пользовательского контента, помимо того, что пользователь импортирует приватно для себя. Итоговый рейтинг назначается анкетой IARC в Play Console.
