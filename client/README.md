# StarHackIt React Frontend

An ES6 React based frontend starter kit:

* ES6/ES7 with `babel`
* Internationalization with `i18next` and `react-intl`
* Find bugs, enforce coding standards with `eslint` and its plugins: `react`, `promise`, `mocha`
* Hot reloading with `react-hot-loader`
* Copy and paste detector with `jscpd`
* Display lint warnings and build errors to directly to the browser with `webpack-hud`
* Unit tests with `karma`, `mocha` and `enzyme`
* Code coverage with `istanbul`
* End to end tests with `nightwatch`
* Concatenation, minification, obfuscation and compression of javascript and css file
* Bundle size and dependencies size under control
* Logging with the `debug` library
* Configuration depending of the environment: dev, uat, prod etc ...

## Development and build process

[npm](https://www.npmjs.com/) is used to define the 3rd party dependencies required by the frontend. *npm* comes with [node](https://nodejs.org) so make sure there are already installed:

    $ node -v
    v5.6

    $ npm -v
    3.6

These are the main *npm* commands for a normal developer workflow:

| npm command    | details  |
|----------------|----------|
| `npm install`  | Install dependencies  |
| `npm start`    | Start a development web server  |
| `npm test`     |  Run the unit tests with Karma |
| `npm run e2e`  |  Intall the Selenium driver for end to end testing |
| `npm run selenium-install`  |  Run the end to end tests with Nigthwatch/Selenium |
| `npm run unit-test-watch` |  Watch the code and run the unit test |
| `npm run build`| Create a production build  |
| `npm run bundle-size`| Create a report to show the size of each dependencies |
| `npm run clean`| Clean the project |
| `npm run lint`| Lint the source code |
| `npm run cpd`| Run the copy and paste detector |

### Install

To download and install the dependencies set in [package.json](package.json):

    $ npm install

### Build & Serve

[Webpack](https://webpack.github.io/) has become the defacto standard for building React frontend, it is configured through 3 files:

* [webpack.config.js](webpack.config.js): the configuration common to all environment.
* [webpack.dev.js](webpack.dev.js): the configuration for development environment. Thanks to the Webpack plugin `OpenBrowserPlugin`, a new browser page will be opened at the right URL i.e: `http://localhost:8080`
* [webpack.prod.js](webpack.prod.js): the configuration for production environment. The `UglifyJsPlugin` and the `CompressionPlugin` plugins are invoked to respectively remove code duplication, obfuscate and compress the code.

To run the development web server:

    $ npm start

> **Hot reloading**: Webpack detects any change in the code, it rebuilds automatically and pushes the new change the browser, no manual browser refresh required.

### Test

#### Unit test with Karma

Unit tests are written as `mocha` test and executed thanks to `karma`:

    $ npm test

> React components are tested with the [enzyme](http://airbnb.io/enzyme/) library.

#### End to end testing with nightwatch

To execute the end to end testing, a.k.a _e2e testing_, first make sure the frontend and backend are running, then run:

    $ npm run selenium-install
    $ npm run nightwatch

The end to end tests are executed by [nightwatch](http://nightwatchjs.org/) which uses the *Selenium* driver API.

The test suite can be found in [test/nightwatch](test/nightwatch)

## Configuration

The file [src/app/config.js](src/app/config.js) gathers the common configuration and the environment specific configuration which is selected by defining the variable `NODE_ENV` to `production`, `development`, `uat` etc ...
The `NODE_ENV` variable is injected through the *webpack* plugin `DefinePlugin`.

## Logging

The [debug](https://github.com/visionmedia/debug) library is a convenient way to log messages with prefix.
Messages can be switch on and off depending on the prefix, see [src/app/app.js](src/app/app.js)

## CORS

To avoid [CORS](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) issues, the *webpack* development server is configured to act as a proxy server, requests beginning with `/api/v1/` are forwarded to the api server located at `http://localhost:9000`. See the *gulp* task `webpack-dev-server` in [gulpfile.js](gulpfile.js)

## Internationalization

The [i18next](http://i18next.com/) provides internationalization support for this project. Translations are retrieved at run time without bloating the application with all translations.

Here are the i18next plugins configured in [src/app/utils/i18n.js](src/app/utils/i18n.js):

* [i18next-browser-languagedetector](https://github.com/i18next/i18next-browser-languageDetector): language detector used in browser environment for i18next
* [i18next-localstorage-cache](https://github.com/i18next/i18next-localStorage-cache): caching layer for i18next using browsers localStorage
* [i18next-xhr-backend](https://github.com/i18next/i18next-xhr-backend): backend layer for i18next using browsers xhr

The translation files are located in [locales](locales) directory.

In your *React* component, import the `i18next` package and invoke the `t` function to translate the given key:


```
import tr from 'i18next';

...

    <h2 >{tr.t('login')}</h2>
```


### Production build

To build the production version:

    $ npm run build

*webpack* will produce a report with all the assets and their respective size.

```
Version: webpack 2.2.1
Time: 50023ms
                                Asset       Size  Chunks                    Chunk Names
                   assets/img/es7.png    7.54 kB          [emitted]
            0.a8b321dbf1b873494250.js    65.3 kB       0  [emitted]
          app.a8b321dbf1b873494250.js     493 kB       2  [emitted]  [big]  app
       vendor.a8b321dbf1b873494250.js     709 kB       3  [emitted]  [big]  vendor
        0.a8b321dbf1b873494250.js.map     470 kB       0  [emitted]
        1.a8b321dbf1b873494250.js.map    57.9 kB       1  [emitted]
      app.a8b321dbf1b873494250.js.map    3.39 MB       2  [emitted]         app
   vendor.a8b321dbf1b873494250.js.map    4.93 MB       3  [emitted]         vendor
         1.a8b321dbf1b873494250.js.gz    3.67 kB          [emitted]
     1.a8b321dbf1b873494250.js.map.gz    9.31 kB          [emitted]
         0.a8b321dbf1b873494250.js.gz    18.2 kB          [emitted]
       app.a8b321dbf1b873494250.js.gz     105 kB          [emitted]
    vendor.a8b321dbf1b873494250.js.gz     186 kB          [emitted]
     0.a8b321dbf1b873494250.js.map.gz     105 kB          [emitted]
   app.a8b321dbf1b873494250.js.map.gz     627 kB          [emitted]  [big]
vendor.a8b321dbf1b873494250.js.map.gz    1.08 MB          [emitted]  [big]


```

To find out exactly the weight of each individual library, the tool [webpack-bundle-size-analyzer](https://github.com/robertknight/webpack-bundle-size-analyzer) creates a report displaying the size and the relative percentage of the dependencies.

```
$ npm run bundle-size

lodash: 720.69 KB (21.8%)
react-dom: 508.77 KB (15.4%)
material-ui: 339.14 KB (10.3%)
intl: 192.53 KB (5.83%)
react: 121.45 KB (3.68%)
mobx: 109.18 KB (3.31%)
react-s-alert: 80.04 KB (2.43%)
i18next: 77.8 KB (2.36%)
react-router: 75.15 KB (2.28%)
core-js: 58.33 KB (1.77%)
inline-style-prefixer: 56.87 KB (1.72%)
lodash.merge: 56.82 KB (1.72%)
buffer: 47.47 KB (1.44%)
history: 46.82 KB (1.42%)
react-redux: 36.2 KB (1.10%)
mobx-react: 35.42 KB (1.07%)
axios: 35.04 KB (1.06%)
reactabular-table: 31.45 KB (0.953%)
checkit: 31.25 KB (0.947%)
fbjs: 30.12 KB (0.913%)
regenerator-runtime: 24.22 KB (0.734%)
react-helmet: 23.75 KB (0.720%)
ladda: 23.56 KB (0.714%)
redux: 20.35 KB (0.617%)
qs: 16.67 KB (0.505%)
bowser: 16.56 KB (0.502%)
react-ga: 15.65 KB (0.474%)
redux-act: 13.15 KB (0.398%)
lodash.throttle: 13 KB (0.394%)
redux-logger: 12.79 KB (0.388%)
deep-diff: 11.21 KB (0.340%)
react-router-redux: 11.05 KB (0.335%)
react-pagify: 9.7 KB (0.294%)
i18next-browser-languagedetector: 9.08 KB (0.275%)
debug: 8.86 KB (0.269%)
react-event-listener: 8.2 KB (0.248%)
babel-runtime: 7.81 KB (0.237%)
recompose: 7.49 KB (0.227%)
react-tap-event-plugin: 7.29 KB (0.221%)
react-ladda: 7.27 KB (0.220%)
style-loader: 6.74 KB (0.204%)
lodash.keys: 6.46 KB (0.196%)
i18next-xhr-backend: 6.33 KB (0.192%)
css-loader: 6.11 KB (0.185%)
path-browserify: 6.04 KB (0.183%)
lodash-es: 5.74 KB (0.174%)
lodash.isarguments: 5.58 KB (0.169%)
react-doc-meta: 5.51 KB (0.167%)
  fbjs: 2.72 KB (49.3%)
  react-side-effect: 1.23 KB (22.3%)
  <self>: 1.56 KB (28.3%)
redux-act-async: 5.38 KB (0.163%)
process: 5.17 KB (0.157%)
i18next-localstorage-cache: 5.15 KB (0.156%)
lodash.isarray: 5.04 KB (0.153%)
react-side-effect: 4.62 KB (0.140%)
query-string: 4.15 KB (0.126%)
deep-equal: 3.8 KB (0.115%)
lodash._getnative: 3.78 KB (0.115%)
base64-js: 3.4 KB (0.103%)
keycode: 2.7 KB (0.0817%)
ms: 2.65 KB (0.0804%)
object-assign: 2.06 KB (0.0624%)
ieee754: 2.01 KB (0.0608%)
warning: 1.76 KB (0.0535%)
invariant: 1.48 KB (0.0449%)
hoist-non-react-statics: 1.35 KB (0.0410%)
shallowequal: 1.16 KB (0.0350%)
symbol-observable: 1.12 KB (0.0341%)
webpack: 1.09 KB (0.0330%)
classnames: 1.08 KB (0.0326%)
segmentize: 1 KB (0.0303%)
exenv: 863 B (0.0255%)
inherits: 672 B (0.0199%)
redux-thunk: 529 B (0.0157%)
hyphenate-style-name: 339 B (0.0100%)
simple-assign: 281 B (0.00832%)
strict-uri-encode: 182 B (0.00539%)
isarray: 132 B (0.00391%)
react-addons-transition-group: 59 B (0.00175%)
react-addons-create-fragment: 59 B (0.00175%)
react-addons-shallow-compare: 53 B (0.00157%)
intl .: 15 B (0.000444%)
<self>: 230.32 KB (6.98%)

```
