# Дипломная работа к профессии Python-разработчик «API Сервис заказа товаров для розничных сетей».

## Описание

Приложение предназначено для автоматизации закупок в розничной сети. Пользователи сервиса — покупатель (менеджер торговой сети, который закупает товары для продажи в магазине) и поставщик товаров.

**Клиент (покупатель):**

- Менеджер закупок через API делает ежедневные закупки по каталогу, в котором
  представлены товары от нескольких поставщиков.
- В одном заказе можно указать товары от разных поставщиков — это
  повлияет на стоимость доставки.
- Пользователь может авторизироваться, регистрироваться и восстанавливать пароль через API.
    
**Поставщик:**

- Через API информирует сервис об обновлении прайса.
- Может включать и отключать прием заказов.
- Может получать список оформленных заказов (с товарами из его прайса).


### Задача

Необходимо разработать backend-часть (Django) сервиса заказа товаров для розничных сетей.

**Базовая часть:**
* Разработка сервиса под готовую спецификацию (API);
* Возможность добавления настраиваемых полей (характеристик) товаров;
* Импорт товаров;
* Отправка накладной на email администратора (для исполнения заказа);
* Отправка заказа на email клиента (подтверждение приема заказа).

**Продвинутая часть:**
* Экспорт товаров;
* Админка заказов (проставление статуса заказа и уведомление клиента);
* Выделение медленных методов в отдельные процессы (email, импорт, экспорт).

### Исходные данные
 
1. Общее описание сервиса
1. [Спецификация (API) - 1 шт.](./reference/screens.md)
1. [Файлы yaml для импорта товаров - 1 шт.](./data/shop1.yaml)
1. [Пример API Сервиса для магазина](./reference//netology_pd_diplom/) 

## Этапы разработки

Разработку Backend рекомендуется разделить на следующие этапы:

Базовая часть:
- [X] 1. [Создание и настройка проекта](./reference/step-1.md)
- [X] 2. [Проработка моделей данных](./reference/step-2.md)
- [X] 3. [Реализация импорта товаров](./reference/step-3.md)
- [X] 4. [Реализация API views](./reference/step-4.md)
- [X] 5. [Полностью готовый backend](./reference/step-5.md)

Продвинутая часть (по желанию, если базовая часть полностью готова):

- [X] 6. [Реализация forms и views админки склада](./reference/step-6-adv.md)
- [X] 7. [Вынос медленных методов в задачи Celery](./reference/step-7-adv.md)


### Чтобы запустить проект необходимо:

- Установить зависимости:

```
pip install -r requirements.txt
```
- Создать базу данных **PostgreSQL**:
```
createdb -U postgres orders_db
```
- Выполнить миграции:

```
manage.py makemigrations && manage.py migrate
manage.py createsuperuser
```
- Создать файл .env и определить в нем следующие переменные окружения:

| Имя переменной   | Описание значения переменной                                                                        |
|------------------|-----------------------------------------------------------------------------------------------------|
| DEBUG            | Режим отладки проекта, по умолчанию True или 1                                                      |
| SECRET_KEY       | Секретный ключ проекта, любой произвольный набор символов                                           |
| ALLOWED_HOSTS    | Разрешенный список хостов (в режиме DEBUG=True **localhost** или **127.0.0.1**)                     |
| POSTGRES_DB      | Имя базы данных, по умолчанию *orders_db*                                                           |
| POSTGRES_USER    | Имя пользователя для подключения к базе данных, по умолчанию *postgres*                             |
| POSTGRES_PASSWORD| Пароль пользователя для подключения к базе данных, по умолчанию *postgres*                          |
| POSTGRES_HOST    | Имя хоста для подключения проекта к БД, по умолчанию *127.0.0.1*                                    |
| POSTGRES_PORT    | Порт для подключения к БД, по умолчанию 5432                                                        |
| EMAIL_HOST       | Почтовый сервер для отправки писем пользователю, для mail.ru (smtp.mail.ru)                         |
| EMAIL_PORT       | Порт почтового сервера, для mail.ru 2525                                                            |
| EMAIL_HOST_USER  | Почтовый адрес пользователя                                                                         |
| EMAIL_HOST_PASS  | Пароль от приложения на почтовом сервере пользователя                                               |

- При необходимости загрузить тестовые данные в базу данных:

```
python manage.py loaddata --format=yaml fixtures/shop1.yaml
```
- Выполнить команду:

```
python manage.py runserver
```
- Прописать адрес Redis сервера в settings.py 

- Выполнить команду во втором терминале:

```
celery -A orders worker -l INFO
```
- Запустить тесты:
```
pytest
```