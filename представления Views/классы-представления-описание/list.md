[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)

## примесь `MultipleObjectMixin()`
---
>django.views.generic.list

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[`ContextMixin()`](классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|

Методы:
|метод|описание|
|---|---|
|`get_context_data(request, **kwargs)`|[`ContextMixin()`](классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|
```python
context = {
	'view': self,
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```



