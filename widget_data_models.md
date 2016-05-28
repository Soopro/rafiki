# Components data model


## dict:media

* `type`: media type 'image','video','audio','application'.
* `mimetype`: media mimetype.
* `src`: media src.
* `title`: media title.
* `link`: media link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

Additional attributes for 'video' and 'audio':

* `controls`: media has controls.
* `loop`: media will loop.
* `autoplay`: media will loop.
* `poster`: media poster.

## dict:background

* `src`: background media src.
* `color`: background color.
* `repeat`: background image repeat mode `no-repeat`(default), `repeat`, `repeat-x`, `repeat-y`.
* `style`: generate style by `src`,`color`,`repeat` for template to use more easier.
* `class`: background media class.

Additional attributes for 'video' and 'audio':

* `controls`: media has controls, always `false`.
* `loop`: media will loop, always `true`.
* `autoplay`: media will loop, always `true`.

## list:gallery

A list of media

* `src`: media src.
* `title`: media title.
* `caption`: media caption.
* `link`: media link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

Additional attributes for 'video' and 'audio':

* `controls`: media has controls.
* `loop`: media will loop.
* `autoplay`: media will loop.
* `poster`: media poster.

## dict:button

* `title`: button title.
* `link`: button link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

## list:collect

A list of dict

* `name`: item title. default is the index of item.
* `value`: item value. default is the index of item.

## list:notes

A list of dict

* `title`: item title. it's required.
* `content`: item content.
* ... may add more attributes future.

## str:code

A string might contain front-end codes. like js/html/css....

## dict:script

A fixed dict.

* `code`: **[ str:code ]** code...
* `meta`: **[ dict ]** meta for custom attributes.
* `status`: **[ int ]** display status `0:off` `1:on`