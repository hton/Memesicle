# Data Safety form answers & Permissions justification — Memesicle (com.pdv.memesicle)

## Data Safety (Play Console → App content → Data safety)

| Question | Answer |
|---|---|
| Does your app collect or share any of the required user data types? | **No** — the app never transmits data to a server. All imported files and metadata stay on the device. |
| Is all user data encrypted in transit? | **N/A or Yes** — the only network traffic is an HTTPS download of a file from a link the user pastes themselves (content download, not outbound user-data transmission). |
| Do you provide a way for users to request their data be deleted? | **Yes, via app uninstall** — data is never stored anywhere but the device; uninstalling removes it completely. |
| Data types (if the form still requires a category) | **Photos and videos**: mark as collected but **not shared**, processed on device only, purpose = *App functionality*, optional, user-initiated via Share/import. |

## Permissions declared in AndroidManifest.xml and why

### SYSTEM_ALERT_WINDOW
Draws the floating sticker/GIF picker panel and the floating quick-access bubble on top of other apps. This is the app's core feature — without it the panel cannot be summoned from inside another app (e.g. a chat). Only appears on explicit user action (tapping the bubble or the in-app button).

### FOREGROUND_SERVICE / FOREGROUND_SERVICE_SPECIAL_USE
Two services (`OverlayService`, `BubbleService`) draw the overlay windows. Android requires a foreground service to show an overlay from the background. `OverlayService` stops itself immediately after the panel closes; `BubbleService` runs only while the user has explicitly enabled the floating bubble via the toggle in settings.

### INTERNET
Used only to download a file from a link the user pastes themselves (the "Add by link" feature / sharing a text link). No background or automatic network activity.

### READ_MEDIA_IMAGES / READ_EXTERNAL_STORAGE (maxSdkVersion 32)
Needed to read the file the user selects through the system file picker when importing a .zip library archive or an image. Access is granted via the system's own picker dialog, not requested directly by the app.

## What this build deliberately does NOT have

This Play-distribution build has **no `BIND_ACCESSIBILITY_SERVICE`**. The sideload build's auto-paste feature (which used an Accessibility Service to synthesize a paste action) was intentionally removed for this build, since Google reviews Accessibility API usage outside its intended purpose very strictly and such apps are routinely rejected or removed. Instead, picking an item copies it to the clipboard and the user pastes manually (long-press → Paste) — one extra step, no accessibility permission required.

## Accessibility API Declaration form (if Play Console still prompts for it)

Answer: **"My app does not use the Accessibility API"** — true for this build (`com.pdv.memesicle`); verified against `AndroidManifest.xml` and the absence of any `AccessibilityService` subclass in the source tree.

## Category & age rating

- **Category**: Tools, or Communication.
- **Expected IARC age rating**: 3+ / PEGI 3 / Everyone — no adult content, violence, gambling, or user-generated-content moderation concerns beyond what the user imports privately for themselves. Final rating is assigned by the IARC questionnaire in Play Console.

---

# Ответы для формы Data Safety и обоснование разрешений — Memesicle (com.pdv.memesicle)

## Data Safety (Play Console → Содержание приложения → Безопасность данных)

| Вопрос | Ответ |
|---|---|
| Собирает или передаёт ли приложение какие-либо из обязательных типов пользовательских данных? | **Нет** — приложение никогда не передаёт данные на сервер. Все импортированные файлы и метаданные остаются на устройстве. |
| Все пользовательские данные зашифрованы при передаче? | **N/A или Да** — единственный сетевой трафик — это HTTPS-загрузка файла по ссылке, которую пользователь сам вставляет (загрузка контента, а не исходящая передача пользовательских данных). |
| Предоставляете ли вы способ запросить удаление данных пользователя? | **Да, через удаление приложения** — данные никогда не хранятся где-либо кроме устройства; удаление приложения полностью их стирает. |
| Типы данных (если форма всё же требует категорию) | **Фото и видео**: отметить как собираемые, но **не передаваемые**, обрабатываются только на устройстве, цель = *Функциональность приложения*, опционально, инициируется пользователем через Share/импорт. |

## Разрешения, объявленные в AndroidManifest.xml, и зачем они нужны

### SYSTEM_ALERT_WINDOW
Отрисовывает плавающую панель выбора стикеров/GIF и плавающую кнопку быстрого доступа над другими приложениями. Это основная функция приложения — без этого разрешения панель невозможно вызвать из другого приложения (например, из чата). Появляется только по явному действию пользователя (нажатие на кнопку или на кнопку в самом приложении).

### FOREGROUND_SERVICE / FOREGROUND_SERVICE_SPECIAL_USE
Два сервиса (`OverlayService`, `BubbleService`) отрисовывают оверлейные окна. Android требует foreground-сервис для показа оверлея из фона. `OverlayService` останавливает себя сразу после закрытия панели; `BubbleService` работает только пока пользователь явно включил плавающую кнопку через переключатель в настройках.

### INTERNET
Используется только для загрузки файла по ссылке, которую пользователь сам вставляет (функция «Добавить по ссылке» / отправка текстовой ссылки через Share). Никакой фоновой или автоматической сетевой активности нет.

### READ_MEDIA_IMAGES / READ_EXTERNAL_STORAGE (maxSdkVersion 32)
Нужно для чтения файла, который пользователь выбирает через системный выбор файлов при импорте .zip-архива библиотеки или изображения. Доступ предоставляется через системный диалог выбора, а не запрашивается приложением напрямую.

## Чего в этой сборке намеренно нет

В этой Play-сборке **нет `BIND_ACCESSIBILITY_SERVICE`**. Функция авто-вставки из sideload-сборки (которая использовала Accessibility Service для симуляции действия вставки) была намеренно убрана из этой сборки, поскольку Google очень строго проверяет использование Accessibility API не по назначению, и такие приложения регулярно отклоняются или удаляются. Вместо этого выбор элемента копирует его в буфer обмена, и пользователь вставляет вручную (долгое нажатие → Вставить) — один дополнительный шаг, разрешение на доступность не требуется.

## Форма декларации Accessibility API (если Play Console всё же запросит)

Ответ: **«Моё приложение не использует Accessibility API»** — верно для этой сборки (`com.pdv.memesicle`); подтверждено проверкой `AndroidManifest.xml` и отсутствием каких-либо подклассов `AccessibilityService` в исходном коде.

## Категория и возрастной рейтинг

- **Категория**: Инструменты (Tools) или Общение (Communication).
- **Ожидаемый рейтинг IARC**: 3+ / PEGI 3 / Для всех — нет контента для взрослых, насилия, азартных игр или проблем модерации пользовательского контента, помимо того, что пользователь импортирует приватно для себя. Итоговый рейтинг назначается анкетой IARC в Play Console.
