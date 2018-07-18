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