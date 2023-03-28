[примесь `MultipleObjectMixin()`](#примесь%20`MultipleObjectMixin()`)

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
|`get_context_data(request, **kwargs)`|[`ContextMixin()`](классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|
|`get_queryset()`|Вызывается из обработчиков методов HTTP-запроса (Например `self.get()`) для задания значения параметру `self.object_list`. Возвращает набор объектов из объекта `self.queryset.all()`, а если он не задан, то из `self.model._defautl_manager.all()` и применяет к нему `order_by()` с параметром из `self.get_ordering()`|
|`get_ordering()`|Возвращает последовательность имен полей из параметра `self.ordering`, по которым должна осуществляться сортировка в `self.get_queryset()`|
|`paginate_queryset(queryset, page_size)`|Вызывается из `self.get_context_data()` если задан параметр `self.paginate_by`. Возвращает настройки пагинатора|
|`get_paginate_by(queryset)`|Возвращает в `self.get_context_data()` значение `self.paginate_by`. Атрибут queryset обязательный, но не используется и ни на что не влияет|
|`get_paginator(queryset, per_page, orphans=0, allow_empty_first_page=True, **kwargs)`|Вызывается из `self.paginate_queryset()`|
|`get_paginate_orphans()`|Вызывается из `self.paginate_sueryset()`. Возвращает значение `self.paginate_orphans`|
|`get_allow_empty()`|Возвращает значение `self.paginate_orphans`|


```python
context = {
	'view': self,
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```



