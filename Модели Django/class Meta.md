# `class Meta:`

```python
from django.db import models

class <ИМЯ МОДЕЛИ>(models.Model):  
	<объявление полей модели> 
    class Meta:  
        <параметр> = <значение>  
  
```

- Опции `Meta`
	[abstract](#abstract)
	[app_label](#app_label)
	[base_manager_name](#base_manager_name)
	[default_manager_name](#default_manager_name)
	[db_table](#db_table)
	[db_tablespace](#db_tablespace)
	[default_related_name](#default_related_name)
	[get_latest_by](#get_latest_by)
	[managed](#managed)
	[order_with_respect_to](#order_with_respect_to)
	[ordering](#ordering)
	[permissions](#permissions)
	[default_permissions](#default_permissions)
	[proxy](#proxy)
	[required_db_features](#required_db_features)
	[required_db_vendor](#required_db_vendor)
	[select_on_save](#select_on_save)
	[indexes](#indexes)
	[unique_together](#unique_together)
	[index_together](#index_together)
	[constraints](#constraints)
	[verbose_name](#verbose_name)
	[verbose_name_plural](#verbose_name_plural)

- Атрибуты `Meta` только для чтения
	[label](#label)
	[label_lower](#label_lower)

## Опции `Meta`
<br><br>
### `abstract`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

```
abstract = True  # эта модель будет базовым абстрактным классом
```
<br><br>
### `app_label`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя приложения, которому принадлежит модель (если модель объявлена нe в приложении, указанном в `INSTALLED_APPS`)
```
app_label = 'myapp'
```
<br><br>
### `base_manager_name`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

```
base_manager_name = 'objects'  # переопределение базового менеджера модели
```
<br><br>
### `default_manager_name`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Переопределение менеджера модели по умолчанию 
Django назначает менеджером по умолчанию первый объект `Manager` в порядке, в котором они объявлены в модели ([подробнее про Менеджеры](../запросы%20Django/Менеджеры.md))
<br><br>
### `db_table`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя таблицы базы данных для хранения данных модели.
По умолчанию таблице присваивается имя `<имя приложения>_<имя класса модели>`
```python
db_table = 'music_album'  # имя таблицы указывать в нижнем регистре
db_table = '"name_left_in_lowercase"'  # экранирование имени для Oracle. Экранирование допустимо и с другими базами данных, но эффект имеется только в Oracle
```
<br><br>
### `db_tablespace`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя табличного пространства базы данных для использования моделью. По умолчанию используется имя табличного пространства из  `DEFAULT_TABLESPACE` если было задано

>Django не создает табличные пространства. Их нужно настроить в СУБД

>PostgreSQL и Oracle поддерживают табличные пространства. SQLite, MariaDB и MySQL не поддерживают.

>Когда вы используете бэкенд, который не поддерживает табличные пространства, Django игнорирует все опции, связанные с табличными пространствами.

```python
class TablespaceExample(models.Model):
    name = models.CharField(max_length=30, db_index=True, db_tablespace="indexes")
    data = models.CharField(max_length=255, db_index=True)
    shortcut = models.CharField(max_length=7)
    edges = models.ManyToManyField(to="self", db_tablespace="indexes")

    class Meta:
        db_tablespace = "tables"
        indexes = [models.Index(fields=['shortcut'], db_tablespace='other_indexes')]
```
<br><br>
### `default_related_name`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя, которое будет использоваться по умолчанию для отношения от связанного объекта обратно к этому. По умолчанию это `<model_name>_set`

>Эта опция также устанавливает `related_query_name`

Если вы намереваетесь создать подкласс для своей модели, чтобы обойти коллизии имен, часть имени должна содержать `'%(app_label)s'` и `'%(model_name)s'`), которые заменяются соответственно именем приложения, в котором находится модель и название модели, оба в нижнем регистре

```python
from django.db import models

class Base(models.Model):
    m2m = models.ManyToManyField(
        OtherModel,
        related_name="%(app_label)s_%(class)s_related",
        related_query_name="%(app_label)s_%(class)ss",
    )

    class Meta:
        abstract = True

class ChildA(Base):
    pass

class ChildB(Base):
    pass
```
<br><br>
### `get_latest_by`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя поля или список имен полей в модели, обычно [`DateField`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.DateField "django.db.models.DateField"), [`DateTimeField`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField") или [`IntegerField`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.IntegerField "django.db.models.IntegerField"). Здесь указываются поля по умолчанию для использования в вашей модели [`Manager`](https://django.fun/ru/docs/django/4.1/topics/db/managers/#django.db.models.Manager "django.db.models.Manager") [`latest()`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.query.QuerySet.latest "django.db.models.query.QuerySet.latest") и [`earliest()`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.query.QuerySet.earliest "django.db.models.query.QuerySet.earliest").

Пример:
```python
# Latest by ascending order_date.
get_latest_by = "order_date"

# Latest by priority descending, order_date ascending.
get_latest_by = ['-priority', 'order_date']
```
<br><br>
### `managed`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

По умолчанию `True`
Если установлено значение `False`, для этой модели не будут выполняться операции создания, изменения или удаления таблицы базы данных. Это полезно, если модель представляет существующую таблицу или представление базы данных, созданное другими способами
<br><br>
### `order_with_respect_to`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Делает этот объект упорядоченным по отношению к заданному полю, обычно это `ForeignKey`
применяется если важно, чтобы объекты зависимой модели были расположены в определенном порядке
```python
rom django.db import models

class Question(models.Model):
    text = models.TextField()
    # ...

class Answer(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    # ...

    class Meta:
        order_with_respect_to = 'question'
```

Когда задано `order_with_respect_to`, у первичного объекта доступны два дополнительных метода для извлечения и установки порядка связанных объектов: `get_RELATED_order()` и `set_RELATED_order()`, где `RELATED` является названием модели в нижнем регистре. Например, если предположить, что объект `Question` имеет несколько связанных объектов `Answer`, возвращаемый список содержит первичные ключи связанных объектов `Answer`:

```python
>>> question = Question.objects.get(id=1)
>>> question.get_answer_order()
[1, 2, 3]
```
Порядок связанных с `Question` объектов `Answer` объектов можно установить, передав список первичных ключей `Answer`:

```python
>>> question.set_answer_order([3, 1, 2])
```

Связанные объекты также получают два метода: `get_next_in_order()` и `get_previous_in_order()`, которые можно использовать для доступа к этим объектам в правильном порядке. Предполагая, что `Answer` объекты упорядочены по `id`:

```python
>>> answer = Answer.objects.get(id=2)
>>> answer.get_next_in_order()
<Answer: 3>
>>> answer.get_previous_in_order()
<Answer: 1>
```

>`order_with_respect_to` добавляет дополнительное поле/столбец в базу данных с именем `_order` и устанавливает для этой модели параметр `ordering`
>
>Следовательно, `order_with_respect_to` и `ordering` не могут использоваться вместе, и порядок, добавленный с помощью `order_with_respect_to`, будет применяться всякий раз, когда вы получаете список объектов этой модели.
<br><br>
### `ordering`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Порядок по умолчанию для объекта, для использования при получении списков объектов:

```python
ordering = ['-order_date']
ordering = ['-pub_date', 'author']
ordering = [F('author').asc(nulls_last=True)]
```

Это кортеж или список строк и/или выражений запроса. Каждая строка представляет собой имя поля с необязательным префиксом «-», который указывает в порядке убывания. Поля без начального «-» будут упорядочены по возрастанию. Используйте знак «?» для случайной сортировки

>Не используется совместно с `order_with_respect_to`
<br><br>
### `permissions`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>
Добавление дополнительных видов разрешений для объектов модели
По умолчанию уже содержит разрешения добавления, удаления, изменения и просмотра

```python
permissions = [('can_deliver_pizzas', 'Can deliver pizzas')]
```
Это список или кортеж из 2-х кортежей в формате `(permission_code, human_readable_permission_name)`.
<br><br>
### `default_permissions`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

По умолчанию используется `('add', 'change', 'delete', 'view')`. Вы можете настроить этот список, например, установив его в пустой список, если ваше приложение не требует каких-либо разрешений по умолчанию. Это должно быть указано в модели перед созданием модели с помощью `migrate`, чтобы предотвратить создание любых пропущенных разрешений.
<br><br>
### `proxy`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Если `proxy=True`, модель, которая является подклассом другой модели, будет рассматриваться как [proxy model](https://django.fun/ru/docs/django/4.1/topics/db/models/#proxy-models).
<br><br>
### `required_db_features`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Список к перечнем функций базы данных, которые должно иметь текущее соединение, чтобы модель рассматривалась на этапе миграции. Например, если вы установите для этого списка значение `['gis_enabled']`, модель будет синхронизироваться только в базах данных с поддержкой ГИС
<br><br>
### `required_db_vendor`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя поддерживаемого поставщика базы данных, к которому относится данная модель. Текущие имена встроенных поставщиков: `sqlite`, `postgresql`, `mysql`, `oracle`. Если этот атрибут не пустой и текущий поставщик соединений не соответствует ему, модель не будет синхронизирована.
<br><br>
### `select_on_save`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

По умолчанию `False`. Если установить `True` будет создаваться дополнительный запрос для проверки сущесвования записи на момент записи
<br><br>
### `indexes`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Список индексов модели. [читать подробнее про индексы модели](Индексы%20модели.md)
```python
from django.db import models

class Customer(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)

    class Meta:
        indexes = [
            models.Index(
	            fields=['last_name', 'first_name']
	        ),
            models.Index(
	            fields=['first_name'], 
	            name='first_name_idx'
	        ),
        ]
```

```python
from django.contrib.postgres.indexes import BrinIndex

class SomeModel(Model):
    created = DatetimeField(
        auto_now_add=True,
    )
    class Meta:
        indexes = (
            BrinIndex(fields=['created']),
        )
```

<br><br>
### `unique_together`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

>Устаревший механизм, который будет исключен в будущем. Вместо него следует использовать  `UniqueConstraint` с параметром класса Meta `constraints`

Наборы имен полей, которые, взятые вместе, должны быть уникальными:

```python
unique_together = [['driver', 'restaurant']]
```

Это список списков, которые должны быть уникальными при совместном рассмотрении. Он используется в админке Django и применяется на уровне базы данных (т.е. соответствующие операторы `UNIQUE` включены в оператор `CREATE TABLE`).

Для удобства `unique_together` может быть одним списком при работе с одним набором полей:

```python
unique_together = ['driver', 'restaurant']
```
`ManyToManyField` нельзя включить в unique_together. (Непонятно, что бы это даже значило!) Если вам нужно проверить уникальность, связанную с `ManyToManyField`, попробуйте использовать сигнал или явное [`through`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ManyToManyField.through "django.db.models.ManyToManyField.through") модели.

`ValidationError`, возникающее во время проверки модели при нарушении ограничения, имеет код ошибки «unique_together».
<br><br>
### `index_together`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

>Устаревший механизм, который будет исключен в будущем. Вместо него следует использовать более функциональный параметр  [`indexes`](#`indexes`)

Наборы имен полей, которые вместе взятые индексируются:

```python
index_together = [
    ["pub_date", "deadline"],
]
```

Этот список полей будет проиндексирован вместе (то есть будет выполнен соответствующий оператор `CREATE INDEX`.)

Для удобства `index_together` может быть одним списком при работе с одним набором полей:

```python
index_together = ["pub_date", "deadline"]
```
<br><br>
### `constraints`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>
Список [Constrains](ограничения%20базы%20данных%20Constrains.md), который вы хотите определить в модели:

```python
from django.db import models

class Customer(models.Model):
    age = models.IntegerField()

    class Meta:
        constraints = [
            models.CheckConstraint(check=models.Q(age__gte=18), name='age_gte_18'),
        ]
```
<br><br>
### `verbose_name`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Удобочитаемое имя для объекта, единственное число:

```python
verbose_name = "pizza"
```

Если оно не задано, Django будет использовать измененную версию имени класса: `CamelCase` становится `camel case`.
<br><br>
### `verbose_name_plural`
***
<div style='text-align:right'><small>параметры класса Meta</small></div>

Имя во множественном числе для объекта:

```python
verbose_name_plural = "stories"
```

Если не задано, Django будет использовать `verbose_name` + `"s"`.

<br><br>
## Атрибуты `Meta` только для чтения
<br><br>
### `label`
***

Представление объекта возвращает `app_label.object_name`, например, `'Polls.Question'`.
<br><br>
### `label_lower`
***

Представление модели возвращает `app_label.model_name`, например, `'Polls.question'`