---
title: "Практическая работа №4: Создание первых Android-приложений на Java"
discipline: "Разработка мобильных приложений"
status: "Active"
author: "УПМ 2"
tags: [android, beginner, java, layout, intent, view-binding, tutorial]
---
## 📋 Содержание

1. [Анатомия Android Studio и первый запуск](#1-анатомия-android-studio-и-первый-запуск)
2. [Структура проекта: куда смотреть и что где лежит](#2-структура-проекта-куда-смотреть-и-что-где-лежит)
3. [Разметка экрана (XML): создаем визуальный интерфейс](#3-разметка-экрана-xml-создаем-визуальный-интерфейс)
4. [Оживление экрана: связываем XML с кодом на Java](#4-оживление-экрана-связываем-xml-с-кодом-на-java)
5. [Ресурсы: почему нельзя писать текст прямо в коде](#5-ресурсы-почему-нельзя-писать-текст-прямо-в-коде)
6. [Второй экран и переходы между ними (Intent)](#6-второй-экран-и-переходы-между-ними-intent)
7. [3 Готовых учебных проекта с построчным разбором](#7-3-готовых-учебных-проекта-с-построчным-разбором)
8. [Топ-12 критических ошибок новичков и их решение](#8-топ-12-критических-ошибок-новичков-и-их-решение)
9. [30 Практических заданий на самостоятельную разработку](#9-30-практических-заданий-на-самостоятельную-разработку)
10. [Чек-лист студента перед показом работы](#10-чек-лист-студента-перед-показом-работы)

---

## 1. Анатомия Android Studio и первый запуск

### Шаг 1. Создание нового проекта

1. Запустите **Android Studio**.
2. В стартовом окне нажмите **New Project** (или выберите в меню: `File -> New -> New Project...`).
3. В списке шаблонов выберите категорию **Phone and Tablet**, а затем шаблон **Empty Views Activity**.

> [!IMPORTANT]
> Обратите внимание: выбирайте именно **Empty Views Activity** (с синей иконкой классической разметки), а **НЕ** _Empty Activity_. Шаблон _Empty Activity_ в современных версиях создает проект на базе декларативного фреймворка Jetpack Compose без использования XML и Java.

4. Нажмите **Next** и заполните параметры проекта:
   - **Name:** Имя приложения (например, `MyFirstApp`).
   - **Package name:** Идентификатор пакета строчными буквами (например, `com.example.myfirstapp`).
   - **Save location:** Путь к папке на диске (крайне желательно без пробелов и русских букв в пути).
   - **Language:** Обязательно переключите на **Java** (по умолчанию часто стоит Kotlin).
   - **Minimum SDK:** Выберите `API 26: Android 8.0 (Oreo)` или `API 28`. Это обеспечит работу на 95%+ реальных устройств.
   - **Build configuration language:** Рекомендуется оставить дефолтный `Kotlin DSL (build.gradle.kts)` или `Groovy DSL`.
5. Нажмите **Finish**. Дождитесь, пока внизу в строке состояния завершится процесс **Gradle Sync** (при первом запуске это может занять 2–5 минут, среда скачивает необходимые компоненты).

### Шаг 2. Подготовка устройства для запуска

#### Вариант А: Запуск на реальном смартфоне (Рекомендуется для слабых ПК)

1. Откройте на смартфоне **Настройки -> О телефоне**.
2. Быстро нажмите 7 раз подряд на пункт **Номер сборки** (Build number), пока не появится надпись _"Вы стали разработчиком!"_.
3. Вернитесь в общее меню настроек -> **Система (или Для разработчиков)** -> включите тумблер **Отладка по USB (USB Debugging)**.
4. Подключите смартфон к компьютеру через USB-кабель. На экране смартфона появится всплывающий запрос _"Разрешить отладку по USB?"_ — поставьте галочку _"Всегда разрешать с этого компьютера"_ и нажмите **ОК**.
5. В верхней панели Android Studio в выпадающем списке устройств появится модель вашего смартфона.


#### Вариант Б: Виртуальный эмулятор (AVD)

1. На верхней панели справа нажмите на значок **Device Manager** (иконка смартфона с роботом).
2. Нажмите кнопку **Create Device** (или знак `+`).
3. Выберите модель (например, `Pixel 8` или `Pixel 7`).
4. Нажмите **Next**, выберите образ системы (например, `VanillaIceCream` или `UpsideDownCake`) и скачайте его по ссылке **Download**.
5. Нажмите **Finish**. Теперь устройство можно запустить кнопкой Play.


---

## 2. Структура проекта: куда смотреть и что где лежит

В левой панели переключите режим отображения проекта в положение **Android** (в верхнем выпадающем меню проводника). Новичку нужны только три папки:

```
app/
├── manifests/
│   └── AndroidManifest.xml       <-- Паспорт приложения (разрешения, список экранов)
├── java/
│   └── com.example.myfirstapp/
│       └── MainActivity.java     <-- Логика экрана: кнопки, расчеты, действия
└── res/
    ├── layout/
    │   └── activity_main.xml     <-- Внешний вид экрана: кнопки, поля ввода, текст
    ├── values/
    │   ├── strings.xml           <-- Все текстовые надписи приложения
    │   ├── colors.xml            <-- Палитра цветов
    │   └── themes.xml            <-- Шрифты, стили кнопок и тема (темная/светлая)
    └── mipmap/ (или drawable/)    <-- Иконки и картинки
```

---

## 3. Разметка экрана (XML): создаем визуальный интерфейс

Откройте файл `res/layout/activity_main.xml`. В правом верхнем углу редактора есть три режима:

- **Code:** Только XML-текст.
- **Design:** Только визуальный редактор с перетаскиванием элементов.
- **Split:** Разделенный экран (слева код, справа живой предпросмотр — **самый удобный режим!**).

### Базовые элементы интерфейса (Виджеты)

1. `TextView` — текстовая метка для отображения надписей, заголовков и результатов.
2. `EditText` — поле ввода, куда пользователь вводит текст, пароль или числа.
3. `Button` — кнопка, на которую можно нажимать.
4. `ImageView` — контейнер для показа картинок.

### Размеры: wrap_content, match_parent и dp

- `wrap_content` — элемент занимает ровно столько места, сколько нужно его содержимому (тексту внутри).
- `match_parent` — элемент растягивается на всю доступную ширину или высоту родительского контейнера.
- `dp` (density-independent pixels) — независимые от плотности пиксели для задания отступов и размеров кнопок (например, `16dp`).
- `sp` (scale-independent pixels) — специальные единицы **только для размера шрифтов** (например, `18sp`), учитывающие системные настройки слабовидящих.

### Контейнер LinearLayout: простой и предсказуемый

Для новичков самым простым является `LinearLayout`. Он выстраивает все элементы строго друг за другом: вертикально (сверху вниз) или горизонтально (слева направо).

### Пример понятной разметки (activity_main.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center_horizontal">

    <!-- Заголовок -->
    <TextView
        android:id="@+id/textViewTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Добро пожаловать!"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="16dp" />

    <!-- Поле для ввода имени -->
    <EditText
        android:id="@+id/editTextName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Введите ваше имя"
        android:inputType="textPersonName"
        android:layout_marginBottom="16dp" />

    <!-- Кнопка действия -->
    <Button
        android:id="@+id/buttonGreet"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Поздороваться"
        android:layout_marginBottom="24dp" />

    <!-- Поле для вывода результата -->
    <TextView
        android:id="@+id/textViewResult"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Здесь появится ответ"
        android:textSize="18sp"
        android:textColor="#333333" />

</LinearLayout>
```

### Задания для закрепления темы

1. Измените атрибут `android:orientation` с `vertical` на `horizontal` и посмотрите в режиме Split, что произошло с кнопками.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal"
    android:padding="24dp"
    android:gravity="center_vertical">
```

2. Добавьте второе поле ввода `EditText` для ввода возраста с атрибутом `android:inputType="number"`.

```xml
<EditText
    android:id="@+id/editTextAge"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Введите ваш возраст"
    android:inputType="number"
    android:layout_marginBottom="16dp" />
```

3. Установите для текста заголовка синий цвет с помощью атрибута `android:textColor="#1976D2"`.

```xml
<TextView
    android:id="@+id/textViewTitle"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Добро пожаловать!"
    android:textSize="24sp"
    android:textStyle="bold"
    android:textColor="#1976D2"
    android:layout_marginBottom="16dp" />
```

4. Сделайте отступ между элементами с помощью `android:layout_marginTop="20dp"`.

```xml
<EditText
    android:id="@+id/editTextName"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:hint="Введите ваше имя"
    android:inputType="textPersonName"
    android:layout_marginTop="20dp"
    android:layout_marginBottom="16dp" />
```

5. Добавьте кнопку «Очистить всё» с текстом красного цвета.

```xml
<Button
    android:id="@+id/buttonClear"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="Очистить всё"
    android:textColor="#D32F2F"
    android:layout_marginTop="12dp" />
```


---

## 4. Оживление экрана: связываем XML с кодом на Java

Теперь откроем файл `MainActivity.java`. Чтобы программа могла реагировать на нажатия и менять текст, нужно выполнить 3 шага:

1. **Объявить переменные** нужных типов (`TextView`, `EditText`, `Button`).
2. **Связать переменные с ID** из разметки через команду `findViewById()`.
3. **Назначить слушатель нажатия** (`OnClickListener`).

### Полный и подробный код MainActivity.java

```java
package com.example.myfirstapp;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    // Шаг 1: Объявляем ссылки на наши элементы экрана
    private TextView textViewTitle;
    private EditText editTextName;
    private Button buttonGreet;
    private TextView textViewResult;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // Эта строка загружает XML-разметку на экран устройства.
        // До нее вызывать findViewById НЕЛЬЗЯ!
        setContentView(R.layout.activity_main);

        // Шаг 2: Находим каждый элемент по его ID из activity_main.xml
        textViewTitle = findViewById(R.id.textViewTitle);
        editTextName = findViewById(R.id.editTextName);
        buttonGreet = findViewById(R.id.buttonGreet);
        textViewResult = findViewById(R.id.textViewResult);

        // Шаг 3: Вешаем слушатель клика на кнопку
        buttonGreet.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Считываем текст, который пользователь напечатал в поле ввода
                String enteredName = editTextName.getText().toString().trim();

                // Проверяем: не пустое ли поле?
                if (enteredName.isEmpty()) {
                    // Показываем всплывающее уведомление (Toast)
                    Toast.makeText(MainActivity.this, "Пожалуйста, введите имя!", Toast.LENGTH_SHORT).show();
                } else {
                    // Формируем приветствие и выводим его в TextView
                    String greeting = "Привет, " + enteredName + "! Рады тебя видеть.";
                    textViewResult.setText(greeting);

                    // Очищаем поле ввода для следующего раза
                    editTextName.setText("");
                }
            }
        });
    }
}
```

> [!TIP]
> **Лямбда-выражение (современный стиль Java):**
> Вместо длинной конструкции `new View.OnClickListener() { ... }` в современной Java можно писать компактно:
>
> ```java
> buttonGreet.setOnClickListener(v -> {
>     // действия при клике
> });
> ```

### Задания для закрепления темы

1. Добавьте всплывающее сообщение `Toast` при успешном выводе приветствия.

```java
if (enteredName.isEmpty()) {
    Toast.makeText(MainActivity.this, "Пожалуйста, введите имя!", Toast.LENGTH_SHORT).show();
} else {
    String greeting = "Привет, " + enteredName + "! Рады тебя видеть.";
    textViewResult.setText(greeting);
    Toast.makeText(MainActivity.this, "Приветствие выведено!", Toast.LENGTH_SHORT).show();
    editTextName.setText("");
}
```

2. Добавьте проверку: если пользователь ввел имя длиной менее 2 символов, выводите ошибку: _"Слишком короткое имя"_.

```java
if (enteredName.isEmpty()) {
    Toast.makeText(this, "Пожалуйста, введите имя!", Toast.LENGTH_SHORT).show();
} else if (enteredName.length() < 2) {
    Toast.makeText(this, "Слишком короткое имя", Toast.LENGTH_SHORT).show();
} else {
    String greeting = "Привет, " + enteredName + "! Рады тебя видеть.";
    textViewResult.setText(greeting);
    editTextName.setText("");
}
```

3. Реализуйте кнопку, которая при клике меняет текст заголовка на _"Текст успешно изменен!"_.

```java
Button buttonChangeTitle = findViewById(R.id.buttonChangeTitle);
buttonChangeTitle.setOnClickListener(v ->
    textViewTitle.setText("Текст успешно изменен!")
);
```

4. Реализуйте кнопку, которая скрывает надпись с результатом с помощью команды `textViewResult.setVisibility(View.GONE);`.

```java
Button buttonHide = findViewById(R.id.buttonHide);
buttonHide.setOnClickListener(v ->
    textViewResult.setVisibility(View.GONE)
);
```

5. Сделайте счетчик: при каждом нажатии на кнопку увеличивайте число в `TextView` на единицу.

```java
private int counter = 0;
private TextView textViewCounter;

// в onCreate:
textViewCounter = findViewById(R.id.textViewCounter);
Button buttonCount = findViewById(R.id.buttonCount);
buttonCount.setOnClickListener(v -> {
    counter++;
    textViewCounter.setText(String.valueOf(counter));
});
```


---

## 5. Ресурсы: почему нельзя писать текст прямо в коде

Если вы напишете в XML `android:text="Привет"`, среда разработки подчеркнет строку желтым цветом и выдаст предупреждение: _Hardcoded string "Привет", should use `@string` resource_.

### Почему это важно:

1. **Мультиязычность:** Вы можете создать перевод на английский, немецкий или китайский язык без изменения единой строчки Java-кода.
2. **Единый центр правок:** Если название компании или слоган встречается на 10 экранах, вы меняете его в одном месте в файле `strings.xml`.

### Как правильно работать со строками:

Откройте `res/values/strings.xml`:

```xml
<resources>
    <string name="app_name">Мое Первое Приложение</string>
    <string name="title_welcome">Добро пожаловать в сервис!</string>
    <string name="hint_enter_name">Введите ваше имя</string>
    <string name="btn_submit">Отправить данные</string>
    <string name="error_empty_field">Это поле не может быть пустым</string>
</resources>
```

В файле разметки XML используйте ссылку через собачку:

```xml
<Button
    android:id="@+id/btnSubmit"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:text="@string/btn_submit" />
```

В коде Java получайте строку так:

```java
String errorMsg = getString(R.string.error_empty_field);
```

### Задания для закрепления темы

1. Вынесите все хардкод-строки из первого проекта в файл `res/values/strings.xml`.

```xml
<resources>
    <string name="app_name">Мое Первое Приложение</string>
    <string name="title_welcome">Добро пожаловать!</string>
    <string name="hint_enter_name">Введите ваше имя</string>
    <string name="btn_greet">Поздороваться</string>
    <string name="result_placeholder">Здесь появится ответ</string>
    <string name="error_empty_name">Пожалуйста, введите имя!</string>
</resources>
```

2. Создайте в `res/values/colors.xml` три кастомных цвета: `brand_blue`, `brand_green`, `brand_gray` и примените их к фону и кнопкам.

```xml
<resources>
    <color name="brand_blue">#1976D2</color>
    <color name="brand_green">#388E3C</color>
    <color name="brand_gray">#607D8B</color>
</resources>
```

3. Добавьте векторную иконку через меню `res -> New -> Vector Asset`, выбрав иконку из стандартной базы Material Design, и выведите ее в `ImageView`.

```xml
<ImageView
    android:id="@+id/imageViewIcon"
    android:layout_width="64dp"
    android:layout_height="64dp"
    android:src="@drawable/ic_launcher_foreground"
    android:layout_marginBottom="16dp" />
```

4. Создайте строковый ресурс с параметром подстановки: `<string name="welcome_user">Привет, %1$s!</string>` и выведите его через `getString(R.string.welcome_user, name)`.

```xml
<string name="welcome_user">Привет, %1$s!</string>
```

5. Создайте альтернативный файл `strings.xml (en)` для английской локализации и проверьте автоматический перевод при смене языка в эмуляторе.

```xml
<resources>
    <string name="app_name">My First App</string>
    <string name="title_welcome">Welcome!</string>
    <string name="hint_enter_name">Enter your name</string>
    <string name="btn_greet">Say hello</string>
    <string name="result_placeholder">Result will appear here</string>
    <string name="error_empty_name">Please enter a name!</string>
</resources>
```


---

## 6. Второй экран и переходы между ними (Intent)

Любое реальное приложение состоит из нескольких экранов. В Android экран называется **Activity**.

### Шаг 1. Создание второй Activity

1. В левой панели нажмите правой кнопкой мыши по папке с пакетом (где лежит `MainActivity`).
2. Выберите: **New -> Activity -> Empty Views Activity**.
3. Назовите экран `SecondActivity` и нажмите **Finish**.
4. Студия автоматически создаст два файла: `SecondActivity.java` и `activity_second.xml`, а также зарегистрирует экран в `AndroidManifest.xml`!

### Шаг 2. Разметка второго экрана (activity_second.xml)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp"
    android:gravity="center">

    <TextView
        android:id="@+id/textViewReceivedData"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="20sp"
        android:text="Ожидание данных..."
        android:layout_marginBottom="20dp" />

    <Button
        android:id="@+id/buttonClose"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Вернуться назад" />

</LinearLayout>
```

### Шаг 3. Переход с передачей данных из MainActivity.java

```java
// Создаем намерение перейти на SecondActivity
Intent intent = new Intent(MainActivity.this, SecondActivity.class);

// Кладем данные по ключу (ключ-значение)
intent.putExtra("EXTRA_USERNAME", enteredName);
intent.putExtra("EXTRA_AGE", 20);

// Запускаем переход!
startActivity(intent);
```

### Шаг 4. Прием данных в SecondActivity.java

```java
package com.example.myfirstapp;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class SecondActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        TextView textViewReceived = findViewById(R.id.textViewReceivedData);
        Button buttonClose = findViewById(R.id.buttonClose);

        // Получаем объект Intent, который открыл этот экран
        Intent incomingIntent = getIntent();

        // Извлекаем данные по тому же ключу, что отправляли
        String userName = incomingIntent.getStringExtra("EXTRA_USERNAME");
        int userAge = incomingIntent.getIntExtra("EXTRA_AGE", 0);

        if (userName != null) {
            textViewReceived.setText("Пользователь: " + userName + "\nВозраст: " + userAge);
        }

        // Закрываем текущий экран и возвращаемся назад
        buttonClose.setOnClickListener(v -> finish());
    }
}
```

### Задания для закрепления темы

1. Создайте третий экран `AboutActivity` с информацией об авторе приложения и кнопкой выхода.

```java
package com.example.myfirstapp;

import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class AboutActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_about);

        Button buttonExit = findViewById(R.id.buttonExit);
        buttonExit.setOnClickListener(v -> finishAffinity()); // закрыть все Activity
    }
}
```

2. Передайте со второго экрана на третий логическое значение `boolean` (например, флаг согласия с правилами).

```java
Intent intent = new Intent(SecondActivity.this, AboutActivity.class);
intent.putExtra("EXTRA_AGREED", true);
startActivity(intent);
```

3. Добавьте проверку: если переданная строка `null`, отображайте надпись: _"Гость"_.

```java
String userName = incomingIntent.getStringExtra("EXTRA_USERNAME");
if (userName == null || userName.isEmpty()) {
    userName = "Гость";
}
textViewReceived.setText("Пользователь: " + userName + "\nВозраст: " + userAge);
```

4. Реализуйте передачу дробного числа `double` (например, баланс счета или температура).

```java
intent.putExtra("EXTRA_BALANCE", 1234.56);
```

5. Создайте кнопку, открывающую системный веб-браузер по ссылке с помощью неявного интента: `new Intent(Intent.ACTION_VIEW, Uri.parse("https://google.com"))`.

```java
Button buttonOpenBrowser = findViewById(R.id.buttonOpenBrowser);
buttonOpenBrowser.setOnClickListener(v -> {
    Intent browserIntent = new Intent(Intent.ACTION_VIEW, Uri.parse("https://google.com"));
    startActivity(browserIntent);
});
```


---

## 7. 3 Готовых учебных проекта с построчным разбором

---

### Проект 1: «Тап-Кликер» (Счетчик кликов со сменой цвета)

**Идея:** Простая игра, где пользователь нажимает кнопку и зарабатывает очки. При достижении круглых чисел меняется цвет фона, а кнопка сброса возвращает счетчик в ноль.

#### Разметка (`activity_main.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/rootLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="32dp"
    android:backgroundColor="#F5F5F5">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Счетчик кликов"
        android:textSize="22sp"
        android:layout_marginBottom="16dp" />

    <TextView
        android:id="@+id/tvCounter"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="0"
        android:textSize="64sp"
        android:textStyle="bold"
        android:textColor="#212121"
        android:layout_marginBottom="32dp" />

    <Button
        android:id="@+id/btnClickMe"
        android:layout_width="200dp"
        android:layout_height="60dp"
        android:text="Кликни меня!"
        android:textSize="18sp"
        android:layout_marginBottom="16dp" />

    <Button
        android:id="@+id/btnReset"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Сбросить"
        style="@style/Widget.Material3.Button.OutlinedButton" />

</LinearLayout>
```

#### Исходный код (`MainActivity.java`)

```java
package com.example.clicker;

import android.graphics.Color;
import android.os.Bundle;
import android.widget.Button;
import android.widget.LinearLayout;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private int score = 0;
    private TextView tvCounter;
    private LinearLayout rootLayout;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        tvCounter = findViewById(R.id.tvCounter);
        Button btnClickMe = findViewById(R.id.btnClickMe);
        Button btnReset = findViewById(R.id.btnReset);
        rootLayout = findViewById(R.id.rootLayout);

        btnClickMe.setOnClickListener(v -> {
            score++;
            tvCounter.setText(String.valueOf(score));

            // Логика поощрения игрока
            if (score == 10) {
                rootLayout.setBackgroundColor(Color.parseColor("#E8F5E9")); // Светло-зеленый
                Toast.makeText(this, "Отличный старт! 10 очков!", Toast.LENGTH_SHORT).show();
            } else if (score == 50) {
                rootLayout.setBackgroundColor(Color.parseColor("#FFF9C4")); // Золотистый
                Toast.makeText(this, "Половина сотни! Ты мастер клика!", Toast.LENGTH_SHORT).show();
            }
        });

        btnReset.setOnClickListener(v -> {
            score = 0;
            tvCounter.setText("0");
            rootLayout.setBackgroundColor(Color.parseColor("#F5F5F5"));
            Toast.makeText(this, "Счет сброшен", Toast.LENGTH_SHORT).show();
        });
    }
}
```

---

### Проект 2: «Калькулятор чаевых и счета»

**Идея:** Пользователь вводит сумму чека в ресторане и количество гостей. Приложение рассчитывает 10% чаевых и итоговую сумму с человека с защитой от вылета при пустых полях.

#### Разметка (`activity_main.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Калькулятор счета"
        android:textSize="26sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp" />

    <EditText
        android:id="@+id/etTotalBill"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Сумма чека (руб.)"
        android:inputType="numberDecimal"
        android:layout_marginBottom="12dp" />

    <EditText
        android:id="@+id/etPersonsCount"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Количество человек"
        android:inputType="number"
        android:layout_marginBottom="24dp" />

    <Button
        android:id="@+id/btnCalculate"
        android:layout_width="match_parent"
        android:layout_height="56dp"
        android:text="Рассчитать итог"
        android:layout_marginBottom="24dp" />

    <TextView
        android:id="@+id/tvTipAmount"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Чаевые (10%): 0.00 руб."
        android:textSize="18sp"
        android:layout_marginBottom="8dp" />

    <TextView
        android:id="@+id/tvPerPerson"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="С каждого гостя: 0.00 руб."
        android:textSize="20sp"
        android:textStyle="bold"
        android:textColor="#00796B" />

</LinearLayout>
```

#### Исходный код (`MainActivity.java`)

```java
package com.example.tipcalculator;

import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;
import java.util.Locale;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText etTotalBill = findViewById(R.id.etTotalBill);
        EditText etPersonsCount = findViewById(R.id.etPersonsCount);
        Button btnCalculate = findViewById(R.id.btnCalculate);
        TextView tvTipAmount = findViewById(R.id.tvTipAmount);
        TextView tvPerPerson = findViewById(R.id.tvPerPerson);

        btnCalculate.setOnClickListener(v -> {
            String billStr = etTotalBill.getText().toString().trim();
            String personsStr = etPersonsCount.getText().toString().trim();

            // Валидация: защита от пустых строк
            if (billStr.isEmpty() || personsStr.isEmpty()) {
                Toast.makeText(this, "Заполните оба поля!", Toast.LENGTH_SHORT).show();
                return;
            }

            try {
                double bill = Double.parseDouble(billStr);
                int persons = Integer.parseInt(personsStr);

                if (bill <= 0) {
                    Toast.makeText(this, "Сумма чека должна быть больше нуля", Toast.LENGTH_SHORT).show();
                    return;
                }

                if (persons <= 0) {
                    Toast.makeText(this, "Количество гостей должно быть не менее 1", Toast.LENGTH_SHORT).show();
                    return;
                }

                // Расчет 10% чаевых
                double tip = bill * 0.10;
                double total = bill + tip;
                double perPerson = total / persons;

                // Вывод результатов с форматированием до двух знаков после запятой
                tvTipAmount.setText(String.format(Locale.getDefault(), "Чаевые (10%%): %.2f руб.", tip));
                tvPerPerson.setText(String.format(Locale.getDefault(), "С каждого гостя: %.2f руб.", perPerson));

            } catch (NumberFormatException e) {
                Toast.makeText(this, "Ошибка ввода чисел!", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

---

### Проект 3: «Визитная карточка студента» (Двухэкранное приложение)

**Идея:** На первом экране студент заполняет данные своего профиля (ФИО, группа, специализация). При нажатии кнопки данные передаются на второй экран, оформленный в виде красивой стилизованной ID-карты.

#### Разметка экрана ввода (`activity_main.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="24dp">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Создание визитки"
        android:textSize="24sp"
        android:textStyle="bold"
        android:layout_marginBottom="24dp" />

    <EditText
        android:id="@+id/etFullName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Фамилия и Имя"
        android:layout_marginBottom="16dp" />

    <EditText
        android:id="@+id/etGroup"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Учебная группа (например, ИВТ-22)"
        android:layout_marginBottom="16dp" />

    <EditText
        android:id="@+id/etSkill"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Любимая технология (Java, SQL, React)"
        android:layout_marginBottom="24dp" />

    <Button
        android:id="@+id/btnGenerateCard"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Сгенерировать карту" />

</LinearLayout>
```

#### Код отправки (`MainActivity.java`)

```java
package com.example.studentcard;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        EditText etFullName = findViewById(R.id.etFullName);
        EditText etGroup = findViewById(R.id.etGroup);
        EditText etSkill = findViewById(R.id.etSkill);
        Button btnGenerate = findViewById(R.id.btnGenerateCard);

        btnGenerate.setOnClickListener(v -> {
            String name = etFullName.getText().toString().trim();
            String group = etGroup.getText().toString().trim();
            String skill = etSkill.getText().toString().trim();

            if (name.isEmpty() || group.isEmpty() || skill.isEmpty()) {
                Toast.makeText(this, "Заполните абсолютно все поля анкеты!", Toast.LENGTH_SHORT).show();
                return;
            }

            Intent intent = new Intent(MainActivity.this, CardActivity.class);
            intent.putExtra("KEY_NAME", name);
            intent.putExtra("KEY_GROUP", group);
            intent.putExtra("KEY_SKILL", skill);
            startActivity(intent);
        });
    }
}
```

#### Разметка карты (`activity_card.xml`)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center"
    android:padding="24dp"
    android:backgroundColor="#ECEFF1">

    <!-- Карточка студента -->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="24dp"
        android:background="#FFFFFF"
        android:elevation="8dp"
        android:layout_marginBottom="32dp">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="СТУДЕНЧЕСКИЙ БИЛЕТ"
            android:textSize="14sp"
            android:letterSpacing="0.1"
            android:textColor="#78909C"
            android:layout_marginBottom="16dp" />

        <TextView
            android:id="@+id/tvCardName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Иван Иванов"
            android:textSize="22sp"
            android:textStyle="bold"
            android:textColor="#263238"
            android:layout_marginBottom="8dp" />

        <TextView
            android:id="@+id/tvCardGroup"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Группа: ПИ-202"
            android:textSize="16sp"
            android:layout_marginBottom="4dp" />

        <TextView
            android:id="@+id/tvCardSkill"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Направление: Android Development"
            android:textSize="16sp"
            android:textColor="#0288D1" />

    </LinearLayout>

    <Button
        android:id="@+id/btnBack"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:text="Назад к анкете" />

</LinearLayout>
```

#### Код приема (`CardActivity.java`)

```java
package com.example.studentcard;

import android.os.Bundle;
import android.widget.Button;
import android.widget.TextView;
import androidx.appcompat.app.AppCompatActivity;

public class CardActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_card);

        TextView tvCardName = findViewById(R.id.tvCardName);
        TextView tvCardGroup = findViewById(R.id.tvCardGroup);
        TextView tvCardSkill = findViewById(R.id.tvCardSkill);
        Button btnBack = findViewById(R.id.btnBack);

        // Считываем значения из Intent
        String name = getIntent().getStringExtra("KEY_NAME");
        String group = getIntent().getStringExtra("KEY_GROUP");
        String skill = getIntent().getStringExtra("KEY_SKILL");

        tvCardName.setText(name);
        tvCardGroup.setText("Группа: " + group);
        tvCardSkill.setText("Стек: " + skill);

        btnBack.setOnClickListener(v -> finish());
    }
}
```

---

## 8. Топ-12 критических ошибок новичков и их решение

| №   | Симптом / Ошибка                                               | Причина                                                                                     | Как исправить                                                                                                                     |
| :-- | :------------------------------------------------------------- | :------------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------------------------------------------------- |
| 1   | **Приложение моментально вылетает при запуске**                | `NullPointerException` при попытке вызвать метод у элемента UI.                             | Вы вызвали `findViewById()` **до** строчки `setContentView(R.layout.activity_main)` или опечатались в ID элемента.                |
| 2   | **Вылет при нажатии на кнопку расчета**                        | `NumberFormatException: For input string: ""`                                               | Вы попытались вызвать `Integer.parseInt(text)`, когда пользователь ничего не ввел. Всегда делайте проверку `if (text.isEmpty())`. |
| 3   | **Всплывающее сообщение Toast не показывается**                | Написали `Toast.makeText(...)`, но забыли `.show()`.                                        | Всегда дописывайте в конце `.show()`!                                                                                             |
| 4   | **Красный класс `R` в коде (`Cannot resolve symbol R`)**       | Синтаксическая ошибка в одном из XML-файлов в папке `res/`.                                 | Откройте панель **Build** внизу, найдите ошибку в XML (незакрытый тег или спецсимвол) и исправьте её.                             |
| 5   | **Текст налезает друг на друга в левом верхнем углу**          | Использован `ConstraintLayout`, но элементам не заданы привязки (Constraints).              | Либо добавьте привязки со всех 4 сторон, либо замените контейнер на `LinearLayout`.                                               |
| 6   | **Второй экран не открывается (вылет при startActivity)**      | Забыли зарегистрировать новую Activity в `AndroidManifest.xml`.                             | Добавьте `<activity android:name=".SecondActivity" android:exported="false" />` внутрь тега `<application>`.                      |
| 7   | **Приложение вылетает при попытке скачивания из сети**         | В `AndroidManifest.xml` не запрошено разрешение на доступ к интернету.                      | Добавьте `<uses-permission android:name="android.permission.INTERNET" />`.                                                        |
| 8   | **При повороте экрана введенные данные пропадают**             | При повороте смартфона Android по умолчанию полностью уничтожает и создает Activity заново. | Для начала зафиксируйте ориентацию в манифесте: `android:screenOrientation="portrait"`.                                           |
| 9   | **В Android Studio подчеркивает всё красным, хотя код верный** | Сбой внутренних кэшей среды разработки.                                                     | Выберите в верхнем меню: `File -> Invalidate Caches... -> Invalidate and Restart`.                                                |
| 10  | **Текст в TextView не обновляется**                            | Вы забыли вызвать `.toString()` при конкатенации или вызвали метод у не той переменной.     | Проверьте: `tvResult.setText(String.valueOf(result));`.                                                                           |
| 11  | **Кнопка не реагирует на нажатия**                             | Слушатель `setOnClickListener` написан, но не прикреплен к кнопке.                          | Убедитесь, что метод вызван именно у объекта кнопки внутри `onCreate()`.                                                          |
| 12  | **Gradle Sync Failed (нет интернета)**                         | Android Studio не смогла скачать плагины сборщика.                                          | Проверьте подключение к сети и отключите прокси/VPN, если они блокируют репозиторий Google Maven.                                 |

---

## 9. Практические задания

Задания распределены по трем уровням сложности для постепенного освоения платформы.

### Уровень 1: Простые одноэкранные утилиты

1. **Светофор:** Экран с тремя кнопками («Красный», «Желтый», «Зеленый»). При нажатии на каждую кнопка меняет фоновый цвет всего экрана на соответствующий.

```java
public class MainActivity extends AppCompatActivity {
    @Override protected void onCreate(Bundle b) {
        super.onCreate(b);
        setContentView(R.layout.activity_main);
        LinearLayout root = findViewById(R.id.root);
        findViewById(R.id.btnRed).setOnClickListener(v -> root.setBackgroundColor(Color.RED));
        findViewById(R.id.btnYellow).setOnClickListener(v -> root.setBackgroundColor(Color.YELLOW));
        findViewById(R.id.btnGreen).setOnClickListener(v -> root.setBackgroundColor(Color.GREEN));
    }
}
```

2. **Генератор случайных чисел (Dice Roller):** Кнопка «Бросить кубик», которая выводит в крупном `TextView` случайное число от 1 до 6.

```java
Random random = new Random();
findViewById(R.id.btnRoll).setOnClickListener(v -> {
    int value = random.nextInt(6) + 1; // 1..6
    ((TextView) findViewById(R.id.tvDice)).setText(String.valueOf(value));
});
```

3. **Мини-тест с одной кнопкой:** Задан вопрос и поле ввода ответа. По кнопке «Проверить» выводится надпись «Верно!» (зеленым) или «Ошибка!» (красным).

```java
EditText etAnswer = findViewById(R.id.etAnswer);
TextView tvVerdict = findViewById(R.id.tvVerdict);
findViewById(R.id.btnCheck).setOnClickListener(v -> {
    String answer = etAnswer.getText().toString().trim().toLowerCase();
    if (answer.equals("париж")) {
        tvVerdict.setText("Верно!");
        tvVerdict.setTextColor(Color.parseColor("#388E3C"));
    } else {
        tvVerdict.setText("Ошибка!");
        tvVerdict.setTextColor(Color.parseColor("#D32F2F"));
    }
});
```

4. **Переключатель видимости (Toggle):** Кнопка «Показать секрет / Скрыть секрет», которая поочередно показывает и скрывает текст на экране (`View.VISIBLE` / `View.GONE`).

```java
TextView tvSecret = findViewById(R.id.tvSecret);
Button btnToggle = findViewById(R.id.btnToggle);
final boolean[] visible = {false};
btnToggle.setOnClickListener(v -> {
    visible[0] = !visible[0];
    tvSecret.setVisibility(visible[0] ? View.VISIBLE : View.GONE);
    btnToggle.setText(visible[0] ? "Скрыть секрет" : "Показать секрет");
});
```

5. **Инвертор текста:** Поле ввода, куда вводится слово, и кнопка, выводящая его задом наперед (с помощью `StringBuilder.reverse()`).

```java
EditText etInput = findViewById(R.id.etInput);
TextView tvOutput = findViewById(R.id.tvOutput);
findViewById(R.id.btnReverse).setOnClickListener(v -> {
    String src = etInput.getText().toString().trim();
    if (src.isEmpty()) { Toast.makeText(this, "Введите слово", Toast.LENGTH_SHORT).show(); return; }
    tvOutput.setText(new StringBuilder(src).reverse().toString());
});
```

6. **Калькулятор возраста питомца:** Перевод человеческих лет собаки в «собачьи» (умножение введенного числа на 7).

```java
EditText etHuman = findViewById(R.id.etHuman);
TextView tvDog = findViewById(R.id.tvDog);
findViewById(R.id.btnCalc).setOnClickListener(v -> {
    String s = etHuman.getText().toString().trim();
    if (s.isEmpty()) { Toast.makeText(this, "Введите возраст", Toast.LENGTH_SHORT).show(); return; }
    try {
        int human = Integer.parseInt(s);
        tvDog.setText("Собачьих лет: " + (human * 7));
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Нужно число!", Toast.LENGTH_SHORT).show();
    }
});
```

7. **Счетчик символов:** Поле ввода текста, под которым в реальном времени или по нажатию кнопки отображается общее количество введенных букв.

```java
EditText etText = findViewById(R.id.etText);
TextView tvCount = findViewById(R.id.tvCount);
etText.addTextChangedListener(new android.text.TextWatcher() {
    public void beforeTextChanged(CharSequence s, int a, int b, int c) {}
    public void onTextChanged(CharSequence s, int a, int b, int c) {
        tvCount.setText("Символов: " + s.length());
    }
    public void afterTextChanged(android.text.Editable s) {}
});
```

8. **Симулятор фонарика:** Кнопка переключения экрана из черного в чисто белый цвет на максимальной яркости.

```java
LinearLayout root = findViewById(R.id.root);
Button btnToggle = findViewById(R.id.btnToggle);
final boolean[] on = {false};
btnToggle.setOnClickListener(v -> {
    on[0] = !on[0];
    root.setBackgroundColor(on[0] ? Color.WHITE : Color.BLACK);
    btnToggle.setText(on[0] ? "Выключить" : "Включить");
});
```

9. **Конвертер сантиметров в дюймы:** Поле ввода числа и перевод сантиметров в дюймы (`дюймы = см / 2.54`).

```java
EditText etCm = findViewById(R.id.etCm);
TextView tvInch = findViewById(R.id.tvInch);
findViewById(R.id.btnConvert).setOnClickListener(v -> {
    String s = etCm.getText().toString().trim();
    if (s.isEmpty()) { Toast.makeText(this, "Введите см", Toast.LENGTH_SHORT).show(); return; }
    try {
        double cm = Double.parseDouble(s);
        double inch = cm / 2.54;
        tvInch.setText(String.format(java.util.Locale.getDefault(), "%.2f дюймов", inch));
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Нужно число!", Toast.LENGTH_SHORT).show();
    }
});
```

10. **Определитель четности:** Программа принимает целое число и сообщает, является ли оно четным или нечетным.

```java
EditText etNumber = findViewById(R.id.etNumber);
TextView tvResult = findViewById(R.id.tvResult);
findViewById(R.id.btnCheckParity).setOnClickListener(v -> {
    String s = etNumber.getText().toString().trim();
    if (s.isEmpty()) { Toast.makeText(this, "Введите число", Toast.LENGTH_SHORT).show(); return; }
    try {
        int n = Integer.parseInt(s);
        tvResult.setText((n % 2 == 0) ? n + " — чётное" : n + " — нечётное");
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Нужно целое число!", Toast.LENGTH_SHORT).show();
    }
});
```


### Уровень 2: Калькуляторы и логические экраны

11. **Индекс массы тела (ИМТ / BMI):** Ввод веса (кг) и роста (см). Расчет по формуле: `вес / (рост/100)^2` с текстовым вердиктом («Дефицит», «Норма», «Избыток»).

```java
EditText etWeight = findViewById(R.id.etWeight);
EditText etHeight = findViewById(R.id.etHeight);
TextView tvBmi = findViewById(R.id.tvBmi);
findViewById(R.id.btnBmi).setOnClickListener(v -> {
    String ws = etWeight.getText().toString().trim();
    String hs = etHeight.getText().toString().trim();
    if (ws.isEmpty() || hs.isEmpty()) { Toast.makeText(this, "Заполните оба поля", Toast.LENGTH_SHORT).show(); return; }
    try {
        double w = Double.parseDouble(ws);
        double h = Double.parseDouble(hs) / 100.0; // см → м
        if (w <= 0 || h <= 0) { Toast.makeText(this, "Значения должны быть > 0", Toast.LENGTH_SHORT).show(); return; }
        double bmi = w / (h * h);
        String verdict;
        if (bmi < 18.5) verdict = "Дефицит массы";
        else if (bmi < 25) verdict = "Норма";
        else if (bmi < 30) verdict = "Избыток массы";
        else verdict = "Ожирение";
        tvBmi.setText(String.format(java.util.Locale.getDefault(), "ИМТ: %.2f — %s", bmi, verdict));
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Ошибка ввода", Toast.LENGTH_SHORT).show();
    }
});
```

12. **Калькулятор расхода топлива:** Ввод пройденного расстояния (км) и потраченных литров бензина. Расчет среднего расхода на 100 км пути.

```java
findViewById(R.id.btnFuel).setOnClickListener(v -> {
    String ds = ((EditText) findViewById(R.id.etDistance)).getText().toString().trim();
    String ls = ((EditText) findViewById(R.id.etLiters)).getText().toString().trim();
    if (ds.isEmpty() || ls.isEmpty()) { Toast.makeText(this, "Заполните поля", Toast.LENGTH_SHORT).show(); return; }
    try {
        double d = Double.parseDouble(ds), l = Double.parseDouble(ls);
        if (d <= 0) { Toast.makeText(this, "Расстояние > 0", Toast.LENGTH_SHORT).show(); return; }
        double per100 = (l / d) * 100;
        ((TextView) findViewById(R.id.tvFuel)).setText(
            String.format(java.util.Locale.getDefault(), "Расход: %.2f л/100 км", per100));
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Ошибка ввода", Toast.LENGTH_SHORT).show();
    }
});
```

13. **Конвертер температур:** Поле ввода градусов Цельсия и кнопки для пересчета в Фаренгейты (`F = C * 1.8 + 32`) и Кельвины.

```java
EditText etCelsius = findViewById(R.id.etCelsius);
TextView tvOut = findViewById(R.id.tvOut);
findViewById(R.id.btnToF).setOnClickListener(v -> {
    String s = etCelsius.getText().toString().trim();
    if (s.isEmpty()) return;
    double c = Double.parseDouble(s);
    double f = c * 1.8 + 32;
    tvOut.setText(String.format(java.util.Locale.getDefault(), "%.2f °F", f));
});
findViewById(R.id.btnToK).setOnClickListener(v -> {
    String s = etCelsius.getText().toString().trim();
    if (s.isEmpty()) return;
    double c = Double.parseDouble(s);
    double k = c + 273.15;
    tvOut.setText(String.format(java.util.Locale.getDefault(), "%.2f K", k));
});
```

14. **Простой калькулятор (4 действия):** Два поля для чисел и четыре отдельные кнопки: `+`, `-`, `*`, `/` с защитой от деления на ноль.

```java
EditText etA = findViewById(R.id.etA);
EditText etB = findViewById(R.id.etB);
TextView tvRes = findViewById(R.id.tvRes);

View.OnClickListener op = v -> {
    String as = etA.getText().toString().trim();
    String bs = etB.getText().toString().trim();
    if (as.isEmpty() || bs.isEmpty()) { Toast.makeText(this, "Введите оба числа", Toast.LENGTH_SHORT).show(); return; }
    try {
        double a = Double.parseDouble(as), b = Double.parseDouble(bs);
        double r;
        int id = v.getId();
        if (id == R.id.btnPlus) r = a + b;
        else if (id == R.id.btnMinus) r = a - b;
        else if (id == R.id.btnMul) r = a * b;
        else {
            if (b == 0) { Toast.makeText(this, "Деление на ноль!", Toast.LENGTH_SHORT).show(); return; }
            r = a / b;
        }
        tvRes.setText("Результат: " + r);
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Ошибка ввода", Toast.LENGTH_SHORT).show();
    }
};

findViewById(R.id.btnPlus).setOnClickListener(op);
findViewById(R.id.btnMinus).setOnClickListener(op);
findViewById(R.id.btnMul).setOnClickListener(op);
findViewById(R.id.btnDiv).setOnClickListener(op);
```

15. **Таймер скидки магазина:** Ввод исходной цены и процента скидки. Расчет суммы скидки и итоговой цены товара.

```java
findViewById(R.id.btnDiscount).setOnClickListener(v -> {
    String ps = ((EditText) findViewById(R.id.etPrice)).getText().toString().trim();
    String ds = ((EditText) findViewById(R.id.etDiscount)).getText().toString().trim();
    if (ps.isEmpty() || ds.isEmpty()) { Toast.makeText(this, "Заполните поля", Toast.LENGTH_SHORT).show(); return; }
    try {
        double price = Double.parseDouble(ps);
        double disc = Double.parseDouble(ds);
        if (disc < 0 || disc > 100) { Toast.makeText(this, "Скидка 0..100%", Toast.LENGTH_SHORT).show(); return; }
        double saved = price * disc / 100.0;
        double finalPrice = price - saved;
        ((TextView) findViewById(R.id.tvDiscount)).setText(String.format(
            java.util.Locale.getDefault(), "Скидка: %.2f руб.\nИтого: %.2f руб.", saved, finalPrice));
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Ошибка ввода", Toast.LENGTH_SHORT).show();
    }
});
```

16. **Конвертер валют (Рубли в Доллары/Евро):** Ввод суммы в рублях и фиксированный курс с выводом результата с округлением.

```java
final double USD = 92.5, EUR = 100.3;
findViewById(R.id.btnUsd).setOnClickListener(v -> {
    String s = ((EditText) findViewById(R.id.etRub)).getText().toString().trim();
    if (s.isEmpty()) return;
    double rub = Double.parseDouble(s);
    ((TextView) findViewById(R.id.tvCurrency)).setText(
        String.format(java.util.Locale.getDefault(), "%.2f USD", rub / USD));
});
findViewById(R.id.btnEur).setOnClickListener(v -> {
    String s = ((EditText) findViewById(R.id.etRub)).getText().toString().trim();
    if (s.isEmpty()) return;
    double rub = Double.parseDouble(s);
    ((TextView) findViewById(R.id.tvCurrency)).setText(
        String.format(java.util.Locale.getDefault(), "%.2f EUR", rub / EUR));
});
```

17. **Генератор надежного PIN-кода:** Кнопка генерации 4-значного или 6-значного кода без повторяющихся подряд цифр.

```java
findViewById(R.id.btnPin).setOnClickListener(v -> {
    Random r = new Random();
    StringBuilder pin = new StringBuilder();
    int prev = -1;
    for (int i = 0; i < 6; i++) {
        int d;
        do { d = r.nextInt(10); } while (d == prev);
        pin.append(d);
        prev = d;
    }
    ((TextView) findViewById(R.id.tvPin)).setText("PIN: " + pin);
});
```

18. **Тест на знание столиц:** Экран показывает название страны и 3 кнопки с вариантами столиц. Подсчет очков за правильные ответы.

```java
private int score = 0;
private int questionIndex = 0;
private final String[][] questions = {
    {"Франция", "Париж", "Лондон", "Берлин"},
    {"Германия", "Берлин", "Мадрид", "Рим"},
    {"Италия",  "Рим",    "Париж",  "Мадрид"}
};

private void showQuestion() {
    if (questionIndex >= questions.length) {
        ((TextView) findViewById(R.id.tvQuestion)).setText("Игра окончена! Очки: " + score);
        findViewById(R.id.btnA).setEnabled(false);
        findViewById(R.id.btnB).setEnabled(false);
        findViewById(R.id.btnC).setEnabled(false);
        return;
    }
    String[] q = questions[questionIndex];
    ((TextView) findViewById(R.id.tvQuestion)).setText("Столица: " + q[0] + "?");
    ((Button) findViewById(R.id.btnA)).setText(q[1]);
    ((Button) findViewById(R.id.btnB)).setText(q[2]);
    ((Button) findViewById(R.id.btnC)).setText(q[3]);
}

private void check(String answer) {
    if (answer.equals(questions[questionIndex][1])) score++;
    questionIndex++;
    showQuestion();
}
```

19. **Калькулятор времени в пути:** Ввод расстояния (км) и средней скорости (км/ч). Расчет времени в часах и минутах.

```java
findViewById(R.id.btnTime).setOnClickListener(v -> {
    String ds = ((EditText) findViewById(R.id.etDist)).getText().toString().trim();
    String ss = ((EditText) findViewById(R.id.etSpeed)).getText().toString().trim();
    if (ds.isEmpty() || ss.isEmpty()) { Toast.makeText(this, "Заполните поля", Toast.LENGTH_SHORT).show(); return; }
    try {
        double d = Double.parseDouble(ds), s = Double.parseDouble(ss);
        if (s <= 0) { Toast.makeText(this, "Скорость > 0", Toast.LENGTH_SHORT).show(); return; }
        double hours = d / s;
        int h = (int) hours;
        int m = (int) Math.round((hours - h) * 60);
        ((TextView) findViewById(R.id.tvTime)).setText(
            String.format(java.util.Locale.getDefault(), "Время: %d ч %d мин", h, m));
    } catch (NumberFormatException e) {
        Toast.makeText(this, "Ошибка ввода", Toast.LENGTH_SHORT).show();
    }
});
```

20. **Оценщик надежности пароля:** Проверка введенного пароля по длине: меньше 6 знаков — слабый, от 6 до 10 — средний, более 10 — надежный.

```java
EditText etPass = findViewById(R.id.etPass);
TextView tvStrength = findViewById(R.id.tvStrength);
etPass.addTextChangedListener(new android.text.TextWatcher() {
    public void beforeTextChanged(CharSequence s, int a, int b, int c) {}
    public void onTextChanged(CharSequence s, int a, int b, int c) {
        int len = s.length();
        String verdict;
        int color;
        if (len < 6) { verdict = "Слабый"; color = Color.parseColor("#D32F2F"); }
        else if (len <= 10) { verdict = "Средний"; color = Color.parseColor("#F57C00"); }
        else { verdict = "Надёжный"; color = Color.parseColor("#388E3C"); }
        tvStrength.setText("Надёжность: " + verdict);
        tvStrength.setTextColor(color);
    }
    public void afterTextChanged(android.text.Editable s) {}
});
```


### Уровень 3: Двухэкранные сценарии и сложные формы

21. **Экран входа и Личный кабинет:** На первом экране ввод логина и пароля (хардкод `admin`/`1234`). При успехе — переход на экран кабинета с отображением имени пользователя.

```java
package com.example.authapp;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class LoginActivity extends AppCompatActivity {
    private static final String VALID_LOGIN = "admin";
    private static final String VALID_PASSWORD = "1234";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);

        EditText etLogin = findViewById(R.id.etLogin);
        EditText etPassword = findViewById(R.id.etPassword);
        Button btnLogin = findViewById(R.id.btnLogin);

        btnLogin.setOnClickListener(v -> {
            String login = etLogin.getText().toString().trim();
            String pass = etPassword.getText().toString().trim();

            if (login.isEmpty() || pass.isEmpty()) {
                Toast.makeText(this, "Заполните оба поля", Toast.LENGTH_SHORT).show();
                return;
            }
            if (login.equals(VALID_LOGIN) && pass.equals(VALID_PASSWORD)) {
                Intent intent = new Intent(LoginActivity.this, ProfileActivity.class);
                intent.putExtra("EXTRA_LOGIN", login);
                startActivity(intent);
                finish();
            } else {
                Toast.makeText(this, "Неверный логин или пароль", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

22. **Оформление заказа пиццы:** Экран выбора размера пиццы и адреса доставки. На втором экране — чек с деталями заказа и итоговой стоимостью.

```java
package com.example.pizza;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.RadioGroup;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class OrderActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_order);

        RadioGroup rgSize = findViewById(R.id.rgSize);
        EditText etAddress = findViewById(R.id.etAddress);
        Button btnOrder = findViewById(R.id.btnOrder);

        btnOrder.setOnClickListener(v -> {
            String address = etAddress.getText().toString().trim();
            if (address.isEmpty()) {
                Toast.makeText(this, "Введите адрес", Toast.LENGTH_SHORT).show();
                return;
            }

            String size;
            int price;
            int id = rgSize.getCheckedRadioButtonId();
            if (id == R.id.rbSmall)       { size = "Маленькая"; price = 400; }
            else if (id == R.id.rbMedium) { size = "Средняя";   price = 600; }
            else                          { size = "Большая";   price = 800; }

            Intent intent = new Intent(OrderActivity.this, ReceiptActivity.class);
            intent.putExtra("EXTRA_SIZE", size);
            intent.putExtra("EXTRA_PRICE", price);
            intent.putExtra("EXTRA_ADDRESS", address);
            startActivity(intent);
        });
    }
}
```

23. **Квиз из двух вопросов:** Вопрос 1 на первом экране, по нажатию «Далее» — переход на экран с вопросом 2, а затем вывод суммарного балла.

```java
package com.example.quiz;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import androidx.appcompat.app.AppCompatActivity;

public class QuizQ1Activity extends AppCompatActivity {
    private static final String CORRECT = "4";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_q1);

        EditText et = findViewById(R.id.etAnswer);
        Button btn = findViewById(R.id.btnNext);

        btn.setOnClickListener(v -> {
            String answer = et.getText().toString().trim();
            int score = answer.equals(CORRECT) ? 1 : 0;

            Intent intent = new Intent(this, QuizQ2Activity.class);
            intent.putExtra("EXTRA_SCORE", score);
            startActivity(intent);
        });
    }
}
```

24. **Конструктор визитки мастера:** Ввод номера телефона, профессии и имени. На втором экране — стилизованная карточка с возможностью нажать на кнопку и перейти в системную звонилку (`ACTION_DIAL`).

```java
package com.example.visitcard;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class InputActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_input);

        EditText etName = findViewById(R.id.etName);
        EditText etProf = findViewById(R.id.etProf);
        EditText etPhone = findViewById(R.id.etPhone);

        ((Button) findViewById(R.id.btnMake)).setOnClickListener(v -> {
            String name = etName.getText().toString().trim();
            String prof = etProf.getText().toString().trim();
            String phone = etPhone.getText().toString().trim();
            if (name.isEmpty() || prof.isEmpty() || phone.isEmpty()) {
                Toast.makeText(this, "Заполните все поля", Toast.LENGTH_SHORT).show();
                return;
            }
            Intent intent = new Intent(this, CardActivity.class);
            intent.putExtra("KEY_NAME", name);
            intent.putExtra("KEY_PROF", prof);
            intent.putExtra("KEY_PHONE", phone);
            startActivity(intent);
        });
    }
}
```

25. **Дневник заметок (Передача текста):** Экран ввода длинного текста заметки. На втором экране — режим чтения крупным шрифтом с кнопкой «Редактировать» (возврат назад).

```java
package com.example.notes;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class EditNoteActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_edit);

        EditText etNote = findViewById(R.id.etNote);
        Button btnSave = findViewById(R.id.btnSave);

        btnSave.setOnClickListener(v -> {
            String text = etNote.getText().toString().trim();
            if (text.isEmpty()) {
                Toast.makeText(this, "Заметка пуста", Toast.LENGTH_SHORT).show();
                return;
            }
            Intent intent = new Intent(this, ViewNoteActivity.class);
            intent.putExtra("KEY_NOTE", text);
            startActivity(intent);
        });
    }
}
```

26. **Калькулятор автокредита:** Расчет ежемесячного платежа по формуле на первом экране и показ графика выплат на втором.

```java
package com.example.loan;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class LoanInputActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_loan_input);

        EditText etAmount = findViewById(R.id.etAmount);
        EditText etRate = findViewById(R.id.etRate);
        EditText etMonths = findViewById(R.id.etMonths);

        ((Button) findViewById(R.id.btnCalc)).setOnClickListener(v -> {
            try {
                double amount = Double.parseDouble(etAmount.getText().toString().trim());
                double rate = Double.parseDouble(etRate.getText().toString().trim());
                int months = Integer.parseInt(etMonths.getText().toString().trim());

                if (amount <= 0 || rate <= 0 || months <= 0) {
                    Toast.makeText(this, "Значения должны быть > 0", Toast.LENGTH_SHORT).show();
                    return;
                }

                Intent intent = new Intent(this, LoanResultActivity.class);
                intent.putExtra("KEY_AMOUNT", amount);
                intent.putExtra("KEY_RATE", rate);
                intent.putExtra("KEY_MONTHS", months);
                startActivity(intent);

            } catch (Exception e) {
                Toast.makeText(this, "Проверьте ввод", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

27. **Электронный билет на поезд:** Ввод станций отправления и прибытия. На втором экране — генерация карточки посадочного талона с текущей датой.

```java
package com.example.train;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;
import java.util.Random;

public class TicketInputActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_ticket_input);

        EditText etFrom = findViewById(R.id.etFrom);
        EditText etTo = findViewById(R.id.etTo);

        ((Button) findViewById(R.id.btnBuy)).setOnClickListener(v -> {
            String from = etFrom.getText().toString().trim();
            String to = etTo.getText().toString().trim();
            if (from.isEmpty() || to.isEmpty()) {
                Toast.makeText(this, "Заполните обе станции", Toast.LENGTH_SHORT).show();
                return;
            }

            int seat = new Random().nextInt(60) + 1; // 1..60

            Intent intent = new Intent(this, TicketActivity.class);
            intent.putExtra("KEY_FROM", from);
            intent.putExtra("KEY_TO", to);
            intent.putExtra("KEY_SEAT", seat);
            startActivity(intent);
        });
    }
}
```

28. **Анкета спортивного трекера:** Ввод целевого количества шагов и пройденных за день. На втором экране — прогресс-бар и процент выполнения нормы.

```java
package com.example.tracker;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import android.widget.EditText;
import android.widget.Toast;
import androidx.appcompat.app.AppCompatActivity;

public class TrackerInputActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_tracker_input);

        EditText etGoal = findViewById(R.id.etGoal);
        EditText etDone = findViewById(R.id.etDone);

        ((Button) findViewById(R.id.btnCheck)).setOnClickListener(v -> {
            try {
                int goal = Integer.parseInt(etGoal.getText().toString().trim());
                int done = Integer.parseInt(etDone.getText().toString().trim());
                if (goal <= 0) {
                    Toast.makeText(this, "Цель должна быть > 0", Toast.LENGTH_SHORT).show();
                    return;
                }
                Intent intent = new Intent(this, TrackerResultActivity.class);
                intent.putExtra("KEY_GOAL", goal);
                intent.putExtra("KEY_DONE", done);
                startActivity(intent);
            } catch (NumberFormatException e) {
                Toast.makeText(this, "Введите числа", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

29. **Мини-словарь терминов:** Список из 3 терминов (кнопки). При клике на любую открывается экран с подробным описанием термина.

```java
package com.example.glossary;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class GlossaryActivity extends AppCompatActivity {

    private void openTerm(String title, String desc) {
        Intent intent = new Intent(this, TermActivity.class);
        intent.putExtra("KEY_TITLE", title);
        intent.putExtra("KEY_DESC", desc);
        startActivity(intent);
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_glossary);

        ((Button) findViewById(R.id.btnActivity)).setOnClickListener(v -> openTerm(
            "Activity",
            "Activity — это один экран приложения. Каждое окно, которое видит пользователь, " +
            "обычно реализуется отдельным классом, наследующим AppCompatActivity. " +
            "Жизненный цикл Activity управляется системой Android: onCreate(), onStart(), " +
            "onResume(), onPause(), onStop(), onDestroy()."));

        ((Button) findViewById(R.id.btnIntent)).setOnClickListener(v -> openTerm(
            "Intent",
            "Intent — это «намерение» выполнить действие. Бывают явные (указываем конкретный " +
            "класс Activity) и неявные (указываем действие, например ACTION_VIEW). " +
            "Через Intent можно передавать данные между экранами с помощью putExtra()."));

        ((Button) findViewById(R.id.btnLayout)).setOnClickListener(v -> openTerm(
            "Layout",
            "Layout — это XML-файл разметки экрана. Основные контейнеры: LinearLayout " +
            "(выстраивает элементы в ряд), ConstraintLayout (гибкое позиционирование), " +
            "FrameLayout, RelativeLayout. Каждый элемент получает id, по которому его " +
            "находит findViewById() в Java-коде."));
    }
}
```

30. **Итоговый проект «Портфолио студента»:** Главный экран с фотографией, кнопками «Обо мне», «Мои навыки», «Контакты», открывающими соответствующие детальные экраны.

```java
package com.example.portfolio;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        ((Button) findViewById(R.id.btnAbout)).setOnClickListener(v ->
                startActivity(new Intent(this, AboutActivity.class)));

        ((Button) findViewById(R.id.btnSkills)).setOnClickListener(v ->
                startActivity(new Intent(this, SkillsActivity.class)));

        ((Button) findViewById(R.id.btnContacts)).setOnClickListener(v ->
                startActivity(new Intent(this, ContactsActivity.class)));
    }
}
```


---
