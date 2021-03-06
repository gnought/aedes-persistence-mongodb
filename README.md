# aedes-persistence-mongodb

![.github/workflows/ci.yml](https://github.com/robertsLando/aedes-persistence-mongodb/workflows/.github/workflows/ci.yml/badge.svg)
[![Dependencies Status](https://david-dm.org/moscajs/aedes-persistence-mongodb/status.svg)](https://david-dm.org/moscajs/aedes-persistence-mongodb)
[![devDependencies Status](https://david-dm.org/moscajs/aedes-persistence-mongodb/dev-status.svg)](https://david-dm.org/moscajs/aedes-persistence-mongodb?type=dev)
\
[![Known Vulnerabilities](https://snyk.io/test/github/moscajs/aedes-persistence-mongodb/badge.svg)](https://snyk.io/test/github/moscajs/aedes-persistence-mongodb)
[![NPM version](https://img.shields.io/npm/v/aedes-persistence-mongodb.svg?style=flat)](https://npm.im/aedes-persistence-mongodb)
[![NPM downloads](https://img.shields.io/npm/dm/aedes-persistence-mongodb.svg?style=flat)](https://npm.im/aedes-persistence-mongodb)

[Aedes][aedes] [persistence][persistence], backed by [MongoDB][mongodb].

See [aedes-persistence][persistence] for the full API, and [Aedes][aedes] for usage.

## Install

```
npm i aedes aedes-persistence-mongodb --save
```

## API

<a name="constructor"></a>
### aedesPersistenceMongoDB([opts])

Creates a new instance of aedes-persistence-mongodb.
It accepts a connections string `url` or you can pass your existing `db` object. Also, you can choose to set a `ttl` (time to live) for your subscribers or packets. This option will help you to empty your db from keeping useless data.

### Options

- `url`: The MongoDB connection url
- `ttl`: Used to set a ttl (time to live) to documents stored in collections
  - `packets`: Could be an integer value that specify the ttl in seconds of all packets collections or an Object that specifies for each collection its ttl in seconds. Packets collections are: `incoming`, `outgoing`, `retained`, `will`.
  - `susbscriptions`: Set a ttl (in seconds)
- `db`: Existing MongoDB instance (if no `url` option is specified)
- `dropExistingIndexes`: Flag used to drop any exsisting index previously created on collections (except default index `_id`)

### Examples

```js
aedesPersistenceMongoDB({
  url: 'mongodb://127.0.0.1/aedes-test', // Optional when you pass db object
  // Optional ttl settings
  ttl: {
      packets: 300, // Number of seconds
      subscriptions: 300,
  }
})
```

With the previous configuration all packets will have a ttl of 300 seconds. You can also provide different ttl settings for each collection:

```js
ttl: {
      packets: {
        incoming: 100,
        outgoing: 100,
        will: 300,
        retained: -1
      }, // Number of seconds
      subscriptions: 300,
}
```

If you want a specific collection to be **persistent** just set a ttl of `-1` or `null` or `undefined`.

If you want to reuse an existing MongoDb instance just set the `db` option:

```js
aedesPersistenceMongoDB({
 db:db
})
```

With mongoose:

```js
aedesPersistenceMongoDB({
 db: mongoose.connection.useDb('myDbName').db
})
```

## License

MIT

[aedes]: https://github.com/moscajs/aedes
[persistence]: https://github.com/moscajs/aedes-persistence
[mongodb]: https://www.mongodb.com
