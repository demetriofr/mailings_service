# **Cервис управления рассылками, администрирования и получения статистики.**
_(project name: mainlings_service)_

**Цель проекта:** помочь в создании рассылок с целью удержания текущих клиентов (вспомогательные рассылки) и 
для информирования / привлечения клиентов («прогревающие» рассылки).


### Реализованные возможности:
_Первый этап (MVP)_
- [ ] Интерфейс заполнения рассылок, то есть CRUD-механизм для управления рассылками.
- [ ] Скрипт рассылки, который работает как из командой строки, так и по расписанию.
- [ ] Настройки конфигурации для периодического запуска рассылки.

_Второй этап (пост MVP)_
- [ ] Разделение прав доступа для различных пользователей.
- [ ] Разработан раздел блог для развития популярности сервиса в интернете.

### Особенности
- Сервис реализован на фреймворки Django 4.2 
- Проект использует интерфейс UI kit Bootstrap 4.0.
- Для работы с периодическими задачами используется crontab-задачи на основе дополнительной библиотеке: 
https://pypi.org/project/APScheduler/
- Для наследования в сущности пользователь, используется модель `AbstractUser`


### Логика работы сервиса
- После создания новой рассылки, если текущее время больше времени начала и меньше времени окончания, то выбираются
из справочника все клиенты, которые указаны в настройках рассылки, и запускается отправка для всех этих клиентов.
- Если создается рассылка со временем старта в будущем, то отправка стартует автоматически по наступлению этого времени 
без дополнительных действий со стороны пользователя сервиса.
- По ходу отправки сообщений собирается статистика в логи рассылки по каждому сообщению для последующего формирования 
отчетов.
- Внешний сервис, который принимает отправляемые сообщения, может долго обрабатывать запрос, отвечать некорректными 
данными, на какое-то время вообще не принимать запросы, поэтому сервис корректно обрабатывает подобные ошибки. 
Проблемы с внешним сервисом не влиять на стабильность работы сервиса рассылок.

### Некоторые важные понятия
**Периодические задачи** — задачи, которые повторяются с определенной частотой, которая задается расписанием.

**crontab** — классический демон, который используется для периодического выполнения заданий в определенное время. 
Регулярные действия описываются инструкциями, помещенными в файлы crontab и в специальные каталоги.

### Функционал
#### пользователя:
* **Не может** случайным образом изменить чужую рассылку.
* **Может** работать только со своим списком клиентов. 
* **Может** работать только со своим списком рассылок.
 
#### менеджера:
* **Может** просматривать любые рассылки.
* **Может** просматривать список пользователей сервиса.
* **Может** блокировать пользователей сервиса.
* **Может** отключать рассылки.
* **Не может** редактировать рассылки.
* **Не может** управлять списком рассылок.
* **Не может** изменять рассылки и сообщения.


### Дополнительное приложение "Блог"
#### Особенности:
- не имеет отдельного интерфейса. 
- использует административную панель для контент-менеджера.
- главная страница блога имеет следующую информацию:
  - количество рассылок всего,
  - количество активных рассылок,
  - количество уникальных клиентов для рассылок,
  - 3 случайные статьи из блога.


### Алгоритм запуска сервиса

1. Клонируйте содержимое данного репозитория к себе. 
2. Установите зависимости в venv: `pip3 install -r requirements.txt`.
3. Создайте базу данных, например, через консоль: 
```
psql -U postgres
create database mailings_service;
```
4. В .env добавите данные из env.sample.
5. Запустите кэширование с помощью redis.
6. Создайте суперюзера использую команду `python3 manage.py ccsu`.
7. Запустите периодические рассылки командой `python manage.py apscheduler_run`.
8. Добавьте группу менеджера через админку или консоль, а также наделите его функционалом.




### Данный проект это курсовая работа № 6 модуля "Django" в курсе "Python-разработчик" 
_Данный учебный проект состоит из двух частей._

### Часть 1. Разработка сервиса
#### Описание задач
- [ ] 1 Реализован интерфейс заполнения рассылок, то есть CRUD-механизм для управления рассылками.
- [ ] 2 Реализован скрипт рассылки, который работает как из командой строки, так и по расписанию.
- [ ] 3 Добавлены настройки конфигурации для периодического запуска задачи.

### Часть 2. Доработка сервиса
#### Описание задач
- [ ] 1 Расширена модель пользователя для регистрации по почте, а также верификации.
- [ ] 2 Добавлен интерфейс для входа, регистрации и подтверждения почтового ящика.
- [ ] 3 Реализовано ограничение доступа к рассылкам для разных пользователей.
- [ ] 4 Реализован интерфейс менеджера.
- [ ] 5 Создан блог для продвижения сервиса.

**Критерии успешного выполнения данной курсовой:**
- [ ] Реализована вся требуемую логику работы системы.
- [ ] Интерфейс понятен и соответствует базовым требованиям системы.
- [ ] Все интерфейсы для изменения и создания сущностей, не относящиеся к стандартной админке, реализованы с помощью 
Django-форм.
- [ ] Все настройки прав доступа правильно реализованы.
- [ ] Использованы как минимум два типа кеширования, в том числе для блога и главной страницы сервиса.
- [ ] Решение размещено на GitHub.