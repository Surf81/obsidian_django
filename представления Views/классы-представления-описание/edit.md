[примесь `FormMixin()`](#примесь%20`FormMixin()`)
[класс `ProcessFormView()`](#класс%20`ProcessFormView()`)
[класс `FormView()`](#класс%20`FormView()`)

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
|`form_valid()`|Вызывается из обработчика метода HTTP-запроса `POST` в случае подтверждения валидности формы. Возвращает HTTP-ответ с редиректом на URL, полученный через `self.get_success_url()`|
|`form_invalid()`|Вызывается из обработчика метода HTTP-запроса `POST` в случае не подтверждения валидности формы. Возвращает HTTP-ответ, возвращенный из `self.render_to_response()`|
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

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_class`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`success_url`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`prefix`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|

Методы:
|метод|описание|
|---|---|
|`get_initial()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_prefix()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_class()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form(form_class=None)`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_form_kwargs()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_success_url()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_valid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`form_invalid()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
|`get_context_data()`|[примесь `FormMixin()`](#примесь%20`FormMixin()`)|
```python
context = {
	'view': self,
	'form': объект формы, переданный из self.post(), или иначе из self.form_class
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```

