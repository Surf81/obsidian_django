[примесь `FormMixin()`](#примесь%20`FormMixin()`)
[класс `ProcessFormView()`](#класс%20`ProcessFormView()`)
[класс `FormView()`](#класс%20`FormView()`)
[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)
[класс `CreateView()`](#класс%20`CreateView()`)
[класс `UpdateView()`](#класс%20`UpdateView()`)
[примесь `DeletionMixin()`](#примесь%20`DeletionMixin()`)
[класс `DeleteView()`](#класс%20`DeleteView()`)

## примесь `FormMixin()`
---
>django.views.generic.edit

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|Справочник где ключ - имя поля формы, а значение - значение по умолчанию поля формы. По умолчанию = {}|
|`form_class`|Ссылка на класс формы. Обязательный к заполнению. По умолчанию `None`|
|`success_url`|Строка или `reverse_lazy()` - путь, куда осуществляется перенаправление после проверки валидности формы. Обязательный к заполнению. По умолчанию `None`|
|`prefix`|Строка. Если в один шаблон передается несколько одинаковых форм, можно указать префикс, чтобы поля отличались. По умолчанию `None`|

Методы:
|метод|описание|
|---|---|
|`get_initial()`|Вызывается из `self.get_form()`. Возвращает значение `self.initial` для передачи в создаваемую форму (значения полей формы по умолчанию|
|`get_prefix()`|Вызывается из `self.get_form_kwargs()`. Возвращает значение `self.prefix` для передачи в параметры формы|
|`get_form_class()`|Вызывается из `self.get_form()`. Возвращает значение `self.form_class`|
|`get_form(form_class=None)`|Вызывается из `self.get_context_data()` если параметр `form` не передан из обработчика метода HTTP-запроса. Возвращает объект формы|
|`get_form_kwargs()`|Вызывается из `self.get_form()`. Возвращает данные для заполнения формы.|
|`get_success_url()`|Вызывается из `self.form_valid()`. Возвращает строку с адресом URL из `self.success_url`|
|`form_valid(form)`|Вызывается из обработчика метода HTTP-запроса `POST` в случае подтверждения валидности формы. Возвращает HTTP-ответ с редиректом на URL, полученный через `self.get_success_url()`. Атрибут `form` не используется|
|`form_invalid(form)`|Вызывается из обработчика метода HTTP-запроса `POST` в случае не подтверждения валидности формы. Возвращает HTTP-ответ, возвращенный из `self.render_to_response()`|
|`get_context_data()`|Вызывается из методов `self.get()`, `self.post()` и прочих. Формирует справочник `context`, который затем будет передаваться в шаблон.<br>По умолчанию контекст содержит одно значение `view`, содержащее instance класса-контроллера. Так же в контекст добавляются значения `**kwargs` (из маршрутизатора) и `self.extra_context` а так же атрибут `form`, содержащий объект класса формы, полученный из обработчика метода HTTP-запроса, или иначе из `self.form_class`|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

## класс `ProcessFormView()`
---
>django.views.generic.edit

>Данный класс используется для наследования. Сам по себе не используется

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|

Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`options(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`get(request, *args, **kwargs)`|Возвращает HTTP-ответ подготовленный методом `self.render_to_response()` с контекстом из `self.get_context_data()` для передачи браузеру в метод `self.dispatch()`.|
|`post(request, *args, **kwargs)`,<br>`put()`|Проверяет форму, полученную методом `self.get_form()` на валидность и в зависимости от результата возвращает HTTP-ответ подготовленный методом `self.form_valid()` или `self.form_invalid()`|


## класс `FormView()`
---
>django.views.generic.edit

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_class`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`success_url`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`prefix`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`template_name`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`template_engine`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`response_class`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`content_type`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|

Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`options(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`get(request, *args, **kwargs)`|[класс `ProcessFormView()`](#класс%20`ProcessFormView()`)|
|`post(request, *args, **kwargs)`,<br>`put()`|[класс `ProcessFormView()`](#класс%20`ProcessFormView()`)|
|`get_initial()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_prefix()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_class()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form(form_class=None)`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_kwargs()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_success_url()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_valid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_invalid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_context_data()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_template_names()`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

## примесь `ModelFormMixin()`
---
>django.views.generic.edit

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_class`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`success_url`|[примесь `FormMixin()`](#примесь%20`FormMixin()`) Не обязательно указывать если у модели настроен `get_absolute_url()`|
|`prefix`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`queryset`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`model`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`slug_field`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`context_object_name`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`slug_url_kwarg`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`pk_url_kwarg`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`query_pk_and_slug`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`fields`|Список полей модели для отображения в форме. Применяется если не указан `self.form_class`. По умолчанию `None` (не должен указываться совместно с `form_class`|

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`object`|Создается при успешной валидации формы. Содержит объект модели|

Методы:
|метод|описание|
|---|---|
|`get_initial()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_prefix()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_class()`|Вызывается из `self.get_form()`. Возвращает значение `self.form_class`, если он задан. Если не задан, то с помощью фабрики форм создается новый класс формы с полями модели, указанными в `self.fields`|
|`get_form(form_class=None)`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_kwargs()`|Вызывается из `self.get_form()`. Возвращает данные для заполнения формы. Добавляет к форме instance для взаимодействия с моделью|
|`get_success_url()`|Вызывается из `self.form_valid()`. Возвращает строку с адресом URL из `self.success_url` для переадрессации в случае подтверждения валидности формы. Если он не задан делается попытка вернуть из объекта модели `get_absolute_url()`|
|`form_valid()`|Вызывается из обработчика метода HTTP-запроса `POST` в случае подтверждения валидности формы. Сохраняет данные формы в модель и возвращает HTTP-ответ с редиректом на URL, полученный через `self.get_success_url()`|
|`form_invalid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_context_data()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`), [примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_object(queryset=None)`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_queryset()`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Не знает о существовании формы и не может получить queryset на основании модели, указанной в форме|
|`get_slug_field()`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_context_object_name(obj)`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class
	'object': # Объект модели
	<context_object_name>: # Объект модели, имя атрибута из get_context_object_name()	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

## класс `CreateView()`
---
>django.views.generic.edit

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_class`|[примесь `FormMixin()`](#примесь%20`FormMixin()`) Не указывается вмести с `self.fields`|
|`success_url`|[примесь `FormMixin()`](#примесь%20`FormMixin()`) Не обязательно указывать если у модели настроен `get_absolute_url()`|
|`prefix`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`queryset`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Приоритет перед `self.model`|
|`model`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Если не задан `self.queryset`|
|`slug_field`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`context_object_name`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`slug_url_kwarg`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`pk_url_kwarg`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`query_pk_and_slug`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`fields`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`) Только в отдельности от `self.form_class`|
|`template_name`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`) Если не указан, применяются `template_name_field` и `template_name_suffix`|
|`template_engine`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`response_class`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`content_type`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`template_name_field`|[примесь `SingleObjectTemplateResponseMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`template_name_suffix`|Конечная часть имени шаблона. Используется если не задан атрибут `self.template_name`. По умолчанию `"_form"`|
|
Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|
|`object`|Содержит объект модели для передачи в контекст|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`options(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`get(request, *args, **kwargs)`|[класс `ProcessFormView()`](#класс%20`ProcessFormView()`) Предварительно очищает `self.object`|
|`post(request, *args, **kwargs)`,<br>`put()`|[класс `ProcessFormView()`](#класс%20`ProcessFormView()`) Предварительно очищает `self.object`|
|`get_initial()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_prefix()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_class()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`get_form(form_class=None)`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_kwargs()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`get_success_url()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`form_valid()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`form_invalid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_context_data()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`), [примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_object(queryset=None)`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_queryset()`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Не знает о существовании формы и не может получить queryset на основании модели, указанной в форме|
|`get_slug_field()`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_context_object_name(obj)`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_template_names()`|[примесь `SingleObjectTemplateResponseMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class
	'object': # Объект модели
	<context_object_name>: # Объект модели, имя атрибута из get_context_object_name()	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```


## класс `UpdateView()`
---
>django.views.generic.edit

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_class`|[примесь `FormMixin()`](#примесь%20`FormMixin()`) Не указывается вмести с `self.fields`|
|`success_url`|[примесь `FormMixin()`](#примесь%20`FormMixin()`) Не обязательно указывать если у модели настроен `get_absolute_url()`|
|`prefix`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`queryset`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Приоритет перед `self.model`|
|`model`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Если не задан `self.queryset`|
|`slug_field`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`context_object_name`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`slug_url_kwarg`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`pk_url_kwarg`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`query_pk_and_slug`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`fields`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`) Только в отдельности от `self.form_class`|
|`template_name`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`) Если не указан, применяются `template_name_field` и `template_name_suffix`|
|`template_engine`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`response_class`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`content_type`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`template_name_field`|[примесь `SingleObjectTemplateResponseMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`template_name_suffix`|Конечная часть имени шаблона. Используется если не задан атрибут `self.template_name`. По умолчанию `"_form"`|
|
Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|
|`object`|Содержит объект модели для передачи в контекст|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`options(request, *args, **kwargs)`|[класс `View()`](классы-представления-описание/base.md#класс%20`View()`)|
|`get(request, *args, **kwargs)`|[класс `ProcessFormView()`](#класс%20`ProcessFormView()`) Предварительно устанавливает `self.object` = `self.get_object()`|
|`post(request, *args, **kwargs)`,<br>`put()`|[класс `ProcessFormView()`](#класс%20`ProcessFormView()`) Предварительно устанавливает `self.object` = `self.get_object()`|
|`get_initial()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_prefix()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_class()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`get_form(form_class=None)`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_kwargs()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`get_success_url()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`form_valid()`|[примесь `ModelFormMixin()`](#примесь%20`ModelFormMixin()`)|
|`form_invalid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_context_data()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`), [примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_object(queryset=None)`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_queryset()`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`) Не знает о существовании формы и не может получить queryset на основании модели, указанной в форме|
|`get_slug_field()`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_context_object_name(obj)`|[примесь `SingleObjectMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectMixin()`)|
|`get_template_names()`|[примесь `SingleObjectTemplateResponseMixin()`](классы-представления-описание/detail.md#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class
	'object': # Объект модели
	<context_object_name>: # Объект модели, имя атрибута из get_context_object_name()	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

## примесь `DeletionMixin()`
---
>django.views.generic.edit

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`success_url`|Строка или `reverse_lazy()` - путь, куда осуществляется перенаправление после проверки валидности формы. Обязательный к заполнению. По умолчанию `None`|

Методы:
|метод|описание|
|---|---|
|`delete(request, *args, **kwargs)`|Вызывается из обработчика метода HTTP-запроса `self.post()`. Удаляет объект, полученный через `self.get_object()` и возвращает HTTP-ответ с редиректом на адрес, полученный через `self.get_success_url()`|
|`post(request, *args, **kwargs)`|Вызывается из `self.dispatch()` и возвращает в него HTTP-ответ, полученный от `self.delete()`|
|`get_success_url()`|Вызывается из `self.delete()`. Возвращает строку с адресом URL из `self.success_url`|


## класс `DeleteView()`
---
>django.views.generic.edit

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
|`template_name_suffix`|Конечная часть имени шаблона. Используется если не задан атрибут `self.template_name`. По умолчанию `"_confirm_delete"`|
|`success_url`|[примесь `DeletionMixin()`](#примесь%20`DeletionMixin()`)|
|`initial`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_class`|Ссылка на класс формы. По умолчанию `Form`|
|`success_url`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`prefix`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|


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
|`__init__(**kwargs)`|Устанавливает значение любых пользовательских атрибутов. Проверка безопасности метода `self.delete()`|
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
|`get_template_names()`|[примесь `SingleObjectTemplateResponseMixin()`](#примесь%20`SingleObjectTemplateResponseMixin()`)|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](классы-представления-описание/base.md#примесь%20`TemplateResponseMixin()`)|
|`delete(request, *args, **kwargs)`|[примесь `DeletionMixin()`](#примесь%20`DeletionMixin()`)|
|`post(request, *args, **kwargs)`|Проверяет форму, полученную методом `self.get_form()` на валидность и в зависимости от результата возвращает HTTP-ответ подготовленный методом `self.form_valid()` или `self.form_invalid()`|
|`get_success_url()`|[примесь `DeletionMixin()`](#примесь%20`DeletionMixin()`)|
|`get_initial()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_prefix()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_class()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form(form_class=None)`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_kwargs()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_success_url()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_valid(form)`|Вызывается из обработчика метода HTTP-запроса `POST` в случае подтверждения валидности формы. Удаляет объект модели. Возвращает HTTP-ответ с редиректом на URL, полученный через `self.get_success_url()`. Атрибут `form` не используется|
|`form_invalid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_context_data()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`), [примесь `SingleObjectMixin()`](#примесь%20`SingleObjectMixin()`)|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class	
	'object': # Объект модели self.object
	<context_object_name>: # Объект модели, имя атрибута из get_context_object_name()
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```


