# Всякие разные полезные штуки

##### Форматирование номера телефона в ссылках
```php
<a class="nav__phone-first" href="tel:{'phone1'|config|ereplace: '/[^0-9]/' : ''}">{'phone1'|config}</a>
```

##### SEO Breadcrumbs

```php
{'pdoCrumbs' | snippet : [
    'showHome' => 1,
    'outputSeparator' => '<li> / </li>',
    'tplWrapper' => '@INLINE <ul itemscope="" itemtype="http://schema.org/BreadcrumbList" id="breadcrumbs">{$output}</ul>',
    'tpl' => '@INLINE
        <li><span itemscope="" itemprop="itemListElement" itemtype="http://schema.org/ListItem">
             <a title="{$menutitle}" itemprop="item" href="{$link}"><span itemprop="name">{$menutitle}</span><meta itemprop="position" content="{$idx}"></a>
        </span></li>',
    'tplCurrent' => '@INLINE
        <li><span itemscope="" itemprop="itemListElement" itemtype="http://schema.org/ListItem">
        <span itemprop="name">{$menutitle}</span><meta itemprop="position" content="{$idx}">
    </span></li>'
]}
```

##### Типичный вызов AjaxForm

```php
{$_modx->runSnippet('!AjaxForm',[
    'form' => '@FILE:chunks/forms/callback.form.tpl',
    'hooks' => 'spam,email,FormItSaveForm',
    'emailTpl' => 'callbck.form.email.tpl',
    'emailSubject' => ('mailSubject'|config),
    'emailTo' => ('siteMail'|config),
    'emailFrom' => ('emailFrom'|config),
    'validate' => 'name:required,email:required',
    'validationErrorMessage' =>'<h4>В форме содержатся ошибки! Заполните, пожалуйста, требуемые поля</h4>',
    'successMessage' => '<h4>Заявка отправлена. Скоро с вами свяжется наш менеджер!</h4>'
])}
```

#### Поменять стартовый экран на экран MiniShop2 или Tickets

заходим в настройки системы, далее в фильтре по ключу отыскиваем 2 значения:

+ welcome_action меняем с welcome на mgr/orders
+ welcome_namespace с core на minishop2
##### Tickets
+ welcome_action меняем с welcome на home
+ welcome_namespace с core на tickets


#### Выбрать соседние товары с превью

```php
{$_modx->runSnippet('pdoNeighbors',[
    'class' => 'modResource',
    'leftJoin' => [
        'Image' => [
            'class' => 'msProductFile',
            'on' => 'modResource.id = Image.product_id AND Image.parent = 0',
        ],
        'Thumb' => [
            'class' => 'msProductFile',
            'on' => 'Image.id = Thumb.parent AND Thumb.path LIKE "%medium%"',
        ]
    ],
    'select' => [
        'modResource' => '*',
        'Image' => 'Image.url as image',
        'Thumb' => 'Thumb.url as thumb',
    ],
])}
```

#### Вырезать теги Fenom (нужно, например, при выводе мета-тегов)

{$_modx->resource.content|preg_replace:'~(?<=\{)(.*?)(?=\})~':'')}

#### Регулярка для валидации мобильного номера по РФ

```php
$phone = preg_replace('/[^\d\+]+/', '', $mobilephone);
$phone = preg_replace('/^8([\d]{10})$/', '+7$1', $phone);
```

#### Office авторизовать админа в контексте WEB
Плагин на событие OnWebPageInit

и код
```php
<?php
if ($modx->event->name == 'OnWebPageInit') {
	if ($modx->user->hasSessionContext('mgr') && !$modx->user->hasSessionContext($modx->context->key)) {
		$modx->user->addSessionContext($modx->context->key);
	}
}
```

#### TinyMCE для introtext
Создаем плагин с текстом:

```php
<?php
$modx->regClientStartupHTMLBlock('<script>Ext.onReady(function() {
if(MODx.loadRTE) MODx.loadRTE("modx-resource-introtext");
});</script>');
```

В списке событий найти событие OnDocFormRender и поставить на против него галочку
После сохранения плагина поле introtext будет представлено в виде редактора TinyMCE