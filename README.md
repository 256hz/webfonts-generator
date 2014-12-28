#webfonts-generator


[![Build Status](https://travis-ci.org/sunflowerdeath/webfonts-generator.svg?branch=master)](https://travis-ci.org/sunflowerdeath/webfonts-generator)

Generator of webfonts from SVG icons.

Features:

* Supported font formats: WOFF, EOT, TTF and SVG.
* Supported browsers: IE8+.
* Generates CSS files and HTML preview, allows to use custom templates. 

##Install

```
npm install webfonts-generator
```

##Usage

```js
var webfontsGenerator = require('webfonts-generator')

webfontsGenerator({
  files: [
    'src/dropdown.svg',
    'src/close.svg'
  ],
  dest: 'dest/'
}, function(error) {
  if (error) console.log('Fail!', error)
  else console.log('Done!')
})
```

##webfontsGenerator(options, done)

###options

Type: `object`

Object with options. See the list of options.

###done

Type: `function(error)`

Function that is called when generation is complete.

##List of options

###files

*required*

Type: `array.<string>`

List of SVG files.

###dest

*required*

Type: `string`

Directory for generated font files.

###fontName

Type: `string`
<br>
Default: `'iconfont'`

Name of font and base name of font files.

###css

Type: `boolean`
<br>
Default: `true`

Whether to generate CSS file.

###cssDest

Type: `string`
<br>
Default: `path.join(options.dest, options.fontName + '.css')`

Path for generated CSS file.

###cssTemplate

Type: `string`

Path of custom CSS template.
Generator uses handlebars templates.

Template receives options from `options.templateOptions` along with the following options:

* fontName
* src `string` &ndash; Value of the `src` property for `@font-face`.
* codepoints `object` &ndash; Codepoints of icons in hex format.

###cssTemplateType

Type: `string`
<br>
Default: `'css'`

When custom template is not specified, this option is used to set type of default template.
Possible values are `css` and `scss`.

`css` template generates classes with names based on values from `options.templateOptions`.

`scss` template generates mixin `webfont-icon` to add icons styles.
<br>
Example of use:

```
@import 'iconfont';

.icon {
  @include webfont-icon('icon');
}
```

*It is safe to use multiple generated mixins together*

###cssFontsPath

Type: `string`
<br>
Default: `options.destCss`

Fonts path used in CSS file.

###html

Type: `boolean`
<br>
Default: `false`

Whether to generate HTML preview.

###htmlDest

Type: `string`
<br>
Default: `path.join(options.dest, options.fontName + '.html')`

Path for generated HTML file.

###htmlTemplate

Type: `string`
<br>
Default: `templates/html.hbs`

HTML template path.
Generator uses handlebars templates.

Template receives options from `options.templateOptions` along with the following options:

* fontName
* styles `string` &ndash; Styles generated with default CSS template.
	(`cssFontsPath` is chaged to relative path from `htmlDest` to `dest`)
* names `array.<string>` &ndash; Names of icons.

###templateOptions

Type: `object`
<br>
Default:
```js
{
	classPrefix: 'icon-',
	baseClass: 'icon'
}
```

Additional data for CSS & HTML templates.

###types

Type: `array<string>`
<br>
Default: `['woff', 'eot']`

Font file types to generate.
Possible values: `svg, ttf, woff, eot`.

###order

Type: `array<string>`
<br>
Default: `['eot', 'woff', 'ttf', 'svg']`

Order of `src` values in `font-face` in CSS file.

###rename

Type: `function(string) -> string`
<br>
Default: basename of file

Function that takes path of file and return name of icon.

###startCodepoint

Type: `number`
<br>
Default: `0xF101`

Starting codepoint. Defaults to beginning of unicode private area.

###codepoints

Type: `object`

Specific codepoints for certain icons.
Icons without codepoints will have codepoints incremented from `startCodepoint` skipping duplicates.

###fontName, fixedWidth, centerHorizontally, normalize, fontHeight, round, descent

Options that are passed directly to
[svgicons2svgfont](https://github.com/nfroidure/svgicons2svgfont).

#License

Public domain, see the `LICENCE` file.

