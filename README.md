# 📝 Bolt Article Extension

This extension provides a "Article" field type, which is a powerful text editor
for creating rich content in your Bolt websites. It can work with grids,
embeds, typographic markup and media. The editor itself is developed by
[Imperavi][article], and is licensed for usage in Bolt.

This extension allows you to add fields of `type: article` in your
ContentTypes, as defined in `contenttypes.yaml`, like any other Field type.

## Installation

Note: Installation is not required if you've installed the default Bolt
project. In that case it's already present. If you've installed Bolt through
different means, you'll need to run the command below:

```bash
composer require bolt/article
```

After installation, you can add it to any ContentType in your
`contenttypes.yaml`, like any other field. For example:

```yaml
blogposts:
    name: Blogposts
    singular_name: Blogpost
    fields:
        title:
            type: text
        slug:
            type: slug
            uses: title
        content:
            type: article
```

The result will be like this:

![](https://user-images.githubusercontent.com/1833361/90976496-ecfd2400-e53d-11ea-8f15-037c97238785.png)

You can configure the editor in `config/extensions/bolt-article.yaml`. This
configuration affects all the instances of the Article field that you've
configured in your ContentTypes. The default configuration looks like this:

```yaml
# Settings for Bolt Article

default:
  plugins: [ blockcode, buttonlink, definedlinks, inlineformat, reorder, tags, underline ]
  source: true
  grid:
    classname: 'article-grid'
    columns: 12
    patterns:
      '4|4|4': 'col-4|col-4|col-4'
      '4|8': 'col-4|col-8'
      '6|6': 'col-6|col-6'
      '8|4': 'col-8|col-4'

plugins:
  ~

```

## Configuring the buttons

Bolt's version of Article ships with all the official plugins and options. you
can add or remove buttons and plugins by configuring them in the `plugins:`
parameters. Check the official Article documentation for [all available
buttons][buttons]. Note that some buttons might require you to enable the
corresponding plugin as well. See here for a list of
[the available plugins][plugins].

## Settings

Where applicable, you can add extra settings under the `default:` key in the
`bolt-article.yaml` configuration. See the documentation for available
settings.

Note that this documentation uses Javascript, whilst Bolt's configuration uses
Yaml. For example, the documentation for '[Paste][paste]' has this example:

```javascript
ArticleEditor('#entry', {
    css: '/your-article-dist-path/',
    paste: {
        autolink: false
    }
});
```

In `bolt-Article.yaml` you can add this as:

```yaml
default:
  buttons: [ …]
  plugins: [ … ]
  css: '/your-article-dist-path/'
  paste:
    autolink: false
```

## Adding custom plugins

If you've written your own block or plugin for Article according to the
documentation [for Creating Plugins][create-plugin] or
[for Creating Blocks][create-block], you can add it to the editor in Bolt, by
placing it in `/public/assets/article/_plugins`. Then, add it to the
`bolt-article.yaml` configuration:

```yaml
default:
  buttons: [ … ]
  plugins: [ … ]

plugins:
  myplugin: [ 'myplugin/myplugin.js', 'myplugin/myplugin.css' ]
```

## Grid setup

Using Article, you can allow the editors to insert Grid elements, splitting up
the "main column" in two or more columns, allowing them more variations for
laying out content.

By default, Article has a simple grid that consists of 12 columns, and allows
the editors to pick '4 + 4 + 4', '4 + 8', '6 + 6' and '8 + 4' as options. The
grid will create HTML like this:

```html
<div class="article-grid">
    <div class="col-6">
    …
    </div>
    <div class="col-6">
    …
    </div>
</div>
```

On the front end of the website, this needs to be styled correctly as CSS, in
order to work properly. This can be done in a few ways:

1. Linking to the default styles.
2. Copy the `grid.css` file to your theme to customize it.
3. Manually styling the grid, using your own CSS.

You can link the `grid.css` that ships with Article using the following:

```twig
<link rel="stylesheet" href="{{ asset('assets/article/grid.css', 'public') }}">
```

If you prefer to copy the file to your own theme, you can link it using:

```twig
<link rel="stylesheet" href="{{ asset('css/article_grid.css', 'theme') }}">
```

Alternatively, if your theme is using Bootstrap or Bulma, you can configure
Article to use the grid format from your preferred framework directly. See the
documentation on [using Bootstrap grid][bootstrap-grid] or [using Bulma
grid][bulma-grid].

-------

The part below is only for _developing_ the extension. Not required for general
usage of the extension in your Bolt Project

## Running PHPStan and Easy Codings Standard

First, make sure dependencies are installed:

```bash
COMPOSER_MEMORY_LIMIT=-1 composer update
```

And then run ECS:

```bash
vendor/bin/ecs check src --fix
```

[article]: https://imperavi.com/article/
[create-plugin]: https://imperavi.com/article/custom-block/how-to/create-a-plugin/
[create-block]: https://imperavi.com/article/custom-block/how-to/create-a-plugin/
[buttons]: https://imperavi.com/article/docs/get-started/
[plugins]: https://imperavi.com/article/plugins/
[paste]: https://imperavi.com/article/docs/settings/paste/
[bootstrap-grid]: https://imperavi.com/article/examples/frameworks/bootstrap-grid/
[bulma-grid]: https://imperavi.com/article/examples/frameworks/bulma-grid/