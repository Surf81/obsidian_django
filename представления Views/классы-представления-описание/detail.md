[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)
[примесь `SingleObjectTemplateResponseMixin()`](#примесь%20`SingleObjectTemplateResponseMixin()`)
[класс `DetailView()`](#класс%20`DetailView()`)


## примесь `SingleObjectMixin()`
---
>django.views.generic.detail

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`queryset`|Имеет приоритет перед `self.model`. Может быть указан объект типа `Manager()` или `QuerySet()` из которого `self.get_queryset()` будет возвращать все записи методом `self.queryset.all()`. По умолчанию `None`<br>Игнорируется если в `self.get_object()` передан queryset|
|`model`|Указывается ссылка на класс модели, из которой `self.get_queryset()` будет возращать все записи методом `self.model._default_manager.all()`, если не задан атрибут `self.queryset`. По умолчанию `None`<br>Игнорируется если в `self.get_object()` передан queryset|
|`slug_field`|Значение (строка), которое возвращается методом `self.get_slug_field()`. По умолчанию "slug" - имя поля модели, в котором хранится слаг|
|`context_object_name`|Значение (строка), которое возвращается методом `self.get_context_object_name()`. Альтернативное имя для значения `object` в контексте. По умолчанию `None`|
|`slug_url_kwarg`|Имя ключа атрибута `**kwargs`, полученного из маршрутизатора, в котором будет искаться значение слага. По умочанию "slug"|
|`pk_url_kwarg`|Имя ключа атрибута `**kwargs`, полученного из маршрутизатора, в котором будет искаться значение первичного ключа. По умолчанию "pk"|
|`query_pk_and_slug`|Если `True`, то при одновременном указании в `**kwargs` `pk` и `slug`, предпочтение при поиске объекта будет отдано `slug`. По умолчанию `False`|

Методы:
|метод|описание|
|---|---|
|`get_object(queryset=None)`|Вызывается из обработчика метода HTTP-запроса (Например `self.get()`). Возвращает объект модели с первичным ключем с именем `self.pk_url_kwarg` или `self.slug_url_kwarg`, значение которого ищется в `self.kwargs` (см. `self.setup()`)<br>Поиск объекта осуществляется в объекте, полученном при вызове методе через параметр `queryset`, а если он не передан, тогда поиск объекта осуществляется в объекте `QuerySet()`, возвращенном методом `get_queryset()`|
|`get_queryset()`|Вызывается из `self.get_object()`, если в него не был передан queryset. Возвращает набор объектов из объекта `self.queryset.all()`, а если он не задан, то из `self.model._defautl_manager.all()`|
|`get_slug_field()`|Вызывается из `self.get_object()`. Возвращает значение атрибута `self.slug_field` в котором хранится имя поля модели, в котором хранится слаг|
|`get_context_object_name(obj)`|Вызывается из `self.get_context_data()`. Задает в контексте альтернативное имя ключу `object`, в котором хранится экземпляр объекта. Альтернативным именем будет являться занчение, указанное в параметре `self.context_object_name`, а если оно не задано, тогда имя модели `obj._meta.model_name`|
|`get_context_data(request, **kwargs)`|Вызывается из методов `self.get()`, `self.post()` и прочих. Формирует справочник `context`, который затем будет передаваться в шаблон.<br>По умолчанию контекст содержит одно значение `view`, содержащее instance класса-контроллера. Так же в контекст добавляются значения `**kwargs` (из маршрутизатора) и `self.extra_context`<br>Если в экемпляре класса есть атрибут `self.object`, содержащий экземпляр объекта модели, то в контекст помещается значение `object`, а так же альтернативный ключ, возвращаемый `self.get_context_object_name()`|
```python
context = {
	'view': self,
	'object': # Объект модели
	<context_object_name>: # Объект модели, имя атрибута из get_context_object_name()
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

## примесь `SingleObjectTemplateResponseMixin()`
---
>django.views.generic.detail

Атрибуты:
|атрибут|описание|
|---|---|
|`template_name`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`) Если не указан, применяются `template_name_field` и `template_name_suffix`|
|`template_engine`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`response_class`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`content_type`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`template_name_field`|Имя поля модели, в котором хранится путь к шаблону. Проверяется если не задан атрибут `self.template_name`. По умолчанию `None`|
|`template_name_suffix`|Конечная часть имени шаблона. Используется если не задан атрибут `self.template_name`. По умолчанию "_detail"|

Методы:
|метод|описание|
|---|---|
|`get_template_names()`|Вызывается из `self.render_to_response()`. Возвращает список имен шаблонов. Если указан `self.template_name`, возвращает список из одного элемента, содержащего значение `self.template_name`<br>Иначе возврашает список из имени шаблона, указанного в поле модели с именем `self.template_name_field` (при наличии), а так же `<имя приложения>/<имя модели>_<суффикс>.html`, где суффикс, это значение `self.template_name_suffix`|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|

## класс `DetailView()`
---
>django.views.generic.detail

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`queryset`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`model`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`slug_field`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`context_object_name`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`slug_url_kwarg`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`pk_url_kwarg`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`query_pk_and_slug`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`template_name`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`) Если не указан, применяются `template_name_field` и `template_name_suffix`|
|`template_engine`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`response_class`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`content_type`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`template_name_field`|[примесь `SingleObjectTemplateResponseMixin()`](#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`template_name_suffix`|[примесь `SingleObjectTemplateResponseMixin()`](#примесь%20`SingleObjectTemplateResponseMixin()`)|

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
|`get_object(queryset=None)`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`get_queryset()`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`get_slug_field()`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`get_context_object_name(obj)`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`get_context_data(request, **kwargs)`|[примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
|`get_template_names()`|[примесь `SingleObjectTemplateResponseMixin()`](#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
```python
context = {
	'view': self,
	'object': # Объект модели self.object
	<context_object_name>: # Объект модели, имя атрибута из get_context_object_name()
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```
