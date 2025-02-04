- [Title](#title)
- [Status](#status)
- [Context](#context)
- [Decision](#decision)
- [Alternatives](#alternatives)
  - [Использование ВКИ как основу для инфраструктуры](#использование-вки-как-основу-для-инфраструктуры)
  - [Использование управляемого S3-сервиса от Ocean](#использование-управляемого-s3-сервиса-от-ocean)

# Title
Выбор инфраструктуры развертывания

# Status
Proposed

# Context
Мы определились с основными компонентами системы, необходимо принять решение об инфраструктуре на которой будет реализовано развертывание. Одной из целей продукта является становление частью экосистемы МТС и развитие в соответствии с техностратегией МТС. Также заявлено требование о геораспределенности, доступности (на уровне 99%) 

# Decision
Мы будем использовать CloudNative принципы реализации систем рекомендованые техностратегией МТС.

Мы будем использовать управляемый Kubernetes-сервси Ocean для развертывания приложений. Это обеспечит:
1. Минимальную поддержку со стороны команды Поколение-М, что экономит ресурсы
2. Устойчивость к сбоям и автоматическое восстановление, позволяя добиться требуемого уровня доступности
3. Эластичное и динамичное масштабирование ресуров для обработки пиковых нагрузок
4. Контроль выделения и потребления ресурсов, что важно для оценки масштабирования и оптимизации

Мы будем использовать управляемые сервисы для stateful-сервисов (БД, хранилищ), такие как:
1. Ocean PostgreSQL
2. MTSCloud S3
3. MTSCloud CDN
Поскольку это в значительной мере экономит ресурсы на поддержку: обновление, безопасность, развертывание и тп.

Мы будем использовать Платформу наблюдаемости как сервис для сбора и отобажения метрик.

Мы будем использовать Mission Control Center для восстановления от аварий, нештатных ситуаций. 

# Alternatives
Мы рассматривали следующие альтернативы:
1. Использование ВКИ как основу для инфраструктуры
2. Использование управляемого S3-сервиса от Ocean

## Использование ВКИ как основу для инфраструктуры
Мы отказались от этого варианта из-за того что она потребует большего кол-ва трудовых ресурсов. Система Поколение-М на этой стадии не обладает высоконагруженными инфраструктурными компонентами, компетенции по разветывания и поддержке которых было бы целесообразно растить внутри, поэтому облачная инфраструктура Ocean выглядит достаточной.

## Использование управляемого S3-сервиса от Ocean
Мы отказались от использование управляемого S3-сервиса от Ocean поскольку он не предоставляет публичных бакетов с доступом через Internet, которые нам необходимы для того чтобы использовать CDN. Мы могли бы открыть доступ к приватному бакету используя S3-Gateway (по сути авторизующее прокси), но это дополнительная работа, которой хотелось бы избежать.