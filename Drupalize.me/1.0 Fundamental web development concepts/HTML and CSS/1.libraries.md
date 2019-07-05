# JavaScript & CSS Libraries

> Cascading Style Sheets (CSS) can be included in a Drupal site in various ways. Whether you want to include CSS files from a custom theme or an external library, you will need to understand the concept of asset libraries and how to use them to include CSS for your Drupal site. To add HTML attributes such as classes or IDs, you will need to know some Twig.

\- https://drupalize.me/topic/css-drupal

## Libraries

In Drupal 7 there was a problem with JavaScript & CSS, this problem was that you would add the files to the `.info` file and the CSS & JavaScript would be loaded on every page even if they were not used or required.

**Enter libraries!** With Drupal 8 you can now create a libraries file to hold all your various assets and include them when you need, instead of just having them there all the time.

### How it works

A libraries file can be added to a `theme` or a `module` and will look something like this `THEMENAME.libraries.yml`, or `MODULENAME.libraries.yml` - *This file should be created even if you don't plan on using it straight away as for the case of modules to get this file picked up by drupal will require an uninstall and re-install.*

If you download a fresh copy of Drupal 8 and look in `core/themes/bartik` you will find a file called: `bartik.libraries.yml` the inside of this file looks like this:

```yaml
global-styling:
  version: VERSION
  css:
    base:
      css/base/elements.css: {}
    component:
      css/components/block.css: {}
      css/components/book.css: {}
      css/components/breadcrumb.css: {}
      css/components/captions.css: {}
      css/components/comments.css: {}
      css/components/contextual.css: {}
      css/components/demo-block.css: {}
      # @see https://www.drupal.org/node/2389735
      css/components/dropbutton.component.css: {}
      css/components/featured-top.css: {}
      css/components/feed-icon.css: {}
      css/components/field.css: {}
      css/components/form.css: {}
      css/components/forum.css: {}
      css/components/header.css: {}
      css/components/help.css: {}
      css/components/highlighted.css: {}
      css/components/item-list.css: {}
      css/components/list-group.css: {}
      css/components/list.css: {}
      css/components/main-content.css: {}
      css/components/menu.css: {}
      css/components/messages.css: {}
      css/components/node.css: {}
      css/components/node-preview.css: {}
      css/components/page-title.css: {}
      css/components/pager.css: {}
      css/components/panel.css: {}
      css/components/primary-menu.css: {}
      css/components/search-form.css: {}
      css/components/search-results.css: {}
      css/components/secondary-menu.css: {}
      css/components/shortcut.css: {}
      css/components/skip-link.css: {}
      css/components/sidebar.css: {}
      css/components/site-branding.css: {}
      css/components/site-footer.css: {}
      css/components/table.css: {}
      css/components/tablesort-indicator.css: {}
      css/components/tabs.css: {}
      css/components/text-formatted.css: {}
      css/components/toolbar.css: {}
      css/components/featured-bottom.css: {}
      css/components/password-suggestions.css: {}
      css/components/ui.widget.css: {}
      # @see https://www.drupal.org/node/2389735
      css/components/vertical-tabs.component.css: {}
      css/components/views.css: {}
      css/components/buttons.css: {}
      css/components/image-button.css: {}
      css/components/ui-dialog.css: {}
    layout:
      css/layout.css: {}
    theme:
      css/colors.css: {}
      css/print.css: { media: print }

messages:
  version: VERSION
  css:
    component:
      css/components/messages.css: { preprocess: false }

color.preview:
  version: VERSION
  css:
    theme:
      color/preview.css: {}
  js:
    color/preview.js: {}
  dependencies:
    - color/drupal.color

maintenance_page:
  version: VERSION
  css:
    theme:
      css/maintenance-page.css: {}
  dependencies:
    - system/maintenance
    - bartik/global-styling
```

What is happening is the theme `bartik` is defining some assets based upon a it's library name, for example the top item is named: `global-styling` and you can see it pulls in css for things such as `toolbar`, `breadcrumb` and `buttons`. With libraries you are not limited to one per file, at the bottom you can see another called: `maintenance_page` which has `dependencies` this means that when this is loaded it will also require it's dependencies in this case it loads in the system maintenance libary as well as the bartik global styling.

```yaml
maintenance_page: # name of the library
  version: VERSION
  css:
    theme:
      css/maintenance-page.css: {} # css to pull in
  dependencies:
    - system/maintenance # require the system maintenance library
    - bartik/global-styling # require the bartik global styling
```

### Understanding what it all means

![Understanding what it all means](https://drupalize.me/sites/default/files/tutorials/define-an-asset-library.png)

\- Image from: https://drupalize.me/tutorial/define-asset-library?p=2512

> Drupal 8 follows a SMACSS-style categorization and CSS files are loaded first based on their category, and then by the order they are listed within a given category. 

The categories are as follows:

1. `base` -- CSS reset/normalize plus HTML element styling
1. `layout` -- macro arrangement of a page, including any grid systems
1. `component` -- discrete, reusable UI elements
1. `state` -- styles that deal with client-side changes to components
1. `theme` -- purely visual styling (look-and-feel) for a component


### Attaching a library

There are three ways to attach a library:

1. Globally via the `.info` file - generally a bad idea (unless making a theme).
1. Conditionally, attaching to something specific via a preprocess hook.
1. Inside a twig file

One thing we always need is the `THEMENAME` or `MODULENAME` plus the `LIBRARYNAME`, generally it will end up looking something like the following: `THEMENAME/LIBRARYNAME` / `MODULENAME/LIBRARYNAME`.

#### Globally

To attach a library globally you can add it to your `.info` file like this:

```yaml
name: Bartik
type: theme
base theme: classy
description: 'A flexible, recolorable theme with many regions and a responsive, mobile-first layout.'
package: Core
libraries:
  - bartik/global-styling #here we are pulling in the global-styling library using THEMENAME/LIBRARYNAME
```

#### Conditionally

You can attach a library for many reasons, such as to a certain page, content type or even if something is included on the page for example to all blocks.

To do this you would use a `hook_preprocess_page` or `page_attachments_alter`.

**Examples:**

Attach our library to a certain page:
```php
/**
* Implements hook_page_attachments_alter
*/
function THEMENAME_page_attachments_alter(array &$page) {
  // Get the current path.
  $path = $current_path = \Drupal::service('path.current')->getPath();
  // If we're on the node listing page, add our library.
  if ($path === '/node') {
    $page['#attached']['library'][] = 'THEMENAME/LIBRARYNAME';
  }
}
```

Attach our library depending on other certain criteria 
```php
/**
* Implements hook_preprocess_page() for PAGE document templates.
*/
function THEMENAME_preprocess_page(&$variables) {
  // check if the current page is the front one
  if ($variables['is_front'] === TRUE) {
    $variables['#attached']['library'][] = 'THEMENAME/LIBRARYNAME';
  }
}
```

#### Twig file

Sometimes you might only want CSS to load if a specif twig file is loaded, you can do this via a preprocess but it is easier to do it directly in the twig file.

There is a twig filter called `attach_library` you can pass in the `THEMENAME/LIBRARYNAM` to get a library loaded when that twig file is rendered.

**Example:**

```twig
{# only attach our library if this is node 1 #} 
{% if node.id == 1 %}
  {{ attach_library('THEMENAME/LIBRARYNAM') }}
{% endif %}
```