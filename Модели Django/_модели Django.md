
## Объявление модели

```python
from django.db import models

class <ИМЯ МОДЕЛИ>(models.Model):  
	
	# поля связи с другой моделью
    <поле> = models.<тип поля связи>(
	    <ИМЯ КЛАССА ДРУГОЙ МОДЕЛИ>,  # указывается в кавычках, если определение
								     # другой модели происходит позже
	    on_delete=models.<ПРАВИЛА УДАЛЕНИЯ>,
	    <прочие параметры поля при необходимости>
	)  
	
	# поля данных
    <поле> = models.<ТИП ПОЛЯ>(  
        <имя поля>=<значение>,  
        <имя поля>=[  
            <значение>,  
            <значение  
        ]  
    )  
    class Meta:  
        <параметр> = <значение>  
  
    def __str__(self):  
        return 
```

## Справочник

### [class Meta:](class%20Meta.md)
