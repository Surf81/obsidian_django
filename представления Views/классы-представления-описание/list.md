
## примесь `MultipleObjectMixin()`
---
>django.views.generic.list

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|[классы-представления-описание/base.md#Примесь%20`ContextMixin()`)|

Методы:
|метод|описание|
|---|---|
|`get_context_data(request, **kwargs)`|Вызывается из методов `self.get()`, `self.post()` и прочих. Формирует справочник `context`, который затем будет передаваться в шаблон.<br>По умолчанию контекст содержит одно значение `view`, содержащее instance класса-контроллера. Так же в контекст добавляются значения `**kwargs` (из маршрутизатора) и `self.extra_context`|
```python
context = {
	'view': self,
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```