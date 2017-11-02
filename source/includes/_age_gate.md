# One-Click Age Gate

## Usage

1. Load the script in the footer of the page, before the closing `</body>`

```html
<script src="https://assets.wine/global/age-gate/v1/age-gate.min.js"></script>
```

2. After the script tag, initialize it, passing in options to customize appearance.

```html
<script>
  AGE_GATE.init({
    'logoSrc' : '/img/logo.jpg'
  });
</script>
```

### Options

* overlayColor : (string) the color (and opacity) of the overlay. Default = `'rgba(0,0,0,.8)'`
* modalBackgroundColor : (string) the background color of the modal box. Default = `'#FFF'`
* modalForegroundColor : (string) the text and button color within the modal. Default = `'#000'`
* textSize : (string) the base text size of the modal. All text sizes are relative to this. Default = `'16px'`
* logoSrc : (string) path to the logo image that should appear at the top of the modal. Default = `''` (nothing)
* takeoverThreshold : (int) pixel width of the viewport, below which the modal converts to takeover. Default = `600`

## Development

### Local setup

1. Clone the repository... you probably already did that if you're reading this.
1. [Insall node](https://nodejs.org/en/download/) if you haven't already 
1. `npm install` This takes a seoncond the first time b/c it's downloading some libraries

### Files

* `build/` contains the compiled, production versions of files
* `src/` pre-compiled source files
* `demo/` dummy test page for local development
* `test/` test suites

### Tasks

#### Run the demo page to preview changes locally
`$ gulp` will run browser-sync and live-reload when changes to _build_ code are made. Visit [localhost:9000](http://localhost:9000). Note: this will _not_ live-reload changes to pre-compiled src/ code (see next task).

#### Build javascript
`$ gulp compress-js` will "uglify()" the source JS and copy to the build/ directory (this is what gets deployed to AWS)

#### Run test suite
`$ gulp test` runs the test suite in test/test.js. We're using the Jasmine library for the specs and Karma for the test runner framework.

### Resources

* [Gulp](http://gulpjs.com/)
* [Node](https://nodejs.org/en/)
* [browser-sync](https://browsersync.io/) / [gulp-browser-sync](https://www.browsersync.io/docs/gulp)
* [Jasmine](https://jasmine.github.io/2.0/introduction.html) / [Karam](https://karma-runner.github.io/1.0/index.html)
