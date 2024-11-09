Вот пример структуры базы данных для бота с таблицами `users` и `habits`, которые помогут хранить данные о пользователях и их привычках.

### Структура базы данных

#### Таблица `users`
   - **Описание**: Хранит информацию о каждом зарегистрированном пользователе.
   - **Поля**:
     - `userId` — уникальный идентификатор пользователя (Primary Key).
     - `telegram_id` — ID пользователя в Telegram (для идентификации).
     - `created_at` — дата и время регистрации пользователя.

   ```sql
   CREATE TABLE users (
       userId SERIAL PRIMARY KEY,
       telegram_id BIGINT UNIQUE NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

#### Таблица `habits`
   - **Описание**: Хранит информацию о привычках, добавленных пользователями.
   - **Поля**:
     - `habitId` — уникальный идентификатор привычки (Primary Key).
     - `userId` — ID пользователя, которому принадлежит привычка (Foreign Key, ссылается на `users.id`).
     - `habit_name` — название привычки (например, "заниматься спортом").
     - `created_at` — дата и время добавления привычки.
     - `last_completed` — дата последнего выполнения привычки.
   
   ```sql
   CREATE TABLE habits (
       habitId SERIAL PRIMARY KEY,
       userId INTEGER REFERENCES users(id) ON DELETE CASCADE,
       habit_name VARCHAR(100) NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
       last_completed DATE
   );
   ```

#### Таблица `habit_tracking`
   - **Описание**: Отдельная таблица для записи выполнения привычек. Позволяет отслеживать выполнение привычек по дням.
   - **Поля**:
     - `trackId` — уникальный идентификатор записи (Primary Key).
     - `habitId` — ID привычки (Foreign Key, ссылается на `habits.id`).
     - `date` — дата выполнения привычки.
     - `status` — статус выполнения (например, `TRUE` для выполнено, `FALSE` для не выполнено).

   ```sql
   CREATE TABLE habit_tracking (
       id SERIAL PRIMARY KEY,
       habit_id INTEGER REFERENCES habits(id) ON DELETE CASCADE,
       date DATE NOT NULL,
       status BOOLEAN DEFAULT TRUE
   );
   ```

### Логика работы таблиц

- **Таблица `users`** хранит основную информацию о каждом пользователе.
- **Таблица `habits`** связывает привычки с пользователями и фиксирует дату последнего выполнения.
- **Таблица `habit_tracking`** записывает данные о ежедневном выполнении, что позволяет анализировать статистику привычек за определенные периоды.

Эта структура позволит эффективно хранить и обрабатывать данные о привычках и прогрессе каждого пользователя.
