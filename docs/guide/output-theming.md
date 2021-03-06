Theming
=======

> Note: This section is under development.

A theme is a directory of view and layout files. Each file of the theme overrides corresponding file of an application
when rendered. A single application may use multiple themes and each may provide totally different experience. At any
time only one theme can be active.

> Note: Themes usually do not meant to be redistributed since views are too application specific. If you want to
  redistribute customized look and feel consider CSS and JavaScript files in form of [asset bundles](structure-assets.md) instead.

Configuring a theme
-------------------

Theme configuration is specified via `view` component of the application. In order to set up a theme to work with basic
application views the following should be in your application config file:

```php
'components' => [
    'view' => [
        'theme' => [
            'pathMap' => ['@app/views' => '@app/themes/basic'],
            'baseUrl' => '@web/themes/basic',
        ],
    ],
],
```

In the above `pathMap` defines a map of original paths to themed paths while `baseUrl` defines base URL for
resources referenced from theme files.

In our case `pathMap` is `['@app/views' => '@app/themes/basic']`. That means that every view in `@app/views` will be
first searched under `@app/themes/basic` and if a view exists in the theme directory it will be used instead of the
original view.

For example, with a configuration above a themed version of a view file `@app/views/site/index.php` will be
`@app/themes/basic/site/index.php`. It basically replaces `@app/views` in `@app/views/site/index.php` with
`@app/themes/basic`.

### Theming modules

In order to theme modules `pathMap` may look like the following:

```php
'components' => [
    'view' => [
        'theme' => [
            'pathMap' => [
                '@app/views' => '@app/themes/basic',
                '@app/modules' => '@app/themes/basic/modules', // <-- !!!
            ],
        ],
    ],
],
```

It will allow you to theme `@app/modules/blog/views/comment/index.php` with `@app/themes/basic/modules/blog/views/comment/index.php`.

### Theming widgets

In order to theme a widget view located at `@app/widgets/currency/views/index.php` you need the following config for
view component theme:

```php
'components' => [
    'view' => [
        'theme' => [
            'pathMap' => ['@app/widgets' => '@app/themes/basic/widgets'],
        ],
    ],
],
```

With the config above you can create themed version of `@app/widgets/currency/index.php` view in
`@app/themes/basic/widgets/currency/index.php`.

Using multiple paths
--------------------

It is possible to map a single path to multiple theme paths. For example,

```php
'pathMap' => [
    '@app/views' => [
        '@app/themes/christmas',
        '@app/themes/basic',
    ],
]
```

In this case, the view will be searched in `@app/themes/christmas/site/index.php` then if it's not found it will check
`@app/themes/basic/site/index.php`. If there's no view there as well application view will be used.

This ability is especially useful if you want to temporary or conditionally override some views.
