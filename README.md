# Secure Store — защищённый интернет-магазин на WordPress + WooCommerce + Docker

Интернет-магазин на базе WordPress и WooCommerce с базовой системой защиты информации, контейнеризацией и HTTPS.

Используемый стек:

* WordPress
* WooCommerce
* Docker
* Nginx
* MariaDB
* Redis

---

# Возможности проекта

* HTTPS через nginx
* Docker-инфраструктура
* WooCommerce интернет-магазин
* Redis object cache
* Двухфакторная аутентификация
* Ограничение попыток входа
* Firewall и базовая защита WordPress
* Журналирование действий пользователей
* Разделение сервисов по контейнерам

---

# Требования

Перед запуском необходимо установить:

| Программа      | Ссылка                                                                                   |
| -------------- | ---------------------------------------------------------------------------------------- |
| Docker Desktop | [Docker Desktop](https://www.docker.com/products/docker-desktop/?utm_source=chatgpt.com) |
| Git            | [Git](https://git-scm.com/downloads?utm_source=chatgpt.com)                              |

---

# Клонирование проекта

```bash
git clone https://github.com/andrfh/Security-store.git
```

---

# Переход в папку проекта

```bash
cd Security-store
```

---

# Структура проекта

```text
Security-store/
│
├── docker-compose.yml
├── .env
│
├── nginx/
│   ├── default.conf
│   └── ssl/
│       ├── cert.pem
│       └── key.pem
│
├── wordpress/
│
└── db/
```

---

# Создание .env файла

В корне проекта создать файл:

```text
.env
```

---

# Содержимое .env

```env
MYSQL_ROOT_PASSWORD=super_secure_root
MYSQL_DATABASE=wordpress
MYSQL_USER=wp_user
MYSQL_PASSWORD=super_secure_password
```

---

# Запуск проекта

В корне проекта выполнить:

```bash
docker compose up -d
```

---

# Проверка контейнеров

```bash
docker ps
```

---

# Должны запуститься контейнеры

* nginx
* wordpress
* mariadb
* redis

---

# Открытие сайта

Перейти в браузере:

```text
https://localhost
```

---

# SSL предупреждение

Так как используется self-signed сертификат, браузер покажет предупреждение безопасности.

Нажать:

```text
Advanced → Continue
```

---

# Установка WordPress

После открытия сайта:

1. Выбрать язык
2. Создать администратора
3. Указать:

   * логин
   * пароль
   * email

Рекомендуется использовать сложный пароль.

---

# Установка WooCommerce

В WordPress admin:

```text
Plugins → Add Plugin
```

Найти и установить:

WooCommerce

---

# Установка плагинов безопасности

---

# 1. WP 2FA

Плагин для двухфакторной аутентификации администраторов.

Установить:

WP 2FA

---

# Настройка WP 2FA

После установки:

```text
WP 2FA → 2FA Policies
```

Настроить:

| Настройка             | Значение      |
| --------------------- | ------------- |
| Enable 2FA            | ON            |
| Apply to roles        | administrator |
| Authentication method | TOTP          |

---

# Подключение приложения

Использовать:

* Google Authenticator
* Aegis
* Authy

Отсканировать QR-код.

---

# 2. WP Activity Log

Плагин для логирования действий пользователей.

Установить:

WP Activity Log

---

# Настройка WP Activity Log

После установки:

```text
WP Activity Log → Enable/Disable Events
```

Включить:

* логирование входов;
* изменения ролей;
* изменения товаров;
* изменения плагинов;
* изменения настроек.

---

# 3. Redis Object Cache

Плагин для объектного кэширования.

Установить:

Redis Object Cache

---

# Настройка Redis

Открыть файл:

```text
wordpress/wp-config.php
```

Добавить:

```php
define('WP_REDIS_HOST', 'redis');
```

---

# Затем в админке WordPress:

```text
Tools → Redis
```

Нажать:

```text
Enable Object Cache
```

---

# 4. Limit Login Attempts Reloaded

Плагин ограничения попыток входа.

Установить:

Limit Login Attempts Reloaded

---

# Настройка Limit Login Attempts

```text
Login Lockout → Settings
```

Рекомендуемые настройки:

| Настройка         | Значение   |
| ----------------- | ---------- |
| Allowed retries   | 3          |
| Lockout time      | 15 minutes |
| Notify on lockout | ON         |

---

# 5. All-In-One Security (AIOS)

Основной security plugin.

Установить:

All-In-One Security

---

# Настройка AIOS

---

# User Accounts

```text
WP Security → User Accounts
```

Включить:

* Detect Default Admin Account
* Password Strength Tool

---

# Login Lockdown

```text
WP Security → User Login
```

Включить:

```text
Enable Login Lockdown
```

Рекомендуемые настройки:

| Настройка          | Значение   |
| ------------------ | ---------- |
| Max login attempts | 3          |
| Retry time         | 5 minutes  |
| Lockout time       | 15 minutes |

---

# Firewall

```text
WP Security → Firewall
```

Включить:

* Basic Firewall Rules
* Additional Firewall Rules
* Pingback Protection
* Disable Trace and Track

---

# File System Security

```text
WP Security → Filesystem Security
```

Включить:

```text
File Change Detection
```

---

# Настройка WordPress безопасности

Открыть файл:

```text
wordpress/wp-config.php
```

---

# Добавить

```php
define('DISALLOW_FILE_EDIT', true);

define('FORCE_SSL_ADMIN', true);

define('WP_AUTO_UPDATE_CORE', true);
```

---

# Что делают настройки

| Настройка           | Назначение                          |
| ------------------- | ----------------------------------- |
| DISALLOW_FILE_EDIT  | отключает встроенный редактор PHP   |
| FORCE_SSL_ADMIN     | включает HTTPS для admin панели     |
| WP_AUTO_UPDATE_CORE | автоматические обновления WordPress |

---

# Проверка Redis

В WordPress:

```text
Tools → Redis
```

Должно отображаться:

```text
Status: Connected
```

---

# Проверка HTTPS

Открыть:

```text
https://localhost
```

В браузере должен использоваться HTTPS.

---

# Проверка Docker

```bash
docker ps
```

---

# Проверка логов

```bash
docker compose logs
```

---

# Остановка проекта

```bash
docker compose down
```

---

# Повторный запуск

```bash
docker compose up -d
```

---

# Реализованные требования

| Требование               | Реализация             |
| ------------------------ | ---------------------- |
| Аутентификация           | WordPress              |
| MFA                      | WP 2FA                 |
| RBAC                     | WordPress Roles        |
| SQL Injection protection | WordPress Database API |
| XSS protection           | WordPress escaping     |
| CSRF protection          | WordPress nonce        |
| Журналирование           | WP Activity Log        |
| Мониторинг               | AIOS                   |
| HTTPS                    | nginx + OpenSSL        |
| Ограничение входа        | Limit Login Attempts   |
| Firewall                 | AIOS                   |
| Производительность       | Redis                  |
| Масштабируемость         | Docker                 |
| Обновления               | WordPress auto updates |
