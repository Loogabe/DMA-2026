---
title: "Практическая работа №3: Конфигурирование AndroidManifest.xml"
discipline: "Разработка мобильных приложений"
status: "Active"
author: "УПМ 2"
tags: [android, manifest, xml, permissions, intent-filter, security]
---

# Практическая работа: Проектирование и настройка AndroidManifest.xml

> [!NOTE]
> **Цель работы:** Изучить архитектурную роль манифеста Android-приложения, освоить регистрацию ключевых системных компонентов, научиться объявлять политики безопасности, настраивать Intent-фильтры, управлять системными разрешениями и аппаратными требованиями.

---

## 📋 Содержание

1. [Вводные понятия и назначение AndroidManifest.xml](#0-вводные-понятия-и-назначение-androidmanifestxml)
2. [Тема 1. Корневая структура и тег `<manifest>`](#тема-1-корневая-структура-и-тег-manifest)
3. [Тема 2. Тег `<application>`: глобальные свойства приложения](#тема-2-тег-application-глобальные-свойства-приложения)
4. [Тема 3. Регистрация четырех фундаментальных компонентов](#тема-3-регистрация-четырех-фундаментальных-компонентов)
5. [Тема 4. Механизм `<intent-filter>` и Deep Links (App Links)](#тема-4-механизм-intent-filter-и-deep-links-app-links)
6. [Тема 5. Разрешения: `<uses-permission>` и модель безопасности](#тема-5-разрешения-uses-permission-и-модель-безопасности)
7. [Тема 6. Аппаратные фильтры `<uses-feature>` и видимость пакетов `<queries>`](#тема-6-аппаратные-фильтры-uses-feature-и-видимость-пакетов-queries)
8. [Тема 7. Метаданные `<meta-data>` и тонкая конфигурация окружения](#тема-7-метаданные-meta-data-и-тонкая-конфигурация-окружения)
9. [50 Смешанных практических заданий (Manifest & Production Cases)](#50-смешанных-практических-заданий)
10. [Критерии оценки и регламент сдачи](#критерии-оценки-и-регламент-сдачи)

---

## 0. Вводные понятия и назначение AndroidManifest.xml

Файл `AndroidManifest.xml` — это центральный декларативный паспорт каждого Android-приложения. До запуска первого байта кода операционная система Android считывает этот файл через службу `PackageManagerService` для определения:

- Уникального идентификатора приложения и его точки входа.
- Списка запрашиваемых привилегий (доступ к камере, геолокации, сети).
- Состава компонентов: Activities (экраны), Services (фоновые службы), Broadcast Receivers (приемники событий), Content Providers (поставщики данных).
- Совместимости устройства с экраном, процессором и датчиками.

---

## Тема 1. Корневая структура и тег `<manifest>`

Корневой элемент XML-документа определяет пространство имен Android и связывает манифест с механизмом слияния (Manifest Merger) во время сборки Gradle.

### Пример кода

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.university.mobileapp">

    <!-- Дочерние узлы: permissions, features, application -->

</manifest>
```

### Задания для закрепления

1. **Декларация пространств имен:** Добавьте в корневой тег пространство имен `xmlns:tools` для управления директивами переопределения конфликтов при слиянии библиотечных манифестов.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.university.mobileapp">
    <!-- Пространство имен tools позволяет использовать директивы:
         tools:node, tools:replace, tools:remove, tools:overrideLibrary -->
</manifest>
```

2. **Исключение из слияния:** Используя атрибут `tools:node="remove"`, напишите директиву удаления нежелательного разрешения, добавляемого внешней сторонней библиотекой.

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.university.mobileapp">

    <!-- Удаляем нежелательное разрешение, навязываемое сторонней библиотекой -->
    <uses-permission
        android:name="android.permission.ACCESS_BACKGROUND_LOCATION"
        tools:node="remove" />

</manifest>
```

3. **Разрешение конфликта атрибутов:** Примените `tools:replace="android:allowBackup"` для принудительной установки собственного флага бэкапа поверх значения зависимости.

```xml
<application
    android:allowBackup="false"
    android:label="@string/app_name"
    tools:replace="android:allowBackup">
    <!-- tools:replace принудительно устанавливает наше значение
         android:allowBackup поверх значения из библиотечного манифеста -->
</application>
```

4. **Определение пространства имен кастомных атрибутов:** Напишите XML-блок манифеста с поддержкой разделяемого ID пользователя (`android:sharedUserId`, с учетом ограничений устаревания).

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.university.mobileapp"
    android:sharedUserId="com.university.shared"
    android:sharedUserLabel="@string/shared_user_label">

    <!-- ВНИМАНИЕ: android:sharedUserId устарел начиная с Android 29 (API 29).
         Приложения, использующие его, не могут публиковаться в Google Play
         для новых версий. Используется только для системных/предустановленных
         приложений, поставляемых в одном подписанном образе. -->
</manifest>
```

5. **Проверка синтаксической валидности:** Составьте минимальный валидный каркас файла `AndroidManifest.xml`, готовый для успешной компиляции утилитой `aapt2`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.university.mobileapp">

    <application
        android:label="@string/app_name"
        android:icon="@mipmap/ic_launcher">
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```


---

## Тема 2. Тег `<application>`: глобальные свойства приложения

Тег `<application>` задает мета-характеристики всего процесса: тему оформления, иконку, кастомный класс-наследник `Application`, правила резервного копирования и политику сетевой безопасности.

### Атрибуты безопасности и оформления

| Атрибут                         | Тип значения   | Назначение                                                      |
| :------------------------------ | :------------- | :-------------------------------------------------------------- |
| `android:name`                  | Класс (`.App`) | Кастомный подкласс `android.app.Application`                    |
| `android:icon` / `roundIcon`    | `@mipmap/*`    | Векторная или растровая адаптивная иконка                       |
| `android:theme`                 | `@style/*`     | Глобальная системная тема (Material 3)                          |
| `android:allowBackup`           | `boolean`      | Разрешение резервного копирования данных через ADB/Cloud        |
| `android:usesCleartextTraffic`  | `boolean`      | Разрешение незашифрованного HTTP-трафика (`false` по умолчанию) |
| `android:networkSecurityConfig` | `@xml/*`       | Ссылка на файл кастомных сертификатов и SSL-Pinning             |
| `android:supportsRtl`           | `boolean`      | Поддержка арабских и еврейских языков с письмом справа налево   |

### Пример кода

```xml
<application
    android:name=".AppController"
    android:allowBackup="false"
    android:icon="@mipmap/ic_launcher"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:label="@string/app_name"
    android:supportsRtl="true"
    android:theme="@style/Theme.ModernApp"
    android:usesCleartextTraffic="false"
    android:networkSecurityConfig="@xml/network_security_config">

    <!-- Компоненты приложения -->
</application>
```

### Задания для закрепления

1. **Защита от утечки данных через бэкап:** Сконфигурируйте тег `<application>` для финансового приложения, полностью запретив бэкап и извлечение данных через утилиту ADB.

```xml
<application
    android:name=".FintechApplication"
    android:allowBackup="false"
    android:fullBackupContent="false"
    android:dataExtractionRules="@xml/data_extraction_rules"
    android:label="@string/app_name"
    android:icon="@mipmap/ic_launcher">
    <!-- allowBackup=false полностью запрещает резервное копирование через ADB и Cloud -->
</application>
```

2. **Подключение SSL-Pinning конфига:** Добавьте атрибут `android:networkSecurityConfig` со ссылкой на XML-ресурс и составьте шаблон самого файла конфигурации безопасности.

```xml
<application
    android:networkSecurityConfig="@xml/network_security_config"
    ... >
</application>
```

3. **Регистрация кастомного Application:** Зарегистрируйте класс `.MainApplication`, в котором будет происходить инициализация DI-контейнера и аналитики.

```xml
<application
    android:name=".MainApplication"
    android:label="@string/app_name"
    android:icon="@mipmap/ic_launcher">
</application>
```

4. **Локализация названия приложения:** Настройте атрибут `android:label` так, чтобы название бралось из строковых ресурсов с поддержкой динамической смены языка.

```xml
<application
    android:label="@string/app_name"
    android:supportsRtl="true">
</application>
```

5. **Поддержка больших куч памяти (Large Heap):** Добавьте атрибут `android:largeHeap="true"` для графического редактора и обоснуйте риски его бездумного использования.

```xml
<application
    android:largeHeap="true"
    ... >
</application>
```


---

## Тема 3. Регистрация четырех фундаментальных компонентов

В Android любой компонент, способный выступать независимой точкой входа в приложение, обязан быть явно объявлен в манифесте.

### Архитектурные требования Android 12+ (API 31+)

Если компонент содержит `<intent-filter>`, атрибут `android:exported` **обязан** быть явно выставлен в `true` или `false`. Несоблюдение приводит к ошибке установки `INSTALL_FAILED_VERIFICATION_FAILURE`.

### Пример кода

```xml
<application ...>

    <!-- 1. Экран авторизации (Точка входа) -->
    <activity
        android:name=".ui.AuthActivity"
        android:exported="true">
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>

    <!-- 2. Внутренний экран профиля -->
    <activity
        android:name=".ui.ProfileActivity"
        android:exported="false"
        android:screenOrientation="portrait"
        android:launchMode="singleTop" />

    <!-- 3. Фоновая служба воспроизведения медиа (Android 14+) -->
    <service
        android:name=".playback.MediaService"
        android:exported="false"
        android:foregroundServiceType="mediaPlayback" />

    <!-- 4. Широковещательный приемник перезагрузки устройства -->
    <receiver
        android:name=".receivers.BootReceiver"
        android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

    <!-- 5. Провайдер безопасного обмена файлами -->
    <provider
        android:name="androidx.core.content.FileProvider"
        android:authorities="${applicationId}.fileprovider"
        android:exported="false"
        android:grantUriPermissions="true">
        <meta-data
            android:name="android.support.FILE_PROVIDER_PATHS"
            android:resource="@xml/file_paths" />
    </provider>

</application>
```

### Задания для закрепления

1. **Регистрация стартовой Activity:** Объявите экран `SplashScreenActivity` в качестве единственной стартовой точки запуска с главного экрана смартфона.

```xml
<activity
    android:name=".ui.SplashScreenActivity"
    android:exported="true"
    android:theme="@style/Theme.App.Starting">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

2. **Изоляция служебного сервиса:** Опишите фоновый сервис синхронизации локальной базы данных, гарантировав невозможность его вызова из внешних приложений.

```xml
<service
    android:name=".sync.LocalDbSyncService"
    android:exported="false" />
```

3. **Блокировка пересоздания при повороте:** Сконфигурируйте для игровой Activity атрибут `android:configChanges="orientation|screenSize|keyboardHidden"`.

```xml
<activity
    android:name=".game.GameActivity"
    android:exported="false"
    android:configChanges="orientation|screenSize|keyboardHidden"
    android:screenOrientation="sensorLandscape" />
```

4. **Выбор режима запуска (launchMode):** Зарегистрируйте экран звонка `IncomingCallActivity` с режимом запуска `singleInstance`.

```xml
<activity
    android:name=".call.IncomingCallActivity"
    android:exported="false"
    android:launchMode="singleInstance"
    android:excludeFromRecents="true"
    android:showOnLockScreen="true" />
```

5. **Безопасная настройка FileProvider:** Напишите полную декларацию `FileProvider` для выдачи временного доступа камере к сохранению сделанного снимка.

```xml
<provider
    android:name="androidx.core.content.FileProvider"
    android:authorities="${applicationId}.fileprovider"
    android:exported="false"
    android:grantUriPermissions="true">
    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_paths" />
</provider>
```


---

## Тема 4. Механизм `<intent-filter>` и Deep Links (App Links)

Интент-фильтры сообщают системе, какие неявные намерения (Implicit Intents) готов обработать компонент: открытие ссылки, отправка текста (`ACTION_SEND`), просмотр геоточки или веб-адреса.

### Структура Intent-фильтра

| Элемент      | Назначение                               | Типичные значения                                        |
| :----------- | :--------------------------------------- | :------------------------------------------------------- |
| `<action>`   | Действие, на которое реагирует компонент | `ACTION_VIEW`, `ACTION_SEND`, `ACTION_DIAL`              |
| `<category>` | Контекст использования                   | `DEFAULT`, `BROWSABLE`, `LAUNCHER`                       |
| `<data>`     | Схема, хост, путь и MIME-тип данных      | `scheme="https"`, `host="shop.ru"`, `mimeType="image/*"` |

### Пример кода (Android App Links с авто-верификацией)

```xml
<activity
    android:name=".ui.ProductDetailsActivity"
    android:exported="true">

    <!-- Кастомная схема: myapp://products/view?id=123 -->
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="myapp" android:host="products" />
    </intent-filter>

    <!-- Официальный App Link (проверяется через assetlinks.json на домене) -->
    <intent-filter android:autoVerify="true">
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:scheme="https"
            android:host="store.university.ru"
            android:pathPrefix="/item/" />
    </intent-filter>

</activity>
```

### Задания для закрепления

1. **Перехват функции "Поделиться" (Share Target):** Зарегистрируйте Activity, способную принимать изображения любого формата через системный диалог `ACTION_SEND`.

```xml
<activity
    android:name=".share.ShareReceiverActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.SEND" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:mimeType="image/*" />
    </intent-filter>
</activity>
```

2. **Обработка звонковых ссылок:** Настройте фильтр для перехвата номеров телефонов со схемой `tel:`.

```xml
<activity
    android:name=".dialer.DialActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="tel" />
    </intent-filter>
</activity>
```

3. **Диплинк с маской пути:** Напишите тег `<data>` с атрибутом `android:pathPattern`, фильтрующий только PDF-документы: `.*\\.pdf`.

```xml
<data
    android:scheme="https"
    android:host="docs.university.ru"
    android:pathPattern=".*\\.pdf" />
```

4. **Фильтрация по нескольким MIME-типам:** Сконфигурируйте интент-фильтр, принимающий текстовые файлы (`text/plain`) и веб-страницы (`text/html`).

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="text/plain" />
    <data android:mimeType="text/html" />
</intent-filter>
```

5. **Настройка авто-верификации (App Link):** Добавьте директиву `android:autoVerify="true"` и опишите назначение связи с файлом `/.well-known/assetlinks.json`.

```xml
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data
        android:scheme="https"
        android:host="store.university.ru"
        android:pathPrefix="/item/" />
</intent-filter>
```


---

## Тема 5. Разрешения: `<uses-permission>` и модель безопасности

Разрешения делятся на нормальные (Normal, выдаются при установке) и опасные (Dangerous, требуют runtime-запроса у пользователя).

### Современные требования Android 13–15+

- Доступ к уведомлениям требует отдельного разрешения `POST_NOTIFICATIONS`.
- Доступ к медиа разделен: `READ_MEDIA_IMAGES`, `READ_MEDIA_VIDEO`, `READ_MEDIA_AUDIO`.
- Сервисы переднего плана требуют явного указания типа разрешения: `FOREGROUND_SERVICE_LOCATION`, `FOREGROUND_SERVICE_CAMERA` и т.д.

### Пример кода

```xml
<manifest ...>

    <!-- 1. Нормальное разрешение (доступ в сеть) -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

    <!-- 2. Опасное разрешение (геолокация) -->
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />

    <!-- 3. Медиафайлы (Android 13+) -->
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />

    <!-- 4. Ограничение по версии SDK (вибрация только до Android 12) -->
    <uses-permission
        android:name="android.permission.VIBRATE"
        android:maxSdkVersion="31" />

    <!-- 5. Декларация собственного разрешения для защиты провайдера -->
    <permission
        android:name="com.university.permission.READ_INTERNAL_LOGS"
        android:protectionLevel="signature" />

</manifest>
```

### Задания для закрепления

1. **Разрешения для геолокационного трекера:** Объявите набор разрешений для непрерывного фонового трекинга: приблизительная геопозиция, точная геопозиция и фоновая геопозиция (`ACCESS_BACKGROUND_LOCATION`).

```xml
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />
```

2. **Разрешение уведомлений:** Добавьте разрешение `POST_NOTIFICATIONS` с директивой поддержки обратной совместимости для старых версий ОС.

```xml
<uses-permission
    android:name="android.permission.POST_NOTIFICATIONS"
    android:maxSdkVersion="33" />
<!-- На API < 33 разрешение не требуется, но объявление безопасно -->
```

3. **Защита собственного компонента:** Создайте кастомное разрешение с уровнем защиты `signature`, чтобы только приложения с тем же ключом подписи могли обращаться к сервису.

```xml
<permission
    android:name="com.university.permission.ACCESS_SYNC_SERVICE"
    android:protectionLevel="signature"
    android:label="@string/perm_sync_label"
    android:description="@string/perm_sync_desc" />

<service
    android:name=".sync.SyncService"
    android:exported="true"
    android:permission="com.university.permission.ACCESS_SYNC_SERVICE" />
```

4. **Ограничение максимальной версии SDK:** Используя атрибут `android:maxSdkVersion`, ограничьте действие разрешения записи во внешнюю память (`WRITE_EXTERNAL_STORAGE`) версией Android 9 (API 28).

```xml
<uses-permission
    android:name="android.permission.WRITE_EXTERNAL_STORAGE"
    android:maxSdkVersion="28" />
```

5. **Точные будильники (Exact Alarms):** Зарегистрируйте разрешение `SCHEDULE_EXACT_ALARM` и объясните строгие правила модерации Google Play для этого типа разрешений.

```xml
<uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM" />
<uses-permission android:name="android.permission.USE_EXACT_ALARM" />
```


---

## Тема 6. Аппаратные фильтры `<uses-feature>` и видимость пакетов `<queries>`

Тег `<uses-feature>` фильтрует отображение приложения в Google Play в зависимости от наличия физических чипов и датчиков на устройстве. Тег `<queries>` (Android 11+) открывает видимость других установленных приложений (Package Visibility).

### Пример кода

```xml
<manifest ...>

    <!-- Требуется камера с автофокусом обязательно -->
    <uses-feature
        android:name="android.hardware.camera"
        android:required="true" />
    <uses-feature
        android:name="android.hardware.camera.autofocus"
        android:required="false" />

    <!-- Требуется модуль BLE (Bluetooth Low Energy) -->
    <uses-feature
        android:name="android.hardware.bluetooth_le"
        android:required="true" />

    <!-- Доступ к внешним пакетам (Package Visibility) -->
    <queries>
        <!-- Разрешить взаимодействие с браузерами -->
        <intent>
            <action android:name="android.intent.action.VIEW" />
            <data android:scheme="https" />
        </intent>
        <!-- Разрешить обращение к приложению оплаты конкретного банка -->
        <package android:name="ru.sberbankmobile" />
    </queries>

</manifest>
```

### Задания для закрепления

1. **Необязательный датчик отпечатка пальца:** Опишите фичу сканера отпечатков пальцев так, чтобы приложение могло устанавливаться на устройства без биометрии.

```xml
<uses-feature
    android:name="android.hardware.fingerprint"
    android:required="false" />
```

2. **Фильтрация устройств с NFC:** Напишите манифест-декларацию для платежного стикера или терминала, требующую физического наличия чипа NFC.

```xml
<uses-feature
    android:name="android.hardware.nfc"
    android:required="true" />
<uses-permission android:name="android.permission.NFC" />
```

3. **Декларация вызова сторонней навигации:** С помощью блока `<queries>` опишите возможность проверки наличия установленного приложения Яндекс Карты или 2ГИС.

```xml
<queries>
    <package android:name="ru.yandex.yandexmaps" />
    <package android:name="ru.dublgis.dgismobile" />
</queries>
```

4. **Телефония и отправка SMS:** Задекларируйте признак `android.hardware.telephony` со значением `required="false"` для обеспечения совместимости приложения с планшетами без SIM-карт.

```xml
<uses-feature
    android:name="android.hardware.telephony"
    android:required="false" />
```

5. **Поддержка контроллеров:** Настройте фичу поддержки геймпадов для Android TV версии вашего приложения.

```xml
<uses-feature
    android:name="android.hardware.gamepad"
    android:required="false" />
<uses-feature
    android:name="android.hardware.touchscreen"
    android:required="false" />
```


---

## Тема 7. Метаданные `<meta-data>` и тонкая конфигурация окружения

Тег `<meta-data>` передает произвольные пары «ключ-значение» системным компонентам и внешним SDK (Google Maps, аналитика, архитектурные провайдеры).

### Пример кода

```xml
<application ...>

    <!-- API ключ картографического сервиса -->
    <meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="@string/google_maps_key" />

    <!-- Отключение автоматической инициализации Firebase Analytics -->
    <meta-data
        android:name="firebase_analytics_collection_deactivated"
        android:value="true" />

    <!-- Файл конфигурации мультиязычности (Per-App Language) -->
    <meta-data
        android:name="android.content.res.LocaleConfig"
        android:resource="@xml/locales_config" />

</application>
```

### Задания для закрепления

1. **Интеграция карт:** Добавьте тег метаданных для внедрения ключа Яндекс MapKit внутри элемента `<application>`.

```xml
<meta-data
    android:name="com.yandex.mapkit.ApiKey"
    android:value="@string/yandex_mapkit_key" />
```

2. **Конфигуратор смены языка внутри приложения:** Зарегистрируйте ресурс `LocaleConfig` для встроенного селектора языков в Android 13+.

```xml
<meta-data
    android:name="android.content.res.LocaleConfig"
    android:resource="@xml/locales_config" />
```

3. **Метаданные внутри конкретной Activity:** Привяжите системный поисковый конфигуратор (`android.app.searchable`) к конкретному экрану `SearchActivity`.

```xml
<activity
    android:name=".search.SearchActivity"
    android:exported="false">
    <meta-data
        android:name="android.app.searchable"
        android:resource="@xml/searchable" />
</activity>
```

4. **Отключение Startup-библиотеки:** Настройте метаданные для ручной инициализации библиотеки `androidx.startup`.

```xml
<provider
    android:name="androidx.startup.InitializationProvider"
    android:authorities="${applicationId}.androidx-startup"
    android:exported="false"
    tools:node="merge">
    <meta-data
        android:name="androidx.work.WorkManagerInitializer"
        android:value="androidx.startup"
        tools:node="remove" />
</provider>
```

5. **Масштабирование экранов (High Refresh Rate):** Добавьте мета-параметры для активации поддержки частоты развертки дисплея 120 Гц.

```xml
<meta-data
    android:name="android.max_aspect"
    android:value="2.4" />
<!-- Реальная поддержка high refresh rate задаётся в теме:
     <item name="android:preferredDisplayModeId">2</item> -->
```


---

## Практические задания:

Задания разделены по реальным сценариям разработки коммерческих приложений (FinTech, IoT, Media, Social, Delivery, Enterprise).

### Блок 1: Архитектура, слияние манифестов и запуск 

1. **Диагностика конфликта библиотек:** Внешняя библиотека объявляет `minSdkVersion="24"`, а ваш проект `minSdkVersion="21"`. Напишите директиву `tools:overrideLibrary`, разрешающую конфликт.

```xml
<uses-sdk
    android:minSdkVersion="21"
    tools:overrideLibrary="com.external.lib" />
```

2. **Скрытие иконки приложения из лаунчера:** Сконфигурируйте Activity так, чтобы она была доступна только по вызову из другого приложения, но не отображалась в списке приложений смартфона.

```xml
<activity
    android:name=".internal.HiddenActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="com.university.internal.OPEN" />
        <category android:name="android.intent.category.DEFAULT" />
    </intent-filter>
</activity>
```

3. **Splash Screen в Android 12+:** Настройте метаданные и тему стартового экрана `Theme.SplashScreen` в блоке Activity в соответствии со стандартами Android Core Splashscreen.

```xml
<activity
    android:name=".ui.MainActivity"
    android:exported="true"
    android:theme="@style/Theme.App.Starting">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

4. **Алиас Activity (activity-alias):** Создайте псевдоним `<activity-alias>` для динамической смены новогодней иконки приложения через `PackageManager`.

```xml
<activity-alias
    android:name=".alias.NewYearIcon"
    android:enabled="false"
    android:exported="true"
    android:icon="@mipmap/ic_launcher_newyear"
    android:targetActivity=".ui.MainActivity">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity-alias>
```

5. **Режим отображения "Картинка в картинке" (PiP):** Включите поддержку Picture-in-Picture (`android:supportsPictureInPicture="true"`) и настройте корректные флаги смены конфигураций экрана.

```xml
<activity
    android:name=".video.PlayerActivity"
    android:exported="false"
    android:supportsPictureInPicture="true"
    android:configChanges="screenSize|smallestScreenSize|screenLayout|orientation" />
```

6. **Многооконный режим (Multi-Window):** Ограничьте минимальные размеры плавающего окна приложения атрибутом `<layout android:minWidth="300dp" android:minHeight="450dp" />`.

```xml
<activity
    android:name=".ui.FloatingActivity"
    android:exported="false"
    android:resizeableActivity="true">
    <layout
        android:minWidth="300dp"
        android:minHeight="450dp"
        android:defaultWidth="400dp"
        android:defaultHeight="600dp" />
</activity>
```

7. **Изоляция в отдельном процессе:** Настройте запуск фонового аудиосервиса в независимом системном процессе операционной системы с помощью атрибута `android:process=":playback_process"`.

```xml
<service
    android:name=".playback.AudioService"
    android:exported="false"
    android:process=":playback_process" />
```

8. **Исключение из меню недавних приложений (Recents):** Настройте атрибут `android:excludeFromRecents="true"` для экрана ввода мастер-пароля.

```xml
<activity
    android:name=".security.MasterPasswordActivity"
    android:exported="false"
    android:excludeFromRecents="true"
    android:noHistory="true" />
```

9. **Защита от скриншотов и превью:** Объясните, какие настройки манифеста влияют на превью экрана в списке задач и как они соотносятся с программным флагом `FLAG_SECURE`.

Настройки манифеста, влияющие на превью в Recents:

- android:excludeFromRecents="true" — полностью убирает из списка.

- android:noHistory="true" — экран не сохраняется в стеке.

Программный флаг WindowManager.LayoutParams.FLAG_SECURE блокирует скриншоты и превью. В манифесте прямого аналога нет — только через код.

10. **Кастомный класс TestInstrumentationRunner:** Задекларируйте тег `<instrumentation>` для запуска кастомного раннера UI-тестов.

```xml
<instrumentation
    android:name="com.university.test.CustomTestRunner"
    android:targetPackage="com.university.mobileapp"
    android:functionalTest="false"
    android:handleProfiling="false"
    android:label="Tests for com.university.mobileapp" />
```


### Блок 2: Безопасность, шифрование и сетевой контур

11. **Блокировка инъекции динамического кода:** Сконфигурируйте тег манифеста для защиты от динамической загрузки вредоносного байт-кода (`android:isolatedProcess="true"` для сервиса).

```xml
<service
    android:name=".sandbox.PluginHostService"
    android:exported="false"
    android:isolatedProcess="true" />
```

12. **Разрешение локального HTTP для разработки:** Напишите XML-конфиг сетевой безопасности, разрешающий незашифрованный трафик исключительно для IP-адреса локального сервера `192.168.1.50` и блокирующий его для внешнего интернета.

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="false" />
    <domain-config cleartextTrafficPermitted="true">
        <domain includeSubdomains="false">192.168.1.50</domain>
    </domain-config>
</network-security-config>
```

13. **Защита Content Provider от атак внедрения:** Спроектируйте объявление провайдера с раздельными правами на чтение и запись (`android:readPermission` и `android:writePermission`).

```xml
<provider
    android:name=".data.SecureProvider"
    android:authorities="${applicationId}.secureprovider"
    android:exported="true"
    android:readPermission="com.university.permission.READ_DATA"
    android:writePermission="com.university.permission.WRITE_DATA" />
```

14. **Декларация временных разрешений на URI:** Настройте тег `<grant-uri-permission android:pathPrefix="/shared_docs/" />` внутри безопасного провайдера данных.

```xml
<provider
    android:name=".data.SharedDocsProvider"
    android:authorities="${applicationId}.docsprovider"
    android:exported="false"
    android:grantUriPermissions="true">
    <grant-uri-permission android:pathPrefix="/shared_docs/" />
</provider>
```

15. **Запрет отладки приложения в продакшене:** Убедитесь в отсутствии уязвимости с помощью директивы `android:debuggable="false"` с механизмом принудительной замены сборщиком.

```xml
<application
    android:debuggable="false"
    ... >
</application>
```

16. **Защита BroadcastReceiver от поддельных широковещательных сообщений:** Закройте неэкспортированный ресивер системным разрешением уровня `signature`.

```xml
<receiver
    android:name=".receivers.SecureReceiver"
    android:exported="true"
    android:permission="com.university.permission.SEND_SECURE_BROADCAST" />
```

17. **Разграничение доступа через системные роли:** Ограничьте вызов Activity разрешением `android.permission.BIND_ACCESSIBILITY_SERVICE`.

```xml
<activity
    android:name=".a11y.ConfigActivity"
    android:exported="true"
    android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE" />
```

18. **Политика обработки резервных копий (Backup Rules):** Создайте файл `@xml/backup_rules` и привяжите его в манифесте через `android:fullBackupContent` и `android:dataExtractionRules`.

```xml
<application
    android:fullBackupContent="@xml/backup_rules"
    android:dataExtractionRules="@xml/data_extraction_rules"
    ... >
</application>
```

19. **Защита от Overlay-атак (Tapjacking):** Сконфигурируйте фильтрацию входящих касаний для экрана подтверждения банковской транзакции.

```xml
<activity
    android:name=".payment.ConfirmTransactionActivity"
    android:exported="false"
    android:filterTouchesWhenObscured="true" />
```

20. **Аудит манифеста утилитой Androbugs:** Выявите 3 критические уязвимости в представленном фрагменте манифеста с открытым `exported="true"` без разрешений.

```xml
<!-- УЯЗВИМОСТЬ 1: exported=true без permission -->
<activity android:name=".AdminActivity" android:exported="true" />

<!-- УЯЗВИМОСТЬ 2: exported=true у провайдера -->
<provider
    android:name=".data.UserProvider"
    android:authorities="com.app.provider"
    android:exported="true" />

<!-- УЯЗВИМОСТЬ 3: exported=true у ресивера без permission -->
<receiver android:name=".receivers.PaymentReceiver" android:exported="true" />
```


### Блок 3: Интент-фильтры, интеграция и Deep Links 

21. **Интеграция с NFC-ридером (NFC Tag Dispatch):** Напишите интент-фильтр для перехвата смарт-карты стандарта Mifare: `android.nfc.action.TECH_DISCOVERED`.

```xml
<activity
    android:name=".nfc.NfcReaderActivity"
    android:exported="true"
    android:launchMode="singleTop">
    <intent-filter>
        <action android:name="android.nfc.action.TECH_DISCOVERED" />
    </intent-filter>
    <meta-data
        android:name="android.nfc.action.TECH_DISCOVERED"
        android:resource="@xml/nfc_tech_filter" />
</activity>
```

22. **Открытие геопозиции на внешней карте:** Сконфигурируйте Activity, перехватывающую запросы по схеме `geo:0,0?q=`.

```xml
<activity
    android:name=".maps.GeoActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <data android:scheme="geo" />
    </intent-filter>
</activity>
```

23. **Обработка вложений электронной почты:** Настройте фильтр для открытия файлов расширения `.docx` и `.xlsx` с MIME-типом `application/*`.

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:mimeType="application/vnd.openxmlformats-officedocument.wordprocessingml.document" />
    <data android:mimeType="application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" />
</intent-filter>
```

24. **Интеграция с Google Assistant / App Actions:** Задекларируйте файл шорткатов `<meta-data android:name="android.app.shortcuts" android:resource="@xml/shortcuts" />`.

```xml
<meta-data
    android:name="android.app.shortcuts"
    android:resource="@xml/shortcuts" />
```

25. **Глубокая ссылка интернет-магазина (Deep Link):** Задекларируйте фильтр, принимающий ссылку вида `https://marketplace.com/catalog/shoes?brand=nike` с верификацией домена.

```xml
<intent-filter android:autoVerify="true">
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
    <data
        android:scheme="https"
        android:host="marketplace.com"
        android:pathPrefix="/catalog/shoes" />
</intent-filter>
```

26. **Перехват нажатия аппаратной кнопки гарнитуры:** Настройте BroadcastReceiver для приема события `android.intent.action.MEDIA_BUTTON`.

```xml
<receiver
    android:name=".media.MediaButtonReceiver"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MEDIA_BUTTON" />
    </intent-filter>
</receiver>
```

27. **Обработка системного поиска:** Зарегистрируйте компонент со специальным действием `android.intent.action.SEARCH`.

```xml
<activity
    android:name=".search.SearchResultsActivity"
    android:exported="true"
    android:launchMode="singleTop">
    <intent-filter>
        <action android:name="android.intent.action.SEARCH" />
    </intent-filter>
    <meta-data
        android:name="android.app.searchable"
        android:resource="@xml/searchable" />
</activity>
```

28. **Открытие экрана создания SMS:** Напишите интент-фильтр со схемой `smsto:` для отправки сообщений.

```xml
<intent-filter>
    <action android:name="android.intent.action.SENDTO" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="smsto" />
</intent-filter>
```

29. **Авторизация через OAuth (Custom Tab Callback):** Сконфигурируйте Activity для перехвата редиректа вида `org.example.app://oauth-callback`.

```xml
<activity
    android:name=".auth.OAuthCallbackActivity"
    android:exported="true"
    android:launchMode="singleTask">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data
            android:scheme="org.example.app"
            android:host="oauth-callback" />
    </intent-filter>
</activity>
```

30. **Открытие файла конфигурации по кастомному расширению:** Сделайте так, чтобы файлы `.mycfg` открывались в вашей программе по умолчанию.

```xml
<intent-filter>
    <action android:name="android.intent.action.VIEW" />
    <category android:name="android.intent.category.DEFAULT" />
    <data android:scheme="file" android:pathPattern=".*\\.mycfg" />
    <data android:mimeType="application/x-mycfg" />
</intent-filter>
```


### Блок 4: Сервисы, фоновые задачи и Android 14+ требования 

31. **Foreground Service типа "Микрофон":** Объявите сервис записи голосовых заметок с обязательным указанием `android:foregroundServiceType="microphone"` и соответствующим разрешением.

```xml
<uses-permission android:name="android.permission.RECORD_AUDIO" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MICROPHONE" />

<service
    android:name=".voice.VoiceNoteService"
    android:exported="false"
    android:foregroundServiceType="microphone" />
```

32. **Foreground Service типа "Геолокация":** Настройте сервис пешего курьера с типом `location` и правами фоновой геолокации.

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />

<service
    android:name=".courier.CourierTrackingService"
    android:exported="false"
    android:foregroundServiceType="location" />
```

33. **Сервис загрузки тяжелых файлов:** Задекларируйте Foreground Service типа `dataSync` с указанием таймаута выполнения.

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_DATA_SYNC" />

<service
    android:name=".download.HeavyDownloadService"
    android:exported="false"
    android:foregroundServiceType="dataSync" />
```

34. **Сервис стриминга видео на ТВ:** Сконфигурируйте сервис с типом `mediaProjection`.

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PROJECTION" />

<service
    android:name=".tv.VideoStreamService"
    android:exported="false"
    android:foregroundServiceType="mediaProjection" />
```

35. **Планировщик WorkManager:** Настройте системные компоненты библиотеки WorkManager через метаданные для отключения дефолтной авто-инициализации.

```xml
<provider
    android:name="androidx.startup.InitializationProvider"
    android:authorities="${applicationId}.androidx-startup"
    android:exported="false"
    tools:node="merge">
    <meta-data
        android:name="androidx.work.WorkManagerInitializer"
        android:value="androidx.startup"
        tools:node="remove" />
</provider>
```

36. **Автозапуск сервиса после перезагрузки:** Соберите правильную комбинацию: разрешение `RECEIVE_BOOT_COMPLETED`, регистрация ресивера и вызов фоновой службы.

```xml
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

<receiver
    android:name=".receivers.BootReceiver"
    android:exported="true"
    android:enabled="true">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED" />
        <action android:name="android.intent.action.LOCKED_BOOT_COMPLETED" />
    </intent-filter>
</receiver>
```

37. **Контроль энергосбережения (Doze Mode):** Задекларируйте разрешение `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` и укажите в комментариях риски блокировки в Google Play.

```xml
<uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />
```

38. **Служба специальных возможностей (Accessibility Service):** Напишите блок объявления службы спец. возможностей с файлом метаданных `accessibility_service_config`.

```xml
<service
    android:name=".a11y.CustomAccessibilityService"
    android:exported="true"
    android:permission="android.permission.BIND_ACCESSIBILITY_SERVICE">
    <intent-filter>
        <action android:name="android.accessibilityservice.AccessibilityService" />
    </intent-filter>
    <meta-data
        android:name="android.accessibilityservice"
        android:resource="@xml/accessibility_service_config" />
</service>
```

39. **Служба синхронизации аккаунтов (SyncAdapter):** Опишите компонент службы синхронизации с метаданными `@xml/syncadapter`.

```xml
<service
    android:name=".sync.SyncService"
    android:exported="true"
    android:process=":sync">
    <intent-filter>
        <action android:name="android.content.SyncAdapter" />
    </intent-filter>
    <meta-data
        android:name="android.content.SyncAdapter"
        android:resource="@xml/syncadapter" />
</service>
```

40. **Служба обоев (Live Wallpaper):** Зарегистрируйте `WallpaperService` с необходимым системным разрешением `android.permission.BIND_WALLPAPER`.

```xml
<uses-permission android:name="android.permission.BIND_WALLPAPER" />

<service
    android:name=".wallpaper.LiveWallpaperService"
    android:exported="true"
    android:permission="android.permission.BIND_WALLPAPER">
    <intent-filter>
        <action android:name="android.service.wallpaper.WallpaperService" />
    </intent-filter>
    <meta-data
        android:name="android.service.wallpaper"
        android:resource="@xml/wallpaper" />
</service>
```


### Блок 5: Аппаратные модули, экраны и адаптивность

41. **Поддержка складных смартфонов (Foldables):** Объявите поддержку динамического изменения геометрии экрана и соотношений сторон через `android:maxAspectRatio` и `android:minAspectRatio`.

```xml
<application
    android:resizeableActivity="true"
    android:maxAspectRatio="2.4"
    android:minAspectRatio="1.0">
</application>
```

42. **Приложение для умных часов (Wear OS):** Настройте манифест с тегом `<uses-feature android:name="android.hardware.type.watch" />`.

```xml
<uses-feature
    android:name="android.hardware.type.watch"
    android:required="true" />
```

43. **Приложение для автомобильных медиасистем (Android Auto):** Добавьте дескриптор метаданных `com.google.android.gms.car.application` с конфигурацией автомобильного интерфейса.

```xml
<meta-data
    android:name="com.google.android.gms.car.application"
    android:resource="@xml/automotive_app_desc" />
```

44. **Приложение для умного ТВ (Android TV):** Настройте категорию `LEANBACK_LAUNCHER` и флаг отсутствия обязательного тачскрина (`android.hardware.touchscreen = false`).

```xml
<uses-feature
    android:name="android.hardware.touchscreen"
    android:required="false" />
<uses-feature
    android:name="android.software.leanback"
    android:required="true" />

<activity android:name=".tv.MainTvActivity" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LEANBACK_LAUNCHER" />
    </intent-filter>
</activity>
```

45. **Поддержка стилуса и планшетного ввода:** Опишите требования к расширенному сенсорному экрану.

```xml
<uses-feature
    android:name="android.hardware.touchscreen"
    android:required="true" />
<uses-feature
    android:name="android.hardware.touchscreen.multitouch.jazzhand"
    android:required="false" />
<uses-feature
    android:name="android.hardware.stylus"
    android:required="false" />
```

46. **Режим высокой плотности пикселей (Density Independence):** Сконфигурируйте узел `<supports-screens>` для запрета работы приложения на устаревших экранах низкой плотности.

```xml
<supports-screens
    android:smallScreens="false"
    android:normalScreens="true"
    android:largeScreens="true"
    android:xlargeScreens="true"
    android:anyDensity="true" />
```

47. **Высокая частота опроса сенсоров:** Задекларируйте разрешение `HIGH_SAMPLING_RATE_SENSORS` для точного гироскопа в гоночной игре.

```xml
<uses-permission android:name="android.permission.HIGH_SAMPLING_RATE_SENSORS" />
```

48. **Интеграция с фонариком устройства:** Опишите признак вспышки камеры `android.hardware.camera.flash` как опциональный.

```xml
<uses-feature
    android:name="android.hardware.camera.flash"
    android:required="false" />
<uses-permission android:name="android.permission.FLASHLIGHT" />
```

49. **Доступ к USB-аксессуарам (OTG):** Настройте интент-фильтр и метаданные для автоматического запуска приложения при подключении внешнего USB-устройства (`android.hardware.usb.action.USB_DEVICE_ATTACHED`).

```xml
<uses-feature
    android:name="android.hardware.usb.host"
    android:required="true" />

<activity
    android:name=".usb.UsbHostActivity"
    android:exported="true">
    <intent-filter>
        <action android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED" />
    </intent-filter>
    <meta-data
        android:name="android.hardware.usb.action.USB_DEVICE_ATTACHED"
        android:resource="@xml/usb_device_filter" />
</activity>
```

50. **Комплексный production-манифест:** Соберите итоговый манифест защищенного финтех-приложения со стартовым экраном, FileProvider, тремя опасными разрешениями, App Link и строгой политикой сетевой безопасности.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    package="com.university.fintech">

    <!-- ============ РАЗРЕШЕНИЯ ============ -->
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
    <uses-permission android:name="android.permission.USE_BIOMETRIC" />
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission
        android:name="android.permission.READ_EXTERNAL_STORAGE"
        android:maxSdkVersion="32" />
    <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />

    <!-- ============ АППАРАТНЫЕ ФИЧИ ============ -->
    <uses-feature android:name="android.hardware.camera" android:required="false" />
    <uses-feature android:name="android.hardware.fingerprint" android:required="false" />
    <uses-feature android:name="android.hardware.location.gps" android:required="false" />

    <!-- ============ ВИДИМОСТЬ ПАКЕТОВ ============ -->
    <queries>
        <intent>
            <action android:name="android.intent.action.VIEW" />
            <data android:scheme="https" />
        </intent>
        <package android:name="ru.sberbankmobile" />
        <package android:name="com.google.android.apps.nbu.paisa.user" />
    </queries>

    <!-- ============ КАСТОМНОЕ РАЗРЕШЕНИЕ ============ -->
    <permission
        android:name="com.university.fintech.permission.ACCESS_SECURE_API"
        android:protectionLevel="signature" />

    <!-- ============ ПРИЛОЖЕНИЕ ============ -->
    <application
        android:name=".FintechApplication"
        android:allowBackup="false"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="false"
        android:icon="@mipmap/ic_launcher"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/Theme.Fintech"
        android:usesCleartextTraffic="false"
        android:networkSecurityConfig="@xml/network_security_config"
        android:enableOnBackInvokedCallback="true">

        <!-- Splash + Main -->
        <activity
            android:name=".ui.SplashActivity"
            android:exported="true"
            android:theme="@style/Theme.Fintech.Starting">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>

        <!-- Экран транзакции с защитой от tapjacking -->
        <activity
            android:name=".payment.ConfirmTransactionActivity"
            android:exported="false"
            android:filterTouchesWhenObscured="true"
            android:excludeFromRecents="true" />

        <!-- App Link для платежей -->
        <activity
            android:name=".payment.PaymentDeepLinkActivity"
            android:exported="true"
            android:launchMode="singleTask">
            <intent-filter android:autoVerify="true">
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data
                    android:scheme="https"
                    android:host="pay.university.ru"
                    android:pathPrefix="/transfer/" />
            </intent-filter>
        </activity>

        <!-- OAuth callback -->
        <activity
            android:name=".auth.OAuthCallbackActivity"
            android:exported="true"
            android:launchMode="singleTask">
            <intent-filter>
                <action android:name="android.intent.action.VIEW" />
                <category android:name="android.intent.category.DEFAULT" />
                <category android:name="android.intent.category.BROWSABLE" />
                <data android:scheme="fintech" android:host="oauth" />
            </intent-filter>
        </activity>

        <!-- FileProvider для камеры (KYC) -->
        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="${applicationId}.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths" />
        </provider>

        <!-- Secure ContentProvider -->
        <provider
            android:name=".data.SecureDataProvider"
            android:authorities="${applicationId}.secureprovider"
            android:exported="false"
            android:readPermission="com.university.fintech.permission.ACCESS_SECURE_API"
            android:writePermission="com.university.fintech.permission.ACCESS_SECURE_API" />

        <!-- Firebase / Crashlytics -->
        <meta-data
            android:name="firebase_analytics_collection_deactivated"
            android:value="false" />
        <meta-data
            android:name="com.google.android.geo.API_KEY"
            android:value="@string/google_maps_key" />

        <!-- LocaleConfig -->
        <meta-data
            android:name="android.content.res.LocaleConfig"
            android:resource="@xml/locales_config" />

    </application>
</manifest>
```


---
