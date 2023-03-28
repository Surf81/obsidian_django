[примесь `FormMixin()`](#примесь%20`FormMixin()`)

## примесь `FormMixin()`
---
>django.views.generic.edit

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[примесь `ContextMixin()`](классы-представления-описание/base.md#примесь%20`ContextMixin()`)|
|`initial`|Справочник где ключ - имя поля формы, а значение - значение по умолчанию поля формы. По умолчанию = {}|
|`form_class`|По умолчанию `None`|
|`success_url`|По умолчанию `None`|
|`prefix`|По умолчанию `None`|

Методы:
|метод|описание|
|---|---|
|`get_initial()`|Вызывается из `self.get_form()`. Возвращает значение `self.initial` для передачи в создаваемую форму (значения полей формы по умолчанию|
|`get_prefix()`||
|`get_form_class()`||
|`get_form(form_class=None)`||
|`get_form_kwargs()`||
|`get_success_url()`||
|`form_valid()`||
|`form_invalid()`||
|`get_context_data()`||

