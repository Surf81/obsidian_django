# Поля отношений Django
<br>
## ### `ManyToManyField`

```python
ManyToManyField(
	to,
	**options
)
```

Отношения многие ко многим. Требуется позиционный аргумент: класс, к которому относится модель, который работает точно так же, как и для `ForeignKey`, включая рекурсивные отношения.

Связанные объекты можно добавлять, удалять или создавать с помощью поля [`RelatedManager`](https://django.fun/ru/docs/django/4.1/ref/models/relations/#django.db.models.fields.related.RelatedManager "django.db.models.fields.related.RelatedManager")
<br><br>
### Представление базы данных[¶](https://django.fun/ru/docs/django/4.1/ref/models/fields/#id1 "Permalink to this heading")

За кулисами Django создает промежуточную таблицу соединений для представления отношения «многие ко многим». По умолчанию это имя таблицы генерируется с использованием имени поля «многие ко многим» и имени таблицы для модели, в которой оно содержится. Поскольку некоторые базы данных не поддерживают имена таблиц выше определенной длины, эти имена таблиц будут автоматически усечены, и будет использован хеш уникальности, например, `Author_books_9cdf`. Вы можете вручную указать имя объединяемой таблицы с помощью параметра [`db_table`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ManyToManyField.db_table "django.db.models.ManyToManyField.db_table").
<br><br>
### Аргументы[¶](https://django.fun/ru/docs/django/4.1/ref/models/fields/#manytomany-arguments "Permalink to this heading")

`ManyToManyField` принимает дополнительный набор аргументов - все необязательные - которые управляют работой отношений.
<br><br>
#### `related_name`
***

То же, что [`ForeignKey.related_name`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ForeignKey.related_name "django.db.models.ForeignKey.related_name").
<br><br>
#### `related_query_name`
***

То же, что [`ForeignKey.related_query_name`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ForeignKey.related_query_name "django.db.models.ForeignKey.related_query_name").
<br><br>
#### `limit_choices_to`
***

То же, что [`ForeignKey.limit_choices_to`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ForeignKey.limit_choices_to "django.db.models.ForeignKey.limit_choices_to").
<br><br>
#### `symmetrical`
***

Используется только в определении ManyToManyFields на самого себя. Рассмотрим следующую модель:

```python
from django.db import models

class Person(models.Model):
    friends = models.ManyToManyField("self")
```

Когда Django обрабатывает эту модель, он идентифицирует, что у него есть `ManyToManyField`, и в результате он не добавляет атрибут `person_set` к классу `Person`. Вместо этого предполагается, что `ManyToManyField` является симметричным, то есть, если я ваш друг, то вы мой друг.

Если вам не нужна симметрия в отношениях «многие ко многим» с `self`, установите `symmetrical` в `False`. Это заставит Django добавить дескриптор для обратной связи, что сделает отношения `ManyToManyField` несимметричными.
<br><br>
#### `through`
***

Django автоматически создаст таблицу для управления отношениями «многие ко многим». Однако, если вы хотите указать промежуточную таблицу вручную, вы можете использовать опцию `through`, чтобы указать модель Django, представляющую промежуточную таблицу, которую вы хотите использовать.

Чаще всего этот параметр используется, когда вы хотите связать [дополнительные данные с отношением «многие ко многим»](https://django.fun/ru/docs/django/4.1/topics/db/models/#intermediary-manytomany).

>Если вам не нужны множественные ассоциации между одними и теми же экземплярами, добавьте UniqueConstraint, включая поля from и to. Автоматически создаваемые в Django таблицы «многие ко многим» включают такое ограничение.

>Рекурсивные отношения, использующие промежуточную модель, не могут определять имена обратных методов доступа, поскольку они будут одинаковыми. Вам нужно установить related_name хотя бы для одного из них. Если вы предпочитаете, чтобы Django не создавал обратную связь, установите для related_name значение '+'.

Если вы не укажете явную модель `through`, все еще существует неявный класс модели `through`, который вы можете использовать для прямого доступа к таблице, созданной для хранения ассоциации. Он имеет три поля для связи моделей.

Если исходная и целевая модели различаются, создаются следующие поля:

-   `id`: первичный ключ отношения.
-   `<containing_model>_id`: `id` модели, которая объявляет `ManyToManyField`.
-   `<other_model>_id`: `` id`` модели, на которую указывает `` ManyToManyField``.

Если `ManyToManyField` указывает на одну и ту же модель, генерируются следующие поля:

-   `id`: первичный ключ отношения.
-   `from_<model>_id`: `` id`` экземпляра, который указывает на модель (то есть исходный экземпляр).
-   `to_<model>_id`: `` id`` экземпляра, на который указывает отношение (то есть экземпляр целевой модели).

Этот класс может использоваться для запроса связанных записей для данного экземпляра модели, как обычная модель:

```python
Model.m2mfield.through.objects.all()
```
<br><br>
#### `through_fields`
***

Используется только когда указана пользовательская промежуточная модель. Django обычно определяет, какие поля промежуточной модели использовать для автоматического установления отношения «многие ко многим». Однако рассмотрим следующие модели:

```python
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=50)

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(
        Person,
        through='Membership',
        through_fields=('group', 'person'),
    )

class Membership(models.Model):
    group = models.ForeignKey(Group, on_delete=models.CASCADE)
    person = models.ForeignKey(Person, on_delete=models.CASCADE)
    inviter = models.ForeignKey(
        Person,
        on_delete=models.CASCADE,
        related_name="membership_invites",
    )
    invite_reason = models.CharField(max_length=64)
```

`Membership` имеет _два_ внешних ключа для `Person` (`person` и `inviter`), что делает отношения неоднозначными, и Django не может знать, какой из них использовать. В этом случае вы должны явно указать, какие внешние ключи должен использовать Django, используя `through_fields`, как в примере выше.

`through_fields` принимает 2-х значный кортеж `('field1', 'field2')`, где `field1` - имя внешнего ключа модели, для которого определен класс `ManyToManyField` (`group` в данном случае) и `field2` - имя внешнего ключа для целевой модели (в данном случае `person`).

Если у вас есть более одного внешнего ключа в промежуточной модели для любой (или даже обеих) моделей, участвующих в отношении многие-ко-многим, вы _должны_ указать `through_fields`. Это также относится к :ref: рекурсивным отношениям \<recursive-relationships\>, когда используется модель-посредник и существует более двух внешних ключей для модели, или вы хотите явно указать, какие два Django следует использовать.
<br><br>
#### `db_table`
***

Имя таблицы, создаваемой для хранения данных «многие ко многим». Если это не предусмотрено, Django примет имя по умолчанию на основе имен: таблицы для модели, определяющей отношение, и имени самого поля.
<br><br>
#### `db_constraint`
***

Определяет, следует ли создавать ограничения в базе данных для внешних ключей в промежуточной таблице. По умолчанию используется `True`, и это почти наверняка то, что вы хотите; установка этого значения в `False` может быть очень плохой для целостности данных. Тем не менее, вот несколько сценариев, где вы можете сделать это:

-   У вас есть устаревшие данные, которые не действительны.
-   Вы разделяете свою базу данных.

Будет ошибкой пропускать оба параметра: `db_constraint` и `through`.
<br><br>
#### `swappable`
***

Управляет реакцией фреймворка миграции, если этот `ManyToManyField` указывает на заменяемую модель. Если `True` - по умолчанию - тогда, если `ManyToManyField` указывает на модель, которая соответствует текущему значению `settings.AUTH_USER_MODEL` (или другой сменной настройки модели), отношение будет хранится в миграции с использованием ссылки на настройку, а не на модель напрямую.

Переопределять это значение `False` нужно только в том случае, если вы уверены, что ваша модель всегда должна указывать на замененную модель - например, если это модель профиля, разработанная специально для вашей пользовательской модели пользователя.

Если сомневаетесь, оставьте для него значение по умолчанию `True`.

>`ManyToManyField` не поддерживает [`validators`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.Field.validators "django.db.models.Field.validators").

>`null` не имеет никакого эффекта, поскольку нет способа требовать отношений на уровне базы данных.