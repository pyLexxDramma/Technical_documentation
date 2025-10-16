# Billing System

## 1. Назначение модуля

Модуль **Billing System** представляет собой **веб-приложение** для управления биллингом и обработки комментариев на
различных платформах. Он позволяет пользователям **администрировать баланс**, обрабатывать комментарии, управлять
настройками и взаимодействовать с несколькими платформами (VK, Telegram, 2GIS, Yandex).

## 2. Основные компоненты и функции

### 2.1. Управление пользователями и аутентификацией

* **class User(UserMixin)**
  ```python
  class User(UserMixin):
  ```
    * **Описание:** Представляет пользователя системы с минимальным набором методов для **Flask-Login**.

* **load_user(user_id)**
  ```python
  @login_manager.user_loader
  def load_user(user_id)
  ```
    * **Описание:** Загружает пользователя по идентификатору для **Flask-Login**.

* **Маршрут `/ego18/login`**
  ```python
  @app.route('/ego18/login', methods=['GET', 'POST'])
  ```
    * **Описание:** Обрабатывает вход пользователей с проверкой логина и пароля.

* **Маршрут `/ego18/logout`**
  ```python
  @app.route('/ego18/logout')
  ```
    * **Описание:** Осуществляет выход пользователя из системы.

### 2.2. Управление балансом и платежами

* **Маршрут `/ego18/billing/admin`**
  ```python
  @app.route('/ego18/billing/admin', methods=['GET', 'POST'])
  ```
    * **Описание:** Панель администратора для управления балансом и просмотра статистики.

* **`get_current_balance()`**
  ```python
  def get_current_balance()
  ```
    * **Описание:** Получает текущий баланс из базы данных.

* **`check_balance_for_comment()`**
  ```python
  def check_balance_for_comment()
  ```
    * **Описание:** Проверяет, достаточно ли баланса для отправки комментария.

* **`deduct_balance_for_comment()`**
  ```python
  def deduct_balance_for_comment()
  ```
    * **Описание:** Списывает стоимость комментария с баланса.

* **Маршрут `/ego18/billing/balance_history`**
  ```python
  @app.route('/ego18/billing/balance_history')
  ```
    * **Описание:** История изменений баланса.

### 2.3. Обработка комментариев и отзывов

* **`send_comment_via_platform(platform: str, comment_id: str, bot_response: str, **kwargs) -> bool`**
  ```python
  def send_comment_via_platform(platform: str, comment_id: str, bot_response: str, **kwargs) -> bool
  ```
    * **Описание:** Универсальная функция для отправки комментариев через различные платформы.
    * **Параметры:**
        * `platform`: Название платформы.
        * `comment_id`: Идентификатор комментария.
        * `bot_response`: Ответ бота.
        * `**kwargs`: Дополнительные аргументы.

* **Маршрут `/ego18/billing/responded_comments`**
  ```python
  @app.route('/ego18/billing/responded_comments')
  ```
    * **Описание:** Просмотр обработанных комментариев.

* **Маршрут `/ego18/billing/approve_responses`**
  ```python
  @app.route('/ego18/billing/approve_responses')
  ```
    * **Описание:** Одобрение ожидающих ответов.

* **`get_pending_responses(order="desc", limit=20, response_type=None, platform_filter=None, search_term=None)`**
  ```python
  def get_pending_responses(order="desc", limit=20, response_type=None, platform_filter=None, search_term=None)
  ```
    * **Описание:** Получает ожидающие ответы из базы данных.
    * **Параметры:**
        * `order`: Порядок сортировки (по умолчанию: `"desc"`).
        * `limit`: Количество возвращаемых записей (по умолчанию: `20`).
        * `response_type`: Тип ответа (опционально).
        * `platform_filter`: Фильтр по платформе (опционально).
        * `search_term`: Термин для поиска (опционально).

* **Маршрут `/ego18/billing/reject_response/<int:pending_id>`**
  ```python
  @app.route('/ego18/billing/reject_response/<int:pending_id>', methods=['POST'])
  ```
    * **Описание:** Отклонение ожидающего ответа.

* **Маршрут `/ego18/billing/edit_response/<int:pending_id>`**
  ```python
  @app.route('/ego18/billing/edit_response/<int:pending_id>', methods=['POST'])
  ```
    * **Описание:** Редактирование ожидающего ответа.

* **Маршрут `/ego18/billing/approve_response_api`**
  ```python
  @app.route('/ego18/billing/approve_response_api', methods=['POST'])
  ```
    * **Описание:** Одобрение ответа через API.

### 2.4. Экспорт и отчетность

* **Маршрут `/ego18/billing/export_comments`**
  ```python
  @app.route('/ego18/billing/export_comments')
  ```
    * **Описание:** Экспорт комментариев в формате Excel.

* **`highlight_search_term(text, search_term)`**
  ```python
  def highlight_search_term(text, search_term)
  ```
    * **Описание:** Подсвечивает найденные ключевые слова в тексте.

* **`clean_text_for_excel(text, max_length=32000)`**
  ```python
  def clean_text_for_excel(text, max_length=32000)
  ```
    * **Описание:** Очищает текст для корректного отображения в Excel.
    * **Параметры:**
        * `text`: Текст для очистки.
        * `max_length`: Максимальная длина текста (по умолчанию: `32000`).

### 2.5. Настройки и миграция базы данных

* **Маршрут `/ego18/billing/update_settings`**
  ```python
  @app.route('/ego18/billing/update_settings', methods=['POST'])
  ```
    * **Описание:** Обновление настроек системы.

* **`migrate_add_post_id_to_comments()`**
  ```python
  def migrate_add_post_id_to_comments()
  ```
    * **Описание:** Добавляет столбец `post_id` в таблицу `comments`.

* **`migrate_add_date_fields_to_comments()`**
  ```python
  def migrate_add_date_fields_to_comments()
  ```
    * **Описание:** Добавляет поля дат в таблицу `comments`.

* **`migrate_add_username_to_comments()`**
  ```python
  def migrate_add_username_to_comments()
  ```
    * **Описание:** Добавляет столбец `username` в таблицы `comments` и `pending_responses`.

* **`migrate_add_status_to_comments()`**
  ```python
  def migrate_add_status_to_comments()
  ```
    * **Описание:** Добавляет столбец `status` в таблицу `comments`.

* **`migrate_platform_specific_settings()`**
  ```python
  def migrate_platform_specific_settings()
  ```
    * **Описание:** Добавляет платформо-специфичные настройки в базу данных.

### 2.6. Мониторинг и статистика

* **`create_database_indexes()`**
  ```python
  def create_database_indexes()
  ```
    * **Описание:** Создает оптимизированные индексы для улучшенной производительности запросов.

* **`monitor_index_usage()`**
  ```python
  def monitor_index_usage()
  ```
    * **Описание:** Мониторит использование индексов в базе данных.

* **`analyze_query_performance()`**
  ```python
  def analyze_query_performance()
  ```
    * **Описание:** Анализирует производительность основных запросов.

### 2.7. Вспомогательные функции

* **`execute_query(query_func, *args, **kwargs)`**
  ```python
  def execute_query(query_func, *args, **kwargs)
  ```
    * **Описание:** Выполняет запрос напрямую без кэширования.

* **`get_post_text(post_id)`**
  ```python
  def get_post_text(post_id)
  ```
    * **Описание:** Получает текст поста напрямую из базы данных.

* **`fix_null_dates_in_comments()`**
  ```python
  def fix_null_dates_in_comments()
  ```
    * **Описание:** Исправляет записи без дат в таблице `comments`.

## 3. Окружение, необходимое для работы

### Зависимости Python

* **Python:** >= 3.8
* **Flask:** Веб-фреймворк для разработки приложений.
* **Flask-Login:** Расширение для управления аутентификацией.
* **requests:** Библиотека для HTTP-запросов.
* **mysql-connector-python:** Драйвер для работы с MySQL.
* **openpyxl:** Библиотека для работы с Excel файлами.
* **logging:** Стандартная библиотека для ведения журналов.

### Переменные окружения

Переменные окружения загружаются из файла *.env* и включают:

* `MYSQL_HOST`, `MYSQL_USER`, `MYSQL_PASSWORD`, `MYSQL_DB`: Параметры подключения к базе данных.
* `FLASK_RUN_PORT`: Порт для запуска Flask сервера.
* `SECRET_KEY`: Секретный ключ для Flask.

## 4. Связи между модулями и дополнительными модулями

| Компонент | Входящие связи | Исходящие связи |
| :--- | :--- | :--- |
| Billing System Main | Все модули | Пользователи (через API платформ) |
| User Auth | Billing System Main | Flask-Login, Session Data |
| Balance Management | Billing System Main | MySQL Database |
| Comment Handler | Billing System Main, Balance Management, OpenAI API | Yandex API, VK API, Telegram API, 2GIS API |
| Export/Reporting | Billing System Main | Excel Files |
| Logging Module | Billing System Main | Лог-файлы |

---

**Заключение:** **Billing System** представляет собой **многофункциональную платформу** для администрирования биллинга и
обработки комментариев на различных платформах. Он обеспечивает централизованное управление пользователями, балансом,
комментариями и интеграцию с различными API.