# Theme Config

## Concept

Theme config define theme information for display and options for use in template. Theme config also contain default values while activity on apps, It will create content types or taxonomies or menus automatically (taxonomy terms or menu item can not create by theme config).

Theme config is required by every theme type.

## Config file

Theme meta is load by this config file, after theme activated theme_meta.options can be modify.
`theme_meta` will inject to template context.


***Required***

* `version`: **[ str ]** Theme version.

* `slug`: **[ str:slug ]** Theme slug consist of only letters, numbers, underscores '\_' and hyphens '-'.

* `title`: **[ str ]** Theme title.

* `capabilities`: **[ list ]** Theme supported app types
	1. `ws` - Website
	2. `wx` - WeChat
	3. `wa` - WebApp


* `form_mode` : **[ int ]** open form editor or not

* `poster`: **[ str ]** Relative path of thumbnail image. recommend `poster.png`.

* `thumbnail`: **[ str ]** Relative path of thumbnail image. recommend `thumbnail.png`.

* `previews`: **[ list ]** A list of relative path of preview.

* `description`: **[ str ]** Theme description.

* `templates`: **[ list ]** A list of template keys, those keys must same as the template file names for both `.html` and `.tpl`, such as `page` for `page.html` and `page.tpl`, ***YOU MUST PUT ALL TEMPLATE FILE IN ROOT OF THEME DIR***. A snapshot PNG image for this template is required. The PNG filename must be exactly same as template key, such as template key `page`, must use `page.png` etc,. All snapshot must place in `snapshot` folder root of the theme exactly. Some template key is belong to *locked* page only, will not allow to other page, such as `index` etc,. You can also mark with `*` and `$` make theme allow to work with *locked* page. *Remember those mark are advance useage. They effect templates setting only. It DOES NOT mean the template filename should be marked as well.*
  1. `*`: Mark to allowed by an pages.
  2. `$`: Mark to allowed by `locked` pages.


***Optional***

* `license`: **[ str ]** Theme license, etc., 'MIT'.

*	`menus`: **[ dict ]** contain menu structure.
	each menu is a **[ dict ]**, key is the menu `slug` **[ str:slug ]**.
  1. `title`: **[ str ]**

*	`content_types`: **[ dict ]** contain content types structure. single type is a **[ dict ]** key will be the content type `slug` **[ str:slug ]**.
  1. `title`: **[ str ]** content type title
  2. `status`: **[ int ]** content type display status, `0 hidden` `1 display`, if it's not given will be default as `1 display`.

	( hidden content type can not display with by url directly,
		but still can get form `pages` context. )

* `reserved_contents`: **[ dict ]** contain reserved contents, such as 'search', 'taxonomy', 'tags', the theme required those page to make some function work. You can do some trick to short those options, but If you can not understand, just follow the original rules. 
  1. `slug`: **[ str ]** reserved content's slug. **TRICK:** If the key include `/` the right part will be use for slug, otherwise the key will be the slug (remember it will be slugify anyway).
  1. `content_type`: **[ str ]** reserved content's content type. default will be `page`. **TRICK:** If the key is include `/`, the left part will be content type.
  2. `template`: **[ str ]** reserved content's template. default will slugify from the key. **TRICK:** you can also use **[ str ]** as the item value, that mean is use this value as template. such as `{"category": "this-is-my-template"}`.
  3. `meta`: **[ dict ]** optional for inject content metas, etc., `title`.

* `allowed_slots`: **[ list ]** contain allowed extesion sluges. If current activated installable extension's `slug` is not in `allowed_slots`, the extension will display not "Not Supported" on admin panel.

*	`taxonomies`: **[ dict ]** contain taxonomies structure.
	each taxonomy is a **[ dict ]**, key will be the taxonomy `slug` **[ str:slug ]**.
  1. `title": **[ str ]**
  2. `content_types`: content_types is a **[ list ]** of **[ str:slug ]** , 
    define content_types slug related to this taxonomy )

* `options`: **[ dict ]** default theme options for `theme_meta.options`, those option could be change after activated.
	1. `sortby` - **[ str ]** sort content by 'key', if the key start with `-` then sort result will be `ASC`, default is 'DESC'
	2. `excerpt_length` - **[ int ]** excerpt text length
	3. `excerpt_ellipsis` - **[ str ]** excerpt text ellipsis
	4. `date_format` - **[ str ]** date string format
	5. `perpage` - **[ int ]** entries of perpage use for create template
  6. `markdown` - **[ bool ]** switcher of markdown mode in content
  7. `ignore_locked_templates` - **[ bool ]** with this option, the template will not skip from editor list if it is belong to locked page, such as `index` page with template `index` is locked as default.
	8. [ custom ] - options allow theme author to create custom options by self.

* `styles`: **[ str ]** custom theme styles, you might need put this in your templates with `<style>` tags. 
  etc., ```<style>{{theme_meta.styles}}<style>```

##### Config sample

[ ***json*** ]

```json

{
	"version":"3.1.1",
	"slug":"simba",
	"author":"Redy",
	"textdomain":"translate",
	"license": "MIT",
	"title":"Soopro official theme",
	"capabilities":["ws"],
	"thumbnail":"soopro.png",
	"previews":[
		"previews_1.jpg",
		"previews_2.jpg",
		"previews_3.jpg",
		"previews_4.jpg"
	],
	"description":"A simple brand website for Soopro official website.",

	"templates":{
		"index":"Homepage",
		"page":"Static Page",
		"feature_left":"Feature Page #1",
		"feature_right":"Feature Page #2",
		"portfolio":"Portfolio Page",
		"contact":"Contact Page",
		"sub_works":"Works Page",
		"error_404":"Error 404 Page"
	},

	"hide_property": ["taxonomy", "tag", "parent"],

	"form_mode": false,

	"menus":{
    "secondary":{"title":"Secondary Menu"}
  },

  "content_types":{
    "works": {"title": "Themes", "status": 0}
  },
  
  "reserved_contents":{
    "xxx": {},
    "post/index", true,
    "test": {
      "content_type": page,
      "template": "test",
      "meta": {
        "title": "Test"
      }
    }
  },
  
  "allowed_slots": ["comment"],
  
  "taxonomies":{
    "category": {
      "slug":,
      "title":"Categories",
      "content_types":["post"]
    }
  },

	"options":{
		"sortby": "updated",
		"excerpt_length":162,
		"excerpt_ellipsis":"&hellip;",
		"date_format": "%B %d, %Y",
		"perpage":12,
		"markdown":false
	}
}

```

key `title` `description` `template:title` is translate supported

[ ***json*** ]

```json

{
	"title": {
		"en":"Title",
		"zh":"标题"
	},
	"templates":{
    "index": {
  	  "en":"Homepage",
			"zh":"首页"
  	},
    "page": {
  	  "en":"Static Page",
			"zh":"静态页"
  	},
    "post": {
  	  "en":"Posts",
			"zh":"文章页"
  	},
		"error_404": "404"
  },
	"description": {
	  "en": "This is a simple default theme, you can use it to write something fun, and experience the basic process of content editing.",
		"zh": "这是一个简单的微信默认主题的，你可以体验到如何编写一篇拥有自定义外观的微信文章。"
	}
}

```


<br><br>

## Editor form mode

Theme able to define editor mode with config key `form_mode` with true.



#### Form view

Supported form view editor. you need add to top level of theme config

**Base**

* `editor_fields`: **[ dict ]**fields of form view
  * `[ template_slug ]`: **[ list ]**
    1 `key` : Data context slug, should not duplicated.
    2 `name`: The name display above input filed.
    3 `type`: Filed type.


**Filed Types**


1. `input` - **[ str ]** short text input with `max: length`
2. `textarea` - **[ str ]** long text input with `max: length`
3. `media` - **[ dict:media ]** calling media modal with `allow_types: list`
4. `bg` - **[ str ]** scalling background modal
5. `gallery` - **[ list:media ]** calling gallery modal with `allow_types: list`
6. `button` - **[ dict:button ]** calling button modal with `allow_types: list`
7. `collect` - **[ list:collect ]** calling collect modal. `limit: int` limit items in the collect.
8. `notes` - **[ list:notes ]** calling notes modal. `limit: int` limit items in the notes.
9. `script` - **[ str ]** calling script modal
10. `saw` - **[ str ]** make SAW/SAMd Editor for rich text. `key` will force to 'content'
11. `select` - **[ str ]** make a select filed with `options: name & value`, use `multiple: true` to define a multiple select.
12. `switch` - **[ int ]** or **[ int ]** smake a switch button with `checked & unchecked`



**Notice:**
`key` is used for *meta* *meta.title*, *meta.featured_img*. If you set type as 'content' will force the key set to `content`.


[ ***json*** ]

```json
{
  "editor_fields":{

  	"[ template_slug ]": [

  		{
  			"key": "title",
  			"name": "Title",
  			"type": "input",
  			"max": 120
  		},

  		{
  			"key": "description",
  			"name": "Description",
  			"type": "textarea",
  			"max": 600
  		},

  		{
  			"key": "featured_img",
  			"name": "Featured Image",
  			"type": "media",
        "allow_types": "",
  		},

  		{
  			"key": "background",
  			"name": "Background",
  			"type": "bg",
        "allow_types": "",
  		},

  		{
  			"key": "gallery",
  			"name": "Gallery",
  			"type": "gallery",
  			"allow_types": "",
  		},

  		{
  			"key": "button",
  			"name": "Some Button",
  			"type": "button"
  		},

  		{
  			"key": "select",
  			"name": "Some Select",
  			"type": "select",
  			"multiple": false,
  			"options": [
  				{"name":"item#1", "value":0}
  				{"name":"item#2", "value":1}
  			]
  		}
  		{
  			"key": "collect",
  			"name": "Collect",
  			"type": "collect",
  			"limit": 10
  		}
  		{
  			"key": "notes",
  			"name": "Notes",
  			"type": "notes",
  			"limit": 10
  		}
  		{
  			"key": "switch",
  			"name": "Some Switcher",
  			"type": "switch",
  			"checked": true,
  			"unchecked": false
  		},
      
  		{
  			"key": "script",
  			"name": "Script",
  			"type": "script"
  		},

  		{
  			"key": "content",
  			"name": "Content",
  			"type": "content"
  		}
      
  	]
  }
}
```

`name` is translate supported
`allow_types`: is depaned on the widget, "*" for all as usual


[ ***json*** ]

```json

{
	"key": "content",
	"name": {
		"en":"Content",
		"zh":"正文内容"
	},
	"type": "saw"
}

```


<br><br>

## Translate supported

Translate context must a **[ dict ]**, key must be Locale code or Lang code
etc., `en_US` or `zh`. if you have multiple Locale of same region, `zh_TW`, `zh_CN`, you can use Locale code to define different translation.

If you wont use translate, just put normal string.

others context might translate automatically by Global trasnlate engine. you should not care about that.
