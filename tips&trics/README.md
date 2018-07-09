# Всякие разные полезные штуки

##### Форматирование номера телефона в ссылках
```php
<a class="nav__phone-first" href="tel:{'phone1'|config|ereplace: '/[^0-9]/' : ''}">{'phone1'|config}</a>
```