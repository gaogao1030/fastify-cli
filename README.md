# fastify-cli

[![js-standard-style](https://img.shields.io/badge/code%20style-standard-brightgreen.svg?style=flat)](http://standardjs.com/) [![Build Status](https://travis-ci.org/fastify/fastify-cli.svg?branch=master)](https://travis-ci.org/fastify/fastify-cli) [![Greenkeeper badge](https://badges.greenkeeper.io/fastify/fastify-cli.svg)](https://greenkeeper.io/)

Command line tools for [Fastify](https://github.com/fastify/fastify).
Generate, write and run an application with one single command!

## Install
```bash
npm install fastify-cli --global
```

## Usage

`fastify-cli` offers a single command line interface for your fastify
project:

```bash
$ fastify
```

Will print an help:

```
fastify command line interface, available commands are:

  * start       start a server
  * generate    generate a new project
  * version     the current fastify-cli version
  * help        help about commands

Launch 'fastify help [command]' to know more about the commands.

The default command is start, you can hit

  fastify start plugin.js

to start plugin.js.
```

### start

You can start any fastify plugin with:

```bash
$ fastify start plugin.js
```

A plugin can be as simple as:

```js
// plugin.js
module.exports = function (fastify, options, next) {
  fastify.get('/', function (req, reply) {
    reply.send({ hello: 'world' })
  })
  next()
}
```

If you are using Node 8+, you can use `async` functions too:

```js
// async-await-plugin.js
module.exports = async function (fastify, options) {
  fastify.get('/', async function (req, reply) {
    return { hello: 'world' }
  })
}
```

For a list of available flags for `fastify start` see the help: `fastify help start`.

If you want to use custom options, just export an options object with your route and run the cli command with the `--options` flag.

```js
// plugin.js
module.exports = function (fastify, options, next) {
  fastify.get('/', function (req, reply) {
    reply.send({ hello: 'world' })
  })
  next()
}

module.exports.options = {
  https: {
    key: 'key',
    cert: 'cert'
  }
}
```

#### Options
You can pass the following options via cli arguments, every options has the corresponding environment variable:

|  Description | Short command | Full command | Environment variable |
| ------------- | ------------- |-------------| ----- |
| Port to listen on (default to 3000) | `-p` | `--port` | `FASTIFY_PORT or PORT` |
| Address to listen on | `-a` | `--address` | `FASTIFY_ADDRESS` |
| Socket to listen on | `-s` | `--socket` | `FASTIFY_SOCKET` |
| Log level (default to fatal) | `-l` | `--log-level` | `FASTIFY_LOG_LEVEL` |
| Prints pretty logs | `-P` | `--pretty-logs` | `FASTIFY_PRETTY_LOGS` |
| Use custom options | `-o` | `--options` | `FASTIFY_OPTIONS` |
| Set the prefix | `-r` | `--prefix` | `FASTIFY_PREFIX` |
| Defines the maximum payload, in bytes,<br>the server is allowed to accept |  | `--body-limit` | `FASTIFY_BODY_LIMIT` |

By default `fastify-cli` runs [`dotenv`](https://www.npmjs.com/package/dotenv), so it will load all the env variables stored in `.env` in your current working directory.


#### fastify version discovery

If Fastify is installed as a project dependency (with `npm install --save fastify`),
then `fastify-cli` will use that version of Fastify when running the server.
Otherwise, `fastify-cli` will use the version of Fastify included within `fastify-cli`.

### generate

`fastify-cli` can also help with generating some project scaffolding to
kickstart the development of your next Fastify application. To use it:

1. `mkdir yourapp`
2. `cd yourapp`
3. `npm init`
4. `fastify generate`
5. `npm install`

The sample code offers you four npm tasks:

* `npm start` - starts the application
* `npm run dev` - starts the application with
  [`pino-colada`](https://github.com/lrlna/pino-colada) pretty logging
  (not suitable for production)
* `npm test` - runs the tests
* `npm run lint` - runs the linter and automatically fixes the errors

You will find three different folders:
- `plugins`: the folder where you will place all your custom plugins
- `services`: the folder where you will declare all your endpoints
- `test`: the folder where you will declare all your test

Finally there will be an `app.js` file, which is your entry point.
It is a standard Fastify plugin and you will not need to add the `listen` method to run the server, just run it with one of the scripts above.

## Contributing
If you feel you can help in any way, be it with examples, extra testing, or new features please open a pull request or open an issue.

The code follows the Standard code style.
[![js-standard-style](https://cdn.rawgit.com/feross/standard/master/badge.svg)](https://github.com/feross/standard)

## License
**[MIT](https://github.com/fastify/fastify-cli/blob/master/LICENSE)**

*The software is provided "as is", without warranty of any kind, express or implied, including but not limited to the warranties of merchantability, fitness for a particular purpose and non infringement. In no event shall the authors or copyright holders be liable for any claim, damages or other liability, whether in an action of contract, tort or otherwise, arising from, out of or in connection with the software or the use or other dealings in the software.*

Copyright © 2016-2018 Fastify Team
