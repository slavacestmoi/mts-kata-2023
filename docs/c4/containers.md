# Диаграмма контейнеров

![containers](containers.png)
<details>
<summary>Исходник в plantuml</summary>

```plantuml
@startuml
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml

LAYOUT_WITH_LEGEND()

title Диаграмма контейнеров системы Поколение-М

Person(guest, "Гость", "Потенциальный пользователь системы Поколение-М")
Person(student, "Студент", "Проходит обучение используя ресурсы и возможности системы")
Person(teacher, "Преподаватель", "Осуществляет преподавательскую деятельность с помощью системы")
Person(support, "СотрудникПоддержки", "Обеспечивает функционирование, техническую поддержку системы")
Person(admin, "Администратор", "Управляет пользователями, учебными материалами, мониторинг вовлеченности аудитории")

System_Boundary(sys, "Система Поколение-М") {
    Container(userwebapp, "ОбразовательнаяПлатформа", "JavaScript, ReactJS, SPA", "Адаптивный веб-интерфейс для обучающихся")
    Container(adminwebapp, "ПанельУправления", "JavaScript, ReactJS, SPA", "Веб-интерфейс управления контентом образовательной платформы для персонала")
    Container(gw, "API Gateway", "Java, SpringBoot/Cloud", "Аутентифицирует и хранит профиль пользователей; Марштутизирует запросы в нижележащие сервисы")
    Container(cm, "CourseManager", "Java, SpringBoot", "Предоставляет API для создания/управления курсами/уроками и тп, а также записи на них")
    Container(fm, "FileManager", "Java, SpringBoot", "Предоставляет API для создания/управления файлами пользователей и учебными материалами")
    Container(em, "ExamManager", "Java, SpringBoot", "Предоставляет API для создания/управления тестами/домашними заданиями/дипломами. Реализует оценочную систему")
    Container(mt, "MigrationTool", "Java, SpringBoot", "Консольное приложение мигрирующее данные пользователей их прогресс, и материалы (видео, текст) в систему")
    ContainerDb(dbms, "Реляционная СУБД", "PostgreSQL", "Хранит информацию о пользователях, курсах и тп")
    ContainerDb(s3private, "Объектное хранилище [Приватный бакет]", "S3-Compatible Object Storage (MTSCloud S3)", "Хранит файловый контент домашних заданий")
    ContainerDb(s3public, "Объектное хранилище [Публичный бакет]", "S3-Compatible Object Storage (MTSCloud S3)", "Хранит файловый контент учебных материалов")
}

System_Ext(mtsa, "МТС Аналитика", "Система веб-, мобильной-, рекламной аналитики")
System_Ext(emailsys, "Система отправки эл.почты", "Коммунальный сервер МТС для рассылок электронной почты")
System_Ext(mtsid, "МТС ID", "Система идентификации пользователей")
System_Ext(mtscloudcdn, "MTSCloud CDN", "Система дистрибьюции контента")
System_Ext(legacy, "Старая система Поколение-М", "Информационный портал с неструктурированными материалами")
System_Ext(webinar, "Webinar", "Система проведения онлайн мероприятий (встреч, уроков, конференций и тп)")

Rel(admin, adminwebapp, "Управляет правами, создает курсы/уроки в")
Rel(admin, mtsa, "Смотрит статистику поведения пользователей в")
Rel(admin, mt, "Запускает миграцию через")

Rel(guest, userwebapp, "Просматривает доступные курсы, регистрируется в")
Rel(student, userwebapp, "Проходит обучение по курсам, сдает дом.работы, оценивает курс в")
Rel(teacher, adminwebapp, "Проверяет домашние работы или задания по конкурсам в")
Rel(support, adminwebapp, "Управляет учетными записями пользователей, назначением курсов, формированием образовательной программы в")

Rel(userwebapp, gw, "Использует API", "http/rest")
Rel(userwebapp, mtsa, "Логирует события пользователей в", "http/rest")

Rel(adminwebapp, gw, "Использует API", "http/rest")

Rel(gw, dbms, "Управляет данными пользователей в", "tcp/jdbc")
Rel(gw, emailsys, "Отправляет уведомление о регистрации в", "smtp")
Rel(gw, mtsid, "Аутентифицирует пользователей в")
Rel(gw, cm, "Вызывает API", "http/rest")
Rel(gw, fm, "Вызывает API", "http/rest")
Rel(gw, em, "Вызывает API", "http/rest")

Rel(cm, dbms, "Управляет данными курсов/уроков в", "tcp/jdbc")
Rel(cm, webinar, "Создает комнаты для воркшопов в", "http/rest")

Rel(em, dbms, "Управляет данными тестов/дз/дипломов/оценок в", "tcp/jdbc")

Rel_U(mt, legacy, "Загружает информацию о пользователях/курсах/уроках", "tcp/jdbc+http")
Rel_U(mt, gw, "Сохраняет информацию о пользователях/курсах/уроках/домашних заданиях/учебных материалах", "http")

Rel(fm, s3private, "Управляет файлами домашних заданий", "http")
Rel(fm, s3public, "Управляет файлами учебных материалов", "http")
Rel(fm, mtscloudcdn, "Экспортирует видео-файл, получает ссылку на него в", "http")

Rel_U(mtscloudcdn, s3public, "Получает файлы для дистрибьюции и кеширования", "http")

@enduml
```
</details>
