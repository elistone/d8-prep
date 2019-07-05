#Twig

> Twig is a modern template engine for PHP
> - Fast: Twig compiles templates down to plain optimized PHP code. The overhead compared to regular PHP code was reduced to the very minimum.
> - Secure: Twig has a sandbox mode to evaluate untrusted template code. This allows Twig to be used as a template language for applications where users may modify the template design.
> - Flexible: Twig is powered by a flexible lexer and parser. This allows the developer to define its own custom tags and filters, and create its own DSL.

\- https://twig.symfony.com/

Twig comes with Drupal 8 as it is apart of the Symfony framework, which is why it has been adopted as the new templating system.

## Attributes object

\- https://drupalize.me/tutorial/classes-and-attributes-twig-templates?p=2512

The attribute object is used to store attributes for html tags, and provides useful methods to update/make changes.

You will most find the attributes object used all over templates in drupal and will look something like this:

```twig
<div{{ attributes }}>
    ...
</div>
```

Which once rendered could look something like this:

```twig
<div class="node node--article" id="node-42" data-custom="a string of custom data">
    ...
</div>
```

All of the classes, id and even custom data came out of the attribute object.

### Add classes via template


Drupal likes you to add the classes via templates if you look in `/core/themes/bartik/templates/node.html.twig` you see an example of this:

```twig
{%
  set classes = [
    'node', # always adds 'node' class
    'node--type-' ~ node.bundle|clean_class, # create class such as node--type-article
    node.isPromoted() ? 'node--promoted', # boolan | will add class if node is promoted
    node.isSticky() ? 'node--sticky', # boolan | will add class if node is sticky
    not node.isPublished() ? 'node--unpublished', # boolan | will add class if node is unpublished
    view_mode ? 'node--view-mode-' ~ view_mode|clean_class,
    'clearfix', # always adds 'clearfix' class
  ]
%}
<article{{ attributes.addClass(classes) }}>
    ...
</article>
```

### Making changes

You can make changes to the attribute object using the following:

- `attributes.addClass('my-class')` - add a new class
- `attributes.removeClass('my-class')` - remove a class
- `attributes.addClass(classes)` - add multiple classes
- `attributes.setAttribute('id', 'myID')` - set attribute e.g. id
- `attributes.setAttribute('data-bundle', node.bundle)` - set custom data attribute
- `attributes.removeAttribute('id')` - remove attribute
- `attributes.addClass('hello').removeClass('goodbye')` - methods can be chained

All methods:

```twig
{# add class #}
<div{{ attributes.addClass('my-class') }}>
    ...
</div>

{# remove class #}
<div{{ attributes.removeClass('my-class') }}>
    ...
</div>

{# add multiple classes #}
{%
  set classes = [
    'my-class',
    'my-other-class',
  ]
%}
<div{{ attributes.addClass(classes) }}>
    ...
</div>

{# set id #}
<div{{ attributes.setAttribute('id', 'myID') }}>
    ...
</div>

{# set custom attribute #}
<div{{ attributes.setAttribute('data-bundle', node.bundle) }}>
    ...
</div>

{# remove custom attribute #}
<div{{ attributes.removeAttribute('id') }}>
    ...
</div>

{# methods can be chained #}
<div{{ attributes.addClass('hello').removeClass('goodbye') }}>
    ...
</div>

```