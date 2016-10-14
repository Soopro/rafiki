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

### sup-widget-text

Make a string meta content editable field.

* `default`: **[ str ]** Default value for this field.


***Example***

```html
<!-- your might have css display problem, then use `<div>` to wrap it. -->
<h1>
<div sup-widget-text ng-model="meta.title" default="I am default text."></div>
<h1>
<!-- just like that -->
<h2 sup-widget-text ng-model="meta.description">Default Content<h2>
```

***DO NOT***
```html
<!-- your might have css display problem, then use `<div>` to wrap it. -->
<h1>
  <div sup-widget-text ng-model="meta.title">{{meta.title}}</div>
<h1>
```
This will massup rendering while you type any spical character. like this:
```
&amp;nbsp;&amp;amp;nbsp;Contact &amp;amp;amp;nbsp;Us
```
------------------------------------------

### sup-widget-media

Call media modal.

* `ng-model`: **[ dict:media ]** the bind data.
* `allow-types`: **[ str ]** allow media type, separate with ','. ('image, video, audio') default is 'image', use '*' for all.

bind it outside image. (bind it on <img> will kill the directive icons.)
if `sup-widget-media='video'` or `sup-widget-media='audio'` this media modal will only suppported video or audio at once, also the icon on canvas will change.


***Example***

```html
<div sup-widget-media ng-model="meta.featured_img" allow-types="image, video">
  <img src="{{meta.featured_img.src}}" title="{{meta.featured_img.title}}"/>
</div>
```

------------------------------------------

### sup-widget-bg

Call background modal.

* `ng-model`: **[ dict:bg ]** the bind media data.
* `allow-types`: **[ str ]** allow media type, separate with ','. ('image, video, audio') default is 'image', use '*' for all.
* `presets`: **[ list ]** Presets of background settings.
  1. `key`: **[ str ]** Usually is same as the css class name.
  2. `label`: **[ str ]** It is for name when display.
  
***Example***

```html
<div sup-widget-bg ng-model="meta.bg"
     allow-types="image, video"
     presets="[{key: 'light-bg', label: _('Light Background')}]"
     ng-style="{'background-image': meta.background.src}"
 class="{{meta.background.class}}">
</div>
```

------------------------------------------

### sup-widget-script

Call script modal.

* `ng-model`: **[ dict:script ]** the bind script data.
  * `code`: **[ str:code ]** you code will be here.
  * `meta`: **[ dict ]** a meta for custom attributes (editor can not change it because there is not UI to modify, it's for future usage).
  * `status`: **[ int ]** display status `0:off` `1:on`
*tips:* You can preset some code inside the html tag as default, but the `<script>` tag is not allowed. The content inside tag will re-rendering after you change the data.

***Example***

```html
<div sup-widget-script ng-model="meta.script"></div>
```

------------------------------------------

### sup-widget-button

Call button modal.

* `ng-model`: **[ dict:button ]** the bind button data.

***Example***

```html
<button sup-widget-button ng-model="meta.link"></button>
<a href="#" sup-widget-button ng-model="meta.link"></a>
```

------------------------------------------

### sup-widget-carousel

Call carousel modal.

*tips:* Mostly carousel is large image or video, only need display the first one while on editor.

* `ng-model`: **[ list:series ]** the bind series data.
* `limit`: **[ int ]** limit of this series, default and max is 12.
* `allow-types`: **[ str ]** allow media types in carousel, separate with ','. ('image, video') default is 'image', use '*' for all.


***Example***

```html
<div sup-widget-carousel
     ng-model="meta.carousel"
     limit="6"
     allow-types="image, video">
  <figure ng-if="meta.carousel.length > 0">
    <img ng-src="{{meta.carousel[0].src}}" title="{{meta.carousel[0].title}}">
    <p>{{meta.carousel[0].caption}}</p>
  </figure>
</div>
```

------------------------------------------

### sup-widget-series

Call series modal.

*tips:* series is similar with carousel, but all series usually display all items in editor. Series also allow to sync a taxonomy terms. 

* `ng-model`: **[ list:series ]** the bind series data.
* `limit`: **[ int ]** limit of this series, default and max is 32.
* `allow-types`: **[ str ]** allow media types in series, separate with ','. ('image, video, audio') default is 'image', use '*' for all.
* `taxonomy`: **[ str ]** allow sync content type.
* `default`: **[ str ]** default data for this series, should be a list of dict with json string format.
  ```
   [
     {
       'title': _('Item Title'),
       'caption': _('Click here to edit series item.'),
       'src': theme_url+'/styles/default_thumbnail.png'
     },
     {
       'title': _('Item Title'),
       'caption': _('Click here to edit series item.'),
       'src': theme_url+'/styles/default_thumbnail.png'
     }
   ]
  ```

**Sub Directive**

Each item of series require a sub directive `sup-widget-series-item`.
It must be work with ng-repeat as usual, because it is need $index to find the the item in list. Except the one for add new item, this is required to add a value to this attrbute, such as `new`, `add` or `create`. `sup-widget-series-item="new"` etc., otherwise the add series item will not funciton.

***Example***

```html
<div sup-widget-series
     ng-model="meta.series"
     limit="24"
     allow-types="*"
     taxonomy="category">
  <figure sup-widget-series-item
          ng-repeat="item in meta.series">
    <img ng-src="{{item.src}}" title="{{item.title}}">
    <p>{{item.caption}}</p>
  </figure>
  <figure sup-widget-series-item="new">
    <img ng-src="{{some_img_src}}" title="Default Title">
    <p>{{'Add new Series Item'}}</p>
  </figure>
</div>
```

------------------------------------------

### sup-widget-lines

Call lines modal.

*tips:* lines will return a list with dict (`name/value`).

* `ng-model`: **[ list ]** a list of dict, {'text': .... }.
* `limit`: **[ int ]** the limit of the list, default is 0, no limit at all.
* `default`: **[ str ]** Its a str but must be a str or a dict with `text` key.
  ```[{'text':'Text...'}]``` otherwise will ignore.

***Example***

```html
<div sup-widget-lines ng-model="meta.swapper"
     default="['Swap Text', {'text': 'Text'}]">
   <b ng-repeat="item in meta.swapper">{{item.text}}</b>
</div>
```

------------------------------------------

### sup-widget-option

Call option modal.

*tips:* option will return a list with dict (`name/value`).

* `ng-model`: **[ dict ]** the options data.
* `structure`: **[ list ]** Its a dict with specific format.
  ```
  {
    {'key': 'email', 'label': 'Email'},
    {'key': 'subject', 'label': 'Subject', 'default': 'Subject text here.'},
    {'key': 'open', 'label': 'Test', 'switch': true},
    {'key': 'choice', 'label': 'Test2', 'default': 'xxx', 'select':['xxx', 'Title']},
  }
  ```
  otherwise will ignore. `switch` and `select` key for defined the field type.
  1. `swicth`: **[ bool ]** mark as a switch. also can use 0 or 1.
  2. `select`: **[ list ]** mark as a select by define a selection list, the value of each item can be `string` only.

***Example***

```html
<div sup-widget-option
     ng-model="meta.mailto"

     structure="{
       {'key': 'email', 'label': 'Email'},
       {'key': 'subject', 'label': 'Subject', 'default': 'Subject text here.'},
       {'key': 'open', 'label': 'Test', 'switch': true},
       {'key': 'choice', 'label': 'Test2', 'default': 'xxx',
        'select':['xxx', 'Title']},
     }"></div>
```

------------------------------------------

### sup-widget-form

Call form modal.

*tips:* form will return a dict for a web form.

* `ng-model`: **[ dict ]** the web form data.

***Tips***
see widget data models document to understand the outputs.

***Example***

```html
<div sup-widget-form
     ng-model="meta.form">
   ...
</div>
```

------------------------------------------

### sup-query

Query contents and inject to `query` context.

* `attrs`: **[ list ]** query fields such as:
  1. `content_type`: **[ str:slug ]** <- use `type` for short.
  2. `slug`: **[ str:slug ]**
  3. `updated`: **[ int ]**
  4. `created`: **[ int ]**
  6. `priority`: **[ int ]**
  7. `parent`: **[ str:slug ]** anything else will turn to a `meta` attrs. such as `date` will to `meta.date`. `attrs` also following a complex rules:
    1. 'type' -> query anything have 'type' key.
    2. ['type', 'slug'] -> query anything have 'type' and 'slug' key.
    3. [{'type':'car'}] -> query 'type' is 'car'.
    4. [{'type':'car', not:true}] -> query 'type' is not 'car'.
    5. [{'type':''}] or [{'type':true}] -> query any have 'type' key.
    8. [{'type': false}] or [{'type': null}] -> query any don't have 'type' key.
    7. [{'type':'', not:true}] -> query any don't have 'type' key.
    8. [{'type': null, force:true}] -> query any 'type' is null.
    9. ['type', {slug:'test'}}] -> query have 'type' key and 'slug' is 'test'.

* `sortby`: **[ list/str ]** sort query results by keys, by default is sort by `DESC`, if the key is start with '+', the key will sort `ASC`.
  *** The result will automatically sort by 'priority' first as ASC, if you want custom it, just add the 'priority' in this feild by your self.***

* `perpage`: **[ int ]** how many entires in results.

* `paged`: **[ int ]** offset of current results.

* `with-content`: **[ bool ]** with content field or not.

* `priority`: **[ bool ]** default sort by priority at first, default is true.

* `ng-model`: **[ list ]** must be keep results in inside `query`. otherwise maybe will pollute other context. etc., `query.pages`.

If `sortby`, `length` not given, will use the options in theme config or system default.

You can also sup-query='post' for query content_type feild is 'post', please note this option will be replace if the `fields` attribute is given.

If the `fields` or `metas` value with list, that mean is only need match one of list element. If you want match multiple value with same key, you have to use several dict to describe it. etc., [{category: 'car'}, {category:'bike'}].
and I am not sure it will work fine, so good luck...

**Results**

the results will be a dict.

* contents: **[ list ]** a list of contents.
* paged: current paged.
* total_pages: max pages with current prepage.
* count: 12 current result length.
* total_count: total contents in database.


```json
{
    "contents": [....],
    "paged": 0,
    "count": 12,
    "total_pages": 4,
    "total_count": 48
}
    
```

***Example***

```html
<div sup-query
     ng-model="query.works"
     attrs="['works', {'category':'car'}]"
     perpage="12"
     paged="0"
     priority="true"
     sortby="['date', 'updated']"
     with-content="false">
  <div ng-repeat="works in query.works.contents">
    <p>{{works}}</p>
  </div>
</div>

<div sup-query='post' ng-model="query.posts">
  <div ng-repeat="post in query.posts.contents">
    <p>{{post}}</p>
  </div>
</div>
```
------------------------------------------

### sup-query-sides

Query next and previous contents by given content id and conditions. Then inject to `query` context.

* `pid`: **[ str ]** A string of content id.
* `attrs`: **[ list ]** Same as `sup-query`.
* `limit`: **[ int ]** Query limit of before and after the content id. Default is 1, maximum is 6.
* `priority`: **[ bool ]** Same as `sup-query`.
* `sortby`: **[ list ]** Same as `sup-query`.

***Example***

```html
<div sup-query-sides
     ng-model="query.works_sides"
     pid="57d53d6b1697963fdbd2ee25"
     attrs="['works', {'category':'car'}]"
     limit="1"
     priority="true"
     sortby="['date', 'updated']">
  <div>
    <p>{{query.works_sides.before}}</p>
    <div ng-repeat="works in query.works_sides.entires_before">
      <p>{{works}}</p>
    </div>
  </div>
  <div>
    <p>{{query.works_sides.after}}</p>
    <div ng-repeat="works in query.works_sides.entires_after">
      <p>{{works}}</p>
    </div>
  </div>
</div>

<div sup-query-sides='post'
     pid="57d53d6b1697963fdbd2ee25"
     ng-model="query.post_sides">
  <div>
    <p>{{query.post_sides.before}}</p>
    <div ng-repeat="post in query.post_sides.entires_before">
      <p>{{post}}</p>
    </div>
  </div>
  <div>
    <p>{{query.post_sides.after}}</p>
    <div ng-repeat="post in query.post_sides.entires_after">
      <p>{{post}}</p>
    </div>
  </div>
</div>
```

------------------------------------------

### sup-editor-open

Open to the new file. usually use with `sup-query` or `sup-query-sides` to open the file from results repeat loop.

* `file`: **[ dict ]** the file data want to open. Or simply use directive attribute to pass the file data.

***Example***

```html
<div ng-repeat="file in query.files.contents">
  <div sup-editor-open file="file"></div>
</div>

<div ng-repeat="item in query.others.contents">
  <div sup-editor-open="item"></div>
</div>
```

------------------------------------------


### sup-editor-create

Create a new file. usually use after the content `sup-query` to create new file than edit it.

* `type`: **[ str ]** content_type of the new file. default is the content type of current page.
* `priority`: **[ str ]** Priority of the new file. This option is usual on homepage of a single page theme, Make more easy to understand how to create block page one by one. default is 0

***Example***

```html
<div sup-editor-open
     type="page"
     prioirty="0">
</div>
```

------------------------------------------

### sup-script-loader

Create safe script intro template. only limit domain will be allow.
such as `libs.soopro.com`

* `path`: **[ str ]** the file data want to open.
* `source`: **[ str ]** default is libs_url. the source url, it will connect with path.
* `noSource`: **[ bool ]** kill the source part, use path directly.

***Example***

```html
<sup-script-loader path="svg-sprites-render.min.js"></sup-script>
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
<h2> Current page path is: {{page.url | path:remove_args }}"></h2>
<h2> Current request path is: {{request.url | path }}"></h2>
```

`remove_args`: **[ bool ]** default is `True`, remove arguments.

------------------------------------------

### args

**Output**

**[ dict ]** Return args from a url as a dict. default is {}

**Usage**

```html
<h2> Current page args is: {{some_url|args:unqiue}}"></h2>
```

`unique`: **[ bool ]** return as a unquie value, otherwise might get a list if there multiple args with same key. default is `True`.

------------------------------------------

### date_formatted

**Output**

**[ str ]** Return date str.

**Usage**

```html
<h2> Date: {{date|date_formatted:to_format}}"></h2>
```

The `date` must be a date string, such as `2012-01-02`.

`to_format`: **[ str ]** give a date string format should to be, default is automatically create from current locale / lang.

---------------------------------

### is_empty

**Output**

**[ dict/list/str ]** Return bool tell it is empty or not.

**Usage**

```html
<h2> List: {{contents|is_empty}}"></h2>
```

---------------------------------

### bg_img

**Output**

**[ str ]** Return bg image stylesheet by given image src.

**Usage**

```html
<div style="{{meta.bg.src|bg_img:default_img_src:thumbnail}}"></div>
```

`default_src`: **[ str ]** default img src if need.
`thumbnail`: **[ bool ]** use thumbnail or not.


---------------------------------

### col_offset

**Output**

**[ str/int ]** Calculate column offset value or class name. Simetime items is too few, can not full fill a row of columns, desiner have to move item little bit to make sure the alignments is nice.

**Tips**

For some css lib, *bootstrap* etc., only the first item should be offset.

**Usage**

```html
<div class="col-sm-4 {{page.series|col_offset:'col-sm-offset-':4}}"></div>
```

`data`: **[ list/int ]** for the length of data.

`pattern`: **[ str ]** output pattern, if this param is not string will return offset value as `int`. A `{}` in the pattern string is mark for offset output position. The offset value will output at end of pattern when `{}` not in there. Default is `None`.

`column`: **[ int ]** The size of a single column. for example, bootstrap column `col-md-4` 4 is the size. Default is `4`.

`row_columns`: **[ int ]** The Size of row. For example, bootstrap is 12. Default is `12`.


---------------------------------

