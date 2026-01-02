# Budget-Planner

Консольное backend-приложение для управления личными финансами: доходы/расходы по категориям, бюджеты, статистика, предупреждения, сохранение/загрузка данных, переводы между пользователями.

## Возможности

- Хранение данных в памяти во время работы.
- Несколько пользователей, авторизация по логину/паролю.
- Доходы и расходы с категориями и комментарием.
- Бюджеты на категории расходов + расчёт остатка и предупреждения.
- Статистика: общие суммы и по категориям, фильтрация по периоду.
- Вывод отчёта в терминал или сохранение в файл.
- Сохранение кошелька в файл при выходе и загрузка при входе.
- Переводы между пользователями.

## Требования

- Java 17+
- Maven

## Запуск в IntelliJ IDEA

1. Открыть проект как Maven.
2. Убедиться, что `pom.xml` подтянул зависимости.
3. Запустить:
   `src/main/java/key/project/budgetplanner/app/ConsoleBudgetPlannerApp.java`

## Хранение файлов

Приложение создаёт директорию `data/`:
- `data/users.db` - список пользователей (login:hash)
- `data/wallets/<login>.bin` - сохранённый кошелёк пользователя

## Команды CLI

### Справка
- `help` -список команд
- `exit` - сохранить данные текущего пользователя и выйти

### Авторизация
- `register <login> <password>`
- `login <login> <password>`
- `logout`
- `whoami`

### Операции
- `add-income <amount> <category> [comment]`
- `add-expense <amount> <category> [comment]`

### Бюджеты
- `set-budget <category> <limit>`
- `budgets`

### Статистика
- `stats [from=YYYY-MM-DD] [to=YYYY-MM-DD]`
- `stats-categories <cat1> <cat2> ... [from=YYYY-MM-DD] [to=YYYY-MM-DD]`

Пример:
- `stats from=2025-01-01 to=2025-12-31`
- `stats-categories Еда Такси from=2025-01-01`

### Отчёты
- `report` — вывести полный отчёт
- `report file reports/report.txt` — сохранить отчёт в файл

### Переводы
- `transfer <toLogin> <amount> [comment]`

Перевод фиксируется как:
- расход у отправителя в категории `Переводы`
- доход у получателя в категории `Переводы`

## Пример сценария

```text
register alice 123
register bob 456
login alice 123
add-income 20000 Зарплата
add-income 40000 Зарплата
add-income 3000 Бонус

add-expense 300 Еда
add-expense 500 Еда
add-expense 3000 Развлечения
add-expense 3000 "Коммунальные услуги"
add-expense 1500 Такси

set-budget Еда 4000
set-budget Развлечения 3000
set-budget "Коммунальные услуги" 2500

report
transfer bob 1000 "возврат долга"
exit
```

### Тестирование

Запуск тестов:
mvn test

### Архитектура

Пакеты:

model - сущности (User, Wallet, Transaction, TransactionType)

service -  бизнес-логика (Auth/Session/Finance/Transfer/Report)

storage -  сохранение и загрузка (users.db + wallets/*.bin)

cli - цикл команд и парсинг

util - валидация и хеширование

exception - пользовательские исключения

