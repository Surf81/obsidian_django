Классы, определенные в этом модуле, создают ограничения базы данных. Они добавляются в опцию модели [Meta.constaraints](class%20Meta.md#constraints)

Необходимо указывать уникальное имя для ограничения. Как таковое, вы не можете нормально указать ограничение на абстрактный базовый класс, поскольку опция [Meta.constaraints](class%20Meta.md#constraints) наследуется подклассами, с точно такими же значениями атрибутов (включая `name`) каждый раз. Чтобы обойти коллизии имен, часть имени может содержать `'%(app_label)s'` и `'%(class)s'`, которые заменяются, соответственно, строчными меткой приложения и именем класса конкретной модели. Например, `CheckConstraint(check=Q(age__gte=18), name='%(app_label)s_%(class)s_is_adult')`.

- Классы:
	[BaseConstraint](#class%20`BaseConstraint`)
	[CheckConstraint](#class%20`CheckConstraint`)
	[UniqueConstraint](#class%20`UniqueConstraint`)
<br><br>
## class `BaseConstraint`
***
```python
BaseConstraint(
	name,
	violation_error_message=None
)
```

Базовый класс для всех ограничений. Подклассы должны реализовывать методы `constraint_sql()`, `create_sql()`, `remove_sql()` и `validate()`

#### name
Уникальное имя для ограничения

#### violation_error_message
Сообщение при ошибке валидации
<br><br>
## class `CheckConstraint`
***
```python
CheckConstraint(
	*,
	check,
	name,
	violation_error_message=None
)
```

Создает контрольное ограничение в базе данных
### check

Объект [`Q`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.Q "django.db.models.Q") или булево [`Expression`](https://django.fun/ru/docs/django/4.1/ref/models/expressions/#django.db.models.Expression "django.db.models.Expression"), который определяет проверку, которую вы хотите, чтобы ограничение выполняло.

Например, 
```python
CheckConstraint(check=Q(age__gte=18), name='age_gte_18') 
```
гарантирует, что поле возраст никогда не будет меньше 18.

>Проверки с нулевыми полями в Oracle должны включать условие, допускающее значения `NULL`, чтобы [`validate()`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.BaseConstraint.validate "django.db.models.BaseConstraint.validate") вела себя так же, как проверка ограничений. Например, если `age` является нулевым полем:
```python
CheckConstraint(check=Q(age__gte=18) | Q(age__isnull=True), name='age_gte_18')
```
<br><br>
## class `UniqueConstraint`
***
```python
UniqueConstraint(
	*expressions,
	fields=(),
	name=None,
	condition=None,
	deferrable=None,
	include=None,
	opclasses=(),
	violation_error_message=None
)
```

Создает уникальное ограничение в базе данных.
<br><br>
### `expressions`

Позиционный аргумент `*expressions` позволяет создавать функциональные уникальные ограничения на выражения и функции базы данных.

Например:
```python
UniqueConstraint(Lower('name').desc(), 'category', name='unique_lower_name_category')
```
создает уникальное ограничение на строчные значения поля `name` в порядке убывания и поля `category` в порядке возрастания по умолчанию.

Функциональные уникальные ограничения имеют те же ограничения базы данных, что и [Index.expressions](Индексы%20модели.md#expressions)
<br><br>
### `fields`

Список имен полей, определяющий уникальный набор столбцов, на которые необходимо наложить ограничение.

Например, 
```python
UniqueConstraint(fields=['room', 'date'], name='unique_booking')
```
гарантирует, что каждый номер может быть забронирован только один раз на каждую дату.
<br><br>
### `condition`

Объект [`Q`](https://django.fun/ru/docs/django/4.1/ref/models/querysets/#django.db.models.Q "django.db.models.Q"), который определяет условие, которое вы хотите, чтобы ограничение выполнялось.

Например:
```python
UniqueConstraint(fields=['user'], condition=Q(status='DRAFT'), name='unique_draft_user')
```
гарантирует, что у каждого пользователя будет только один черновик.

>Эти условия имеют те же ограничения базы данных, что и [Index.condition](Индексы%20модели.md#condition)
<br><br>
### `deferrable`

Установите этот параметр для создания откладываемого уникального ограничения. Принимаются значения `Deferrable.DEFERRED` или `Deferrable.IMMEDIATE`. Например:

```python
from django.db.models import Deferrable, UniqueConstraint

UniqueConstraint(
    name='unique_order',
    fields=['order'],
    deferrable=Deferrable.DEFERRED,
)
```

По умолчанию ограничения не откладываются. Отложенное ограничение не будет выполняться до конца транзакции. Немедленное ограничение будет выполняться сразу после каждой команды.

>MySQL, MariaDB и SQLite.
Откладываемые уникальные ограничения игнорируются в MySQL, MariaDB и SQLite, поскольку ни один из них их не поддерживает.

>Предупреждение
Отложенные уникальные ограничения могут привести к ошибке [performance penalty](https://www.postgresql.org/docs/current/sql-createtable.html#id-1.9.3.85.9.4).
<br><br>
### `include`

Список или кортеж имен полей, которые должны быть включены в охватывающий уникальный индекс в качестве неключевых столбцов. Это позволяет использовать сканирование только по индексу для запросов, которые выбирают только включенные поля ([`include`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.UniqueConstraint.include "django.db.models.UniqueConstraint.include")) и фильтруют только по уникальным полям ([`fields`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.UniqueConstraint.fields "django.db.models.UniqueConstraint.fields")).

Например:
```python
UniqueConstraint(name='unique_booking', fields=['room', 'date'], include=['full_name'])
```
позволит фильтровать по `room` и `date`, также выбирая `full_name`, при этом получая данные только из индекса.

>`include` поддерживается только в PostgreSQL.

Столбцы без ключей имеют те же ограничения базы данных, что и [Index.include](Индексы%20модели.md#include)
<br><br>
### `opclasses`

Имена операторов [PostgreSQL operator classes](https://www.postgresql.org/docs/current/indexes-opclass.html) для использования для этого уникального индекса. Если вам требуется пользовательский класс оператора, вы должны предоставить его для каждого поля в индексе.

Например:
```python
UniqueConstraint(name='unique_username', fields=['username'], opclasses=['varchar_pattern_ops'])
```
создает уникальный индекс на `username`, используя `varchar_pattern_ops`.

>`opclasses` игнорируются для всех баз данных, кроме PostgreSQL.
<br><br>
### `violation_error_message`

Сообщение об ошибке, используемое, когда `ValidationError` возникает во время [model validation](https://django.fun/ru/docs/django/4.1/ref/models/instances/#validating-objects). По умолчанию используется [`BaseConstraint.violation_error_message`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.BaseConstraint.violation_error_message "django.db.models.BaseConstraint.violation_error_message").

Это сообщение _не используется_ для [`UniqueConstraint`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.UniqueConstraint "django.db.models.UniqueConstraint")с [`fields`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.UniqueConstraint.fields "django.db.models.UniqueConstraint.fields") и без [`condition`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.UniqueConstraint.condition "django.db.models.UniqueConstraint.condition"). Такие [`UniqueConstraint`](https://django.fun/ru/docs/django/4.1/ref/models/constraints/#django.db.models.UniqueConstraint "django.db.models.UniqueConstraint")показывают то же сообщение, что и ограничения, определенные с помощью [`Field.unique`](https://django.fun/ru/docs/django/4.1/ref/models/fields/#django.db.models.Field.unique "django.db.models.Field.unique") или в [`Meta.unique_together`](https://django.fun/ru/docs/django/4.1/ref/models/options/#django.db.models.Options.constraints "django.db.models.Options.constraints").