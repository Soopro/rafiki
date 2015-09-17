# Theme Config

## Concept

Theme config define theme information for display and options for use in template. Theme config also contain default values while activity on apps, It will create content types or taxonomies or menus automatically (taxonomy terms or menu item can not create by theme config).

Theme config is required by every theme type.

## Config file

Theme meta is load by this config file, after theme activated theme_meta.options can be modify.
`theme_meta` will inject to template context.


***Required***

* `version`: **[ str ]** Theme version.

* `alias`: **[ str:alias ]** Theme alias consist of only letters, numbers, underscores '\_' and hyphens '-'.

* `title`: **[ str ]** Theme title.

* `capability`: **[ list ]** Theme supported app types
	1. `ws` - Website
	2. `wx` - WeChat
	3. `wa` - WebApp


* `mode` : **[ int ]** Flag of mode, default is 0
	1. `0` - Form view
	2. `1` - Canvas view
	3. `2` - Markdown view


* `thumbnail`: **[ str ]** Relative path of thumbnail image. recommend `.png`.

* `previews`: **[ list ]** A list of relative path of preview, recommend `.png`.

* `description`: **[ str ]** Theme description.

* `templates`: **[ dict ]** template structure.
	`[ str:alias ] : [ str ]`


***Optional***

* `license`: **[ str ]** Theme license, etc., 'MIT'.

* `textdomain`: **[ str ]** Translate file's textdomain, default is 'translate'

* `hide_property`: **[ list ]** A list of hidden property alias, those filed will hidden from property panel. property alias must same as the key of meta. etc., taxonomy, tag, parent.

*	`menus`: **[ list ]** contain menu structure.
	each menu is a **[ dict ]** `"alias": [ str:alias ], "title": [ str ]`

*	`content_types`: **[ list ]** contain content types structure.
	each content type is a **[ dict ]** `"alias": [ str:alias ], "title": [ str ]`
	( alias start with '\_' will mark as hidden content type,
	  hidden content type can not display with by url directly,
		but still can get form `pages` context. )

*	`taxonomies`: **[ list ]** contain taxonomies structure.
	each taxonomy is a **[ dict ]** `"alias": [ str:alias ], "title": [ str ], "content_types":[[ str ]]`
	( content_types is a **[ list ]**, define content_types related to this taxonomy )

* `options`: **[ dict ]** default theme options for `theme_meta.options`, those option could be change after activated.
	1. `sortby` - **[ str ]** sort content by 'key', if the key start with `-` then sort result will be `ASC`, default is 'DESC'
	2. `excerpt_length` - **[ int ]** excerpt text length
	3. `excerpt_ellipsis` - **[ str ]** excerpt text ellipsis
	4. `date_format` - **[ str ]** date string format
	5. `perpage` - **[ int ]** entries of perpage use for create template
	6. [ custom ] - options allow theme author to create custom options by self.



##### Config sample

[ ***json*** ]

```json

{
	"version":"3.1.1",
	"alias":"simba",
	"author":"Redy",
	"textdomain":"translate",
	"license": "MIT",
	"title":"Soopro official theme",
	"capability":["ws"],
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

	"mode": 1,

	"menus":[
		{"alias":"secondary", "title":"Secondary Menu"}
	],

	"content_types":[
		{"alias":"_works", "title":"Works"}
	],

	"taxonomies":[
    {
      "alias":"category",
      "title":"Categories",
      "content_types":["post"]
    }
  ],

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

## Editor mode

Theme able to define editor mode with config key `mode`

mode `0`: use form view editor. need form structure in config [- ［default］

mode `1`: use canvas editor. theme must contain `.tpl`

mode `2`: use markdown editor. theme must contain `.tpl` 
(same one with canvas view, system will automatically replace `saw` to a markdown render. if you already have .tpl, then you have nothing to do.)



#### Form view

Supported form view editor. you need add to top level of theme config

**Base**

* `editor_fields`: **[ dict ]**fields of form view
  * `[ template_alias ]`: **[ list ]**
    1 `key` : Data context alias, should not duplicated.
    2 `name`: The name display above input filed.
    3 `type`: Filed type.


**Filed Types**

1. `input` - **[ str ]** short text input with `max: length`
2. `textarea` - **[ str ]** slong text input with `max: length`
3. `media` - **[ dict:media ]** scalling media modal
4. `bg` - **[ str ]** scalling background modal
5. `gallery` - **[ list:media ]** scalling gallery modal
6. `button` - **[ dict:button ]** scalling button modal
7. `script` - **[ str ]** scalling script modal
8. `saw` - **[ str ]** smake SAW Editor for rich text only.
9. `select` - **[ str ]** smake a select filed with `options: name & value`
10. `switch` - **[ int ]** or **[ int ]** smake a switch button with `checked & unchecked`



**Notice:**
`key` is used for *meta* *meta.title*, *meta.featured_img*, but *content* is only for 'content'.


[ ***json*** ]

```json
{
  "editor_fields":{

  	"[ template_alias ]": [

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
  			"type": "media"
  		},

  		{
  			"key": "background",
  			"name": "Background",
  			"type": "bg"
  		},

  		{
  			"key": "gallery",
  			"name": "Gallery",
  			"type": "gallery"
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
  			"options": [
  				{"name":"item#1", "value":0}
  				{"name":"item#2", "value":1}
  			]
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
  			"type": "saw"
  		}
      
  	]
  }
}
```

key `name` is translate supported

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
