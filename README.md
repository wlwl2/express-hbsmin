# express-hbsmin

Express middleware (HTML minifier). Read the warning.

This is meant to be a heavily modified version of
https://github.com/melonmanchan/express-minify-html (@melonmanchan's
express-minify-html). Not sure if that will happen.

## Warning: Before you use this.

Causes problems with spacing (you possibly have to change css).
Load times are longer, but file size is smaller.

## Description

This express middleware simply enhances the regular 'render' method of the
response object for minifying HTML.

## Usage

```sh
npm install --save express-hbsmin
```

```js

const express = require('express')
const minifyHbs = require('express-hbsmin')

const app = express()

app.use(minifyHbs({
  override: true,
  exception_url: false,
  htmlMinifier: {
    removeComments: true,
    collapseWhitespace: true,
    collapseBooleanAttributes: true,
    removeAttributeQuotes: true,
    removeEmptyAttributes: true,
    minifyJS: true
  }
}))

app.get('hello', function (req, res, next) {
  res.render('helloTemplate', {hello: 'world'}, function(err, html) {
    // The output is minified, huzzah!
    console.log(html)
    res.send(html)
  })
})

```

Set 'override' to false if you don't want to hijack the ordinary res.render
function. This adds an additional res.renderMin function to the response object
to render minimized HTML. 

The 'htmlMinifier' opts are simply passed on to the html-minifier plugin. For
all the available configuration options, see [the original
repo!](https://github.com/kangax/html-minifier/blob/gh-pages/README.md)

If no callback is provided, res.render/res.renderMin sends the minified HTML to
the client just as the regular express res.render does. Otherwise, the callback
is called with the error object and the minified HTML content, as demonstrated
above.

the `exception_url` optional parameter is a single value, or an array of
strings, regexes and functions that can be used to check whether minifying
should be skipped entirely.

```js

exception_url: [
    'url_to_avoid_minify_html', // String.
    /regex_to_analyze_req_to_avoid_minify/i, // Regex.
    function(req, res) { // Function.
        // Code to analyze req and decide if skips or not minify.
        // Needs to return a boolean value.
        return true
    }
]

```

Full examples can naturally be found under the 'examples'-folder of this
repository!