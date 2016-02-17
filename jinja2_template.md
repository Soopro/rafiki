# Jinja2 Template

## Concept

 It's use Jinja2 template engine. All jinja2 syntax is able to use in those template.

All pages and Site metas and Theme meats is in context. 'g' (without ext) file, is for perpare global context.

Developer can custom new context in any template file or 'g' file.

***Remember! Any context is data object, those data can use in template whatever you want. It's not magic, and it's not javascript either. Don't be stupid. try print out the context if this document didn't let know what inside.***

<br><br>

## File naming

template file extensions must be `.html` only. otherwise even you can use in theme, but our system can not host it safely.

* `g`: g file is no file ext.
* `includes`: every template for include, recommend to use '\_' to mark on file name. etc., `_header_.html`. (if only start with '\_' is ok.)

<br><br>

## Jinja2 Basic knowledges

Template must write by Jinja2.

***operators***
* `or`: or
* `and`: and
* `not`: opposite, etc., `if not menu.primary`
* `in`: check item is in the list or key is in the dict,
etc., `if item in menu.primary`
* `is`: equals '==='.
* `is not`: not equals '!=='.
* `==`: equals, not much different with `is` while in template.
* `!=`: not equals.
* `()`: group conditions.
etc., `if (title is not None or desc is None) and advanced`


Learn more from here [Jinja2 Docs](http://jinja.pocoo.org/docs/dev/templates)

------------------------------------------

### Set

Use `set` to given a new context value.

***Example***
```html
{% set my_value = 'any thing you want here' %}

```

------------------------------------------

### If

Use `if` to rendering template by condition.

***Example***
```html
{% if menu.primary %}
<h1>I Have this menu</h1>
{% endif %}
```

------------------------------------------

### Loop

Use `for` to loop a list or dict

there some in loop context can be useful:

`loop.index`:	The current iteration of the loop. (1 indexed)
`loop.index0`:	The current iteration of the loop. (0 indexed)
`loop.revindex`:	The number of iterations from the end of the loop (1 indexed)
`loop.revindex0`:	The number of iterations from the end of the loop (0 indexed)
`loop.first`:	True if first iteration.
`loop.last`:	True if last iteration.
`loop.length`:	The number of items in the sequence.
`loop.cycle`:	A helper function to cycle between a list of sequences. See the explanation below.
`loop.depth`:	Indicates how deep in deep in a recursive loop the rendering currently is. Starts at level 1
`loop.depth0`:	Indicates how deep in deep in a recursive loop the rendering currently is. Starts at level 0

***Example***

```html
<!-- list -->
{% for item in list %}
<h1> {{item.title}} </h1>
{% endfor %}

<!-- dict -->
{% for key, value in list %}
<h1> {{key}}: {{value}} </h1>
{% endfor %}
```

------------------------------------------

### filter:striptags

Use `filter:striptags` to strip tags from a str. usually for Website head metas.

***Example***

```html
<head>
  <title>{{ meta.title | striptags }}</title>
</head>
```

------------------------------------------

### i18n support *(Advanced)*

***this is for advanced usege***

1. you have follow the flask i18n rules to create translate file. (po, mo)
2. use `{{_('The text need to be translate')}}`

Learn more form google please.

------------------------------------------


### Idea of Class in meta

***this is for advanced usege***

Sometime `object.meta.class`, sometime `object.class`.

If the object is beyound **page** the class attribute will be `object.meta.class`. 
if it's in a page or some kind node, will be `object.class`.

Because the current page attributes will under `meta` context,
you don't wont put it `meta.background.meta.class`...


------------------------------------------

<br><br>


## Base Context

Base Context is define by system, unable to modify while in template.
You must use base context for url in template,
such as `theme_url` `libs_url` `base_url` .
Some context is no detail description, you have to print it by your self.

* `locale`: **[ str ]** App locale. etc., 'en_US'

* `gfw`: **[ bool ]** App is behind gfw or not.

* `lang`: **[ str ]** App language, language base code by locale. etc., 'en'

* `translates`: **[ list/None ]** Multi language support. if no translates data will return None. Each translate output is **[ dict ]**  `"key" : [ str ], "name": [ str ], "url": [ str ]`
  translates in defined in `site_meta`, but will removed after translate context is generated.

  *tips:* the translate source could be a **[ dict ]** or **[ list ]**
  ```json
  {
     "zh_CN":{"name":"汉语","url":"http://....."},
     "en_US":{"name":"English","url":"http://....."}
  }
  ```
  or 
  ```json
  [
     {"key":"zh_CN", "name":"汉语", "url":"http://....."},
     {"key":"en_US", "name":"English", "url":"http://....."}
  ]
  ```
  

* `socials`: **[ list/None ]** Multi social support. Each social is a **[ dict ]** .

  1. `key`: [ str ] key.
  2. `name`: [ str ] name.
  3. `url`: [ str ] target url.
  4. `ico`: [ str ] social media icon image url, theme will use `key` for icon name as usual, if `ico` is given can be override it.
  5. `image`: [ str ] image url of the social media, like QR code.
  6. `code`: [ str ] for some script required by the social seo.
  7. `headmeta`: [ str ] for metadata in html `<head>`

  socials in defined in `site_meta`, but will removed after socials context is generated.

  *tips:* the social source is a **[ dict ]**

  ```json
  {
     "facebook":{
       "name":"Facebook",
       "url":"http://...",
       "code":"...",
       "headmeta": "....",
       "ico":"http://...",
       "image": "http://..."
     },
     "twitter":{
       "name":"Twitter",
       "url":"http://...",
       "code":"...",
       "headmeta": "....",
       "ico":"http://...",
       "image": "http://..."
     }
  }
  ```
  or 
  ```json
  [
     {
       "key":"facebook",
       "name":"Fackbook",
       "url":"http://...",
       "code":"...",
       "headmeta": "....",
       "ico":"http://...",
       "image": "http://..."
     },
     {
       "key":"twitter",
       "name":"Twitter",
       "url":"http://...",
       "code":"...",
       "headmeta": "....",
       "ico":"http://...",
       "image": "http://..."
     }
  ]
  ```

* `sa`: **[ dict ]** built-in analytics. `sa.app` for whole app, `sa.page` for a page (default is current).

* `theme_url`: **[ str ]** Theme url. (without trailing slash)

* `libs_url`: **[ str ]** Libs url for public. (without trailing slash)

* `base_url`: **[ str ]** current website base url. (without trailing slash)

* `request`: **[ request ]** current request.

* `args`: **[ dict ]** current request args.

* `theme_meta`: **[ dict ]** Theme metas.

* `site_meta`: **[ dict ]** Site metas.

* `content`: **[ str ]** rich text content of current page.

* `meta`: **[ dict ]** meta of current page.

* `pages`: **[ list ]** all published pages of this website.

* `tax` or `taxonomy`: **[ dict ]** return whole website's taxonomy system.
  it's very complex, try print and learn.

* `menu`: **[ dict ]** return whole website's menu system.

------------------------------------

### Site Meta

Site meta is the attributes of the whole site. the owner able to define custom meta by their self.

Site meta field can be 'blank' if never input.

Site meta can also edit by hardcode mode, could input anything with JSON format, but might cause UI display problem, whatever... no zuo no die.

Use `site_meta` context in theme template.

* `title`: **[ str ]** site title.

* `description`: **[ str ]** site description.

* `type`: **[ str ]** site type. Could be `ws` `wx` `wa` ...

* `alias`: **[ str ]** site app alias.

* `id`: **[ str ]** site app id.

* `logo`: **[ str ]** logo media src.

* `copyright`: **[ str ]** for copyright.

* `license`: **[ str ]** for license such as ***沪ICP备14049689号***.

* `keywords`: **[ str ]** for keywords of html meta in head.

* `contacts`: **[ str ]** for contacts info which you want use in every page.

* `analytics`: **[ str ]** custom analysis code.

* `seo`: **[ str ]** seo code.

* ... *Custom fields*


------------------------------------

### Theme meta

Theme meta is the attributes of the whole site. the theme author able to define custom meta by their self, but the `theme_meta.options` can be modify by editor.

Learn more from **Theme Config** section.


------------------------------------

### Single Page

Page is contain both 'meta' and 'content'.
* `meta`: dict of page data.
  * `alias`: **[ str ]** content alias. (generated by filename)
  * `title`: **[ str ]** title. 
  * `url`: **[ str ]** url.
  * `type`: **[ str ]** content type. (generated by system or custom by editor)
  * `related_type`: **[ str ]** relate to some content type.
  * `priority`: **[ str ]** proiority.
  * `author`: **[ str ]** author.
  * `template`: **[ str ]** template alias.
  * `parent`: **[ str ]** parent page alias.
  * `status`: **[ int ]** page status draft 0 or publish 1.
  * `updated`: **[ int ]** updated timestamp. (generated by system)
  * `created`: **[ int ]** created timestamp. (generated by system)
  * `date`: **[ str ]** date by input.
  * `date_formatted`: **[ str ]** formatted date. (generated by system)
  * `content`: **[ str ]** content.
  * `description`: **[ str ]** description. (worte by editor, use excerpt if has no description)
  * `excerpt`: **[ str ]** content excerpt. (generated by system)
  * `is_front`: **[ bool ]** if page is front page.
  * `is_404`: **[ bool ]** if page is 404 page.
  * `is_current`: **[ bool ]** if it is current page. (in `pages` list only.)
  * ... *Custom fields*

* `content`: rich text content of this page.

------------------------------------

### Pages

Pages is a list for all records, each page's data just almost same as Single page's `meta` but added with `content`. there is no `meta` level dict anymore.
etc., `page.title`, `page.author`, `page.content`.

------------------------------------

### Taxonomy

For `ws` website app only.

* `taxonomy`: **[ dict ]** contain all taxonomies of this site. use taxonomy alias to get the taxonomy. etc., taxonomy['category'] to return the category. also use 'tax' for short.
  * `title`: **[ str ]** taxonomy title.
  * `alias`: **[ str ]** taxonomy alias.
  * `locked`: **[ bool ]** taxonomy is unable delete by editor view or not. (some taxonomy is force created by our system. it might not be delete from editor panel, but still can delete by many other ways. you can only use this attribute to recognize which taxonomy is forced by system, not other usage at all...)
  * content_types: **[ list ]** contain all content types alias which related with this taxonomy.
  * `terms`: **[ list ]** contain all terms in this taxonomy.
    1. `title`: **[ str ]** Title
    2. `priority`: **[ int ]** Priority of this term.
    3. `parent`: **[ str ]** Parent term alias.
    4. `meta`: **[ dict ]**
       1. `description`: **[ str ]** Description.
       2. `pic`: **[ str ]** Pic url, if you want add a picture for this term then copy and paste a absolute url here.
       3. `class`: **[ str ]** css class.

***Example***

```html
{% set categories = tax.category %}
{% for cat in categories %}
  <h2>Title: {{cat.title}}</h2>
  <h2>Alias: {{cat.alias}}</h2>
{% endfor %}
```

------------------------------------

### Menu

For `ws` website app only.

* `menu`: **[ dict ]** contain all menus of this site. use menu alias to get the menu item list. etc., menu['primary'] to return all item from primary menu.

* [ menu items ]: **[ list ]** all item of the menu.
get menu items by menu[menu_alias]

  * `title`: **[ str ]** menu item title.
  * `alias`: **[ str ]** menu item alias.
  * `link`: **[ str ]** menu item link input by editor.
  * `target`: **[ str ]** menu item link target, `_blank` or something else.
  * `url`: **[ str ]** menu item url, generated by link with absolute url.
  * `meta`: **[ dict ]** menu item meta. contain `class` as default.
    1. `class`: **[ str ]** css class.
  * `nodes`: **[ list ]** menu item's children nodes. all nodes is same structure as item.


***Example***

```html
{% for item in menu.primary %}
  <a target="{{item.target}}" class="{{item.meta.class}}" href="{{item.url}}">
   Title: {{item.title}}
  </a>
  {% for child in menu.nodes %}
  <div>
  <a href="{{child.url}}" target="{{child.target}}"
   class="{{child.meta.class}} {{child.alias}}">
    {{child.title}}
  </div>
  {% endfor %}
{% endfor %}
```

<br><br>


### Helpers

Helper is built-in function which injected to Jinja2 template context by our system.

---------------------------------

### rope

rope is a helper function can sort raw data by given orders.

**Output**

**[ list ]** Return a list of sorted results.

**Usage**

```python
rope(raw_pages, sort_by[, desc, priority])
```

`raw_pages`: **[ list ]** or **[ dict ]** original data.

`sort_by`: **[ str ]** or **[ list ]** sort key, if the key is start with '-', the key will sort `ASC`. if multiple key is given by list, different key can use `DESC` or `ASC` by start with '-' or not.

`desc`: **[ bool ]** default is `True`. Final result is sort `DESC` or not.

`priority`: **[ bool ]** default is `True`. Give `priority` as first sort key as default. priority will beyond every other sort keys. if you won't sort with priority at all, set it to False. *** The priority key be ASC as default ***

*tips*: If you want custom the sort `priority` key, so you can do this:
```
rope(pages, sort_by=["updated", "-priority"])
```
In this case the `priority` key must with `-`, because I want keep this 'priority' as ASC. if you want DESC just remove the `-`.


***Example***
```python
result_pages = rope(pages, sort_by="updated", desc=True, priority=True)
```

---------------------------------

### saltshaker

saltshaker is a helper function can filter raw data intro a list by custom conditions.

**Output**

**[ list ]** Return a list of result matched conditions.

**Usage**

```python
saltshaker(raw_salts, conditions[, limit, intersection, sort_by])
```

`raw_salts`: **[ list ]** or **[ dict ]** original data. At most time dict will be sucks.

`conditions`: **[ list ]** condition options, the item of list could be **[ dict ]** data must match both key/value, **multiple key/value must use different dict**, or just use a key as **[ str ]**. etc., `[{'type':'test'},'thumbnail']` mean is has key 'type' and value is 'test', and has key 'thumbnail'. `{'type':''}` or `{'type':true}` is match if have 'type' key. `{'type':false}` or `{"type":None}` is match if not have 'type' key. If you need find result is opposited by the condition value, must add key  `"not":True`, for example `{"type":"test", "not":True}`. 
If you really want match empty string `''`or `None`, use `{'type':None, force:True}`
The condition value also supported **[ list ]**, the condition will be matched if any one of item is matched.
***Note*** if the target value is a list and condition gives not a list, in this case will test the condition value is in target value (list) or not to decide match result.


`limit`: **[ int ]** limit of results.

`intersection`: **[ bool ]** default is `True`. if True, results must be matched all conditions, False is matched any condition.

`sort_by`: **[ str ]** or  **[ list ]** default is `updated`. sort result by key. default use 'DESC' sort, unless the key is start with '-' then will 'ASC' sort. (this sort_by is base on `rope` now.)

***Example***
```python
result_pages = saltshaker(pages, [{'type':'test'}, 'thumbnail', {'category': ['news', 'war']},
                                  {'some_option':True, 'not':True}}], limit=12, intersection=True,
                                  sort_by='updated')
```

***Attention***: DO NOT sort or query those keys below, it might not working while in angular sup-query !!!
```
[
    'date_formatted', 'excerpt', 'description',
    'markdown', 'content', 
    'id', 'app_id', 'attrs', 'url', 'status'
]
```

---------------------------------

### glue

gule is a helper function can add new args intro url safely.

**Output**

**[ str ]** Return a parsed url with added args.

**Usage**

```python
gule([args, url])
```

`args`: **[ dict ]** default is `None`. The new args you want add to url, must be dict format and all value must be **[ str ]** or **[ int ]**. etc., `{"key":"value"}`. if args is not given or not a dict, gule will take current request args only.

`url`: **[ str ]** default is `None`. The url you want parse with gule. if not given will use current url without any query args.

***Example***

```python
relative_path_args = glue(args)
```

---------------------------------

### stapler

stapler is a helper function can make pagination.
you can use it with ***gule*** to make pagination entires.

**Output**

**[ dict ]** Return a dict for paginator.

The output dict will follow:
* `pages`: result pages for current paged.
* `max`: how many pages total.
* `paged`: current paged number.

Use those data in your template do whatever you want.

**Usage**

```python
stapler(raw_pages[, paged, perpage])
```

stapler dose not process any url query args.
You have to put default paged value at first time you get the page. then add paged argument intro prev/next link, you better use **gule** to make it easier and safe, or you just geek enough...

After you get current url with paged argument,
you will able to use `args` context to get back the current paged number.

etc., use **gule** add new argument '{"paged":'1'}' on paginator link. current paged could get paged from args['paged'] then you can use to +1 or -1 to make prev/next then give to stapler as paged param.

`raw_pages`: **[ list ]** original list you want to paging.

`paged`: **[ int ]** default is `1`. current paged number.

`perpage`: **[ int ]** default is `12`. how many entires for each page. (usually you get this from theme_meta.options.perpage.)


***Example***

```python
booklet = stapler(pages, paged=1, perpage=12)
```

---------------------------------

### barcode

barcode is a helper function can count data field.
You can use it for count how many post with same terms.
etc., use to count 'category',
you will get a list:
```
  [{'count': 2, 'key': u'test'}, {'count': 2, 'key': u'fuck'}, {'count': 1, 'key': u'inneral'}, {'count': 1, 'key': u'xxx'}, {'count': 1, 'key': u'ahha'}]
```

After that you can use `taxonomy` context to make your own Category count.

**Output**

**[ dict ]** Return a dict counted how many entries has same
value of specified field.

The output dict will follow:
* `key`: **[ str ]** the content key. for category, the key equals alias, use taxonomy context to find the title. for tags the key equals title. (see the example below.)

**Usage**

```python
barcode(raw_pages[, field, sort[, desc]])
```

`raw_pages`: **[ list ]** original list you want to count.

`field`: **[ str ]** default is `category`. field you want to cound.

`sort`: **[ bool ]** default is `True`. return sorted result.

`desc`: **[ bool ]** default is `True`. return sorted result by `DESC`.


***Example***

```python
<!-- this part will create a dict of cateory terms title -->
{% set term_title = {} %}
{% for term in tax.category.terms %}
{% do term_title.update({term.alias:term.title}) %}
{% endfor %}

<!-- use for get category -->
{% for bar in barcode(pages, field="category") %}
Category: {{term_title[bar.key]}} Count: {{bar.count}}
{% endfor %}

<!-- use for get tags -->
{% for bar in barcode(pages, 'tags') %}
Tag: {{bar.key}} Count: {{bar.count}}
{% endfor %}

```

---------------------------------

### timemachine

timemachine is a helper function can group data by datetime.

**Output**

**[ list ]** Return a list contained grouped datas.
the format looks like ```[((2015,1), [{..grouped_list..}])]```


**Usage**

```python
timemachine(raw_pages[, field, precision, time_format, reverse])
```

`raw_pages`: **[ list ]** original list you want to group.

`field`: **[ str ]** default is `date`. group by this field. make sure the value must be kind of date, could be string date `str` `unicode`, timestamp `int`,
datetime 'datetime', otherwise will crash. (you might never get `datetime` type of date, but timemachine supported it anyway.)

`precision`: **[ str ]** default is `month`. precision of the datetime. could be:
  * `year`
  * `month` <- default, also will fail back if wrong value is given
  * `day`
  * `hour`
  * `minute`
  * `second`

`time_format`: **[ str ]** default is `%Y-%m-%d`. unix time format.
This is learn it from: https://docs.python.org/2/library/datetime.html

`reverse`: **[ bool ]** default is `True`. reversed result, that will be 'DESC'

***Example***

```python
sorted_pages = timemachine(pages, filed='date', precision='month',
                           time_format='%Y-%m-%d', reverse=True)
```

---------------------------------

### straw

straw is a helper function can directly find one dict item by key/value form a list.

**Output**

**[ dict ]** Return the item data.


**Usage**

```python
straw(raw_list, value[, key])
```

`raw_pages`: **[ list ]** original list you want to count.

`value`: **[ any ]** the value must find match.

`key`: **[ str ]** the key must find match. default is `id`.


***Example***

```python
next_page = straw(pages, value = next_page.id, key = 'id')
```

---------------------------------

### gutter

gutter is a helper function can find next/prev pages by specific structure. (for book app).

**Output**

**[ dict ]** Return a dict with simple information of next and prev page.

The output dict will follow:

* `prev_page`: **[ dict ]** prev page.
  * `title`: **[ str ]** page title.
  * `alias`: **[ str ]** page alias.
  * `url`: **[ str ]** page url.
  
* `next_page`: **[ dict ]** next page.
  * `title`: **[ str ]** page title.
  * `alias`: **[ str ]** page alias.
  * `url`: **[ str ]** page url.


**Usage**

```python
gutter(pid, structures)
```

`pid`: **[ str/ObjectID ]** current page id.

`structures`: **[ list ]** contain `menu` like dict, the attribute can be custom by theme developer.


***Example***

```python
page_gutter = gutter(meta.id, menu.gutter.nodes)
```


The old version `gutter` is deprecated. Looks don't need that heavy.
------------------------------
~~gutter is a helper function can group pages by specific structure.~~

**Output**

~~**[ list ]** Return a list of grouped pages by structures.~~

~~The output dict will follow:~~
1. ~~`alias`: **[ str ]** group alias.~~
2. ~~`title`: **[ str ]** group title.~~
3. ~~`meta`: **[ dict ]** group custom meta, usually is empty.~~
4. ~~`pages`: **[ list ]** grouped pages. all element in this `pages` is same as single page.~~


**Usage**

~~```python
gutter(raw_pages, structures)
```~~

~~`raw_pages`: **[ list ]** original list you want to group.~~

~~`structures`: **[ list ]** contain `menu` like dict, the attribute can be custom by theme developer.~~
~~```{"alias": "chapter-1", "title": "Chapter 1", "nodes": [...]}```~~


***Example***

~~```python
bookpages = gutter(pages, menu['gutter'])
```~~

---------------------------------

<br><br>

## Filters

Filter is built-in function which injected to Jinja2 template context by our system.

---------------------------------

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
