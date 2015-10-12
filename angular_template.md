# Angular Template

## Concept

It's use Angular template engine to make the Canvas Editor View. Almost all angular syntax is able to use in those template.

All pages and Site metas and Theme meats is in context, just like jinja2 template context.

**`script` element is not allow in angular template. and `a` element will disable default behavior (unable link to anywhere.)  **

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
* `!`: opposite, etc., `if not menu.priamry`
* `indexOf`: check item is in the list or key is in the dict,
etc., `if menu.priamry.indexOf(item)`
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
<div ng-if="menu.priamry">
</div>
```

------------------------------------------

### ng-show

Use `ng-show` to display element by condition.

***Example***
```html
<div ng-show="menu.priamry">
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
<div ng-repeat="item in menu.priamry">
<h1> {{item.title}} </h1>
</div>

<!-- dict -->
<div ng-repeat="(key, value) in menu.priamry">
<h1> {{key}}: {{value}} </h1>
</div>
```

------------------------------------------


<br><br>


## Extended data type

### dict:media

* `src`: media src.
* `title`: media title.
* `link`: media link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

### list:gallery

A list of media

* `src`: media src.
* `title`: media title.
* `caption`: media caption.
* `link`: media link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

### dict:button

* `title`: button title.
* `link`: button link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

### str:code

A string might contain front-end codes. like js/html/css....


<br><br>



## Directives

There is may custom directives for template. Also support native angular directives, but you have learn by you self.

------------------------------------------

### sup-editor-meta

Make a string meta content editable field.

* `default`: **[ str ]** Default value for this field.

bind it beside the text you want make it to editable.

***Example***

```html
<!-- your might have css display problem, then use `<div>` to wrap it. -->
<h1>
<div sup-editor-meta default="I am default text."></div>
<h1>
<!-- just like that -->
<h2 sup-editor-meta default="I am default text."><h2>
```

------------------------------------------

### sup-editor-media

Call media modal.

* `ng-model`: **[ dict:media ]** the bind data.

bind it outside image. (bind it on <img> will kill the directive icons.)

***Example***

```html
<div sup-editor-media ng-model="meta.featured_img">
  <img src="{{meta.featured_img.src}}" title="{{meta.featured_img.title}}"/>
</div>
```

------------------------------------------

### sup-editor-widget-bg

Call background modal.

* `ng-model`: **[ dict:media ]** the bind media data.

***Example***

```html
<div sup-editor-widget-bg ng-model="meta.background"
  ng-style="{'background-image': meta.background.src}"
  class="{{meta.background.class}}">
</div>
```

------------------------------------------

### sup-editor-widget-script

Call script modal.

* `ng-model`: **[ str:code ]** the bind script data.

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

***Example***

```html
<div sup-editor-widget-gallery ng-model="meta.gallery">
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

### sup-editor-content-query

Query contents and inject to `query` context.

`feilds` and `metas` must use key/value, and following a complex rules:
1. The value can be **[ str ]** or **[ list ]**
2. The key start with `-` will query content which don't have this key.
3. The value start with `-` will exclude content which has the value.
4. The value start with `+` or not operators will remove from exclude.
5. The value could be '*' if you want every thing with that key.
6. The complex rules is execute by orders.
7. Most time you don't have do that complex ...


* `feilds`: **[ dict ]** query fields. such as:
  1. `content_types` <- use `type` for short.
  2. `alias`
  3. 'updated'
  4. 'created'
  5. `loccked`

* `metas`: **[ dict ]** query metas. any meta in a content can be use.

* `sortby`: **[ list/str ]** sort query results by keys, if the key is start with '-', the key will sort `ASC`. if multiple key is given by list, different key can use `DESC` or `ASC` by start with '-' or not. 
  *** The result will automatically sort by 'priority' first as ASC, if you want custom it, just add the 'priority' in this feild by your self.***

* `length`: **[ int ]** how many entires in results.

* `ng-model`: **[ list ]** must be keep results in inside `query`. otherwise maybe will pollute other context.
etc., `query.pages`.

***Example***

```html
<div sup-editor-content-query ng-model="query.pages"
  fields="{'type':'post'}"
  metas="{'category': '*'}">
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

Set new variable intro context `g` while in template. Usually for create default context need to be reuse. must be keep results in inside `query`. otherwise maybe will pollute other context.

* `ng-model`: **[ any ]** data you want create.

***Example***

```html
<sup-set ng-model="g.default_img_360x360"
         value="{{theme_url+'/styles/default_img_360x360.png'}}"></sup-set>
```

------------------------------------------

### sup-angular-wysiwyg

Rich content editor for content.

* `ng-model`: **[ str ]** rich content.
* `default`: **[ str ]** default content for this field.

***Example***

```html
<div sup-angular-wysiwyg ng-model="g.default_img_360x360"
 default="Default Content..."><div>
```

------------------------------------------

<br><br>


## Filters

There is may custom filters for template. Also support native angular filters, but you have learn by you self.

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

---------------------------------
