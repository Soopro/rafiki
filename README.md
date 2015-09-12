version: 1.0.0

# Theme Develop Documentation

1. Theme Config
2. Jinja2 Tempalte
3. Angular Tempalte


## Things you need to learn

Before you read any document of this repo, you have to learn some definition:

* `alias`: This is a human readable key string. `alias` must be **unique** in same level, just like a filename in same folder, or a path of url. Developer can use alias definition key like string in template. For example: you have to make a photo slider with dynamic contents, you need a key to define #id of each slides, this is good situation to use content's alias to make those keys.

* `content_type`: Content type define types of different content source. one content can not archive in different content type, just like `folder` with `file`. It's totally different with Category (assume you understand what is category).

* `taxonomy`: Just taxonomy. The item of taxonomy call term. *Category* is a kind of taxonomy. developer may define custom taxonomy other than category as well, that's why use a general name.


## Learning paths of theme develop

1. [Learn Pyco to know how to start localhost to develop.](https://github.com/Soopro/pyco)
2. [Learn how to write theme config to define your theme.](theme_config.md)
3. [Learn Jinja2 template to know how to write template language.](jinja2_template.md)
4. [Learn Peon to know the develop tools.](https://github.com/Soopro/peon)
5. [If you good engouh, learn Angular template to know how to write template able to use canvas editor language.](angular_template.md)
