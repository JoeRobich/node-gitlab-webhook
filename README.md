# node-gitlab-webhook
Gitlab Webhooks handler based on Node.js. Support multiple handlers.

## Language

- [中文](https://github.com/excaliburhan/node-gitlab-webhook/blob/master/docs/zh_CN.md)

## Instructions

This library is modified for Gitlab, Github version here: [node-github-webhook](https://github.com/excaliburhan/node-github-webhook).

If you want to know the settings of Gitlab webhooks, please see: [gitlab webhooks](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html).

## Installation

`npm install node-gitlab-webhook --save`

## Usage

```js
var http = require('http')
var createHandler = require('node-gitlab-webhook')
var handler = createHandler([ // multiple handlers
  { path: '/webhook1', secret: 'secret1' },
  { path: '/webhook2', secret: 'secret2' }
])
// var handler = createHandler({ path: '/webhook1', secret: 'secret1' }) // single handler

http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(7777)

handler.on('error', function (err) {
  console.error('Error:', err.message)
})

handler.on('push', function (event) {
  console.log(
    'Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref
  )
  switch (event.path) {
    case '/webhook1':
      // do sth about webhook1
      break
    case '/webhook2':
      // do sth about webhook2
      break
    default:
      // do sth else or nothing
      break
  }
})
```
