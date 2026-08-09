🧠 Пак 1 — Dart (база)
- В чём разница между var, final и const?
    var - ключевое слово для объявления типа переменной. Тип данных определяется компилятором на основе присвоенного значения
    final - ключевое слово, используется для объявления переменных, значение которых можно присвоить только один раз, после чего оно становится иммутабельным во время рантайма
    const - ключевое слово для создания констант времени компиляции. Отличие от final, в том, что вычисляется до запуска программы.
- Что такое null safety и зачем он нужен?
    это механизм, который гарантирует, что переменные не могут содержать значение null (отсутствие значения), если вы явно не разрешите это.
    Нужен для:
    * Предотвращение ошибок времени выполнения (Runtime Errors): Главная цель — исключить критические ошибки NoSuchMethodError или Null check operator used on a null value, * которые возникают, когда программа пытается обратиться к свойству или методу переменной, равной null.
    * Статический анализ при компиляции: Компилятор Dart заранее сообщает об ошибке, если вы используете потенциально null переменную, не проверив её. Это переносит отладку с этапа запуска на этап написания кода.
    * Четкое различие типов: Теперь переменные делятся на:
        Необнуляемые (non-nullable): По умолчанию, например, String name = 'Dart';. Она обязана всегда иметь значение.
        Обнуляемые (nullable): Разрешают null, если поставить знак ?, например, String? name = null;.
    * Производительность: Зная, что переменная гарантированно не null, компилятор Dart может оптимизировать код, так как не нужно генерировать проверки на null в рантайме
- Чем late отличается от обычной переменной?
    это ключевое слово, используемое для отложенной (ленивой) инициализации переменных. Оно позволяет объявить переменную, не присваивая ей значение сразу, и гарантирует компилятору, что значение будет установлено позже, до первого обращения к ней
- Разница между List, Set и Map?
    List - упорядоченный список с индексами, допускает дубликаты 
    Set - неупорядоченная коллекция с уникальными значениями 
    Map - набор ключ-значение для быстрого поиска, ключи уникальны, а значения нет
- Что такое immutable объект?
    Это объект, состояние которого нельзя изменить после создания 
- Как работают named параметры и optional параметры?
    Именованные параметры в Dart заключаются в фигурные скобки {} и вызываются по имени (name: "value"), что делает код читабельным и позволяет менять порядок аргументов. По умолчанию они опциональны, но могут быть обязательными с ключевым словом required. Опциональные позиционные параметры заключаются в квадратные скобки []
- В чём разница между == и identical()?
    == сравнивает значения объектов ( с возможностью переопределения), а identical проверяет указывают ли две ссылки на один и тот же экземпляр в памяти
- Что такое factory constructor?
    специальный тип конструктора, определяемый ключевым словом factory. Не создает новый экземпляр класса автоматически, а управяет логикой создания. Например вернуть синглтон, инициализировать экземпляр класса из json
- Как работает cascade оператор (..)?
    позволяет выполнить последовательность операций над одним объектом, не повторяя имя переменной каждый раз
- Что такое extension methods?
     это инструмент, позволяющий добавлять новый функционал (методы, геттеры, сеттеры, операторы) к уже существующим классам или типам данных, не меняя их исходный код и не используя наследование. Используется в dart как замена множественного наследования
⚙️ Пак 2 — Dart (продвинутый)
- Что такое event loop в Dart?
     это однопоточный механизм, обеспечивающий асинхронность и выполнение задач без блокировки основного потока (main isolate). Он работает непрерывно, обрабатывая синхронный код, а затем — очереди задач
- Разница между Future и Stream?
    Future - это одно асинхронное событие или ошибка, а Stream это последовательность асинхронных событий, поступающих со временем
- Как работает async/await под капотом?
    Ключевое слово async указывает, что функция может работать асинхронно и возвращает Future. Даже если функция не содержит await, она неявно возвращает Future.
    Ключевое слово await используется внутри async функции и приостанавливает выполнение функции до тех пор, пока Future (результат работы другой асинхронной операции) не завершится. Важно: await не блокирует основной поток (isolate) Dart.
- Что такое Isolate и чем он отличается от потоков?
    это независимый поток выполнения, имеющий собственный цикл событий (Event Loop) и изолированную память, не разделяемую с другими изолятами. В отличие от обычных потоков (Threads), изоляты не используют общую память, поэтому работают параллельно, не требуя сложных блокировок (mutex). Они используются для тяжелых вычислений, чтобы не блокировать UI
- Как обрабатываются ошибки в Future?
    В Dart ошибки в асинхронном коде распространяются через Future.

        Способ 1. try/catch с async-await
        Future<void> fetchData() async {
        try {
            final result = await api.getData();
            print(result);
        } catch (e, stackTrace) {
            print('Ошибка: $e');
        }
        }

        При использовании await ошибки выглядят как обычные синхронные исключения.

        Способ 2. catchError()
        api.getData()
        .then((value) {
            print(value);
        })
        .catchError((e) {
            print(e);
        });
        Важный момент
        try {
        api.getData(); // без await
        } catch (e) {
        print(e);
        }

        Ошибка НЕ будет поймана.

        Потому что Future выполняется позже.

        Правильно:

        try {
        await api.getData();
        } catch (e) {}

- Что такое Zone в Dart?

    Zone — это контекст выполнения асинхронного кода.

    Можно представить как "обёртку" над event loop.

    Zone позволяет:

    - перехватывать ошибки
    - переопределять print
    - хранить контекст
    - логировать операции

    Пример
    runZonedGuarded(() {
    throw Exception('Crash');
    }, (error, stack) {
    print('Caught: $error');
    });

    Используется для глобального перехвата ошибок приложения.

    Во Flutter

    Обычно:

    runZonedGuarded(() {
    runApp(MyApp());
    }, (error, stack) {
    FirebaseCrashlytics.instance.recordError(error, stack);
    });

- Разница между microtask queue и event queue?

    У Dart две очереди событий.

    Microtask Queue

    Высокий приоритет.

    scheduleMicrotask(() {
        print('microtask');
    });
    Event Queue

    Обычные события.

    Future(() {
        print('event');
    });

    Порядок выполнения
    Future(() => print('event'));

    scheduleMicrotask(() => print('microtask'));

    print('sync');

    Результат:

    sync
    microtask
    event

    Когда используется microtask
    - Future.then()
    - async/await продолжение выполнения
    - внутренние механизмы Dart

- Когда использовать StreamController?

    Когда нужно самостоятельно управлять потоком данных.

    Пример
    final controller = StreamController<int>();

    controller.stream.listen((value) {
    print(value);
    });

    controller.add(1);
    controller.add(2);

    Использовать когда

    ✔ события сокета

    ✔ websocket

    ✔ BLE

    ✔ GPS

    ✔ собственная event-система

    Не использовать когда

    Если данные уже приходят через готовый Stream.

    Например:

    FirebaseFirestore.instance
        .collection('users')
        .snapshots();

    Тут StreamController не нужен.

- Что такое sync и async**?

    Это генераторы последовательностей.

    sync*

    Возвращает Iterable.

    Iterable<int> numbers() sync* {
        yield 1;
        yield 2;
        yield 3;
    }

    Использование:

    for (final n in numbers()) {
    print(n);
    }
    
    async*

    Возвращает Stream.

    Stream<int> numbers() async* {
        yield 1;

        await Future.delayed(
            Duration(seconds: 1),
        );

        yield 2;
    }

    yield - Отправляет элемент наружу.

    yield* - Делегирует другой Stream/Iterable.

    Stream<int> all() async* {
        yield* numbers();
    }

- Как работает Garbage Collector в Dart?

    Dart использует автоматическое управление памятью.

    Разработчик не освобождает память вручную.

    Основная идея

    Объект удаляется, если на него больше нет ссылок.

    var user = User();

    user = null;

    Старый объект становится кандидатом на удаление.

    Generational GC

    Dart делит память на поколения.

    Young Generation Новые объекты.

    Большинство объектов живут очень мало.

    Например:

    Widget build() {
    return Text('Hello');
    }

    Old Generation Долгоживущие объекты.

    Например:

    class UserRepository {}
    Почему это быстро

    Большинство временных объектов очищаются очень дешево.

    Что может вызвать утечки памяти

    GC не спасёт если ссылки остаются живыми.

    Пример:

    static List users = [];

    users.add(BigObject());

    Пока объект в списке — GC его не удалит.

📱 Пак 3 — Flutter (база)
- Что такое Widget?

    Widget — это описание части интерфейса.

    Очень важно понимать:

    Widget — это НЕ элемент UI на экране.

    Widget — это просто конфигурация будущего UI.

    Например:

    Text('Hello')

    Это не текст на экране.

    Это объект:

    class Text extends StatelessWidget

    который описывает:

    - какой текст показывать
    - какой шрифт использовать
    - какой цвет использовать
    Аналогия: Widget можно представить как чертёж дома.
    Сам дом строится позже системой рендеринга.

- Разница между StatelessWidget и StatefulWidget?

    StatelessWidget: Не хранит изменяемое состояние.
    После создания: виджет сам себя изменить не может.

    StatefullWidget: Имеет изменяемое состояние.

    Widget неизменяемый.

    State изменяемый.

    Это важнейшая концепция Flutter.

- Что такое BuildContext?

    Это ссылка на место виджета в дереве.
    Через context Flutter понимает:
    -где находится виджет
    -кто его родитель
    -какие зависимости доступны

- Как работает метод build()?

    Когда вызывается build():
    -setState()
    -изменение родителя
    -смена темы
    -изменение локали
    -изменение MediaQuery
    -hot reload

    Типичный вопрос: Вызывается ли build при каждом кадре?
    Нет. Только когда элемент помечен как dirty.

- Что такое дерево виджетов (widget tree)?

    Widget Tree — дерево всех виджетов приложения.
    Widget Tree пересоздаётся очень часто. Это нормально. Виджеты лёгкие объекты.
    Flutter каждый раз пересоздает весь экран?
    - Нет. Создаются новые Widget-объекты. Но Flutter сравнивает деревья и обновляет только нужные части.

- Разница между hot reload и hot restart?
    hot reload возможно только в дебаг режиме, не перезапускает main и не изменяет состояние
    hot restart перезапускает main и пересоздает состояние
- Что такое pubspec.yaml?
- Как работает навигация в Flutter?
- Что такое Scaffold?
- Как обрабатываются нажатия (GestureDetector vs InkWell)?
🏗 Пак 4 — Flutter (средний уровень)
- Что такое Element tree и Render tree?

    Во флаттер существует три дерева - widget tree, element tree, render tree

    Widget Tree - Это описание UI.
    Element Tree - Это "живые" экземпляры виджетов. Связывает widget и renderobject
    Element хранит:
    -BuildContext
    -состояние дерева
    -ссылки на детей
    -ссылки на RenderObject

    Самое важное:
    -Element Tree живёт долго.
    -Widget Tree пересоздаётся постоянно.
    -Element Tree старается переиспользоваться.

    Render Tree
    Отвечает за:
    - layout
    - размеры
    - позиционирование
    - рисование

- Разница между setState() и другими способами обновления UI?

    setState() - Самый простой механизм.

    Что делает реально: markNeedsBuild()
    То есть помечает Element как dirty.

    Flutter позже вызывает: build()
    Недостаток - Перестраивается весь StatefulWidget.

- Почему нельзя вызывать setState() в build()?

    Что произойдёт?

    build()
    ↓
    setState()
    ↓
    build()
    ↓
    setState()
    ↓
    build()

    Бесконечный цикл.

    Получим ошибку:

    setState() or markNeedsBuild()
    called during build

- Что такое keys и зачем они нужны?

    Keys помогают Flutter идентифицировать виджеты между rebuild.

    Без key Flutter сравнивает:
    -runtimeType
    -позиция в дереве

    С key Flutter использует ключ как идентификатор.
    Пример списка:

    ListView(
    children: [
        TodoItem(key: ValueKey(todo.id))
    ],
    )

    Зачем нужны Keys?
    Когда:
    -список сортируется
    -список фильтруется
    -элементы удаляются
    -элементы перемещаются

    Иначе состояние может "переехать" не туда.

- Разница между GlobalKey и ValueKey?

    ValueKey - Самый популярный. Используется для идентификации элементов.
    ValueKey(user.id)

    ObjectKey - Основан на объекте.
    ObjectKey(user)

    UniqueKey - Всегда уникальный. Каждый rebuild новый.
    UniqueKey()

    GlobalKey - Особый ключ.
    Позволяет получить доступ к:
    -State
    -Context
    -Widget
    из любого места.

    Пример:

    final formKey = GlobalKey<FormState>();
    formKey.currentState?.validate();
    Почему GlobalKey дорогой?

    Flutter вынужден искать элемент по всему дереву.

    Использовать только когда реально нужен доступ к State.

- Как работает layout система Flutter?

    Flutter использует правило:
    1. Constraints go down
    2. Sizes go up
    3. Parent sets position

    Шаг 1.
    Родитель передает ограничения.
    -maxWidth
    -maxHeight
    -minWidth
    -minHeight

    Например:

    Scaffold говорит:
    Ширина = весь экран
    Высота = весь экран

    Шаг 2.
    Ребёнок выбирает размер.

    Например:

    Text('Hello')

    скажет:

    Мне нужно 40px

    Шаг 3.
    Родитель размещает ребёнка.

- Что такое constraints?

    Constraints — ограничения размеров.

    Пример:

    BoxConstraints(
    minWidth: 0,
    maxWidth: 300,
    )

    Flutter всегда работает через constraints.

    Частая ошибка
    Column(
    children: [
        ListView(...)
    ]
    )

    Ошибка:

    Vertical viewport was given
    unbounded height

    Почему?

    Column говорит:

    Высота бесконечна

    ListView отвечает:

    Мне тоже нужна бесконечная высота

    Конфликт.

    Решение:

    Expanded(
    child: ListView(...)
    )

- Как оптимизировать перерисовки?

    1. Использовать const
    const Text('Hello')

    Flutter может переиспользовать объект.

    2. Дробить UI

    Плохо:

    HomePage

    на 1000 строк.

    Хорошо:

    HeaderWidget
    ProfileWidget
    MenuWidget

    3. Локализовать state

    Плохо:

    setState()

    на весь экран.

    Лучше:

    ValueListenableBuilder
    BlocBuilder
    Consumer

    4. Использовать ListView.builder

    Плохо:

    Column(
    children: 1000 items
    )

    Хорошо:

    ListView.builder()

    5. Избегать тяжёлых операций в build()

    Плохо:

    build() {
    users.sort();
    }

    build должен быть максимально дешёвым.

🚀 Пак 5 — Flutter (продвинутый)
- Как работает pipeline рендеринга во Flutter?

    Когда состояние меняется:

    setState(() {
    counter++;
    });

    Flutter НЕ перерисовывает экран сразу.

    Он запускает Rendering Pipeline.

    Общая схема
    setState()
        ↓
    Build
        ↓
    Layout
        ↓
    Paint
        ↓
    Compositing
        ↓
    Rasterization
        ↓
    GPU
    1. Build Phase

    Создаются новые Widget'ы.

    Widget build(BuildContext context) {
    return Text('$counter');
    }

    Flutter сравнивает:

    старое дерево Widget
    новое дерево Widget

    и обновляет Element Tree.

    2. Layout Phase

    Считаются размеры.

    Работает правило:

    Constraints go down
    Sizes go up
    Parent sets position

    Например:

    Container(
    width: 200,
    child: Text('Hello'),
    )

    Container сообщает ограничения ребёнку.

    Ребёнок возвращает размер.

    3. Paint Phase

    Создаются команды рисования.

    Например:

    нарисовать текст
    нарисовать фон
    нарисовать границу

    На этом этапе пиксели ещё не рисуются.

    Созётся Display List.

    4. Compositing Phase

    Flutter решает:

    какие слои можно объединить
    какие нужно рисовать отдельно

    Появляются Layer Objects.

    5. Rasterization

    Работу забирает движок.

    Display List превращается в реальные пиксели.

    6. GPU

    GPU рисует изображение на экране.

- Что происходит при вызове setState()?

    Реально происходит следующее
    setState(() {
    counter++;
    });

    Внутри:

    markNeedsBuild();

    Flutter помечает Element как dirty

    На следующем кадре:

    Scheduler
    ↓
    Build Owner
    ↓
    Rebuild Dirty Elements

    Вызывается build(). Далее Flutter делает diff.

    Старое дерево:
    Column
    Text(1)

    Новое дерево:
    Column
    Text(2)

    Меняется только Text.

- Как работает InheritedWidget?
- Что такое Provider / Riverpod / Bloc (концептуально)?
- Как работает dependency injection во Flutter?

    DI — передача зависимостей извне.

    Плохо:

    class UserRepository {
    }

    class UserBloc {
    final repository = UserRepository();
    }

    Жёсткая связь.

    Хорошо:

    class UserBloc {
    final UserRepository repository;

    UserBloc(this.repository);
    }

    Теперь можно подменять зависимости.

    Популярные решения:

    Provider
    Riverpod
    GetIt
    Injectable
    GetIt

    Очень часто встречается.

    Регистрация:

    getIt.registerSingleton(
    UserRepository(),
    );

    Получение:

    getIt<UserRepository>();

- Что такое RepaintBoundary и когда он нужен?

    По умолчанию Flutter может перерисовывать большое дерево.

    Например:

    Dashboard
    ├─ Chart
    ├─ Header
    ├─ Footer

    Если Chart постоянно меняется:

    Paint
    Paint
    Paint
    Paint

    Можно отделить:

    RepaintBoundary(
    child: ChartWidget(),
    )

    Теперь Flutter создаёт отдельный слой.

    При изменении Chart:

    перерисуется только Chart
    Когда использовать?

    ✔ анимации

    ✔ графики

    ✔ карты

    ✔ камеры

    ✔ сложные кастомные виджеты

    Когда НЕ использовать?

    Везде подряд.

    Каждый RepaintBoundary создаёт дополнительный Layer.

    Слишком много слоёв тоже плохо.

- Как работает Navigator 2.0?

    Navigator 1.0:

    Navigator.push(...)
    Navigator.pop(...)

    Императивный подход.

    Navigator 2.0:

    Декларативный подход.

    Вместо:

    Сделай push

    говорим:

    Вот текущее состояние навигации

    Пример:

    Navigator(
    pages: [
        MaterialPage(child: HomePage()),
        if (showProfile)
        MaterialPage(
            child: ProfilePage(),
        ),
    ],
    )

    Flutter сам вычисляет:

    push
    pop
    replace

    Используется в:

    GoRouter
    Beamer
    AutoRoute

- Что такое Flutter Engine и из чего он состоит?

    Flutter состоит из нескольких частей.

    Framework

    Написан на Dart.

    Это:
    -Widgets
    -Material
    -Cupertino
    -Navigator
    -Animation
    -Engine
    Написан на C++.
    Отвечает за:
    -рендеринг
    -текст
    -события
    -работу с платформой
    
    Embedder
    Связь с Android/iOS.
    Схема:
    Flutter App
        ↓
    Framework
        ↓
    Engine
        ↓
    Embedder
        ↓
    Android / iOS

    Из чего состоит Engine?
    Ключевые компоненты:
    -Skia (или Impeller)
    -Dart VM
    -Text Engine
    -Platform Channels
    -Dart VM
    Выполняет код Dart.

    Impeller
    Новый рендерер Flutter.
    Заменяет Skia на многих платформах.
    Плюсы:
    -меньше лагов
    -меньше shader jank
    -предсказуемый рендеринг

- Как происходит взаимодействие с нативным кодом (Platform Channels)?

    Через Platform Channels.

    Например:

    Flutter хочет узнать заряд батареи.

    Flutter:

    const platform =
        MethodChannel('battery');

    Вызов:

    await platform.invokeMethod(
    'getBatteryLevel',
    );

    Android получает запрос.
    Kotlin:
    MethodChannel(...)
    Возвращает результат.

    Ответ приходит обратно в Flutter.

    Схема:
    Flutter
    ↓
    MethodChannel
    ↓
    Android/iOS
    ↓
    MethodChannel
    ↓
    Flutter

    Типы каналов:
    - MethodChannel Запрос → ответ.
    - EventChannel Поток событий.

    Например:
    GPS
    акселерометр
    BLE
    BasicMessageChannel

    Двусторонний обмен сообщениями.

- Как тестировать Flutter-приложения?

    Unit Test. Тестируем бизнес-логику.
    Пример:
        expect(
        calculator.sum(2, 2),
        4,
        );

    Самые быстрые.

    Widget Test. Тестируем виджет изолированно.

    await tester.pumpWidget(
    MyWidget(),
    );

    Проверяем:
        expect(
        find.text('Hello'),
        findsOneWidget,
        );

    Integration Test. Тестируем всё приложение.

    Пример:
    - открыть экран
    - нажать кнопку
    - получить результат

🔥 Пак 6 — Практика / Архитектура
- Как ты организуешь структуру проекта?

    Плохой вариант
    lib/
    ├─ screens/
    ├─ widgets/
    ├─ models/
    ├─ services/
    └─ utils/

    Пока проект маленький — работает.

    Когда становится 100+ экранов:

    screens/
    user_screen.dart
    user_edit_screen.dart
    user_profile_screen.dart
    user_details_screen.dart

    Начинается хаос.

    Хороший вариант — Feature First

    Я обычно использую структуру по фичам.

    lib/
    ├─ core/
    │
    ├─ features/
    │   ├─ auth/
    │   ├─ profile/
    │   ├─ settings/
    │   └─ orders/
    │
    └─ app/

    Внутри фичи:

    auth/
    ├─ data/
    ├─ domain/
    └─ presentation/
    Пример
    auth/
    ├─ data
    │   ├─ repositories
    │   ├─ models
    │   └─ datasources
    │
    ├─ domain
    │   ├─ entities
    │   ├─ repositories
    │   └─ usecases
    │
    └─ presentation
        ├─ screens
        ├─ widgets
        └─ bloc
    Почему так лучше?

    Фича полностью изолирована.

    Удаляем auth:

    удаляем одну папку

    и проект продолжает работать.

- Какую архитектуру используешь и почему? (MVC, MVVM, Clean Architecture)

    MVC (Model View Controller)
    Старый подход.
    Во Flutter редко используется.

    MVVM (Model View ViewModel)
    Популярен в мобильной разработке.
    Пример:
    UI
    ↓
    ViewModel
    ↓
    Repository

    Clean Architecture
    Слои:
    Presentation
        ↓
    Domain
        ↓
    Data

    Presentation
    UI.
    -Screens
    -Widgets
    -Bloc
    -Provider
    -Riverpod
    
    Domain Бизнес-логика.
    -Entities
    -UseCases
    -Repository Contracts
    
    Data Работа с данными.
    -API
    -Cache
    -Database

    Главное правило - Внешние слои зависят от внутренних.
    Никогда наоборот.

    Пример
    UI
    ↓
    GetUserUseCase
    ↓
    UserRepository
    ↓
    ApiClient

    Для средних и крупных проектов использую Feature First + элементы Clean Architecture. Это позволяет масштабировать приложение, изолировать бизнес-логику, упрощать тестирование и замену источников данных.

- Как обрабатываешь ошибки в приложении?
- Как работаешь с API?
- Как кешируешь данные?
- Как реализуешь авторизацию?
- Как масштабировать приложение?
- Как обеспечиваешь тестируемость?
- Как оптимизируешь производительность?
- Как работаешь с состоянием приложения?
💣 Бонус — Каверзные вопросы
- Почему Flutter быстрый?
- В чём минусы Flutter?
- Когда Flutter — плохой выбор?
- Что лучше: const виджеты или нет и почему?
- Почему иногда UI не обновляется?
- Что будет, если вызвать setState() после dispose()?
- Почему нельзя хранить BuildContext?
- В чём проблема вложенных ListView?
- Как избежать memory leaks?
- Что происходит при hot reload на уровне кода?