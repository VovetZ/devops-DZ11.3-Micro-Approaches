# devops-DZ11.3-Micro-Approaches

 

# Домашнее задание к занятию «Микросервисы: подходы»

 

Вы работаете в крупной компании, которая строит систему на основе микросервисной архитектуры.

Вам как DevOps-специалисту необходимо выдвинуть предложение по организации инфраструктуры для разработки и эксплуатации.

 

 

## Задача 1: Обеспечить разработку

 

Предложите решение для обеспечения процесса разработки: хранение исходного кода, непрерывная интеграция и непрерывная поставка.

Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

 

Решение должно соответствовать следующим требованиям:

- облачная система;

- система контроля версий Git;

- репозиторий на каждый сервис;

- запуск сборки по событию из системы контроля версий;

- запуск сборки по кнопке с указанием параметров;

- возможность привязать настройки к каждой сборке;

- возможность создания шаблонов для различных конфигураций сборок;

- возможность безопасного хранения секретных данных (пароли, ключи доступа);

- несколько конфигураций для сборки из одного репозитория;

- кастомные шаги при сборке;

- собственные докер-образы для сборки проектов;

- возможность развернуть агентов сборки на собственных серверах;

- возможность параллельного запуска нескольких сборок;

- возможность параллельного запуска тестов.

 

Обоснуйте свой выбор.

 

## Ответ

 

Предлагаемое решение для обеспечения процесса разработки - GitLab. По требованиям:

 

- облачная система;

Соответствует. Есть возможность установить его как на собственном сервере, так и в облаке (с дополнительной оплатой за некоторые опции)

- система контроля версий Git;

Соответствует. Поддерживает систему контроля версий Git

- репозиторий на каждый сервис;

Соответствует.

- запуск сборки по событию из системы контроля версий;

Соответствует.

- запуск сборки по кнопке с указанием параметров;

Соответствует. Запуск сборки по кнопке настраивается с указанием параметров настраивается через создание файла gitlab-ci, в котором прописаны параметры такого запуска в качестве переменных.

- возможность привязать настройки к каждой сборке;

Соответствует. Н .gitlab-ci.yml, где можно прописать настройки (инструкции) для каждой сборки.

- возможность создания шаблонов для различных конфигураций сборок;

Соответствует.

- возможность безопасного хранения секретных данных (пароли, ключи доступа);

Соответствует. При загрузке данных на GitLab требуется ввести пароль и логин. Есть возможность использования SSH-ключей для авторизации. Поддерживается двухфакторная аутентификация, интеграция с пользовательскими каталогами (AD/LDAP), гранулярный доступ к объектам в GitLab, поддержка токенов и SSO.

- несколько конфигураций для сборки из одного репозитория;

Соответствует.

- кастомные шаги при сборке;

Соответствует.

- собственные докер-образы для сборки проектов;

Соответствует.Репозиторий контейнеров GitLab дает возможность создавать безопасное хранилище кастомных образов контейнеров Docker. Причем для этого не придется задействовать дополнительные инструменты — возможности скачивания и загрузки образов внедрены в среду управления репозиторием Git по умолчанию.

- возможность развернуть агентов сборки на собственных серверах;

Соответствует.

- возможность параллельного запуска нескольких сборок;

Соответствует. GitLab CI/CD запускает сборку через GitLab Runners (это изолированные виртуальные машины, которые выполняют предопределенные шаги через API GitLab CI). Runner осуществляет процесс тестирования и сборки проекта в среде GitLab по заданной инструкции.  gitlab-runner register можно использовать несколько раз (что позволит запускать параллельные сборки), будет несколько раннеров на одном сервере.

- возможность параллельного запуска тестов.

Соответствует. Runner выполняет тесты при каждом коммите в отдельном Docker контейнере. За счет него реализуется автоматическое тестирование кода.

 

## Задача 2: Логи

 

Предложите решение для обеспечения сбора и анализа логов сервисов в микросервисной архитектуре.

Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

 

Решение должно соответствовать следующим требованиям:

- сбор логов в центральное хранилище со всех хостов, обслуживающих систему;

- минимальные требования к приложениям, сбор логов из stdout;

- гарантированная доставка логов до центрального хранилища;

- обеспечение поиска и фильтрации по записям логов;

- обеспечение пользовательского интерфейса с возможностью предоставления доступа разработчикам для поиска по записям логов;

- возможность дать ссылку на сохранённый поиск по записям логов.

 

Обоснуйте свой выбор.

 

 

## Ответ

 

Самый распространенный вариант - стек ELK (Elasticsearch, Logstash и Kibana).

 

Функциональность ELK позволяет его использовать в качестве централизованного хранилища журналов, агрегатора событий с удобной навигацией, аналитической системы с алгоритмом машинного обучения.

 

Logstash в потоковом режиме работает одновременно со множеством разных источников данных (СУБД, файлы, системные логи, веб-приложения и пр.), фильтруя и преобразуя их для отправки в хранилище ES.

 

NoSQL Elasticsearch позволяет загружать JSON-объекты, которые автоматически индексируются и добавляются в базу поиска. Это позволяет ускорить прототипирование поисковых  решений. Возможность передачи данных журнала в центральную систему. Благодаря наличию встроенных анализаторов текста Elasticsearch автоматически выполняет токенизацию, лемматизацию, стемминг и прочие преобразования текста для решения NLP-задач, связанных с поиском данных.

 

Kibana – визуальный инструмент для Elasticsearch, чтобы взаимодействовать с данными, которые хранятся в индексах ES. Веб-интерфейс Kibana позволяет быстро создавать и обмениваться динамическими панелями мониторинга, включая таблицы, графики и диаграммы, которые отображают изменения в ES-запросах в реальном времени.

 

## Задача 3: Мониторинг

 

Предложите решение для обеспечения сбора и анализа состояния хостов и сервисов в микросервисной архитектуре.

Решение может состоять из одного или нескольких программных продуктов и должно описывать способы и принципы их взаимодействия.

 

Решение должно соответствовать следующим требованиям:

- сбор метрик со всех хостов, обслуживающих систему;

- сбор метрик состояния ресурсов хостов: CPU, RAM, HDD, Network;

- сбор метрик потребляемых ресурсов для каждого сервиса: CPU, RAM, HDD, Network;

- сбор метрик, специфичных для каждого сервиса;

- пользовательский интерфейс с возможностью делать запросы и агрегировать информацию;

- пользовательский интерфейс с возможностью настраивать различные панели для отслеживания состояния системы.

 

Обоснуйте свой выбор.

 

 

## Ответ

 

Мониторинг микросервисов — непростая задача: в режиме реального времени нужно отслеживать как состояние отдельных компонентов, так и состояние системы в целом. Помимо технических показателей зачастую нужно проверять ещё и бизнес показатели.

 

Я бы предложил бы связку из Prometheus + Grafana.

 

Prometheus — open source проект, написана на Golang и Ruby. По сути это один executable файл, который нужно скачать и запустить вместе с компонентами Prometheus. Cовместим с Docker, доступен на Docker Hub. Может быть установлен как в контейнере, так и на физическом сервере или виртуальной машине.

 

Основной задачей является хранение и мониторинг определенных объектов. Примеры: Linux-сервер, сервер NGINX, процесс в ОС, сервер БД, микросервис. Prometheus собирает и хранит метрики в виде временных рядов, т.е. информация о метриках хранится с меткой времени, в которую она была записана.

 

Обычно работает в связке с Grafana, которая предоставляет веб-интерфейс для работы с данными, на основе собранных данных воспроизводит графики. Grafana позволяет пользователям создавать дашборды с панелями, каждая из которых отображает определенные показатели в течение установленного периода времени.

 


 
