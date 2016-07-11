version: 1.8.5

# Theme Develop Documentation

1. [Theme Config](theme_config.md)
2. [Theme Preset](theme_preset.md)
3. [Jinja2 Template](jinja2_template.md)
4. [Angular template](angular_template.md)
5. [Widget Data Models](widget_data_models.md)


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
5. [If you good engouh, learn Angular template to know how to write template able to use canvas editor view.](angular_template.md)
6. [If you nothing other to do, learn markdown template to know how to write template able to use markdown editor view.](angular_template.md)
7. [Know more about widget data model.](widget_data_model.md)
8. [Get gitignore file sample.](gitignore.md)

------------------------
<br><br>

## Basic rules
1. file name can be include charector [_-.0-9a-zA-z], otherwise will not working.

## Basic step of develop a theme

1. Install tools.
  * `python 2.7`
  * `pyco` [Get here](https://github.com/Soopro/pyco)
  * `peon` [Get here](https://github.com/Soopro/peon)
  * Editor or IDE, any way your choice ...

2. Prepare folders and files.
  * `html`: for original html files.
  * `content`: for content examples.
  * `theme`: for theme develop.
    1. Place theme src file into `src` folder.
    2. Place a `pyco` into folder.
    3. Create a `peon.json` into folder.

  ```
  html/
    /styles/
    /index.html
    /...

  content/
    /site.json
    /index.md
    /...

  theme/
    /src/
      /styles/
      /g
      /index.html
    /pyco/
      /...
  ```

3. Prepare contents. Make `.md` files in `pyco/content`, and 'site.json' (learn it form pyco docs.)

4. Write basic theme. You don't have to finish it just basic structure. (learn it from jinja2 template docs.)

5. Ready to start.
  1. Write peon.json
  2. Get in `theme` folder with terminal.
  3. Run 'peon -w' command in `theme` folder.
  4. Peon will automatically start watching files change in `theme/src` and also start a localhost server with pico.

  ```json
  {
    "watch":{
    "src": "src",
    "dest": "pyco/themes/dev",
    "clean": true,
    "server": true,
    "pyco": "pyco",
    "skip_includes": "html"
    }
  }
  ```

6. Continue write your template files and debug it.
