---
title: "Практическая работа №2: Знакомство с языком Java для Android-разработки"
discipline: "Разработка мобильных приложений"
status: "Active"
author: "УПМ 2"
tags: [java, android, oop, mobile-dev, laboratory-work]
---

# Практическая работа: Введение в язык Java в контексте мобильной разработки

> [!NOTE]
> **Цель работы:** Освоить базовый синтаксис языка Java, принципы объектно-ориентированного программирования (ООП), механизмы интерфейсов и стандартные структуры данных, необходимые для разработки нативных компонентов мобильных приложений.

---

## 📋 Содержание

1. [Вводные требования и инструментарий](#0-вводные-требования-и-инструментарий)
2. [Тема 1. Базовый синтаксис, примитивные типы и операторы](#тема-1-базовый-синтаксис-примитивные-типы-и-операторы)
3. [Тема 2. Управляющие конструкции и операторы ветвления](#тема-2-управляющие-конструкции-и-операторы-ветвления)
4. [Тема 3. Массивы, строки и форматирование данных](#тема-3-массивы-строки-и-форматирование-данных)
5. [Тема 4. Методы и модульность](#тема-4-методы-и-модульность)
6. [Тема 5. Классы, объекты и инкапсуляция](#тема-5-классы-объекты-и-инкапсуляция)
7. [Тема 6. Наследование и полиморфизм](#тема-6-наследование-и-полиморфизм)
8. [Тема 7. Абстрактные классы и интерфейсы](#тема-7-абстрактные-классы-и-интерфейсы)
9. [Тема 8. Коллекции и Generics](#тема-8-коллекции-и-generics)
10. [Тема 9. Исключения и безопасность выполнения](#тема-9-исключения-и-безопасность-выполнения)
11. [50 Смешанных практических заданий (Mobile Logic Focus)](#50-смешанных-практических-заданий)
12. [Критерии оценки и регламент сдачи](#критерии-оценки-и-регламент-сдачи)

---

## 0. Вводные требования и инструментарий

Для выполнения заданий рекомендуется использовать актуальное окружение разработки:

- **JDK:** OpenJDK 21 LTS или OpenJDK 25.
- **IDE:** Android Studio (Ladybug / Hedgehog / Jellyfish) или IntelliJ IDEA Community / Ultimate.
- **Система сборки:** Gradle (Kotlin DSL или Groovy DSL).

> [!TIP]
> При написании консольных классов для отладки создавайте классический метод точки входа:
>
> ```java
> public class Main {
>     public static void main(String[] args) {
>         System.out.println("Java for Android is ready!");
>     }
> }
> ```

---

## Тема 1. Базовый синтаксис, примитивные типы и операторы

Язык Java является строго типизированным. Примитивные типы хранят значения непосредственно в стеке памяти, что критично для энергоэффективности мобильных чипов.

### Таблица типов данных

| Тип       | Размер     | Диапазон / Значение         | Мобильный контекст                        |
| :-------- | :--------- | :-------------------------- | :---------------------------------------- |
| `byte`    | 8 бит      | -128 .. 127                 | Буферы сырых сетевых пакетов, аудио       |
| `short`   | 16 бит     | -32 768 .. 32 767           | Обработка датчиков (акселерометр)         |
| `int`     | 32 бит     | \(-2^{31}\) .. \(2^{31}-1\) | ID ресурсов `R.id.*`, индексы списков     |
| `long`    | 64 бит     | \(-2^{63}\) .. \(2^{63}-1\) | Таймстемпы Unix (мс), ID сущностей SQLite |
| `float`   | 32 бит     | IEEE 754                    | Координаты экрана, плотность dp/sp        |
| `double`  | 64 бит     | IEEE 754                    | Геолокация (GPS широта и долгота)         |
| `boolean` | 1 бит/байт | `true` / `false`            | Флаги видимости, состояния переключателей |
| `char`    | 16 бит     | `\u0000` .. `\uffff`        | Одиночные символы ввода                   |

### Пример кода

```java
public class ScreenMetrics {
    public static void main(String[] args) {
        final double LATITUDE = 54.0105;
        final double LONGITUDE = 38.2917;

        int screenWidthPx = 1080;
        float density = 2.75f;
        int screenWidthDp = (int) (screenWidthPx / density);

        boolean isLocationEnabled = true;

        System.out.println("Координаты: " + LATITUDE + ", " + LONGITUDE);
        System.out.println("Ширина экрана в dp: " + screenWidthDp);
        System.out.println("Статус GPS: " + (isLocationEnabled ? "Включен" : "Отключен"));
    }
}
```

### Задания для закрепления

1. **Конвертер плотности (dp в px):** Напишите программу, принимающую значение размера в `dp` и коэффициент плотности экрана `dpi` (например, 1.5 для hdpi, 2.0 для xhdpi, 3.0 для xxhdpi), и вычисляющую пиксели.

```java
public class DpToPxConverter {
    public static int dpToPx(float dp, float density) {
        return (int) (dp * density);
    }

    public static void main(String[] args) {
        float dp = 48f;
        float[] densities = {1.0f, 1.5f, 2.0f, 3.0f};
        String[] names = {"mdpi", "hdpi", "xhdpi", "xxhdpi"};
        for (int i = 0; i < densities.length; i++) {
            System.out.printf("%s: %.0fdp -> %dpx%n", names[i], dp, dpToPx(dp, densities[i]));
        }
    }
}
```

2. **Парсинг таймстемпа:** Создайте переменную типа `long`, содержащую миллисекунды. Вычислите количество полных минут, секунд и часов без использования сторонних библиотек времени.

```java
public class TimestampParser {
    public static void main(String[] args) {
        long millis = 3_725_000L; // 1 час 2 мин 5 сек
        long totalSeconds = millis / 1000;
        long hours = totalSeconds / 3600;
        long minutes = (totalSeconds % 3600) / 60;
        long seconds = totalSeconds % 60;
        System.out.printf("%d ч %d мин %d сек%n", hours, minutes, seconds);
    }
}
```

3. **Расчет расхода батареи:** Дана емкость аккумулятора смартфона (мАч) и среднее потребление модуля связи (мА) и дисплея (мА). Рассчитайте ориентировочное время автономной работы в часах.

```java
public class BatteryEstimator {
    public static double estimateHours(int capacityMah, int radioMa, int displayMa) {
        int totalConsumption = radioMa + displayMa;
        if (totalConsumption <= 0) return 0;
        return (double) capacityMah / totalConsumption;
    }

    public static void main(String[] args) {
        System.out.printf("Автономность: %.2f ч%n", estimateHours(4500, 150, 350));
    }
}
```

4. **Валидатор диапазона координат:** Реализуйте проверку широты (от -90.0 до 90.0) и долготы (от -180.0 до 180.0) через логические операторы `&&` и `||`.

```java
public class CoordinateValidator {
    public static boolean isValid(double lat, double lon) {
        return lat >= -90.0 && lat <= 90.0 && lon >= -180.0 && lon <= 180.0;
    }

    public static void main(String[] args) {
        System.out.println(isValid(54.0105, 38.2917));   // true
        System.out.println(isValid(95.0, 200.0));        // false
    }
}
```

5. **Побитовые флаги разрешений:** Реализуйте установку, снятие и проверку разрешений приложения (`CAMERA = 1`, `LOCATION = 2`, `STORAGE = 4`) с помощью побитовых операций (`|`, `&`, `~`).

```java
public class PermissionFlags {
    public static final int CAMERA = 1;      // 001
    public static final int LOCATION = 2;    // 010
    public static final int STORAGE = 4;     // 100

    public static int grant(int flags, int perm) { return flags | perm; }
    public static int revoke(int flags, int perm) { return flags & ~perm; }
    public static boolean has(int flags, int perm) { return (flags & perm) != 0; }

    public static void main(String[] args) {
        int perms = 0;
        perms = grant(perms, CAMERA);
        perms = grant(perms, LOCATION);
        System.out.println("Есть камера: " + has(perms, CAMERA));       // true
        perms = revoke(perms, CAMERA);
        System.out.println("Есть камера: " + has(perms, CAMERA));       // false
        System.out.println("Есть локация: " + has(perms, LOCATION));    // true
    }
}
```


---

## Тема 2. Управляющие конструкции и операторы ветвления

Мобильные приложения постоянно реагируют на внешние события: переключение вкладок, изменение интернет-соединения, жизненный цикл Activity/Fragment.

### Пример кода (Modern Switch Expression)

```java
public class NetworkStateEvaluator {
    public enum NetworkType { NONE, GPRS, LTE, WIFI, NR_5G }

    public static String getBufferStrategy(NetworkType type) {
        return switch (type) {
            case NONE -> "Оффлайн: показать локальный кэш";
            case GPRS -> "Экономичный режим: низкое разрешение картинок";
            case LTE, WIFI -> "Стандартный режим: прогрессивная загрузка";
            case NR_5G -> "Ультра режим: предварительная загрузка 4K контента";
        };
    }
}
```

### Задания для закрепления

1. **Определение ориентации экрана:** Напишите логику, которая по переданной ширине и высоте окна выводит `"PORTRAIT"`, `"LANDSCAPE"` или `"SQUARE"`.

```java
public class OrientationDetector {
    public static String detect(int w, int h) {
        if (w == h) return "SQUARE";
        return w > h ? "LANDSCAPE" : "PORTRAIT";
    }
    public static void main(String[] args) {
        System.out.println(detect(1080, 1920)); // PORTRAIT
        System.out.println(detect(1920, 1080)); // LANDSCAPE
        System.out.println(detect(500, 500));   // SQUARE
    }
}
```

2. **Классификатор статуса HTTP-ответа:** Используя `switch`, верните категорию ответа сервера по его коду: Информационный (1xx), Успешный (2xx), Перенаправление (3xx), Ошибка клиента (4xx), Ошибка сервера (5xx).

```java
public class HttpStatusClassifier {
    public static String classify(int code) {
        return switch (code / 100) {
            case 1 -> "Информационный";
            case 2 -> "Успешный";
            case 3 -> "Перенаправление";
            case 4 -> "Ошибка клиента";
            case 5 -> "Ошибка сервера";
            default -> "Неизвестный";
        };
    }
    public static void main(String[] args) {
        System.out.println(classify(200));
        System.out.println(classify(404));
        System.out.println(classify(503));
    }
}
```

3. **Симулятор таймера повторных попыток (Backoff):** С помощью цикла `for` смоделируйте 5 попыток подключения к серверу с экспоненциальной задержкой (1с, 2с, 4с, 8с, 16с).

```java
public class BackoffSimulator {
    public static void main(String[] args) {
        int delay = 1;
        for (int attempt = 1; attempt <= 5; attempt++) {
            System.out.println("Попытка #" + attempt + " через " + delay + " с");
            delay *= 2;
        }
    }
}
```

4. **Пропуск поврежденных пакетов:** Дан цикл прохода по массиву идентификаторов сообщений чата. Если ID равен `-1` (ошибка), выполните `continue`. Если ID равен `0` (конец сессии), выполните `break`.

```java
public class ChatPacketFilter {
    public static void main(String[] args) {
        int[] ids = {101, 102, -1, 103, -1, 104, 0, 105};
        for (int id : ids) {
            if (id == -1) { System.out.println("Пропуск поврежденного"); continue; }
            if (id == 0) { System.out.println("Конец сессии"); break; }
            System.out.println("Обработка сообщения: " + id);
        }
    }
}
```

5. **Контроль ввода пин-кода:** Используя цикл `do-while`, реализуйте логику проверки 4-значного кода с ограничением до 3 попыток.

```java
import java.util.Scanner;

public class PinCodeValidator {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        final String CORRECT = "1234";
        int attempts = 0;
        boolean success = false;
        do {
            System.out.print("Введите пин-код: ");
            String input = sc.nextLine();
            attempts++;
            if (CORRECT.equals(input)) { success = true; break; }
            System.out.println("Неверно. Осталось попыток: " + (3 - attempts));
        } while (attempts < 3);
        System.out.println(success ? "Доступ разрешен" : "Карта заблокирована");
    }
}
```


---

## Тема 3. Массивы, строки и форматирование данных

При рендеринге списков (RecyclerView) и обработке REST API ключевую роль играют массивы и неизменяемые строки `String` вместе с `StringBuilder`.

### Пример кода

```java
public class TextSanitizer {
    public static void main(String[] args) {
        String rawInput = "  +7 (999) 123-45-67  ";
        String cleanPhone = rawInput.trim().replaceAll("[^0-9+]", "");

        StringBuilder logBuilder = new StringBuilder();
        logBuilder.append("Пользователь авторизован с номером: ")
                  .append(cleanPhone)
                  .append(" [Время: ")
                  .append(System.currentTimeMillis())
                  .append("]");

        System.out.println(logBuilder.toString());
    }
}
```

### Задания для закрепления

1. **Нормализация поискового запроса:** Напишите метод, очищающий введенный пользователем поисковый запрос от лишних концевых пробелов, приводящий строку к нижнему регистру и заменяющий множественные пробелы на один.

```java
public class SearchQueryNormalizer {
    public static String normalize(String query) {
        return query.trim().toLowerCase().replaceAll("\\s+", " ");
    }
    public static void main(String[] args) {
        System.out.println(normalize("  Android   Studio   Ladybug  "));
    }
}
```

2. **Реверс массива кадров анимации:** Дан массив строковых имен кадров анимации. Разверните его задом наперед без создания второго массива.

```java
public class FrameReverser {
    public static void reverse(String[] arr) {
        for (int i = 0, j = arr.length - 1; i < j; i++, j--) {
            String tmp = arr[i]; arr[i] = arr[j]; arr[j] = tmp;
        }
    }
    public static void main(String[] args) {
        String[] frames = {"f1", "f2", "f3", "f4"};
        reverse(frames);
        System.out.println(java.util.Arrays.toString(frames));
    }
}
```

3. **Маскирование номера банковской карты:** Примите строку из 16 цифр и верните ее в формате `**** **** **** 1234`.

```java
public class CardMasker {
    public static String mask(String card) {
        String digits = card.replaceAll("\\D", "");
        if (digits.length() != 16) throw new IllegalArgumentException("Нужно 16 цифр");
        return "**** **** **** " + digits.substring(12);
    }
    public static void main(String[] args) {
        System.out.println(mask("4276123456789012"));
    }
}
```

4. **Поиск пиковых значений акселерометра:** В массиве из 100 значений измерений датчика найдите максимальный всплеск (максимальную разность между соседними элементами).

```java
public class AccelerometerPeak {
    public static int findMaxJump(float[] data) {
        int idx = -1;
        float max = Float.MIN_VALUE;
        for (int i = 1; i < data.length; i++) {
            float diff = Math.abs(data[i] - data[i - 1]);
            if (diff > max) { max = diff; idx = i; }
        }
        return idx;
    }
    public static void main(String[] args) {
        float[] data = {0.1f, 0.2f, 0.15f, 5.0f, 0.3f};
        System.out.println("Пик на индексе: " + findMaxJump(data));
    }
}
```

5. **Генератор URL-параметров:** Дан строковый массив ключей и массив значений одинаковой длины. Соберите строку GET-запроса вида `?key1=val1&key2=val2` с использованием `StringBuilder`.

```java
public class UrlParamsBuilder {
    public static String build(String[] keys, String[] values) {
        if (keys.length != values.length) throw new IllegalArgumentException();
        StringBuilder sb = new StringBuilder("?");
        for (int i = 0; i < keys.length; i++) {
            if (i > 0) sb.append("&");
            sb.append(keys[i]).append("=").append(values[i]);
        }
        return sb.toString();
    }
    public static void main(String[] args) {
        System.out.println(build(
            new String[]{"id", "source"},
            new String[]{"452", "push"}
        ));
    }
}
```


---

## Тема 4. Методы и модульность

Методы организуют бизнес-логику презентеров и ViewModel. Java поддерживает перегрузку методов (overloading) и аргументы переменной длины (varargs).

### Пример кода

```java
public class NotificationHelper {
    public static void showToast(String message) {
        showToast(message, 2000);
    }

    public static void showToast(String message, int durationMs) {
        System.out.println("[TOAST] " + message + " (Длительность: " + durationMs + "ms)");
    }

    public static void logTags(String category, String... tags) {
        System.out.print("[" + category + "] Теги: ");
        for (String tag : tags) {
            System.out.print("#" + tag + " ");
        }
        System.out.println();
    }
}
```

### Задания для закрепления

1. **Перегрузка валидатора:** Напишите метод `isValid(String email)` и его перегруженную версию `isValid(String email, boolean checkDomain)`.

```java
public class EmailValidator {
    public static boolean isValid(String email) {
        return isValid(email, true);
    }
    public static boolean isValid(String email, boolean checkDomain) {
        if (email == null || !email.contains("@")) return false;
        if (checkDomain) {
            String domain = email.substring(email.indexOf("@") + 1);
            return domain.contains(".");
        }
        return true;
    }
    public static void main(String[] args) {
        System.out.println(isValid("user@mail.ru"));
        System.out.println(isValid("user@mail", true));
        System.out.println(isValid("user@mail", false));
    }
}
```

2. **Форматирование валюты:** Реализуйте метод с параметрами `double amount` и `String currencySymbol`, возвращающий красиво оформленную строку для корзины магазина.

```java
public class CurrencyFormatter {
    public static String format(double amount, String symbol) {
        return String.format("%s%.2f", symbol, amount);
    }
    public static void main(String[] args) {
        System.out.println(format(1234.5, "₽"));
        System.out.println(format(99.99, "$"));
    }
}
```

3. **Калькулятор суммарного размера кэша:** Создайте метод `calculateCache(long... fileSizesInBytes)`, возвращающий сумму всех файлов в мегабайтах (`double`).

```java
public class CacheCalculator {
    public static double calculateCache(long... fileSizesInBytes) {
        long total = 0;
        for (long s : fileSizesInBytes) total += s;
        return total / (1024.0 * 1024.0);
    }
    public static void main(String[] args) {
        System.out.printf("%.2f MB%n", calculateCache(1024, 2048, 512));
    }
}
```

4. **Рекурсивный поиск вложений:** Напишите рекурсивный метод для подсчета общего количества элементов во вложенной структуре папок устройства.

```java
import java.util.*;

public class FolderCounter {
    static class Folder {
        String name;
        List<Folder> subfolders = new ArrayList<>();
        List<String> files = new ArrayList<>();
        Folder(String name) { this.name = name; }
    }

    public static int countItems(Folder folder) {
        int count = folder.files.size();
        for (Folder sub : folder.subfolders) count += countItems(sub);
        return count;
    }

    public static void main(String[] args) {
        Folder root = new Folder("root");
        root.files.add("a.txt");
        Folder sub = new Folder("sub");
        sub.files.add("b.txt");
        sub.files.add("c.txt");
        root.subfolders.add(sub);
        System.out.println("Всего элементов: " + countItems(root)); // 3
    }
}
```

5. **Сравнение версий приложения:** Напишите метод `int compareVersions(String v1, String v2)`, возвращающий `1`, если `v1 > v2`, `-1`, если `v1 < v2`, и `0`, если версии равны (например, "1.12.0" и "1.9.4").

```java
public class VersionComparator {
    public static int compareVersions(String v1, String v2) {
        String[] a = v1.split("\\.");
        String[] b = v2.split("\\.");
        int len = Math.max(a.length, b.length);
        for (int i = 0; i < len; i++) {
            int n1 = i < a.length ? Integer.parseInt(a[i]) : 0;
            int n2 = i < b.length ? Integer.parseInt(b[i]) : 0;
            if (n1 != n2) return Integer.compare(n1, n2);
        }
        return 0;
    }
    public static void main(String[] args) {
        System.out.println(compareVersions("1.12.0", "1.9.4")); // 1
        System.out.println(compareVersions("2.0.0", "2.0.0"));  // 0
        System.out.println(compareVersions("1.0.0", "1.0.1"));  // -1
    }
}
```


---

## Тема 5. Классы, объекты и инкапсуляция

Инкапсуляция защищает целостность внутреннего состояния мобильного экрана. Для неизменяемых моделей данных (DTO) в современном Java используются `record`.

### Пример кода

```java
// Традиционный класс с инкапсуляцией
public class UserProfile {
    private final long id;
    private String username;
    private int loyaltyPoints;

    public UserProfile(long id, String username) {
        this.id = id;
        setUsername(username);
        this.loyaltyPoints = 0;
    }

    public long getId() { return id; }

    public String getUsername() { return username; }

    public void setUsername(String username) {
        if (username == null || username.trim().isEmpty()) {
            throw new IllegalArgumentException("Имя пользователя не может быть пустым");
        }
        this.username = username.trim();
    }

    public void addPoints(int points) {
        if (points > 0) {
            this.loyaltyPoints += points;
        }
    }
}

// Современный DTO в виде record (начиная с Java 16+)
record PushNotificationDto(String title, String body, long timestamp, boolean isRead) {}
```

### Задания для закрепления

1. **Модель экрана настроек (SettingsModel):** Спроектируйте класс с приватными полями `isDarkMode`, `volumeLevel` (от 0 до 100) и `appLanguage`. Обеспечьте валидацию уровня громкости в сеттере.

```java
public class SettingsModel {
    private boolean isDarkMode;
    private int volumeLevel;
    private String appLanguage;

    public boolean isDarkMode() { return isDarkMode; }
    public void setDarkMode(boolean v) { this.isDarkMode = v; }

    public int getVolumeLevel() { return volumeLevel; }
    public void setVolumeLevel(int v) {
        if (v < 0 || v > 100) throw new IllegalArgumentException("Громкость 0..100");
        this.volumeLevel = v;
    }

    public String getAppLanguage() { return appLanguage; }
    public void setAppLanguage(String lang) {
        if (lang == null || lang.isBlank()) throw new IllegalArgumentException();
        this.appLanguage = lang;
    }
}
```

2. **DTO корзины интернет-магазина:** Создайте `record CartItem(String id, String title, double price, int count)`. Добавьте в него метод вычисления общей стоимости позиции.

```java
public record CartItem(String id, String title, double price, int count) {
    public double total() { return price * count; }
    public static void main(String[] args) {
        CartItem item = new CartItem("p1", "Телефон", 29999.99, 2);
        System.out.printf("Итого: %.2f%n", item.total());
    }
}
```

3. **Счетчик непрочитанных пушей:** Разработайте класс `BadgeCounter`, в котором значение счетчика нельзя установить в отрицательное число, а инкремент и декремент происходят через отдельные методы.

```java
public class BadgeCounter {
    private int count = 0;
    public int getCount() { return count; }
    public void increment() { count++; }
    public void decrement() { if (count > 0) count--; }
    public void reset() { count = 0; }
}
```

4. **Инкапсулированный таймер сессии:** Создайте класс `SessionTracker` с приватными полями времени входа и последнего действия. Напишите метод проверки, истекла ли сессия (timeout = 15 минут).

```java
public class SessionTracker {
    private final long loginTime;
    private long lastActionTime;
    private static final long TIMEOUT_MS = 15 * 60 * 1000;

    public SessionTracker(long loginTime) {
        this.loginTime = loginTime;
        this.lastActionTime = loginTime;
    }

    public void onUserAction(long now) { lastActionTime = now; }
    public boolean isExpired(long now) { return (now - lastActionTime) > TIMEOUT_MS; }
    public long getLoginTime() { return loginTime; }
}
```

5. **Модель геопозиции:** Создайте класс `GeoPoint` с неизменяемыми полями широты и долготы, валидируемыми в конструкторе.

```java
public class GeoPoint {
    private final double lat;
    private final double lon;

    public GeoPoint(double lat, double lon) {
        if (lat < -90 || lat > 90) throw new IllegalArgumentException("Широта");
        if (lon < -180 || lon > 180) throw new IllegalArgumentException("Долгота");
        this.lat = lat;
        this.lon = lon;
    }

    public double getLat() { return lat; }
    public double getLon() { return lon; }
}
```


---

## Тема 6. Наследование и полиморфизм

Наследование позволяет переиспользовать базовое поведение виджетов, а полиморфизм — единообразно отрисовывать разнородные элементы списков (списки карточек, баннеров и кнопок).

### Пример кода

```java
// Базовый элемент UI
public abstract class UiComponent {
    protected int id;
    protected boolean isVisible;

    public UiComponent(int id) {
        this.id = id;
        this.isVisible = true;
    }

    public abstract void render();
}

// Дочерний компонент кнопки
public class ButtonComponent extends UiComponent {
    private final String text;

    public ButtonComponent(int id, String text) {
        super(id);
        this.text = text;
    }

    @Override
    public void render() {
        System.out.println("Рендер кнопки [" + id + "]: '" + text + "'");
    }
}

// Дочерний компонент изображения
public class ImageComponent extends UiComponent {
    private final String imageUrl;

    public ImageComponent(int id, String imageUrl) {
        super(id);
        this.imageUrl = imageUrl;
    }

    @Override
    public void render() {
        System.out.println("Рендер изображения [" + id + "] по адресу: " + imageUrl);
    }
}
```

### Задания для закрепления

1. **Иерархия экранов приложения:** Создайте базовый класс `BaseScreen` (с методами `onOpen()`, `onClose()`) и наследников: `LoginScreen`, `HomeScreen`, `SettingsScreen`.

```java
public abstract class BaseScreen {
    public void onOpen() { System.out.println("Открытие экрана " + getClass().getSimpleName()); }
    public void onClose() { System.out.println("Закрытие экрана " + getClass().getSimpleName()); }
}
class LoginScreen extends BaseScreen {}
class HomeScreen extends BaseScreen {}
class SettingsScreen extends BaseScreen {}
```

2. **Полиморфный обработчик аналитики:** Реализуйте базовый класс `AnalyticsEvent` и подклассы `ClickEvent`, `PurchaseEvent`, `ScreenViewEvent`. Напишите сервис, принимающий `AnalyticsEvent` и выводящий разную логику логирования.

```java
import java.util.*;

abstract class AnalyticsEvent {
    abstract String describe();
}
class ClickEvent extends AnalyticsEvent {
    private final String target;
    ClickEvent(String target) { this.target = target; }
    String describe() { return "Click: " + target; }
}
class PurchaseEvent extends AnalyticsEvent {
    private final double amount;
    PurchaseEvent(double amount) { this.amount = amount; }
    String describe() { return "Purchase: " + amount; }
}
class ScreenViewEvent extends AnalyticsEvent {
    private final String screen;
    ScreenViewEvent(String screen) { this.screen = screen; }
    String describe() { return "ScreenView: " + screen; }
}

class AnalyticsService {
    void send(AnalyticsEvent e) { System.out.println("-> " + e.describe()); }
}
```

3. **Модели сенсоров смартфона:** Создайте класс `DeviceSensor` и наследников `GyroscopeSensor` и `LightSensor`. Реализуйте полиморфный метод `readData()`.

```java
abstract class DeviceSensor {
    abstract double readData();
}
class GyroscopeSensor extends DeviceSensor {
    double readData() { return Math.random() * 360; }
}
class LightSensor extends DeviceSensor {
    double readData() { return Math.random() * 10000; }
}
```

4. **Виджеты с кастомной отрисовкой:** Напишите метод `drawScreen(List<UiComponent> components)`, который итерируется по коллекции и вызывает метод `render()` для каждого элемента независимо от его типа.

```java
import java.util.List;

public class ScreenRenderer {
    public static void drawScreen(List<UiComponent> components) {
        for (UiComponent c : components) c.render();
    }
}
```

5. **Тарифные планы подписки:** Создайте базовый класс `Subscription` с расчетом стоимости и подклассы: `MonthlySubscription`, `FamilySubscription` (с учетом количества пользователей), `AnnualDiscountSubscription`.

```java
abstract class Subscription {
    abstract double monthlyCost();
}
class MonthlySubscription extends Subscription {
    double monthlyCost() { return 299.0; }
}
class FamilySubscription extends Subscription {
    private final int users;
    FamilySubscription(int users) { this.users = users; }
    double monthlyCost() { return 199.0 * users; }
}
class AnnualDiscountSubscription extends Subscription {
    double monthlyCost() { return 249.0 * 0.8; } // скидка 20%
}
```


---

## Тема 7. Абстрактные классы и интерфейсы

Интерфейсы определяют контракты взаимодействия. В Android через интерфейсы традиционно реализуются обработчики кликов (`OnClickListener`), колбэки сетевых вызовов и сервис-локаторы.

### Пример кода

```java
public interface OnItemClickListener<T> {
    void onItemClick(T item, int position);

    // Default-метод (Java 8+)
    default void onItemLongClick(T item, int position) {
        System.out.println("Долгое нажатие на элемент: " + position);
    }
}

public interface NetworkSyncable {
    void syncWithCloud();
}

public class NoteItem implements NetworkSyncable {
    private final String content;

    public NoteItem(String content) {
        this.content = content;
    }

    @Override
    public void syncWithCloud() {
        System.out.println("Синхронизация заметки: " + content);
    }
}
```

### Задания для закрепления

1. **Контракт хранилища данных:** Создайте интерфейс `KeyValueStorage` с методами `save(String key, String value)`, `get(String key)`, `clear()`. Реализуйте класс `MemoryStorage`.

```java
import java.util.HashMap;
import java.util.Map;

public interface KeyValueStorage {
    void save(String key, String value);
    String get(String key);
    void clear();
}

class MemoryStorage implements KeyValueStorage {
    private final Map<String, String> data = new HashMap<>();
    public void save(String key, String value) { data.put(key, value); }
    public String get(String key) { return data.get(key); }
    public void clear() { data.clear(); }
}
```

2. **Колбэк загрузки изображения:** Напишите интерфейс `ImageLoadCallback` с методами `onSuccess(String bitmapRef)` и `onError(Throwable error)`.

```java
public interface ImageLoadCallback {
    void onSuccess(String bitmapRef);
    void onError(Throwable error);
}
```

3. **Слушатель жизненного цикла фоновой задачи:** Спроектируйте интерфейс `BackgroundTaskListener` со стандартным методом `onProgress(int percentage)`.

```java
public interface BackgroundTaskListener {
    default void onProgress(int percentage) {
        System.out.println("Прогресс: " + percentage + "%");
    }
}
```

4. **Множественная реализация:** Создайте класс `MediaFile`, реализующий два интерфейса: `Playable` (метод `play()`, `stop()`) и `Shareable` (метод `shareViaBluetooth()`).

```java
interface Playable { void play(); void stop(); }
interface Shareable { void shareViaBluetooth(); }

class MediaFile implements Playable, Shareable {
    public void play() { System.out.println("Воспроизведение"); }
    public void stop() { System.out.println("Остановка"); }
    public void shareViaBluetooth() { System.out.println("Отправка по BT"); }
}
```

5. **Функциональный интерфейс для фильтрации:** Напишите аннотированный `@FunctionalInterface` `PredicateValidator<T>` с методом `boolean validate(T data)` и протестируйте его через лямбда-выражение.

```java
@FunctionalInterface
interface PredicateValidator<T> {
    boolean validate(T data);
}

class PredicateDemo {
    public static void main(String[] args) {
        PredicateValidator<String> notEmpty = s -> s != null && !s.isBlank();
        System.out.println(notEmpty.validate("Java"));
    }
}
```


---

## Тема 8. Коллекции и Generics

Коллекции (`List`, `Set`, `Map`) составляют основу передачи списков в адаптеры UI и сопоставления связей типа "Ключ-Значение".

### Пример кода

```java
import java.util.*;

public class ChatRepository {
    private final Map<String, List<String>> userDialogs = new HashMap<>();

    public void addMessage(String userId, String message) {
        userDialogs.computeIfAbsent(userId, k -> new ArrayList<>()).add(message);
    }

    public List<String> getDialog(String userId) {
        return userDialogs.getOrDefault(userId, Collections.emptyList());
    }

    public static void main(String[] args) {
        ChatRepository repo = new ChatRepository();
        repo.addMessage("user_42", "Привет, заказ доставлен?");
        repo.addMessage("user_42", "Да, спасибо!");

        System.out.println("Сообщения пользователя 42: " + repo.getDialog("user_42"));
    }
}
```

### Задания для закрепления

1. **Удаление дубликатов контактов:** Дан список телефонных номеров с дубликатами. Используя `Set`, верните очищенную от повторов коллекцию с сохранением исходного порядка добавления (`LinkedHashSet`).

```java
import java.util.*;

public class ContactsDeduplicator {
    public static List<String> dedup(List<String> phones) {
        return new ArrayList<>(new LinkedHashSet<>(phones));
    }
    public static void main(String[] args) {
        System.out.println(dedup(Arrays.asList("+7999", "+7888", "+7999", "+7111")));
    }
}
```

2. **Очередь сетевых запросов:** Реализуйте имитацию очереди синхронизации данных `Queue<String>` (FIFO), обрабатывая элементы по мере поступления.

```java
import java.util.*;

public class RequestQueue {
    private final Queue<String> queue = new LinkedList<>();
    public void enqueue(String req) { queue.offer(req); }
    public String process() { return queue.poll(); }
    public boolean isEmpty() { return queue.isEmpty(); }
}
```

3. **Обобщенный ответ API (Generic ApiResponse):** Создайте класс `ApiResponse<T>` с полями `int statusCode`, `T data`, `String errorMessage` и булевым геттером `isSuccessful()`.

```java
public class ApiResponse<T> {
    private final int statusCode;
    private final T data;
    private final String errorMessage;

    public ApiResponse(int statusCode, T data, String errorMessage) {
        this.statusCode = statusCode;
        this.data = data;
        this.errorMessage = errorMessage;
    }

    public boolean isSuccessful() { return statusCode >= 200 && statusCode < 300; }
    public T getData() { return data; }
    public String getErrorMessage() { return errorMessage; }
    public int getStatusCode() { return statusCode; }
}
```

4. **Сортировка товаров по цене и популярности:** Дан `List<Product>`. Отсортируйте коллекцию с помощью `Comparator` сначала по возрастанию цены, а при равной цене — по рейтингу.

```java
import java.util.*;

record Product(String name, double price, double rating) {}

public class ProductSorter {
    public static List<Product> sort(List<Product> products) {
        List<Product> copy = new ArrayList<>(products);
        copy.sort(Comparator.comparingDouble(Product::price)
                            .thenComparing(Comparator.comparingDouble(Product::rating).reversed()));
        return copy;
    }
}
```

5. **Кэш экранов (LRU Cache концепт):** Спроектируйте простейший механизм кэширования последних открытых фрагментов с ограничением максимальной емкости в 5 элементов.

```java
import java.util.*;

public class LruScreenCache {
    private final int capacity;
    private final LinkedHashMap<String, String> cache;

    public LruScreenCache(int capacity) {
        this.capacity = capacity;
        this.cache = new LinkedHashMap<>(16, 0.75f, true) {
            protected boolean removeEldestEntry(Map.Entry<String, String> eldest) {
                return size() > LruScreenCache.this.capacity;
            }
        };
    }

    public void put(String key, String screen) { cache.put(key, screen); }
    public String get(String key) { return cache.get(key); }
}
```


---

## Тема 9. Исключения и безопасность выполнения

Падение мобильного приложения (Crash) недопустимо. Грамотная обработка исключений гарантирует стабильность приложения при потере сети или повреждении входных данных.

### Пример кода

```java
public class ConfigParser {
    public static int parsePort(String portString) {
        try {
            return Integer.parseInt(portString);
        } catch (NumberFormatException e) {
            System.err.println("Ошибка преобразования порта. Установлен порт по умолчанию 8080: " + e.getMessage());
            return 8080;
        } finally {
            System.out.println("Проверка порта завершена.");
        }
    }
}
```

### Задания для закрепления

1. **Пользовательское исключение отсутствия сети:** Создайте проверяемое исключение `NoInternetException` и напишите метод имитации запроса, выбрасывающий его при флаге `hasConnection == false`.

```java
public class NoInternetException extends Exception {
    public NoInternetException(String message) { super(message); }
}

class NetworkClient {
    public static void request(boolean hasConnection) throws NoInternetException {
        if (!hasConnection) throw new NoInternetException("Нет подключения к сети");
        System.out.println("Запрос выполнен");
    }
}
```

2. **Конструкция try-with-resources:** Напишите метод чтения файла локального конфига с автоматическим закрытием потока ввода (`BufferedReader`).

```java
import java.io.*;

public class ConfigReader {
    public static String readConfig(String path) {
        StringBuilder sb = new StringBuilder();
        try (BufferedReader reader = new BufferedReader(new FileReader(path))) {
            String line;
            while ((line = reader.readLine()) != null) sb.append(line).append("\n");
        } catch (IOException e) {
            System.err.println("Ошибка чтения: " + e.getMessage());
        }
        return sb.toString();
    }
}
```

3. **Парсинг JSON-поля возраста:** Реализуйте метод `int parseAge(String ageStr)`, выбрасывающий непроверяемое исключение `InvalidUserDataException`, если возраст отрицательный или превышает 130.

```java
public class InvalidUserDataException extends RuntimeException {
    public InvalidUserDataException(String message) { super(message); }
}

class AgeParser {
    public static int parseAge(String ageStr) {
        int age;
        try { age = Integer.parseInt(ageStr); }
        catch (NumberFormatException e) { throw new InvalidUserDataException("Возраст не число"); }
        if (age < 0 || age > 130) throw new InvalidUserDataException("Возраст вне диапазона");
        return age;
    }
}
```

4. **Множественные блоки catch:** Напишите блок обработки `try-catch`, раздельно обрабатывающий `NullPointerException`, `IndexOutOfBoundsException` и общий `Exception`.

```java
public class MultiCatchDemo {
    public static void safeProcess(String[] data) {
        try {
            System.out.println(data[0].length());
        } catch (NullPointerException e) {
            System.err.println("Null: " + e.getMessage());
        } catch (IndexOutOfBoundsException e) {
            System.err.println("Индекс: " + e.getMessage());
        } catch (Exception e) {
            System.err.println("Общая ошибка: " + e.getMessage());
        }
    }
}
```

5. **Безопасное извлечение значения из Bundle:** Напишите утилитный метод, безопасно читающий строковое значение по ключу, возвращающий значение по умолчанию при возникновении любой ошибки.

```java
class Bundle {
    private final java.util.Map<String, String> data = new java.util.HashMap<>();
    public void putString(String key, String value) { data.put(key, value); }
    public String getString(String key) { return data.get(key); }
}

public class BundleHelper {
    public static String safeGetString(Bundle bundle, String key, String defaultValue) {
        try {
            String value = bundle.getString(key);
            return value != null ? value : defaultValue;
        } catch (Exception e) {
            return defaultValue;
        }
    }
}
```


---

## Практические задания:

Каждое задание представляет собой фрагмент реальной логики мобильного приложения (Android UI-стейт, работа с хранилищем, сетью, сенсорами и валидацией).

### Блок 1: Авторизация, безопасность и валидация ввода

1. **Валидатор надежности пароля:** Проверьте пароль на соответствие критериям: минимум 8 символов, хотя бы одна заглавная буква, одна цифра и один специальный знак (`!@#$%^&*`).

```java
public class PasswordValidator {
    public static boolean isStrong(String pwd) {
        if (pwd == null || pwd.length() < 8) return false;
        boolean upper = false, digit = false, special = false;
        String specials = "!@#$%^&*";
        for (char c : pwd.toCharArray()) {
            if (Character.isUpperCase(c)) upper = true;
            else if (Character.isDigit(c)) digit = true;
            else if (specials.indexOf(c) >= 0) special = true;
        }
        return upper && digit && special;
    }
}
```

2. **Нормализатор телефонных номеров:** Принимайте строку в произвольном формате (например, `8 (999) 000-11-22` или `+7 999 000 11 22`) и приводите её к строгому стандарту E.164 (`+79990001122`).

```java
public class PhoneNormalizer {
    public static String toE164(String raw) {
        String digits = raw.replaceAll("[^0-9]", "");
        if (digits.startsWith("8") && digits.length() == 11) digits = "7" + digits.substring(1);
        if (!digits.startsWith("7")) digits = "7" + digits;
        return "+" + digits;
    }
    public static void main(String[] args) {
        System.out.println(toE164("8 (999) 000-11-22"));   // +79990001122
        System.out.println(toE164("+7 999 000 11 22"));    // +79990001122
    }
}
```

3. **Генератор одноразового SMS-кода:** Напишите метод генерации 6-значного числового OTP-кода.

```java
import java.security.SecureRandom;

public class OtpGenerator {
    private static final SecureRandom RND = new SecureRandom();
    public static String generate() {
        return String.format("%06d", RND.nextInt(1_000_000));
    }
}
```

4. **Проверка срока действия JWT-токена:** Дан Unix-таймстемп истечения токена в секундах. Определите, активен ли токен относительно текущего системного времени.

```java
public class JwtExpiry {
    public static boolean isActive(long expSeconds) {
        long nowSeconds = System.currentTimeMillis() / 1000;
        return expSeconds > nowSeconds;
    }
}
```

5. **Маскировка персональных данных (PII):** Замаскируйте адрес электронной почты: `alexander.ivanov@mail.ru` -> `a***********v@mail.ru`.

```java
public class EmailMasker {
    public static String mask(String email) {
        int at = email.indexOf('@');
        if (at <= 0) return email;
        String name = email.substring(0, at);
        if (name.length() <= 2) return "*".repeat(name.length()) + email.substring(at);
        return name.charAt(0) + "*".repeat(name.length() - 2) + name.charAt(name.length() - 1) + email.substring(at);
    }
    public static void main(String[] args) {
        System.out.println(mask("alexander.ivanov@mail.ru"));
    }
}
```

6. **Блокировщик брутфорса:** Реализуйте класс `LoginThrottler`, который после 5 неудачных попыток ввода пароля блокирует ввод на 60 секунд.

```java
public class LoginThrottler {
    private int failedAttempts = 0;
    private long blockedUntil = 0;
    private static final int MAX_ATTEMPTS = 5;
    private static final long BLOCK_MS = 60_000;

    public boolean canAttempt() {
        return System.currentTimeMillis() >= blockedUntil;
    }

    public void onFailedAttempt() {
        failedAttempts++;
        if (failedAttempts >= MAX_ATTEMPTS) {
            blockedUntil = System.currentTimeMillis() + BLOCK_MS;
            failedAttempts = 0;
        }
    }

    public void onSuccess() { failedAttempts = 0; }
}
```

7. **Шифратор перестановкой для локальных заметок:** Напишите простейший обратимый шифратор строк для сокрытия черновиков в локальной базе.

```java
public class ColumnarCipher {
    public static String encrypt(String text, int key) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < text.length(); i++) sb.append((char) (text.charAt(i) + key));
        return sb.toString();
    }
    public static String decrypt(String cipher, int key) {
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < cipher.length(); i++) sb.append((char) (cipher.charAt(i) - key));
        return sb.toString();
    }
}
```

8. **Проверка биометрической готовности:** Напишите логику оценки возможности аутентификации по отпечатку пальца на основе набора булевых флагов оборудования и разрешений.

```java
public class BiometricCheck {
    public static boolean canAuthenticate(boolean hasHardware, boolean hasEnrolled,
                                          boolean permissionGranted, boolean notLocked) {
        return hasHardware && hasEnrolled && permissionGranted && notLocked;
    }
}
```

9. **Контроль сессии по тайм-ауту:** Реализуйте сброс состояния экрана в случае неактивности пользователя более 3 минут.

```java
public class SessionTimeoutController {
    private long lastActivity = System.currentTimeMillis();
    private static final long TIMEOUT = 3 * 60 * 1000;

    public void onUserActivity() { lastActivity = System.currentTimeMillis(); }
    public boolean shouldReset() { return System.currentTimeMillis() - lastActivity > TIMEOUT; }
}
```

10. **Валидатор промокода:** Проверьте правильность промокода по маске: 4 заглавные латинские буквы, дефис, 4 цифры (например, `SALE-2026`).

```java
public class PromoCodeValidator {
    public static boolean isValid(String code) {
        return code != null && code.matches("[A-Z]{4}-\\d{4}");
    }
    public static void main(String[] args) {
        System.out.println(isValid("SALE-2026"));  // true
        System.out.println(isValid("sale-2026"));  // false
    }
}
```


### Блок 2: Работа со списками, каталогами и кэшем (RecyclerView Logic)

11. **DiffUtil-компаратор элементов списка:** Напишите метод, принимающий старый и новый списки элементов `NewsItem` и возвращающий список ID измененных и удаленных позиций.

```java
import java.util.*;

record NewsItem(long id, String title) {}

public class DiffComparator {
    public static List<Long> findChanges(List<NewsItem> oldList, List<NewsItem> newList) {
        Set<Long> oldIds = new HashSet<>();
        for (NewsItem i : oldList) oldIds.add(i.id());
        Set<Long> newIds = new HashSet<>();
        for (NewsItem i : newList) newIds.add(i.id());

        List<Long> changed = new ArrayList<>();
        for (NewsItem i : newList) if (!oldIds.contains(i.id())) changed.add(i.id());
        for (NewsItem i : oldList) if (!newIds.contains(i.id())) changed.add(i.id());
        return changed;
    }
}
```

12. **Пагинация ленты новостей:** Напишите класс `PaginationHelper`, который по номеру страницы `page` и размеру страницы `pageSize = 20` извлекает нужный срез из общего массива новостей.

```java
import java.util.*;

public class PaginationHelper<T> {
    private final List<T> source;
    private final int pageSize;

    public PaginationHelper(List<T> source, int pageSize) {
        this.source = source;
        this.pageSize = pageSize;
    }

    public List<T> getPage(int page) {
        int from = page * pageSize;
        if (from >= source.size()) return Collections.emptyList();
        int to = Math.min(from + pageSize, source.size());
        return source.subList(from, to);
    }
}
```

13. **Группировка контактов по первой букве:** Дан список имен. Сгруппируйте их в `Map<Character, List<String>>` для отображения заголовков секций.

```java
import java.util.*;
import java.util.stream.Collectors;

public class ContactGrouper {
    public static Map<Character, List<String>> group(List<String> names) {
        return names.stream().collect(Collectors.groupingBy(
            n -> Character.toUpperCase(n.charAt(0)),
            TreeMap::new,
            Collectors.toList()
        ));
    }
}
```

14. **Полнотекстовый фильтр списка:** Реализуйте метод фильтрации каталога товаров по вхождению подстроки в наименование или артикул без учета регистра.

```java
import java.util.*;
import java.util.stream.Collectors;

record Product(String name, String sku) {}

public class CatalogFilter {
    public static List<Product> filter(List<Product> catalog, String query) {
        String q = query.toLowerCase();
        return catalog.stream()
            .filter(p -> p.name().toLowerCase().contains(q) || p.sku().toLowerCase().contains(q))
            .collect(Collectors.toList());
    }
}
```

15. **Карусель промо-баннеров:** Реализуйте циклическое получение следующего элемента баннера по индексу текущего клика.

```java
public class BannerCarousel {
    private final int size;
    private int current = 0;

    public BannerCarousel(int size) { this.size = size; }
    public int next() {
        current = (current + 1) % size;
        return current;
    }
}
```

16. **Подсчет суммарной стоимости корзины с учетом промокода:** Рассчитайте сумму позиций с учетом скидок на отдельные категории товаров.

```java
import java.util.*;

record CartLine(String category, double price, int count, double discountPercent) {}

public class CartTotal {
    public static double total(List<CartLine> lines) {
        double sum = 0;
        for (CartLine l : lines) {
            double lineSum = l.price() * l.count();
            sum += lineSum * (1 - l.discountPercent() / 100.0);
        }
        return sum;
    }
}
```

17. **Удаление свайпом с возможностью отмены (Undo):** Спроектируйте логику временного буфера удаленного элемента списка с таймером фиксации удаления.

```java
import java.util.*;

public class UndoDeleteBuffer<T> {
    private T deleted;
    private long deletedAt;
    private static final long UNDO_WINDOW = 5000;

    public void delete(T item) { deleted = item; deletedAt = System.currentTimeMillis(); }
    public Optional<T> undo() {
        if (deleted != null && System.currentTimeMillis() - deletedAt <= UNDO_WINDOW) {
            T item = deleted;
            deleted = null;
            return Optional.of(item);
        }
        return Optional.empty();
    }
}
```

18. **Сортировка чатов по времени последнего сообщения:** Отсортируйте список диалогов так, чтобы сверху оказались диалоги с самыми свежими сообщениями.

```java
import java.util.*;

record Chat(String name, long lastMessageTime) {}

public class ChatSorter {
    public static List<Chat> sort(List<Chat> chats) {
        List<Chat> copy = new ArrayList<>(chats);
        copy.sort(Comparator.comparingLong(Chat::lastMessageTime).reversed());
        return copy;
    }
}
```

19. **Поиск дубликатов в галерее по контрольной сумме:** Напишите алгоритм поиска повторяющихся файлов по размеру и имени.

```java
import java.util.*;

record MediaFile(String name, long size) {}

public class DuplicateFinder {
    public static Map<String, List<MediaFile>> findDuplicates(List<MediaFile> files) {
        Map<String, List<MediaFile>> map = new HashMap<>();
        for (MediaFile f : files) {
            String key = f.name() + "|" + f.size();
            map.computeIfAbsent(key, k -> new ArrayList<>()).add(f);
        }
        map.entrySet().removeIf(e -> e.getValue().size() < 2);
        return map;
    }
}
```

20. **Ограничитель емкости кэша картинок:** Реализуйте стратегию вытеснения самого старого файла (FIFO), если суммарный объем картинок превысил 100 МБ.

```java
import java.util.*;

public class FifoImageCache {
    private final long maxBytes;
    private long currentBytes = 0;
    private final LinkedHashMap<String, Long> files = new LinkedHashMap<>();

    public FifoImageCache(long maxBytes) { this.maxBytes = maxBytes; }

    public void add(String name, long size) {
        files.put(name, size);
        currentBytes += size;
        Iterator<Map.Entry<String, Long>> it = files.entrySet().iterator();
        while (currentBytes > maxBytes && it.hasNext()) {
            Map.Entry<String, Long> eldest = it.next();
            currentBytes -= eldest.getValue();
            it.remove();
        }
    }

    public int size() { return files.size(); }
}
```


### Блок 3: Сеть, парсинг данных и офлайн-синхронизация

21. **Парсер параметров диплинка (Deep Link):** Извлеките параметры маршрутизации из URL вида `app://shop/product?id=452&source=push`.

```java
import java.util.*;

public class DeepLinkParser {
    public static Map<String, String> parse(String url) {
        Map<String, String> params = new HashMap<>();
        int q = url.indexOf('?');
        if (q < 0) return params;
        String[] pairs = url.substring(q + 1).split("&");
        for (String pair : pairs) {
            String[] kv = pair.split("=");
            if (kv.length == 2) params.put(kv[0], kv[1]);
        }
        return params;
    }
    public static void main(String[] args) {
        System.out.println(parse("app://shop/product?id=452&source=push"));
    }
}
```

22. **Симулятор Retry-политики запроса:** Смоделируйте выполнение сетевого вызова с 3 повторами при возникновении `SocketTimeoutException`.

```java
public class RetryPolicy {
    public static void executeWithRetry(Runnable task, int maxRetries) {
        int attempt = 0;
        while (true) {
            try {
                task.run();
                return;
            } catch (RuntimeException e) {
                attempt++;
                if (attempt >= maxRetries) {
                    System.err.println("Все попытки исчерпаны: " + e.getMessage());
                    return;
                }
                System.out.println("Повтор #" + attempt);
            }
        }
    }
}
```

23. **Очередь отложенных офлайн-действий:** Создайте класс `OfflineActionQueue`, сохраняющий лайки и комментарии при отсутствии сети и отправляющий их пачкой при подключении.

```java
import java.util.*;

public class OfflineActionQueue {
    private final Queue<Runnable> actions = new LinkedList<>();

    public void enqueue(Runnable action) { actions.offer(action); }

    public void flush() {
        System.out.println("Отправка " + actions.size() + " отложенных действий");
        while (!actions.isEmpty()) actions.poll().run();
    }
}
```

24. **Слияние локальных данных с сервером (Conflict Resolver):** Напишите резолвер конфликта версии заметки: если серверная версия новее локальной, обновлять локальную, иначе отправлять запрос на перезапись.

```java
record Note(String id, String text, long version) {}

public class ConflictResolver {
    public static Note resolve(Note local, Note server) {
        return server.version() > local.version() ? server : local;
    }
}
```

25. **Оценка скорости скачивания файла:** По количеству байт и времени в миллисекундах рассчитайте скорость передачи в КБ/с и Мбит/с.

```java
public class DownloadSpeed {
    public static void report(long bytes, long millis) {
        if (millis <= 0) return;
        double kbPerSec = (bytes / 1024.0) / (millis / 1000.0);
        double mbitPerSec = (bytes * 8.0) / (millis / 1000.0) / 1_000_000;
        System.out.printf("%.2f KB/s, %.2f Мбит/с%n", kbPerSec, mbitPerSec);
    }
}
```

26. **Парсинг заголовков пагинации сервера:** Извлеките из заголовка `Link: <https://api.com/items?page=3>; rel="next"` номер следующей страницы.

```java
import java.util.regex.*;

public class LinkHeaderParser {
    public static int nextPage(String header) {
        Matcher m = Pattern.compile("page=(\\d+)").matcher(header);
        return m.find() ? Integer.parseInt(m.group(1)) : -1;
    }
    public static void main(String[] args) {
        System.out.println(nextPage("<https://api.com/items?page=3>; rel=\"next\"")); // 3
    }
}
```

27. **Проверка актуальности кэша по ETag:** Реализуйте логику: если переданный `ETag` совпадает с сохраненным, возвращать ответ `304 Not Modified` без загрузки тела.

```java
public class ETagCache {
    public static boolean isNotModified(String requestEtag, String cachedEtag) {
        return requestEtag != null && requestEtag.equals(cachedEtag);
    }
}
```

28. **Форматирование байтов в читаемый вид:** Переведите число байт в строку: `1024` -> `"1.0 KB"`, `1048576` -> `"1.0 MB"`, `1536` -> `"1.5 KB"`.

```java
public class ByteFormatter {
    public static String format(long bytes) {
        if (bytes < 1024) return bytes + " B";
        double kb = bytes / 1024.0;
        if (kb < 1024) return String.format("%.1f KB", kb);
        double mb = kb / 1024.0;
        if (mb < 1024) return String.format("%.1f MB", mb);
        return String.format("%.1f GB", mb / 1024.0);
    }
    public static void main(String[] args) {
        System.out.println(format(1024));
        System.out.println(format(1536));
        System.out.println(format(1048576));
    }
}
```

29. **Имитация веб-сокета для биржевого виджета:** Напишите интерфейс слушателя котировок валют и генератор событий с интервалом.

```java
import java.util.*;
import java.util.function.Consumer;

interface QuoteListener { void onQuote(String symbol, double price); }

class StockFeed {
    private final List<QuoteListener> listeners = new ArrayList<>();
    private final Random rnd = new Random();

    public void subscribe(QuoteListener l) { listeners.add(l); }

    public void start(String symbol) {
        double price = 100;
        for (int i = 0; i < 5; i++) {
            price += rnd.nextDouble() * 2 - 1;
            for (QuoteListener l : listeners) l.onQuote(symbol, price);
        }
    }
}
```

30. **Валидатор ответа API:** Напишите метод проверки обязательных полей JSON-объекта профиля на `null`.

```java
import java.util.*;

public class ApiResponseValidator {
    public static boolean hasRequiredFields(Map<String, Object> json, String... required) {
        for (String field : required) {
            if (json.get(field) == null) return false;
        }
        return true;
    }
}
```


### Блок 4: Управление состоянием экрана и UI-архитектура

31. **Конечный автомат экрана загрузки (UI State MVI):** Реализуйте переключение состояний экрана через `sealed`-иерархию или `enum`: `Loading`, `Success(data)`, `Empty`, `Error(message)`.

```java
public sealed interface UiState permits UiState.Loading, UiState.Success, UiState.Empty, UiState.Error {
    record Loading() implements UiState {}
    record Success(Object data) implements UiState {}
    record Empty() implements UiState {}
    record Error(String message) implements UiState {}
}
```

32. **Дебаунсер кликов (Anti-Double Click):** Напишите класс-обертку над кликом, предотвращающий повторный вызов действия, если с момента прошлого клика прошло менее 500 мс.

```java
public class ClickDebouncer {
    private long lastClick = 0;
    private final long threshold;

    public ClickDebouncer(long thresholdMs) { this.threshold = thresholdMs; }

    public boolean canClick() {
        long now = System.currentTimeMillis();
        if (now - lastClick >= threshold) {
            lastClick = now;
            return true;
        }
        return false;
    }
}
```

33. **Стек навигации экранов (BackStack):** Реализуйте собственную структуру данных стека экранов с поддержкой операций `push(Screen)`, `pop()` и `popToRoot()`.

```java
import java.util.*;

public class BackStack<T> {
    private final Deque<T> stack = new ArrayDeque<>();

    public void push(T screen) { stack.push(screen); }
    public T pop() { return stack.poll(); }
    public void popToRoot() {
        while (stack.size() > 1) stack.pop();
    }
    public T current() { return stack.peek(); }
}
```

34. **Моделирование темной и светлой темы:** Спроектируйте класс `ThemePalette`, возвращающий шестнадцатеричные коды цветов (HEX) в зависимости от выбранного режима оформления.

```java
public class ThemePalette {
    public static String background(boolean dark) { return dark ? "#121212" : "#FFFFFF"; }
    public static String text(boolean dark)       { return dark ? "#FFFFFF" : "#000000"; }
    public static String accent(boolean dark)     { return dark ? "#BB86FC" : "#6200EE"; }
}
```

35. **Расчет прогресса заполнения профиля:** Определите процент заполненности аккаунта пользователя (аватар, био, телефон, почта, город).

```java
public class ProfileProgress {
    public static int percent(boolean avatar, boolean bio, boolean phone, boolean email, boolean city) {
        int total = 5;
        int filled = 0;
        if (avatar) filled++;
        if (bio) filled++;
        if (phone) filled++;
        if (email) filled++;
        if (city) filled++;
        return filled * 100 / total;
    }
}
```

36. **Инвертор цвета текста для контрастности:** По заданному цвету фона (RGB) вычислите, какой цвет текста отображать для читаемости — белый или черный (по формуле яркости YIQ).

```java
public class ContrastTextColor {
    public static String textColor(int r, int g, int b) {
        double yiq = (r * 299 + g * 587 + b * 114) / 1000.0;
        return yiq >= 128 ? "#000000" : "#FFFFFF";
    }
}
```

37. **Форматирование счетчика лайков:** Переведите большие числа в сокращения: `950` -> `"950"`, `1200` -> `"1.2K"`, `1500000` -> `"1.5M"`.

```java
public class LikesFormatter {
    public static String format(long count) {
        if (count < 1000) return String.valueOf(count);
        if (count < 1_000_000) return String.format("%.1fK", count / 1000.0);
        return String.format("%.1fM", count / 1_000_000.0);
    }
    public static void main(String[] args) {
        System.out.println(format(950));
        System.out.println(format(1200));
        System.out.println(format(1_500_000));
    }
}
```

38. **Менеджер системных диалогов:** Реализуйте класс очереди показа диалоговых окон, исключающий перекрытие одного всплывающего окна другим.

```java
import java.util.*;

public class DialogManager {
    private final Queue<String> queue = new LinkedList<>();
    private boolean showing = false;

    public void show(String dialog) {
        if (showing) { queue.offer(dialog); return; }
        showing = true;
        System.out.println("Показ: " + dialog);
    }

    public void dismiss() {
        showing = false;
        if (!queue.isEmpty()) show(queue.poll());
    }
}
```

39. **Валидатор состояния кнопки "Оплатить":** Кнопка активна только при условии: корзина не пуста, выбран способ оплаты, адрес доставки подтвержден.

```java
public class PayButtonValidator {
    public static boolean isEnabled(boolean cartNotEmpty, boolean paymentSelected, boolean addressConfirmed) {
        return cartNotEmpty && paymentSelected && addressConfirmed;
    }
}
```

40. **Транслятор ошибок для пользователя:** Напишите метод, переводящий системные исключения (`TimeoutException`, `UnknownHostException`) в понятные человекочитаемые подсказки на русском языке.

```java
public class ErrorTranslator {
    public static String translate(Throwable t) {
        String name = t.getClass().getSimpleName();
        return switch (name) {
            case "SocketTimeoutException" -> "Сервер не отвечает. Попробуйте позже.";
            case "UnknownHostException" -> "Нет подключения к интернету.";
            case "ConnectException" -> "Не удалось подключиться к серверу.";
            default -> "Произошла ошибка. Повторите попытку.";
        };
    }
}
```


### Блок 5: Аппаратные функции, геолокация и фоновые задачи

41. **Расчет расстояния между двумя GPS-точками:** Реализуйте формулу гаверсинусов (Haversine formula) для определения дистанции в метрах между координатами пользователя и курьера.

```java
public class Haversine {
    private static final double R = 6_371_000;

    public static double distance(double lat1, double lon1, double lat2, double lon2) {
        double dLat = Math.toRadians(lat2 - lat1);
        double dLon = Math.toRadians(lon2 - lon1);
        double a = Math.sin(dLat / 2) * Math.sin(dLat / 2)
                 + Math.cos(Math.toRadians(lat1)) * Math.cos(Math.toRadians(lat2))
                 * Math.sin(dLon / 2) * Math.sin(dLon / 2);
        return R * 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
    }
}
```

42. **Определитель вхождения в геозону (Geofencing):** Напишите метод, определяющий, находится ли точка с координатами `(lat, lon)` внутри окружности радиуса `R` с центром в `(centerLat, centerLon)`.

```java
public class Geofence {
    public static boolean isInside(double lat, double lon, double cLat, double cLon, double radiusMeters) {
        return Haversine.distance(lat, lon, cLat, cLon) <= radiusMeters;
    }
}
```

43. **Энергосберегающий планировщик геолокации:** Изменяйте частоту опроса датчика GPS в зависимости от уровня заряда батареи: > 50% (каждые 5 сек), 15-50% (каждые 30 сек), < 15% (раз в 5 минут).

```java
public class GpsScheduler {
    public static long intervalMs(int batteryPercent) {
        if (batteryPercent > 50) return 5_000;
        if (batteryPercent >= 15) return 30_000;
        return 5 * 60_000;
    }
}
```

44. **Детектор падения смартфона (акселерометр):** Напишите алгоритм, определяющий состояние невесомости / резкого ускорения по 3 осям `(X, Y, Z)` при превышении критического порога.

```java
public class FallDetector {
    private static final double THRESHOLD = 25.0; // м/с²

    public static boolean isFall(double x, double y, double z) {
        double magnitude = Math.sqrt(x * x + y * y + z * z);
        return magnitude > THRESHOLD;
    }
}
```

45. **Шагомер на основе пиковых амплитуд:** Дан массив показаний вертикального ускорения. Подсчитайте количество совершенных шагов по локальным максимумам выше заданного барьера.

```java
public class StepCounter {
    public static int countSteps(double[] acceleration, double threshold) {
        int steps = 0;
        boolean above = false;
        for (double a : acceleration) {
            if (!above && a > threshold) { above = true; steps++; }
            else if (above && a < threshold * 0.7) above = false;
        }
        return steps;
    }
}
```

46. **Контроллер яркости по датчику освещенности:** Преобразуйте люксы (Lux) внешнего освещения в процент яркости экрана (0-100%) по логарифмической шкале.

```java
public class BrightnessController {
    public static int brightnessFromLux(double lux) {
        if (lux <= 0) return 0;
        double percent = Math.log10(lux + 1) / Math.log10(10_001) * 100;
        return (int) Math.min(100, Math.max(0, percent));
    }
}
```

47. **Планировщик фоновой синхронизации данных:** Проверьте совокупность системных условий для старта тяжелой синхронизации: подключение к Wi-Fi + устройство подключено к зарядному устройству.

```java
public class SyncScheduler {
    public static boolean canSync(boolean isWifi, boolean isCharging) {
        return isWifi && isCharging;
    }
}
```

48. **Монитор расхода мобильного трафика:** Спроектируйте класс, суммирующий трафик раздельно для мобильной сети и Wi-Fi с предупреждением при достижении лимита в 5 ГБ.

```java
public class TrafficMonitor {
    private long mobileBytes = 0;
    private long wifiBytes = 0;
    private static final long LIMIT = 5L * 1024 * 1024 * 1024;

    public void addMobile(long b) {
        mobileBytes += b;
        if (mobileBytes > LIMIT) System.err.println("Превышен лимит мобильного трафика!");
    }

    public void addWifi(long b) { wifiBytes += b; }

    public long getMobileBytes() { return mobileBytes; }
    public long getWifiBytes() { return wifiBytes; }
}
```

49. **Плеер аудиофайлов (State Machine):** Смоделируйте жизненный цикл аудиоплеера: `IDLE` -> `INITIALIZED` -> `PREPARED` -> `PLAYING` -> `PAUSED` -> `STOPPED`. Блокируйте недопустимые переходы.

```java
public class AudioPlayer {
    enum State { IDLE, INITIALIZED, PREPARED, PLAYING, PAUSED, STOPPED }
    private State state = State.IDLE;

    public void initialize() { transition(State.IDLE, State.INITIALIZED); }
    public void prepare()    { transition(State.INITIALIZED, State.PREPARED); }
    public void play()       { transition(State.PREPARED, State.PLAYING); }
    public void pause()      { transition(State.PLAYING, State.PAUSED); }
    public void resume()     { transition(State.PAUSED, State.PLAYING); }
    public void stop()       { transition(State.PLAYING, State.STOPPED); }

    private void transition(State from, State to) {
        if (state != from) throw new IllegalStateException(
            "Недопустимый переход: " + state + " -> " + to);
        state = to;
        System.out.println("Состояние: " + state);
    }
}
```

50. **Логгер крашей приложения:** Реализуйте запись стектрейса ошибки, версии Android, модели смартфона и свободного места на накопителе в форматированный отчет об аварийном завершении.

```java
import java.io.*;
import java.time.*;

public class CrashLogger {
    public static String buildReport(Throwable t, String androidVersion,
                                     String deviceModel, long freeSpaceBytes) {
        StringBuilder sb = new StringBuilder();
        sb.append("=== CRASH REPORT ===\n");
        sb.append("Time: ").append(LocalDateTime.now()).append("\n");
        sb.append("Android: ").append(androidVersion).append("\n");
        sb.append("Device: ").append(deviceModel).append("\n");
        sb.append("Free space: ").append(freeSpaceBytes / (1024 * 1024)).append(" MB\n");
        sb.append("Exception: ").append(t.getClass().getName()).append(": ")
          .append(t.getMessage()).append("\n");
        sb.append("Stack trace:\n");
        for (StackTraceElement el : t.getStackTrace()) {
            sb.append("  at ").append(el).append("\n");
        }
        return sb.toString();
    }

    public static void writeToFile(Throwable t, String path) {
        try (BufferedWriter w = new BufferedWriter(new FileWriter(path))) {
            w.write(buildReport(t, "Android 14", "Pixel 8", 1_000_000_000L));
        } catch (IOException e) {
            System.err.println("Не удалось записать лог: " + e.getMessage());
        }
    }
}
```


---
