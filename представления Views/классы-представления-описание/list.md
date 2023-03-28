[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)

## примесь `MultipleObjectMixin()`
---
>django.views.generic.list

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[`ContextMixin()`](классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|
|`allow_empty`|По умолчанию `True`|
|`queryset`|Имеет приоритет перед `self.model`. Может быть указан объект типа `Manager()` или `QuerySet()` из которого `self.get_queryset()` будет возвращать все записи методом `self.queryset.all()`. По умолчанию `None`<br>Игнорируется если в `self.get_context_data()` передан object_list|
|`model`|Указывается ссылка на класс модели, из которой `self.get_queryset()` будет возращать все записи методом `self.model._default_manager.all()`, если не задан атрибут `self.queryset`. По умолчанию `None`<br>Игнорируется если в `self.get_context_data()` передан object_list|
|`paginate_by`|По умолчанию `None`|
|`paginate_orphans`|По умолчанию = 0|
|`context_object_name`|По умолчанию `None`|
|`paginator_class`|По умолчанию `Paginator`|
|`page_kwarg`|По умолчанию "page"|
|`ordering`|Строка(Имя поля (полей через запятую) сортировкиВозвращается `self.get_ordering()`. По умолчанию `None`|
    
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



