# pipelines-php


<img width="1542" height="379" alt="image" src="https://github.com/user-attachments/assets/05dd0042-5e51-4aa8-b1fc-9fda2bae891a" />
# 🐘 pipelines-php

Учебный проект: **CI-пайплайн на GitHub Actions** для PHP-приложения.

---

## 📖 О проекте

Простое PHP-приложение (`getGreeting`), для которого настроен CI-пайплайн.
При каждом `push` и `pull request` в ветки `main` / `master` автоматически:

- 🧹 проверяется синтаксис PHP (`php -l`) и валидность `composer.json`
- ✅ запускаются unit-тесты через **PHPUnit**
- 🐳 собирается **Docker-образ** (multi-stage: `php:8.2-cli` → `php:8.2-cli`) и сохраняется как артефакт

---

## 📂 Структура проекта

```
.
├── .github/
│   └── workflows/
│       └── ci.yml          # GitHub Actions workflow
├── src/
│   └── index.php           # основной код
├── tests/
│   └── test.php            # тесты (PHPUnit)
├── Dockerfile              # multi-stage: builder → runtime
├── .dockerignore
└── README.md
```

---

## ⚙️ Как работает пайплайн

| Job | Шаг | Что делает |
|-----|-----|-----------|
| **lint** | Composer validate | Проверка `composer.json` |
| | Syntax check | `php -l` для всех файлов |
| **test** | Install | `composer install` + `phpunit` |
| | Run tests | `vendor/bin/phpunit tests/` |
| **docker-build** | Buildx | Сборка образа с кэшем GHA |
| | Save + Upload | Сохраняет образ как артефакт (`docker-image.tar.gz`) |
| | Test | `docker run --rm my-php-app:latest` |

---

## 🐳 Проверка локально

### Сборка образа
<img width="724" height="185" alt="image" src="https://github.com/user-attachments/assets/dd603f8e-28ee-4519-903b-02ad35f7674e" />

```bash
docker build -t my-php-app:latest .
```

### Запуск контейнера

```bash
docker run --rm my-php-app:latest
```

Ожидаемый вывод:

```
Hello from PHP in Docker! 🐳
```

---

## 🧪 Тесты локально

```bash
composer install
vendor/bin/phpunit tests/
```

---

## 🏗️ Стек

- **PHP 8.2**
- **Composer** — зависимости
- **PHPUnit** — тесты
- **Docker** — multi-stage сборка
- **GitHub Actions** — CI

---

## 📄 Лицензия

MIT — свободно для учебных целей.
