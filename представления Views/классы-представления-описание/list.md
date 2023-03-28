[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)
[примесь `MultipleObjectTemplateResponseMixin()`](#примесь%20`MultipleObjectTemplateResponseMixin()`)


## примесь `MultipleObjectMixin()`
---
>django.views.generic.list

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[`ContextMixin()`](классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|
|`allow_empty`|Если `True` - пагинатору разрешено создавать страницу, даже если отсустствуют объекты для вывода. Если `False` выйдет страница 404. По умолчанию `True`. Используется если задан `self.paginate_by`|
|`queryset`|Имеет приоритет перед `self.model`. Может быть указан объект типа `Manager()` или `QuerySet()` из которого `self.get_queryset()` будет возвращать все записи методом `self.queryset.all()`. По умолчанию `None`<br>Игнорируется если в `self.get_context_data()` передан object_list|
|`model`|Указывается ссылка на класс модели, из которой `self.get_queryset()` будет возращать все записи методом `self.model._default_manager.all()`, если не задан атрибут `self.queryset`. По умолчанию `None`<br>Игнорируется если в `self.get_context_data()` передан object_list|
|`paginate_by`|Количество элементов на одной странице пагинатора. По умолчанию `None`|
|`paginate_orphans`|Минимальное количество элементов на последней странице. Иначе остаток будет добавляться к предпоследней странице. По умолчанию = 0. Используется если задан `self.paginate_by`|
|`context_object_name`|Значение (строка), которое возвращается методом `self.get_context_object_name()`. Альтернативное имя для значения `object_list` в контексте. По умолчанию `None`|
|`paginator_class`|Ссылка на класс применяемого пагинатора. По умолчанию `Paginator`. Используется если задан `self.paginate_by`|
|`page_kwarg`|Имя ключа значения в `**kwargs`, полученного из маршрутизатора(имеет приоритет), или в методе `GET` HTTP-запроса в котором передается номер страницы для пагинатора. По умолчанию "page". Используется если задан `self.paginate_by`|
|`ordering`|Строка(Имя поля сортировки) или последовательность (имена полей сортировки). Если имя с минусом - обратная сортировка. Возвращается в `self.get_queryset()` через `self.get_ordering()`. По умолчанию `None`<br>Игнорируется если в `self.get_context_data()` передан object_list|
    
Методы:
|метод|описание|
|---|---|
|`get_queryset()`|Вызывается из обработчиков методов HTTP-запроса (Например `self.get()`) для задания значения параметру `self.object_list`. Возвращает набор объектов из объекта `self.queryset.all()`, а если он не задан, то из `self.model._defautl_manager.all()` и применяет к нему `order_by()` с параметром из `self.get_ordering()`|
|`get_ordering()`|Возвращает последовательность имен полей из параметра `self.ordering`, по которым должна осуществляться сортировка в `self.get_queryset()`|
|`paginate_queryset(queryset, page_size)`|Вызывается из `self.get_context_data()` если задан параметр `self.paginate_by`. Возвращает настройки пагинатора|
|`get_paginate_by(queryset)`|Возвращает в `self.get_context_data()` значение `self.paginate_by`. Атрибут queryset обязательный, но не используется и ни на что не влияет|
|`get_paginator(queryset, per_page, orphans=0, allow_empty_first_page=True, **kwargs)`|Вызывается из `self.paginate_queryset()`|
|`get_paginate_orphans()`|Вызывается из `self.paginate_sueryset()`. Возвращает значение `self.paginate_orphans`|
|`get_allow_empty()`|Вызывается из `self.paginate_sueryset()`. Возвращает значение `self.allow_empty`|
|`get_context_object_name(object_list)`|Вызывается из `self.get_context_data()`. Задает в контексте альтернативное имя ключу `object_list`, в котором хранится queryset. Альтернативным именем будет являться занчение, указанное в параметре `self.context_object_name`, а если оно не задано, тогда имя модели `obj._meta.model_name+"_list"`|
|`get_context_data(request, **kwargs)`|Вызывается из методов `self.get()`, `self.post()` и прочих. Формирует справочник `context`, который затем будет передаваться в шаблон.<br>По умолчанию контекст содержит одно значение `view`, содержащее instance класса-контроллера. Так же в контекст добавляются значения `**kwargs` (из маршрутизатора) и `self.extra_context`, а так же атрибуты: `paginator`, `page_obj`, `is_paginated`, `object_list` и атрибут с именем, возвращаемым `self.get_context_object_name()`|

```python
context = {
	'view': self,
	'paginator': None или экземпляр класса self.paginator_class,
	'page_obj': None или экземпляр класса Page() с записами текущей страницы,
	'is_paginated': True если применяется пагинатор или False,
	'object_list': queryset cо всеми записями,
	<get_context_object_name()>: Альтернативное хранилище object_list
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

## примесь `MultipleObjectTemplateResponseMixin()`
---
>django.views.generic.list

Атрибуты:
|атрибут|описание|
|---|---|
|`template_name`|[`TemplateResponseMixin()`](классы-представления-описание/base.md#Примесь%20`TemplateResponseMixin()`)|
|`template_engine`|[`TemplateResponseMixin()`](классы-представления-описание/base.md#Примесь%20`TemplateResponseMixin()`)|
|`response_class`|[`TemplateResponseMixin()`](классы-представления-описание/base.md#Примесь%20`TemplateResponseMixin()`)|
|`content_type`|[`TemplateResponseMixin()`](классы-представления-описание/base.md#Примесь%20`TemplateResponseMixin()`)|

Методы:
|метод|описание|
|---|---|
|`get_template_names()`|Вызывается из `self.render_to_response()`. Возвращает список имен шаблонов. Если указан `self.template_name`, возвращает список из одного элемента, содержащего значение `self.template_name`<br>Иначе возврашает список из одного значения `<имя приложения>/<имя модели>_list.html`|
|`render_to_response(context, **response_kwargs)`|[`TemplateResponseMixin()`](классы-представления-описание/base.md#Примесь%20`TemplateResponseMixin()`)|

## класс `ListView()`
---
>django.views.generic.list

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`extra_context`|[`ContextMixin()`](классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|
|`allow_empty`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`queryset`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`model`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`paginate_by`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`paginate_orphans`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`context_object_name`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`paginator_class`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`page_kwarg`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`ordering`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|

Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|
|`object`|устанавливается методом `self.get()`|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`options(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`get(request, *args, **kwargs)`|Определен в `BaseDetailView()`. Возвращает HTTP-ответ подготовленный методом `self.render_to_response()` с контекстом из `self.get_context_data()` для передачи браузеру в метод `self.dispatch()`. Заполняет атрибут класса `self.object`, который берется из `self.get_object()`|
|`get_queryset()`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_ordering()`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`paginate_queryset(queryset, page_size)`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_paginate_by(queryset)`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_paginator(queryset, per_page, orphans=0, allow_empty_first_page=True, **kwargs)`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_paginate_orphans()`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_allow_empty()`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_context_object_name(object_list)`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|
|`get_context_data(request, **kwargs)`|[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)|

