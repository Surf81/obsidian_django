# Поля отношений Django
<br>

## `ForeignKey`

```python
ForeignKey(
	to,
	on_delete,
	**options
)
```

Отношения многие-к-одному. Требуются два позиционных аргумента: класс, к которому относится модель, и опция `on_delete`.

Чтобы создать рекурсивное отношение - объект, который имеет отношение «многие-к-одному» с самим собой - используйте 
```python
models.ForeignKey('self', on_delete = models.CASCADE)
```

Если вам нужно создать отношение на модель, которая еще не была определена, вы можете использовать имя модели, а не сам объект модели:

```python
from django.db import models

class Car(models.Model):
    manufacturer = models.ForeignKey(
        'Manufacturer',
        on_delete=models.CASCADE,
    )
    # ...

class Manufacturer(models.Model):
    # ...
    pass
```

Отношения, определенные следующим образом [абстрактные модели](https://django.fun/ru/docs/django/4.1/topics/db/models/#abstract-base-classes), разрешаются, когда модель подклассифицируется как конкретная модель, и не относятся к `app_label` абстрактной модели:

`products/models.py`[¶](https://django.fun/ru/docs/django/4.1/ref/models/fields/#id3 "Постоянная ссылка на код")
```python
from django.db import models

class AbstractCar(models.Model):
    manufacturer = models.ForeignKey('Manufacturer', on_delete=models.CASCADE)

    class Meta:
        abstract = True
```

`production/models.py`[¶](https://django.fun/ru/docs/django/4.1/ref/models/fields/#id4 "Постоянная ссылка на код")
```python
from django.db import models
from products.models import AbstractCar

class Manufacturer(models.Model):
    pass

class Car(AbstractCar):
    pass

# Car.manufacturer will point to `production.Manufacturer` here.
```

Для ссылки на модели, определенные в другом приложении, вы можете явно указать модель с полной меткой приложения. Например, если описанная выше модель `Manufacturer` определена в другом приложении под названием `production`, вам необходимо использовать:

```python
class Car(models.Model):
    manufacturer = models.ForeignKey(
        'production.Manufacturer',
        on_delete=models.CASCADE,
    )
```

Такого рода ссылки, называемые ленивыми отношениями, могут быть полезны при разрешении циклических зависимостей импорта между двумя приложениями.

Индекс базы данных автоматически создается для `ForeignKey`. Вы можете отключить это, установив `db_index` в `False`. Возможно, вы захотите избежать накладных расходов на индекс, если вы создаете внешний ключ для согласованности, а не для объединений, или если вы будете создавать альтернативный индекс, такой как индекс с частичным или множественным столбцом.
<br><br>
### Представление базы данных[¶](https://django.fun/ru/docs/django/4.1/ref/models/fields/#database-representation "Permalink to this heading")

За кулисами Django добавляет `"_id"` к имени поля, чтобы создать имя столбца базы данных. В приведенном выше примере таблица базы данных для модели `Car` будет иметь столбец `factory_id`. (Вы можете изменить это явно, указав `db_column`). Однако ваш код никогда не должен иметь дело с именем столбца базы данных, если вы не пишете пользовательский SQL. Вы всегда будете иметь дело с именами полей вашего модельного объекта.
<br><br>
### Аргументы[¶](https://django.fun/ru/docs/django/4.1/ref/models/fields/#arguments "Permalink to this heading")

`ForeignKey` принимает другие аргументы, которые определяют детали работы отношений.
<br><br>
#### `on_delete`
***

При удалении объекта, на который ссылается `ForeignKey`, Django будет эмулировать поведение ограничения SQL, заданного аргументом `on_delete`. Например, если у вас есть обнуляемым `ForeignKey` и вы хотите, чтобы он был установлен в null, когда ссылочный объект удален:

```python
user = models.ForeignKey(
    User,
    models.SET_NULL,
    blank=True,
    null=True,
)
```

`on_delete` не создает ограничение SQL в базе данных. Поддержка опций каскадного уровня базы данных [может быть реализовано позже](https://code.djangoproject.com/ticket/21961).

Возможные значения для `on_delete` находятся в [`django.db.models`](https://django.fun/ru/docs/django/4.1/topics/db/models/#module-django.db.models "django.db.models"):

-   `CASCADE`
    
    Каскадное удаление. Django эмулирует поведение ограничения SQL ON DELETE CASCADE, а также удаляет объект, содержащий ForeignKey.
    
    [`Model.delete()`](https://django.fun/ru/docs/django/4.1/ref/models/instances/#django.db.models.Model.delete "django.db.models.Model.delete") не вызывается в связанных моделях, но сигналы [`pre_delete`](https://django.fun/ru/docs/django/4.1/ref/signals/#django.db.models.signals.pre_delete "django.db.models.signals.pre_delete") и [`post_delete`](https://django.fun/ru/docs/django/4.1/ref/signals/#django.db.models.signals.post_delete "django.db.models.signals.post_delete") отправляются для всех удаленных объектов.
    
-   `PROTECT`
    
    Предотвращает удаление объекта, на который есть ссылка, путем вызова [`ProtectedError`](https://django.fun/ru/docs/django/4.1/ref/exceptions/#django.db.models.ProtectedError "django.db.models.ProtectedError"), подкласса [`django.db.IntegrityError`](https://django.fun/ru/docs/django/4.1/ref/exceptions/#django.db.IntegrityError "django.db.IntegrityError").
    
-   `RESTRICT`
    
    Предотвращает удаление указанного объекта путем вызова [`RestrictedError`](https://django.fun/ru/docs/django/4.1/ref/exceptions/#django.db.models.RestrictedError "django.db.models.RestrictedError") (подкласс [`django.db.IntegrityError`](https://django.fun/ru/docs/django/4.1/ref/exceptions/#django.db.IntegrityError "django.db.IntegrityError")). В отличие от [`PROTECT`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.PROTECT "django.db.models.PROTECT"), удаление ссылочного объекта допускается, если он также ссылается на другой объект, который удаляется в той же операции, но через отношение `CASCADE`.
    
    Рассмотрим этот набор моделей:
    
```python
    class Artist(models.Model):
        name = models.CharField(max_length=10)
    
    class Album(models.Model):
        artist = models.ForeignKey(Artist, on_delete=models.CASCADE)
    
    class Song(models.Model):
        artist = models.ForeignKey(Artist, on_delete=models.CASCADE)
        album = models.ForeignKey(Album, on_delete=models.RESTRICT)
```
    
    `Artist` можно удалить, даже если это подразумевает удаление `Album`, на который ссылается `Song`, потому что `Song` также ссылается на `Artist` через каскадные отношения. Например:
    
    >>> artist_one = Artist.objects.create(name='artist one')
    >>> artist_two = Artist.objects.create(name='artist two')
    >>> album_one = Album.objects.create(artist=artist_one)
    >>> album_two = Album.objects.create(artist=artist_two)
    >>> song_one = Song.objects.create(artist=artist_one, album=album_one)
    >>> song_two = Song.objects.create(artist=artist_one, album=album_two)
    >>> album_one.delete()
    # Raises RestrictedError.
    >>> artist_two.delete()
    # Raises RestrictedError.
    >>> artist_one.delete()
    (4, {'Song': 2, 'Album': 1, 'Artist': 1})
    
-   `SET_NULL`
    
    Устанавливает `ForeignKey` null; это возможно только в том случае, если опция `null` равно `True`.
    
-   `SET_DEFAULT`
    
    Устанавливает для `ForeignKey` значение по умолчанию; значение по умолчанию для `ForeignKey` должно быть указано в описании поля.
    
-   `SET`()
    
    Установите `ForeignKey` в значение, переданное в `SET()`, или, если передана вызываемая переменная, результат ее вызова. В большинстве случаев передача вызываемой переменной необходима для того, чтобы избежать выполнения запросов в момент импорта вашего `models.py`:
    
```python
    from django.conf import settings
    from django.contrib.auth import get_user_model
    from django.db import models
    
    def get_sentinel_user():
        return get_user_model().objects.get_or_create(username='deleted')[0]
    
    class MyModel(models.Model):
        user = models.ForeignKey(
            settings.AUTH_USER_MODEL,
            on_delete=models.SET(get_sentinel_user),
        )
```
    
-   `DO_NOTHING`
    
    Не предпринимает никаких действий. Если ваша база данных обеспечивает ссылочную целостность, это вызовет [`IntegrityError`](https://django.fun/ru/docs/django/4.1/ref/exceptions/#django.db.IntegrityError "django.db.IntegrityError"), если вы вручную не добавите ограничение SQL `ON DELETE` в поле базы данных.
    
<br><br>
#### `limit_choices_to`
***

Устанавливает ограничение на доступные варианты выбора для этого поля, когда это поле отображается с помощью `ModelForm` или в админке (по умолчанию все объекты в наборе запросов доступны для выбора). Можно использовать словарь, объект [`Q`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.Q "django.db.models.Q") или вызываемый объект, возвращающий словарь или объект [`Q`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.Q "django.db.models.Q").

Например:
```python
staff_member = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    limit_choices_to={'is_staff': True},
)
```
заставляет соответствующее поле в `ModelForm` перечислять только `Users`, которые имеют `is_staff=True`. Это может быть полезно в админке Django.

Вызываемая форма может быть полезна, например, при использовании совместно с модулем Python `datetime` для ограничения выбора по диапазону дат. Например:

```python
def limit_pub_date_choices():
    return {'pub_date__lte': datetime.date.today()}

limit_choices_to = limit_pub_date_choices
```

Если `limit_choices_to` равно или возвращает [`Q объект`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.Q "django.db.models.Q"), который полезен для [сложных запросов](https://django.fun/ru/docs/django/4.1/topics/db/queries/#complex-lookups-with-q), тогда он будет влиять только на варианты, доступные в админке, если поле не указано в [`raw_id_fields`](https://django.fun/ru/docs/django/4.1/ref/contrib/admin/#django.contrib.admin.ModelAdmin.raw_id_fields "django.contrib.admin.ModelAdmin.raw_id_fields") в `ModelAdmin` для модели.

>Если для `limit_choices_to` используется вызываемый объект, он будет вызываться каждый раз, когда создается новая форма. Он также может быть вызван при проверке модели, например, командами управления или в админке. Админка создает наборы запросов для проверки входных данных своей формы в различных крайних случаях несколько раз, поэтому существует вероятность, что ваш вызываемый объект может быть вызван несколько раз.
<br><br>
#### `related_name`
***

Имя, используемое для отношения от связанного объекта обратно к нему. Это также значение по умолчанию для [`related_query_name`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ForeignKey.related_query_name "django.db.models.ForeignKey.related_query_name") (имя, используемое для имени обратного фильтра из целевой модели). Смотрите [документацию по связанным объектам](https://django.fun/ru/docs/django/4.1/topics/db/queries/#backwards-related-objects) для полного объяснения и примера. Обратите внимание, что вы должны установить это значение при определении отношений [абстрактных моделей](https://django.fun/ru/docs/django/4.1/topics/db/models/#abstract-base-classes); и когда вы делаете это, то [доступен специальный синтаксис](https://django.fun/ru/docs/django/4.1/topics/db/models/#abstract-related-name).

Если вы предпочитаете, чтобы Django не создавал обратную связь, установите для `related_name` значение `'+'` или завершите его с помощью `'+'`. Например, это гарантирует, что модель `User` не будет иметь обратной связи с этой моделью:

```python
user = models.ForeignKey(
    User,
    on_delete=models.CASCADE,
    related_name='+',
)
```
<br><br>
#### `related_query_name`
***

Имя, используемое для обратного имени фильтра из целевой модели. По умолчанию используется значение [`related_name`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.ForeignKey.related_name "django.db.models.ForeignKey.related_name") или [`default_related_name`](https://django.fun/ru/docs/django/4.1/ref/models/options/#django.db.models.Options.default_related_name "django.db.models.Options.default_related_name"), если установлено, в противном случае по умолчанию используется имя модели:

```python
# Declare the ForeignKey with related_query_name
class Tag(models.Model):
    article = models.ForeignKey(
        Article,
        on_delete=models.CASCADE,
        related_name="tags",
        related_query_name="tag",
    )
    name = models.CharField(max_length=255)

# That's now the name of the reverse filter
Article.objects.filter(tag__name="important")
```

Например `related_name`, `related_query_name` поддерживает интерполяцию меток приложения и классов посредством [специального синтаксиса](https://django.fun/ru/docs/django/4.1/topics/db/models/#abstract-related-name).
<br><br>
#### `to_field`
***

Поле связанного объекта, к которому относится отношение. По умолчанию Django использует первичный ключ связанного объекта. Если вы ссылаетесь на другое поле, это поле должно иметь `unique=True`.
<br><br>
#### `db_constraint`
***

Определяет, следует ли создать ограничение в базе данных для этого внешнего ключа. По умолчанию используется `True`, и это почти наверняка то, что вы хотите; установка этого значения в `False` может быть очень плохой для целостности данных. Тем не менее, вот несколько сценариев, где вы можете сделать это:

-   У вас есть устаревшие данные, которые не действительны.
-   Вы разделяете свою базу данных.

Если для этого параметра установлено значение `False`, доступ к связанному объекту, который не существует, вызовет исключение `DoesNotExist`.
<br><br>
#### `swappable`
***

Управляет реакцией фреймворка миграций, если этот `ForeignKey` указывает на заменяемую модель. Если это `True` - по умолчанию - тогда, если `ForeignKey` указывает на модель, которая соответствует текущему значению `settings.AUTH_USER_MODEL` (или другой сменной настройке модели), связь будет хранится в миграции с использованием ссылки на настройку, а не на модель напрямую.

Переопределять это значение `False` нужно только в том случае, если вы уверены, что ваша модель всегда должна указывать на замененную модель - например, если это модель профиля, разработанная специально для вашей пользовательской модели пользователя.

Установка этого значения в `False` не означает, что вы можете ссылаться на заменяемую модель, даже если она поменяна местами - `False` означает, что миграции, выполненные с помощью этого ForeignKey, всегда будут ссылаться на указанную вами точную модель (так что это не удастся легко сломать, если пользователь пытается запустить с моделью User, которую вы не поддерживаете, например).

Если сомневаетесь, оставьте для него значение по умолчанию `True`.