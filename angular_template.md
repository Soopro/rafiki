# Angular Template

## Concept

It's use Angular template engine to make the Canvas Editor View. Almost all angular syntax is able to use in those template.

All context just like jinja2 template context, such as pages and Site metas and Theme meats.

**`script` tag is not allow in angular template. and `a` element will disable default behavior (unable link to anywhere.)  **

You can only upload the whole theme to our system for preview.
You can use our Develop tool `peon` to do it. (A secret key is required, which you can find from your security section of our system.)

***Remember! Any context is data object, those data can use in template whatever you want. It's not magic, and it's not javascript either. Don't be stupid. try print out the context if this document didn't let know what inside.***

<br><br>

## File naming

template file extensions must be `.tpl` only. otherwise will never work.

**Those template didn't support include and ng-include**

*you may use [Peon](https://github.com/Soopro/peon) to help you get .html `include` like jinja2.*

<br><br>

## Basic knowledges

Angular syntax must use on html tags. Also remember the if

*Remember it is different with jinja2, it's javascript. If you are not good enough to deal with it, please use Form view editor.*

***operators***

* `||`: or
* `&&`: and
* `!`: opposite, etc., `if not menu.primary`
* `indexOf`: check item is in the list or key is in the dict,
etc., `if menu.primary.indexOf(item)`
* `===`: equals jinja2 `is`.
* `==`: equals.
* `!=`: not equals.
* `()`: group conditions.
etc., `if (title != None or desc is None) and advanced`

***shortcodes***

* [%theme_url%]: shortcode for theme\_url, work everwhere in tpl. if you really want to have a str, you can use html entity. `&#37;` to replace the %. ```[&#37;theme\_url&#37;]```

------------------------------------------

### ng-init

Use `ng-init` to given a new context value in .tpl, equals jinja2 `set`

***Example***
```html
<div ng-init="my_value='any thing you want here'">
</div>
```

------------------------------------------

### ng-if

Use `ng-if` to rendering element by condition.

***Example***
```html
<div ng-if="menu.primary">
</div>
```

------------------------------------------

### ng-show

Use `ng-show` to display element by condition.

***Example***
```html
<div ng-show="menu.primary">
</div>
```

------------------------------------------

### ng-class or ng-style

Use `ng-class` or ng-style to define dynamic style sheets.

***Example***
```html
<div ng-style="{'bacgkround': 'red'}"></div>
<div ng-class="{'is-front': meta.is_front == true}"></div>
```

------------------------------------------

### Loop

Use `ng-repeat` to loop a list or dict

there some in loop context can be useful:

`$index`:	number iterator offset of the repeated element (0..length-1)
`$first`:	boolean	true if the repeated element is first in the iterator.
`$middle`:	boolean	true if the repeated element is between the first and last in the iterator.
`$last`:	boolean	true if the repeated element is last in the iterator.
`$even`:	boolean	true if the iterator position $index is even (otherwise false).
`$odd`:	boolean	true if the iterator position $index is odd (otherwise false).

***Example***

```html
<!-- list -->
<div ng-repeat="item in menu.primary">
<h1> {{item.title}} </h1>
</div>

<!-- dict -->
<div ng-repeat="(key, value) in menu.primary">
<h1> {{key}}: {{value}} </h1>
</div>
```

------------------------------------------

### i18n support *(Advanced)*

***this is for advanced usege***

1. share with jinja i18n translate file. (po, mo)
2. different way use in `.tpl` file (only):
  * `{{_('The text need to be translate')}}` translate text, support angular expression. albe to work with other expressions, etc., `{{_('transtext') + 'Yes'}}`, if no expression the translated text will render into page directly.
  * `<div any-attr="_('The text need to be translate')" >` same with above, but for attrbuites. while in attribute the text alway return with apos `'`. surround with `{{...}}` if you want render it directly.


------------------------------------------

<br><br>



## Directives

There is may custom directives for template. Also support native angular directives, but you have learn by you self.

------------------------------------------

### sup-editor-meta

Make a string meta content editable field.

* `default`: **[ str ]** Default value for this field.


***Example***

```html
<!-- your might have css display problem, then use `<div>` to wrap it. -->
<h1>
<div sup-editor-meta ng-model="meta.title" default="I am default text."></div>
<h1>
<!-- just like that -->
<h2 sup-editor-meta ng-model="meta.description">Default Content<h2>
```

***DO NOT***
```html
<!-- your might have css display problem, then use `<div>` to wrap it. -->
<h1>
  <div sup-editor-meta ng-model="meta.title">{{meta.title}}</div>
<h1>
```
This will massup rendering while you type any spical character. like this:
```
&amp;nbsp;&amp;amp;nbsp;Contact &amp;amp;amp;nbsp;Us
```
------------------------------------------

### sup-editor-media

Call media modal.

* `ng-model`: **[ dict:media ]** the bind data.
* `allow-types`: **[ str ]** allow media type, separate with ','. ('image, video, audio') default is 'image', use '*' for all.

bind it outside image. (bind it on <img> will kill the directive icons.)
if `sup-editor-media='video'` or `sup-editor-media='audio'` this media modal will only suppported video or audio at once, also the icon on canvas will change.


***Example***

```html
<div sup-editor-media ng-model="meta.featured_img" allow-types="image, video">
  <img src="{{meta.featured_img.src}}" title="{{meta.featured_img.title}}"/>
</div>
```

------------------------------------------

### sup-editor-widget-bg

Call background modal.

* `ng-model`: **[ dict:background ]** the bind media data.
* `allow-types`: **[ str ]** allow media type, separate with ','. ('image, video, audio') default is 'image', use '*' for all.

***Example***

```html
<div sup-editor-widget-bg ng-model="meta.background"
 allow-types="image, video"
 ng-style="{'background-image': meta.background.src}"
 class="{{meta.background.class}}">
</div>
```

------------------------------------------

### sup-editor-widget-script

Call script modal.

* `ng-model`: **[ dict:script ]** the bind script data.
  * `code`: **[ str:code ]** you code will be here.
  * `meta`: **[ dict ]** a meta for custom attributes (editor can not change it because there is not UI to modify, it's for future usage).
  * `status`: **[ int ]** display status `0:off` `1:on`
*tips:* You can preset some code inside the html tag as default, but the `<script>` tag is not allowed. The content inside tag will re-rendering after you change the data.

***Example***

```html
<div sup-editor-widget-script ng-model="meta.script"></div>
```

------------------------------------------

### sup-editor-widget-button

Call button modal.

* `ng-model`: **[ dict:button ]** the bind button data.

***Example***

```html
<button sup-editor-widget-button ng-model="meta.link_button"></button>
<a href="#" sup-editor-widget-button ng-model="meta.link_button"></a>
```

------------------------------------------

### sup-editor-widget-gallery

Call gallery modal.

*tips:* When you need display gallery as preview, you have to put first gallery item intro the gallery element.

* `ng-model`: **[ list:gallery ]** the bind gallery data.
* `allow-types`: **[ str ]** allow media types in gallery, separate with ','. ('image, video, audio') default is 'image', use '*' for all.

***Example***

```html
<div sup-editor-widget-gallery ng-model="meta.gallery"
 allow-types="image, video">
  <figure> <!-- figure is not really necessary here -->
    <img src="{{meta.gallery[0].src}}" title="{{meta.gallery[0].title}}">
    <p>{{meta.gallery[0].caption}}</p>
  </figure>
</div>
```

------------------------------------------

### sup-editor-widget-collect

Call collect modal.

*tips:* collect will return a list with dict (`name/value`).

* `ng-model`: **[ list:collect ]** the bind collect data.
* `limit`: **[ int ]** the limit of the collection, must > 0.
* `default`: **[ str ]** Its a str but must be specific format list.
  ```[{'name':'Key', 'value':'Value'}]```
  otherwise will covert to empty list.

***Example***

```html
<div swapper sup-editor-widget-collect ng-model="meta.swapper"
 default="[{'name':'0', 'value':'Swap Text'}]">
   <b ng-repeat="item in meta.swapper">{{item.value}}</b>
</div>
```

------------------------------------------

### sup-editor-widget-notes

Call notes modal.

*tips:* cnotes will return a list with dict (`title/content`).

* `ng-model`: **[ list:notes ]** the bind notes data.
* `limit`: **[ int ]** the limit of the notes, must > 0.
* `default`: **[ str ]** Its a str but must be specific format list.
  ```[{'title':'Title', 'Content':'.....'}]```
  otherwise will covert to empty list.

***Example***

```html
<div swapper sup-editor-widget-collect ng-model="meta.swapper"
 default="[{'title':'0', 'Content':'Swap Text'}]">
   <b ng-repeat="item in meta.swapper">{{item.Content}}</b>
</div>
```

------------------------------------------

### sup-query

Query contents and inject to `query` context.

`fields` and `metas` must use list or str, and following a complex rules:
1. 'type' -> query anything have 'type' key.
2. ['type', 'slug'] -> query anything have 'type' and 'slug' key.
3. [{'type':'car'}] -> query 'type' is 'car'.
4. [{'type':'car', not:true}] -> query 'type' is not 'car'.
5. [{'type':''}] or [{'type':true}] -> query any have 'type' key.
8. [{'type': false}] or [{'type': null}] -> query any don't have 'type' key.
7. [{'type':'', not:true}] -> query any don't have 'type' key.
8. [{'type': null, force:true}] -> query any 'type' is null.
9. ['type', {slug:'test'}}] -> query have 'type' key and 'slug' is 'test'.

* `fields`: **[ list ]** query fields (anything alse will skip). such as:
  1. `content_type`: **[ str:slug ]** <- use `type` for short.
  2. `slug`: **[ str:slug ]**
  3. `updated`: **[ int ]**
  4. `created`: **[ int ]**
  5. `locked`: **[ bool ]**
  6. `priority`: **[ int ]**
  7. `parent`: **[ str:slug ]**

* `metas`: **[ dict ]** query metas. any meta in a content can be use.

* `sortby`: **[ list/str ]** sort query results by keys, by default is sort by `ASC`, if the key is start with '-', the key will sort `DESC`. if multiple key is given by list, different key can use `DESC` or `ASC` by start with '-' or not.
  *** The result will automatically sort by 'priority' first as ASC, if you want custom it, just add the 'priority' in this feild by your self.***

* `length`: **[ int ]** how many entires in results.

* `desc`: **[ bool ]** want DESC the whole results, default is true.
* `priority`: **[ bool ]** default sort by priority at first, default is true.


* `ng-model`: **[ list ]** must be keep results in inside `query`. otherwise maybe will pollute other context. etc., `query.pages`.

If `sortby`, `length` not given, will use the options in theme config or system default.

You can also sup-query='post' for query content_type feild is 'post', please note this option will be replace if the `fields` attribute is given.

If the `fields` or `metas` value with list, that mean is only need match one of list element. If you want match multiple value with same key, you have to use several dict to describe it. etc., [{category: 'car'}, {category:'bike'}].
and I am not sure it will work fine, so good luck...

***Example***

```html
<div sup-query ng-model="query.works"
  fields="[{'type':'works'}]"
  metas="['featured_img', {'category':'car'}]"
  length="12"
  sortby="['date', 'updated']">
  <div ng-repeat="page in query.pages">
    <p>{{page.content}}</p>
  </div>
</div>

<div sup-query='post' ng-model="query.posts">
  <div ng-repeat="page in query.pages">
    <p>{{page.content}}</p>
  </div>
</div>
```

------------------------------------------

### sup-editor-open

Open to the new file. usually use with `sup-editor-content-query` to open the file from results repeat loop.

* `file`: **[ dict ]** the file data want to open.

***Example***

```html
<div ng-repeat="file in query.pages">
  <div sup-editor-open file="file"></div>
</div>
```

------------------------------------------

### sup-script

Create safe script intro template. only limit domain will be allow.
such as `libs.soopro.com`

* `path`: **[ str ]** the file data want to open.
* `source`: **[ str ]** default is libs_url. the source url, it will connect with path.
* `noSource`: **[ bool ]** kill the source part, use path directly.

***Example***

```html
<sup-script path="svg-sprites-render.min.js?md5=null"></sup-script>
```

------------------------------------------

### sup-set

Set new variable intro context `g` while in template. Usually for create default context need to be reuse. 

* `ng-model`: **[ any ]** data you want create.
* `value`: **[ any ]** for gestalt data, like `str`, `number`, `bool` ...
* `object`: **[ dict ]** or **[ list ]** for object data

***Example***

```html
<sup-set ng-model="g.default_img_360x360"
         value="{{theme_url+'/styles/default_img_360x360.png'}}"></sup-set>
<sup-set ng-model="g.opts"
         value="{{theme_meta.options}}"></sup-set>
```

------------------------------------------

### sup-angular-wysiwyg

Rich content editor for content.

* `ng-model`: **[ str ]** rich content.
* `default`: **[ str ]** default content for this field.

***Example***

```html
<div sup-angular-wysiwyg ng-model="content"
 default="Default Content..."><div>
```

------------------------------------------

<br><br>


## Filters

There is may custom filters for template. Also support native angular filters, but you have learn by you self.

------------------------------------------
### trust
**Output**

**[ str ]** Return a str processed by $sce.trustAsHtml.

**Usage**

```html
<div ng-bind-html="some.html | trust">
```

------------------------------------------

### thumbnail

**Output**

**[ str ]** Return a thumbnail url by given media src.

**Usage**

```html
<img src="{{media.src | thumbnail}}">
```

---------------------------------

### type

type filter is base on saltshaker.

**Output**

**[ list ]** Return a list match the content type.

**Usage**

```html
{% for page in pages | type('post'[, limit, sorty_by]) %}
{% endfor %}
```

`limit`: **[ int ]** how many result you want to have.

`sort_by`: **[ str ]** or **[ list ]** sort result by key. (this sort_by is share same rule with `rope`.)

---------------------------------

### url

A relative path for link will cause problem while the base_url is dynamic.
so use this filter every time you make sure a link is url.

For example button has link, but the link may be a relative path of the site.
you have to use  `button.link | url` to make sure it is url.

**Output**

**[ str ]** Return a absolute url.

**Usage**

```html
<a href="{{button.link | url}}">Button</a>
```

---------------------------------

### path

**Output**

**[ str ]** Return a current path after given url.

**Usage**

```html
<h2> Current page path is: {{page.url | path([remove_args]) }}"></h2>
<h2> Current request path is: {{request.url | path }}"></h2>
```

`remove_args`: **[ bool ]** default is `True`, remove arguments.

------------------------------------------

### args

**Output**

**[ dict ]** Return args from a url as a dict. default is {}

**Usage**

```html
<h2> Current page args is: {{request.url|args([unqiue])}}"></h2>
```

`unique`: **[ bool ]** return as a unquie value, otherwise might get a list if there multiple args with same key. default is `True`.

------------------------------------------

<br><br>


## Advance tricks

### Default content with context

Only supported few context, such as:
* `theme_url`
* `base_url`
* `libs_url`
* translate functions `_()` and `_t()`.

You can just type those context as default value for directives below:
* `sup-editor-meta`
* `sup-editor-widget-script`
* `sup-angular-wysiwyg`

Example:
```
<h2 sup-editor-meta ng-model="meta.description">Default {{_('Content')}}<h2>
```