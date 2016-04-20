# Preset Config

## Concept

Theme preset define some preset contents for the theme.
Editor can use preset to insert preset content intro rich content edtior (SAW).

Theme preset is not required.

## Preset file

It's a json file, and will NOT expose while rendering.

`key`: Preset key, might use for preset-id while insert to content. it is optional.

`name`: Preset name.

`preview`: A url of preview image. 'theme_url' will add automatically if only path is given.

`code`: The preset codes, can be html or js.


[ ***json*** ]

```json
[
  {
      "name": {
          "en":"english",
          "zh":"中文"
      },
      "preview": "img/preview.png",
      "code": "...."
  },
  {
      "key": "preset-key"
      "name": "Preset name 2",
      "preview": "img/preview2.png",
      "code": "..."
  }
]
```


## Translate supported

Translate context must a **[ dict ]**, key must be Locale code or Lang code
etc., `en_US` or `zh`. if you have multiple Locale of same region, `zh_TW`, `zh_CN`, you can use Locale code to define different translation.

If you wont use translate, just put normal string.


For the preset code, you can use translation just like angular template did.
etc., `_('Translate me')`