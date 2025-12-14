flowchart TD
    %% Слой 1: Пользовательский интерфейс и точка входа
    A[Пользователи/Инженеры] -->|HTTPS| B[Веб-Интерфейс<br>(React Host + Micro Frontends)]
    
    %% Слой 2: API Gateway и контур контроля
    B -->|REST API| C[API Gateway<br>(Nginx)<br>* Маршрутизация<br>* Аутентификация<br>* Базовая валидация<br>* Rate Limiting]
    
    %% Связи внутри контура контроля
    C -->|Запрос на запуск| D[Policy Engine<br>(Open Policy Agent - OPA)<br>* Проверка политик]
    C -->|Логи событий| E[CEP Engine<br>(Apache Flink)<br>* Корреляция событий<br>* Выявление паттернов<br>* Генерация инцидентов]
    
    D -->|Контекст для проверки| E
    E -->|Запрос политики| D
    
    %% Слой 3: Ядро платформы и оркестратор
    D -->|Вердикт:<br>Allow/Deny| F[Core Platform<br>(Существующие микросервисы)<br>* Tool Management<br>* Deployment Service]
    E -->|Инцидент| G[Orchestrator<br>(SOAR)<br>* Исполнение сценариев<br>* Изоляция
