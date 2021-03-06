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

## dict:bg

* `src`: background media src.
* `color`: background color.
* `repeat`: background image repeat mode `no-repeat`(default), `repeat`, `repeat-x`, `repeat-y`.
* `style`: generate style by `src`,`color`,`repeat` for template to use more easier.
* `class`: background media class.
* `preset`: The preset key, usually is same as the css class name.

Additional attributes for 'video' and 'audio':

* `controls`: media has controls, always `false`.
* `loop`: media will loop, always `true`.
* `autoplay`: media will loop, always `true`.

## list:series

A list of media

* `src`: item media src.
* `type`: item media type, could be `image`, `video`, `audio`, `application`
* `mimetype`: item media mimetype.
* `title`: item title.
* `caption`: item caption.
* `tags`: item tags, could be use for front-end sort.
* `link`: item link if have one.
* `target`: target for item link. '\_blank' or something else.
* `class`: item class.
* `misc`: misc contents, for extra html contents.

Additional attributes for 'video' and 'audio':

* `controls`: item media has controls or not.
* `loop`: item media is loop or not.
* `autoplay`: item media is autplay or not.
* `poster`: item media poster.


## list:carousel

A list of carousel media

* `src`: carousel src.
* `type`: carousel type, could be `image`, `video`
* `mimetype`: media mimetype.
* `title`: media title.
* `caption`: media caption.
* `link`: media link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.
* `misc`: misc contents, for extra html contents.


## dict:button

* `title`: button title.
* `link`: button link if have one.
* `target`: target for media link. '\_blank' or something else.
* `class`: media class.

## list:lines

A list of dict

* `text`: the line text.


## dict:script

A fixed dict.

* `code`: **[ str:code ]** code...
* `meta`: **[ dict ]** meta for custom attributes.
* `status`: **[ int ]** display status `0:off` `1:on`


## dict:form

A list of dict

* action: **[ str ]** event slug or email address.
* type: **[ str ]** form type, `mailto` or `event`.
* fields: **[ list ]** a list of dict.
  1. label: **[ str ]** field label.
  2. name: **[ str ]** field name, this name is the name attr on html tag.
  3. placeholder: **[ str ]** field placeholder.
  4. type: **[ str ]** field types, input, textarea, selector, etc.,
  5. value: **[ str ]** field value, some value is a pattern such as selector raido box...
  6. status: **[ int ]** `0` Normal, `1` Required, `2` Disabled.
  
* custom: [** str **] custom form codes.
