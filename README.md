# hyperdb-mesh

Automatically authorise HyperDB peers to create a mesh, this code was extracted from
 the fantasic Cabal [cabal-core on Github](https://github.com/cabal-club/cabal-core).

HyperDB allows peers to be authorised to replicate and using [discovery-channel](https://www.npmjs.com/package/discovery-swarm), peers are automatically authorised to form the mesh network.

As such you need to keep your discovery channel secret or run this in a controlled environment.
 I am trying to figure out a way to provide some authorisation process to run in untrusted
 places but I need to think about that.

# Install

```sh
npm install hyperdb-mesh
```

# Usage

### Peer A

```js

var mesh = Mesh('./demo.db', null, { id: 'mesh1' })
var db   = mesh1.db

mesh.on('ready', function () {

  console.log('peer A key ', db.key)
  // bec87d64bfdc33b6b02779666e46a3f01f0aa07f721c2177a44d64c4a6b50afd

  db.put('/hello', 'world', function (e) {
    if (e) throw e
  })
})

```

### Peer B

Peer B running in a different computer or folder.

```js

var key = 'bec87d64bfdc33b6b02779666e46a3f01f0aa07f721c2177a44d64c4a6b50afd'

var mesh = Mesh('./demo.db', key, { id: 'mesh2' })
var db   = mesh1.db

mesh.on('ready', function () {
  console.log('peer A key ', db.key)
  db.get('/hello', function (e, d) {
    if (!e) console.log('winning ', d[0].value)
  })
})

```

# hyperdb-mesh events & methods

### ready event

```js

mesh.on('ready', function () {
  console.log('underlying hyperDB is ready')
})

```

### HyperDB

The underlying HyperDB instance is accessible from .db

```js

console.log(mesh.db.key.toString('hex'))

```

Pass in [HyperDB](https://github.com/mafintosh/hyperdb) options like so:

```js

var mesh = Mesh('./json_api.db', key, { id: 'api', options: { valueEncoding: 'json' } })

```

### Todo

* Run tests on travis
* Test on Windows
* More testing
* Global password or extra auth strategies for peers
