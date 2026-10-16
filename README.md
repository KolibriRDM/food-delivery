# Food Delivery

Учебный проект на Go: три независимых микросервиса, отдельная инфраструктура и собственный балансировщик каталога.

Этот репозиторий объединяет проекты через **Git submodules**. Каждый проект сохраняет свою историю коммитов, Go-модуль и CI. Общий репозиторий фиксирует версии, которые используются вместе.

| Репозиторий | Назначение |
|---|---|
| [catalog-service](https://github.com/KolibriRDM/catalog-service) | рестораны и меню, HTTP и gRPC, кэш Redis |
| [orders-service](https://github.com/KolibriRDM/orders-service) | заказы, идемпотентность, rate limiter, transactional outbox и публикация в Kafka |
| [notifications-service](https://github.com/KolibriRDM/notifications-service) | обработка событий Kafka, дедупликация, DLQ и хранение уведомлений |
| [infrastructure](https://github.com/KolibriRDM/infrastructure) | Docker Compose, Go-балансировщик, PostgreSQL, Kafka, Redis, мониторинг, логи, аналитика и совместные проверки |

## Клонирование

```sh
git clone --recurse-submodules https://github.com/KolibriRDM/food-delivery.git
cd food-delivery
```

Если репозиторий уже клонирован без submodules:

```sh
git submodule update --init --recursive
```

## Запуск

С установленным Go и работающим Docker Desktop:

```sh
cd infrastructure
cp .env.example .env
make up
make status
```

Перед копированием `.env.example` убедитесь, что у вас ещё нет настроенного `.env`.
Подробные команды, требования и проверки описаны в README каждого проекта. GUI, мониторинг, логи и аналитика включаются отдельными целями Makefile инфраструктуры.

Flutter-клиент находится отдельно и в этот набор репозиториев не входит.

## Обновление рабочего каталога

Чтобы получить версии, зафиксированные в общем репозитории:

```sh
git pull --ff-only
git submodule update --init --recursive
```

Рабочие изменения коммитятся и отправляются из соответствующего вложенного репозитория. Затем в общем репозитории коммитится новая ссылка на его коммит. После клонирования submodules находятся на конкретных коммитах; перед разработкой переключитесь внутри нужного проекта на `main` через `git switch main`.
