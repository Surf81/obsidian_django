[класс `View()`](#класс%20`View()`)
[класс `RedirectView()`](#класс%20`RedirectView()`)
[примесь `ContextMixin()`](#примесь%20`ContextMixin()`)
[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`)
[класс `TemplateView()`](#класс%20`TemplateView()`)


## класс `View()`
---
>django.views.generic.base

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|список имен допустимых HTTP методов. По умолчанию хранит список ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']|

Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|Устанавливает значение любых пользовательских атрибутов|
|`as_view(**initkwargs)`|Возвращает ссылку на функцию, которая, при обращении в ней, вызывает метод `self.setup()`, затем `self.dispatch()` и возвращает результат его исполнения (HTTP-Ответ)<br>Этод метод нужно вставить в маршрутизатор<br>- `initkwargs`: именованные аргументы для переопределения атрибутов класса-представления. Если атрибут у класса отсутствует, вызывается исключение `TypeError`|
|`setup(request, *args, **kwargs)`|Запускается при получении управления от маршрутизатора. Устанавливает значение атрибутов объекта класса: `self.head`, `self.request`, `self.args`, `self.kwargs`<br>>`*args` является устаревшим и используется только в старых версиях Django|
|`dispatch(request, *args, **kwargs)`|Запускается при получении управления от маршрутизатора после метода `self.setup()`. Проверяет тип HTTP-метода в `request`, сравнивает его со списком допустимых методов в `http_method_names` и возвращает результат исполнения метода класса с именем, совпадающим с именем HTTP-метода (Например `self.get()`). Иначе вызывает метод `self.http_method_not_allowed()`|
|`http_method_not_allowed(request, *args, **kwargs)`|Вызывается из `dispatch()`. Возвращает ответ типа `HttpResponseNotAllowed()`|
|`options(request, *args, **kwargs)`|Обрабатывает запрос, выполненный HTTP-методом `OPTIONS`|
|`get()`, `post()` ... и другие методы|Не определены в классе `View` и требуют ручного добавления. Без добавления обработчиков этих методов класс `View` бесполезный.<br>Формат объевления: `methodname(self, request, *args, **kwargs)`|

## класс `RedirectView()`
---
>django.views.generic.base

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](#класс%20`View()`)|
|`permanent`|Если `True`, метод обработки HTTP-запроса вернет постоянный редирект `HttpResponsePermanentRedirect()`, а иначе временный редирект `HttpResponseRedirect()`. По умолчанию `False`|
|`url`|Строка с URL-адресом, на который будет осуществляться редирект. Может содержать текстовые шаблоны `'%(key_name)s'` куда будут подставляться `**kwargs` из `self.get_redirect_url()`. По умолчанию `None`<br>Если `url` не задан, будет проверяться значение `self.pattern_name`|
|`pattern_name`|Строка с именем маршрута для редиректа. Используется, если не задан `self.url`. По умолчанию `None`|
|`query_string`|Если `True`, то из запроса `request` в URL добавятся ключи поиска (при наличии) (Например '?page=10'). По умолчанию `False`|

Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`get_redirect_url(*args, **kwargs)`|Вызывается из обработчика метода HTTP-запроса (Например `self.get()`). Возвращает путь `self.url` с заполнеными шаблонами или `reverse()` на `pattern_name`, в конце которых добавлены ключи поичка если `query_string`|
|`get(request, *args, **kwargs)`, `post()`, `head()`, `options()`, `delete()`, `put()`, `patch()`|Обработчик метода HTTP-запрса. Вызывается из `self.dispatch()`, возвращает HTTP-ответ редиректа на адрес `self.url` или на маршрутизатор `self.pattern_name` в формате класса `HttpResponseRedirect()` или `HttpResponsePermanentRedirect()` (зависит от `self.permanent`)|


## примесь `ContextMixin()`
---
>django.views.generic.base

Атрибуты:
|атрибут|описание|
|---|---|
|`extra_context`|Словарь, значения которого будут добавлены в контекст. По умолчанию `None`|

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

## примесь `TemplateResponseMixin()`
---
>django.views.generic.base

Атрибуты:
|атрибут|описание|
|---|---|
|`template_name`|Строка с именем шаблона. По умолчанию `None`. Обязательный к указанию (или переопределить `get_template_names()`|
|`template_engine`|"Движок", который должен обрабатывать шаблон. По умолчанию `None`|
|`response_class`|Ссылка на класс, с помощью которого будет осуществляться рендер шаблона и подготовка HTTP-ответа. По умолчанию `TemplateResponse`|
|`content_type`|`MIME`-тип ответа и его кодировка. По умолчанию `None` (используется кодировка по умолчанию)|

Методы:
|метод|описание|
|---|---|
|`get_template_names()`|Возвращает список имен шаблонов для передачи в `self.render_to_response()`. По умолчанию возвращает список из одного элемента, содержащего значение `self.template_name`|
|`render_to_response(context, **response_kwargs)`|Вызывается из методов `self.get()`, `self.post()` и т.д. Возвращает подготовленный HTTP-ответ для передачи в браузер. HTTP-ответ готовится с помощью класса, указанного в атрибуте `self.response_class`|

## класс `TemplateView()`
---
>django.views.generic.base

Настраиваемые атрибуты:
|атрибут|описание|
|---|---|
|`http_method_names`|[класс `View()`](#класс%20`View()`)|
|`extra_context`|[примесь `ContextMixin()`](#Примесь%20`ContextMixin()`)|
|`template_name`|[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`) Обязательный (или переопределить `get_template_names()`|
|`template_engine`|[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`)|
|`response_class`|[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`)|
|`content_type`|[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`)|

Доступные атрибуты:
|атрибут|описание|
|---|---|
|`request`|устанавливается методом `self.setup()`|
|`args`|устанавливается методом `self.setup()`|
|`kwargs`|устанавливается методом `self.setup()`|

Методы:
|метод|описание|
|---|---|
|`__init__(**kwargs)`|[класс `View()`](#класс%20`View()`)|
|`as_view(**initkwargs)`|[класс `View()`](#класс%20`View()`)|
|`setup(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`dispatch(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`http_method_not_allowed(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`options(request, *args, **kwargs)`|[класс `View()`](#класс%20`View()`)|
|`get(request, *args, **kwargs)`|Вызывается из `self.dispath()`. Обработчик метода HTTP-запроса `GET`.<br>Получает контекст, собранный с помощью `self.get_context_data()` и возвращает в `self.dispatch()` HTTP-ответ, подготовленный с помощью `self.render_to_response()`|
|`get_context_data(request, **kwargs)`|[примесь `ContextMixin()`](#примесь%20`ContextMixin()`)|
|`get_template_names()`|[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`)|
|`render_to_response(context, **response_kwargs)`|[примесь `TemplateResponseMixin()`](#примесь%20`TemplateResponseMixin()`)|
```python
context = {
	'view': self,
	**self.extra_context,
	**kwargs # kwargs, переданные в get_context_data()
}
```
