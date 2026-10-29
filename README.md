Практична робота 8 — Робота з асинхронним кодом в Dart
Студент: Устич Максим 
Група: alk-43 
Спеціальність: 121 Інженерія програмного забезпечення 
Варіант: 1 — Моніторинг розумної теплиці
Опис
Flutter-застосунок імітує панель моніторингу розумної теплиці.

Реалізовано:
- паралельне початкове завантаження даних;
- FutureBuilder ;
- StreamBuilder ;
- власні StreamController ;
- оновлення температури, вологості та освітленості;
- стани завантаження, помилки, порожніх даних та успішних даних;
- кнопку "Повторити";
- тайм-аут;
- кнопку «Пауза/Продовжити»;
- підсвічування перевищення порога;
- SnackBar для порогових значень;
- коректне закриття таймерів та потоків у dispose() .
Структура проекту
lib/
├── models/
│   ├── greenhouse_summary.dart
│   └── sensor.dart
├── screens/
│   └── greenhouse_screen.dart
├── services/
│   └── fake_greenhouse_api.dart
├── widgets/
│   └── sensor_card.dart
└── main.dart
Future.wait
У lib/services/fake_greenhouse_api.dart початкові дані завантажуються паралельно:
final results = await Future.wait<Object>([
  loadSensors(),
  loadThresholds(),
  loadDailySummary(),
]).timeout(
  const Duration(seconds: 3),
);
FutureBuilder
FutureBuilder<GreenhouseInitialData> використано у lib/screens/greenhouse_screen.dart .

Future створюється в initState() , а чи не в build() .
StreamBuilder
StreamBuilder<SensorReading> використано у lib/widgets/sensor_card.dart .

Кожен датчик має окремий поток показань.
StreamController
Власні StreamController<SensorReading>.broadcast() створені у lib/services/fake_greenhouse_api.dart .

Ошибка окремого датчика передається через addError() і не закриває потік.
Тайм-аут
Початкове завантаження має тайм-аут:
.timeout(const Duration(seconds: 3))
При помилці користувач бачити кнопку «Повторити».
Пауза та продовження
Кнопка "Пауза" зупиняє Timer.periodic , а "Продовжити" знову запускає джерело подій.
Dispose
У dispose() екрана викликається:
_api.dispose();
Метод сервісу скасовує таймер і закриває всі StreamController .
Імітація мережі
Справжні HTTP-запити не використовуються. Затримка імітується через Future.delayed(...) .

У нормальному режимі:
failureRate: 0.15
Для демонстрації помилки тимчасово використовувалось:
failureRate: 1.0
Запуск
flutter pub get
flutter analyze
flutter test
flutter run -d chrome
Скріншоти станів
Завантаження

Помилка

Порожні дані

Дани

Оновлення в реальному часі та перевищення порога

Використані технології
- Flutter
- Dart
- Future
- async/await
- FutureBuilder
- Stream
- StreamBuilder
- StreamController
- Timer
- Future.wait
- timeout
- Git
Декларація використання ШІ
Інструмент: ChatGPT (OpenAI).

Мета використання: допомога зі структурою Flutter-проекту, асинхронною логікою, поясненням вимог, виправленням помилок та підготовкою README.

Приклади запитів:
 «Допоможи виконати практичну роботу 8, варіант 1 – моніторинг розумної теплиці. Потрібні FutureBuilder, StreamBuilder, StreamController, Future.wait, timeout, dispose та стани завантаження/ошибки/порожніх даних». 
"Перевір помилку flutter analyze / flutter test і запропонуй виправлення".

Фрагменти, створені за допомогою ШІ: структура моделей та сервісу, код асинхронного завантаження та потоків, віджети відображення станів, тест та README.

Власне доопрацювання: код запускався та перевірявся у Visual Studio Code; виконано flutter analyze і flutter test ; вручну перевірено оновлення датчиків, стан помилки, порожній стан, перевищення порогів та SnackBar; створено скріншоти для docs/ .