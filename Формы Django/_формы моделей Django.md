
# Формы моделей Django

## Объявление с помощью фабрики классов
---
```python
from django.forms.models modelform_factory, 
from django.forms.fields import CharField
from django.forms.widgets import Select, NumberInput
from .models import MyModel

ClassForm = modelform_factory(
				MyModel,  # Модель, на основе которой создается форма
				# Одновременно может указываться или fields или exclude:
				fields = ('field2', 'field1'),  # Поля, отображаемые в форме
												# в указанном порядке
				exclude = ('field3', 'field4'), # Поля, игнорируемые формой
				labels = {'field1': 'Поле 1', 'field2': 'Поле 2'}, # Подписи полей
				# Если поля подпись не указана, используется verbose_name
				error_messages = {   # Сообщения об ошибках валидации
					'field1': {
						'null': 'Поле не должно быть пустым',
						'blank': 'Поле необходимо заполнить',
						# Все коды ошибок валидации указаны ниже
					},
					'field2': {
						'invalid': 'Неверный формат значения',
					}
				},
				help_texts = {    # Подсказки полей
					'field1': 'Введите значение такое-то',
					'field2': 'Введите значение такое-то'
				},
				field_classes = {   # Переопределение типа полей формы
					'field1': CharField
				},
				widgets = {   # Виджеты отображения элементов
					'field1': Select(attrs={'size': 8}),
					'field2': NumberInput()
				},
				form = <базовая форма, связанная с моделью>
			)
...
class MyCreateView(CreateView):
	form_class = ClassForm
```

## Быстрое объявление формы
---
> Описание полей совпадает с параметрами фабрики классов
```python
from django.forms.models import ModelForm
from django.forms.fields import DecimalField
from django.forms.widgets import Select
from .models import MyModel

class ClassForm(ModelForm):  # Необходимо наследоваться от ModelForm
							 # так как класс Meta описан в нем
	class Meta:
		model = MyModel
		fields = ('field1', 'field2')
		exclude = ()
		labels = {'field1': "Поле 1"}
		error_messages = {
			'field1': {
				'null': 'Сообщение об ошибке'
			}
		}
		help_texts = {
			'field1': 'Подсказка'
		}
		field_classes = {
			'field1': DecimalField
		}
		widgets = {
			'field1': Select(attrs={'size': 8})
		}
...
class MyCreateView(CreateView):
	form_class = ClassForm
```

## Полное объявление формы
---
```python
from django.forms.models import ModelForm
from django.forms.fields import DecimalField, CharField
from django.forms.widgets import Select, Textarea
from .models import MyModel1, MyModel2

class ClassForm(ModelForm):
	# Можно детально описать все или некоторые поля формы модели:
	field1 = CharField(label='Поле 1', widget=Textarea())
	field2 = ModelChoiceField(
				queryset=MyModel2.objects.all(),
				label='Поле 2',
				help_text='Подсказка',
				widget=Select(attrs={'size': 8})
			 )
	# Можно так же объявить дополнительные поля, отсутствующие в модели:
	new_field = CharField(label='new field')
	# Если в форме будут указаны поля с типом, не наследуемым от Field,
	# они будут отсеяны метаклассом (фабрикой класса)
	
	class Meta: # Можно указать настройки для оставшихся полей, не
				# объявленных детально
			    # Если поле объявлено детально, то быстрые настройки для него
			    # будут проигнорированы
		model = MyModel
		fields = ('field1', 'new_field', 'field2') # Если дополнительные поля
				# явно не указать в fields, они отобразятся в конце формы
		exclude = ()
		labels = {'field1': "Поле 1"}
		error_messages = {
			'field1': {
				'null': 'Сообщение об ошибке'
			}
		}
		help_texts = {
			'field1': 'Подсказка'
		}
		field_classes = {
			'field1': DecimalField
		}
		widgets = {
			'field1': Select(attrs={'size': 8})
		}

# Валидация
	def clean_field1(self):
		val = self.cleaned_data['field1']
		if val = 'Прошлогодний снег':
			raise ValidationError('К продаже не допускается')
		return val.capitalize() # Возвращенное в поле значение будет с большой буквы


...
class MyCreateView(CreateView):
	form_class = ClassForm
```


## Объект формы
---
```python
from django.core.exeptions import NON_FIELDS_ERRORS

class ClassForm(ModelForm):
	pass

form = ClassForm(request.POST)

# Валидация
form.is_valid() # `True` если валидация прошла успешно
form.errors # Словарь с ошибками валидации
form.errors['field1'] # Ошибки поля 1
form.errors[NON_FIELDS_ERRORS] # Ошибки, относящиеся ко всей форме

form.save() # Сохранить данные в связанную с моделью форму

f = form.save(commit=False) # Возвращает созданную, но еще не сохраненную запись модели
							# для возможности проверки значений полей или изменения
if not f.title:
	f.title = '123'
f.save()	

# Если в модели используется связь многие со многими и нужно применить
# save(commit=False)
form = MyForm(request.POST)
if form.is_valid():
	m = form.save(commit=False)
	# какие-то действия
	m.save()
	form.save_m2m() # Для создания связи многие-со-многими

form.cleaned_data # Словарь, в который помещаются отвалидированные значения
```

## [Коды ошибок валидации](../Валидация/сообщения%20об%20ошибках%20валидации.md)

## [Типы полей формы](Fields%20-%20поля%20формы.md)


