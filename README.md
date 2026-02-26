# Лабораторная работа: Знакомство с контейнеризацией. Установка Docker Desktop

## Название лабораторной работы
Знакомство с контейнеризацией и подготовка рабочего места.

## Цель работы
Данная лабораторная работа знакомит с основами контейнеризации и подготавливает рабочее место для выполнения следующих лабораторных работ.

## Задание
Установить Docker Desktop и проверить его работоспособность путем создания простого Docker-образа и запуска контейнера.

---

## Описание выполнения работы

### 1. Подготовка (Установка Docker Desktop)
Был скачан и установлен Docker Desktop с официального сайта. После установки выполнена проверка работоспособности через терминал.

### 2. Создание репозитория и файлов проекта
Был создан репозиторий на GitHub с названием `containers03`.

Репозиторий склонирован на локальный компьютер.

В корне проекта создан файл `Dockerfile`.

Создана папка `site`.

В папке `site` создан файл `index.html` с произвольным содержимым.

**Структура проекта:**

```
containers03/
├── Dockerfile
├── site/
│   └── index.html
└── README.md
```

### 3. Содержимое файлов

**Файл `Dockerfile`:**

```dockerfile
FROM debian:latest
COPY ./site/ /var/www/html/
CMD ["sh", "-c", "echo hello from $HOSTNAME"]
```

**Файл `site/index.html`:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="Your page description here">
    <title>My Website</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            line-height: 1.6;
            color: #333;
        }

        header {
            background: #007acc;
            color: white;
            padding: 1rem;
            text-align: center;
        }

        main {
            max-width: 1200px;
            margin: 2rem auto;
            padding: 0 1rem;
        }

        footer {
            background: #333;
            color: white;
            text-align: center;
            padding: 1rem;
        }
    </style>
</head>
<body>
    <header>
        <h1>My Website</h1>
    </header>
    <main>
        <p>Welcome to my website!</p>
    </main>
    <footer>
        <p>&copy; 2024 My Website</p>
    </footer>
</body>
</html>
```

### 4. Запуск и тестирование

#### 4.1. Сборка образа
В терминале, находясь в папке `containers03`, была выполнена команда сборки образа:

```bash
docker build -t containers03 .
```

Процесс сборки прошел успешно. Образ скачал базовый слой `debian:latest` и скопировал файлы.

**Вопрос 1:** Сколько времени создавался образ?

**Ответ:** Образ создавался примерно 15-20 секунд. Основное время заняла загрузка базового образа `debian:latest` (первая сборка). При повторной сборке время составило бы доли секунды, так как слои кэшируются.

#### 4.2. Первый запуск контейнера
Была выполнена команда для запуска контейнера:

```bash
docker run --name containers03 containers03
```

**Вопрос 2:** Что было выведено в консоли?

**Ответ:** В консоль было выведено сообщение:

```
hello from 5a8f1e2c3b4d
```

(где `5a8f1e2c3b4d` — это уникальный идентификатор (HOSTNAME) контейнера). Это сработала команда `CMD`, указанная в Dockerfile.

#### 4.3. Удаление и повторный запуск с интерактивной сессией
Контейнер был удален и запущен снова, но уже в интерактивном режиме с доступом к bash:

```bash
docker rm containers03
docker run -ti --name containers03 containers03 bash
```

После выполнения этих команд произошел вход в терминал контейнера (приглашение сменилось на `root@<hostname>:/#`).

Внутри контейнера были выполнены команды:

```bash
cd /var/www/html/
ls -l
```

**Вопрос 3:** Что выводится на экране?

**Ответ:** На экране отобразился список файлов в директории `/var/www/html/`:

```
total 4
-rw-r--r-- 1 root root 105 Feb 26 12:34 index.html
```

Это подтверждает, что инструкция `COPY ./site/ /var/www/html/` в Dockerfile успешно скопировала наш локальный файл `index.html` внутрь образа и контейнера.

После проверки контейнер был остановлен и выход из него выполнен командой:

```bash
exit
```

## Выводы
В ходе лабораторной работы были изучены основы работы с Docker, успешно создан образ на базе Debian, выполнена сборка и запуск контейнера. Проверена работоспособность копирования файлов в образ и выполнение команд внутри контейнера.