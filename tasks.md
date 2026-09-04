# Задачи подготовки проекта Edopp
1
> Статус: ~20% выполнено

---

## 1. Среда разработки

- [x] Убедиться, что установлена Java 17+ (рекомендуется JDK 17 или 21) — **Java 17.0.18**
- [x] Настроить Maven (версия 3.8+) или Gradle — **Maven 3.9.16**
- [x] Установить IDE (IntelliJ IDEA / VS Code) с плагинами для Java и Spring Boot
- [x] Установить Docker Desktop (или другой Docker-движок) для запуска сервисов — **Docker 29.5.2**
- [x] Установить Kubernetes-кластер (kind) для локальной разработки — **kind v0.33.0, кластер "edopp"**

## 2. Инфраструктура (Docker)

### 2.1 Базы данных
- [x] Добавить PostgreSQL 15 в `docker-compose.yml` (уже есть)
- [ ] Добавить MongoDB 7 для хранения документов
- [ ] Добавить Redis 7 для кэширования, Redlock и rate-limiting

### 2.2 Apache Kafka
- [x] Zookeeper (уже есть)
- [x] Kafka Broker (уже есть)
- [x] Confluent Schema Registry (уже есть)
- [ ] Добавить Kafka UI (например, Ecosystem Kafka UI или Redpanda Console)
- [ ] Настроить топикы для producer/consumer

### 2.3 Наблюдаемость
- [ ] Добавить Prometheus для сбора метрик
- [ ] Добавить Grafana для визуализации
- [ ] Добавить Loki для агрегации JSON-логов
- [ ] Добавить Jaeger для трейсинга OpenTelemetry

### 2.4 Итоговый `docker-compose.yml` должен включать:
- [x] PostgreSQL 15
- [ ] MongoDB 7
- [ ] Redis 7
- [x] Zookeeper
- [x] Kafka Broker
- [x] Schema Registry
- [ ] Prometheus
- [ ] Grafana
- [ ] Loki
- [ ] Jaeger

## 3. Зависимости проекта (pom.xml)

### 3.1 Spring Boot / Security
- [x] Обновить Spring Security до версии 6 (совместимой с Spring Boot 3)
- [ ] Добавить Spring Security OAuth2 Resource Server для OAuth 2.1
- [ ] Настроить JWT-авторизацию с refresh-token механизмом

### 3.2 Базы данных
- [ ] Добавить `spring-boot-starter-data-mongodb` для MongoDB
- [ ] Добавить `spring-boot-starter-data-redis` для Redis
- [x] Добавить Spring Data JPA для PostgreSQL (уже есть)

### 3.3 gRPC
- [ ] Добавить плагин `protobuf-maven-plugin` для генерации кода из `.proto` файлов
- [ ] Добавить зависимость `io.grpc:grpc-netty-shaded`
- [ ] Добавить зависимость `io.grpc:grpc-protobuf`
- [ ] Добавить зависимость `io.grpc:grpc-stub`
- [ ] Добавить зависимость `com.google.protobuf:protobuf-java`

### 3.4 Наблюдаемость
- [ ] Добавить `micrometer-registry-prometheus` для экспорта метрик
- [ ] Добавить `opentelemetry-api`, `opentelemetry-sdk`, `opentelemetry-exporter-jaeger`
- [ ] Добавить логгер с поддержкой JSON-формата (Loki)

### 3.5 Безопасность
- [ ] Добавить `owasp-dependency-check` плагин для проверки зависимостей
- [ ] Добавить зависимость для Trivy (через CI/CD)

## 4. Код приложения

### 4.1 Модели и репозитории
- [x] Создать `UserEntity` с JPA-аннотациями
- [x] Создать `UserRepository` с `findByUsername`
- [x] Настроить datasource + hibernate в `application.yml`
- [ ] Создать MongoDB-документы для хранения неструктурированных данных
- [ ] Создать Redis-кэш для часто используемых данных
- [ ] Настроить Redis-кэш через `@Cacheable`, `@CacheEvict`

### 4.2 gRPC-сервисы
- [ ] Создать `.proto` файлы для внутренних RPC-вызовов
- [ ] Сгенерировать Java-классы из proto-файлов
- [ ] Реализовать gRPC-сервер внутри Spring Boot приложения
- [ ] Настроить gRPC-клиент для внутренних вызовов

### 4.3 Kafka
- [x] Зависимость `spring-kafka` в pom.xml
- [x] Зависимость `avro` 1.11.3
- [x] Зависимость `kafka-avro-serializer` 7.6.0
- [x] Настройка producer/consumer в `application.yml`
- [ ] Создать Avro-схемы для событий
- [ ] Реализовать Kafka Producer с Avro-сериализацией через Schema Registry
- [ ] Реализовать Kafka Consumer с Avro-десериализацией
- [ ] Добавить Kafka Streams для обработки потоковых данных (при необходимости)

### 4.4 Безопасность
- [x] `JwtTokenProvider` — генерация, валидация, парсинг токенов
- [x] `JwtAuthenticationFilter` — извлечение JWT из заголовка Authorization
- [x] `SecurityConfig` — CSRF отключён, stateless сессия, JWT-фильтр в цепочке
- [x] `BCryptPasswordEncoder` — настроен как Bean
- [x] `AuthenticationManager` — настроен как Bean
- [ ] Настроить OAuth 2.1 авторизацию (Authorization Code Flow)
- [ ] Реализовать refresh-token механизм
- [ ] Настроить rate-limiting через Redis + Redlock
- [ ] Настроить HTTPS/TLS 1.3

### 4.5 Наблюдаемость
- [ ] Настроить экспорт метрик в Prometheus (`/actuator/prometheus`)
- [ ] Настроить распределённый трейсинг через OpenTelemetry
- [ ] Настроить логирование в JSON-формате для Loki
- [ ] Добавить Grafana-дашборды для мониторинга

## 5. CI/CD (GitHub Actions)

### 5.1 Этап проверки (check)
- [ ] Настроить checkstyle для проверки стиля кода
- [ ] Настроить unit-тесты (JUnit 5 + Mockito)
- [ ] Проверить покрытие кода (coverage ≥ 80%)
- [ ] Запустить Testcontainers для интеграционных тестов с PostgreSQL, Kafka, Redis

### 5.2 Этап сборки
- [ ] Собрать Docker-образ через Docker Buildx (multi-platform build)
- [ ] Прогнать Trivy для сканирования уязвимостей в Docker-образе
- [ ] Прогнать OWASP Dependency-Check для проверки Java-зависимостей

### 5.3 Этап развёртывания
- [ ] Настроить Helm-чарты для развёртывания в Kubernetes (k3d)
- [ ] Настроить sealed-secrets для безопасного хранения секретов
- [ ] Настроить Helm values для staging и production сред

## 6. Kubernetes (k3d / Helm)

- [ ] Создать кластер k3d для локальной разработки
- [ ] Настроить Helm-чарты для приложения
- [ ] Настроить ConfigMap и Secret для конфигурации
- [ ] Настроить Ingress для маршрутизации запросов
- [ ] Настроить Horizontal Pod Autoscaler (HPA)
- [ ] Настроить liveness и readiness probes

## 7. Документация

- [ ] Написать README.md с описанием проекта и инструкциями по запуску
- [ ] Описать API (Swagger/OpenAPI)
- [ ] Описать архитектуру проекта (диаграммы)
- [ ] Описать структуру CI/CD пайплайна

## 8. Финальная проверка

- [ ] Приложение запускается локально через Docker Compose
- [ ] Все интеграционные тесты проходят успешно
- [ ] Покрытие кода ≥ 80%
- [ ] Нет критических уязвимостей (Trivy, OWASP Dependency-Check)
- [ ] Метрики доступны в Prometheus
- [ ] Дашборды настроены в Grafana
- [ ] Трейсинг работает в Jaeger
- [ ] Приложение развёрнуто в k3d через Helm