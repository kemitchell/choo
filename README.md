# choo [![stability][0]][1]
[![npm version][2]][3] [![build status][4]][5] [![test coverage][6]][7]
[![downloads][8]][9] [![js-standard-style][10]][11]

A framework for creating sturdy web applications. Built on years of industry
experience and distills the essence of functional architectures into a
productive package.

## Features
- __minimal size:__ weighing under `8kb`, `choo` is a tiny little framework
- __single state:__ immutable single state helps reason about changes
- __minimal tooling:__ built for the cutting edge `browserify` compiler
- __transparent side effects:__ using "effects" and "subscriptions" brings
  clarity to IO
- __omakase:__ composed out of a balanced selection of open source packages
- __idempotent:__ renders seemlessly in both Node and browsers
- __very cute:__ choo choo!

## Usage
```js
const choo = require('choo')

const app = choo()
app.model('title', {
  state: {
    title: 'my-demo-app'
  },
  reducers: {
    'update': (action, state) => ({ title: action.payload })
  },
  effects: {
    'update': (action, state, send) => (document.title = action.payload)
  }
})

const mainView = (params, state, send) => choo.view`
  <main class="app">
    <h1>${state.title}</h1>
    <label>Set the title</label>
    <input
      type="text"
      placeholder=${state.title}
      oninput=${(e) => send('title:update', { payload: e.target.value })}>
  </main>
`

app.router((route) => [
  route('/', mainView)
])

const tree = app.start()
document.body.appendChild(tree)
```

## Perform HTTP requests

## Concepts
- __state:__ a single object that contains all application state, should only
  ever be modified by `reducers`
- __reducers:__ syncronous functions that modify `state`
- __effects:__ asyncronous functions that perform IO. Effects should call
  `send()` when done
- __subscriptions:__ streams of data that can either be written to or read from

## API
### app = choo()
Create a new `choo` app

### app.model(name?, obj)
Create a new model. Models modify data and perform IO. Obj takes the following
arguments:
- __state:__ object. Key value store of initial values
- __reducers:__ object. Syncronous functions that modify state. Each function
  has a signature of `(action, state)`
- __effects:__ object. Asyncronous functions that perform IO. Each function has
  a signature of `(action, state, send)` where `send` is a reference to
  `app.send()`

If a `name` string is passed as a first argument, `reducers` and `signatures`
will be prefixed by the name. So if name is "user" and a reducer called
"update" is registered, it would be accessed as `'user:update'` in `send()`.

### choo.view\`html\`
Tagged template string HTML builder. See
[`yo-yo`](https://github.com/maxogden/yo-yo) for full documentation. Views
should be passed to `app.router()`

### app.router(params, state, send)
Creates a new router. See
[`sheet-router`](https://github.com/yoshuawuyts/sheet-router) for full
documentation. Registered views have a signature of `(params, state, send)`,
where `params` is URI partials.

### tree = app.start()
Start the application. Returns a DOM element that can be mounted using
`document.body.appendChild()`.

## Packages used
- __views:__ [`yo-yo`](https://github.com/maxogden/yo-yo)
- __models:__ [`send-action`](https://github.com/sethvincent/send-action),
  [`xtend`](https://github.com/raynos/xtend)
- __routes:__ [`sheet-router`](https://github.com/yoshuawuyts/sheet-router)

## Optimizing
To bring down file size, consider running the following `browserify`
transforms:
- [unassertify](https://github.com/twada/unassertify) - remove `assert()`
  statements which reduces file size. Use as a `--global` transform
- [varify](https://github.com/thlorenz/varify) - replace `const` with `var`
  statements. Use as a `--global` transform
- [uglifyify](https://github.com/hughsk/uglifyify) - minify your code using
  UglifyJS2. Use as a `--global` transform

## Packages that work well together
- [xhr](https://github.com/Raynos/xhr) - small XHR wrapper
- [tachyons](https://github.com/tachyons-css/tachyons) - functional CSS for
  humans

## Installation
```sh
$ npm install choo
```

## License
[MIT](https://tldrlegal.com/license/mit-license)

[0]: https://img.shields.io/badge/stability-experimental-orange.svg?style=flat-square
[1]: https://nodejs.org/api/documentation.html#documentation_stability_index
[2]: https://img.shields.io/npm/v/choo.svg?style=flat-square
[3]: https://npmjs.org/package/choo
[4]: https://img.shields.io/travis/yoshuawuyts/choo/master.svg?style=flat-square
[5]: https://travis-ci.org/yoshuawuyts/choo
[6]: https://img.shields.io/codecov/c/github/yoshuawuyts/choo/master.svg?style=flat-square
[7]: https://codecov.io/github/yoshuawuyts/choo
[8]: http://img.shields.io/npm/dm/choo.svg?style=flat-square
[9]: https://npmjs.org/package/choo
[10]: https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat-square
[11]: https://github.com/feross/standard