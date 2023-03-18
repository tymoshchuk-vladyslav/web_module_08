# Домашнє завдання #8

## Перша частина

* __Вихідні дані:__

У нас є json файл з авторами та їх властивостями: дата та місце народження, короткий опис біографії.

Вміст файлу `authors.json`.

Також ми маємо наступний json файл із цитатами від цих авторів.

Вміст файлу `quotes.json`.

* __Порядок виконання:__

Створіть хмарну базу даних Atlas MongoDB
За допомогою ODM Mongoengine створіть моделі для зберігання даних із цих файлів у колекціях authors та quotes.
Під час зберігання цитат (quotes), поле автора в документі повинно бути не рядковим значенням, а Reference fields полем, де зберігається ObjectID з колекції authors.
Напишіть скрипти для завантаження json файлів у хмарну базу даних.
Реалізуйте скрипт для пошуку цитат за тегом, за ім'ям автора або набором тегів. Скрипт виконується в нескінченному циклі і за допомогою звичайного оператора input приймає команди у наступному форматі команда: значення. Приклад:
name: Steve Martin — знайти та повернути список всіх цитат автора Steve Martin;
tag:life — знайти та повернути список цитат для тега life;
tags:life,live — знайти та повернути список цитат, де є теги life або live (примітка: без пробілів між тегами life, live);
exit — завершити виконання скрипту;
Виведення результатів пошуку лише у форматі utf-8;

* __Додаткове завдання:__

Подумайте та реалізуйте для команд name:Steve Martin та tag:life можливість скороченого запису значень для пошуку, як name:st та tag:li відповідно;
Виконайте кешування результату виконання команд name: та tag: за допомогою Redis, щоб при повторному запиті результат пошуку брався не з MongoDB бази даних, а з кешу;

* ПІДКАЗКА

Для команд name:st та tag:li використовуйте регулярні вирази в String queries

## Друга частина

* __Порядок виконання:__


    docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3.11-management

Напишіть два скрипти: consumer.py та producer.py. Використовуючи RabbitMQ, організуйте за допомогою черг імітацію розсилки email контактам.

Використовуючи ODM Mongoengine, створіть модель для контакту. Модель обов'язково повинна включати поля: повне ім'я, email та логічне поле, яке має значення False за замовчуванням. Воно означає, що повідомлення контакту не надіслано і має стати True, коли буде відправлено. Інші поля для інформаційного навантаження можете придумати самі.

Під час запуску скрипта producer.py він генерує певну кількість фейкових контактів та записує їх у базу даних. Потім поміщає у чергу RabbitMQ повідомлення, яке містить ObjectID створеного контакту, і так для всіх згенерованих контактів.

Скрипт consumer.py отримує з черги RabbitMQ повідомлення, обробляє його та імітує функцією-заглушкою надсилання повідомлення по email. Після надсилання повідомлення необхідно логічне поле для контакту встановити в True. Скрипт працює постійно в очікуванні повідомлень з RabbitMQ.

    WIKI
    Функція-заглушка (англ. stub function) - функція, що не виконує жодної осмисленої дії, що повертає порожній результат або вхідні дані у незмінному вигляді. Те саме, що заглушка методу.

    Заглушка може імітувати поведінку існуючого коду (наприклад, процедури на віддаленому комп'ютері) або бути тимчасовою заміною ще не створеного коду. Наприклад, замість функції, що виконує складні обчислення, можна тимчасово (доки не буде написана сама функція) поставити заглушку, що завжди повертає 1, і налагоджувати інші функції, що залежать від неї.

* __Додаткове завдання:__

Введіть у моделі додаткове поле телефонний номер. Також додайте поле, що відповідає за кращий спосіб надсилання повідомлень — SMS по телефону або email. Нехай producer.py відправляє у різні черги контакти для SMS та email. Створіть два скрипти consumer_sms.py та consumer_email.py, кожен з яких отримує свої контакти та обробляє їх.