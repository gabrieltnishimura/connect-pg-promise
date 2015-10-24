# Connect PG Promise

Adapted from Connect PG Simple

## Installation

```bash
npm install connect-pg-promise
```

Once npm installed the module, you need to create the **session** table in your database. For that you can use the [table.sql] (https://github.com/voxpelli/node-connect-pg-simple/blob/master/table.sql) file provided with the module: 

```bash
psql mydatabase < node_modules/connect-pg-simple/table.sql
```

Or simply play the file via a GUI, like the pgAdminIII queries tool.

## Usage

Simple example (Express 4):

```javascript
var session = require('express-session');

app.use(session({
  store: new (require('connect-pg-promise')(session))(),
  secret: process.env.FOO_COOKIE_SECRET,
  cookie: { maxAge: 30 * 24 * 60 * 60 * 1000 } // 30 days
}));
```

## Advanced options

* **db** - Recommended. If you want the session store to use the same database module (compatible with [pg](https://www.npmjs.org/package/pg) / [pg.js](https://www.npmjs.org/package/pg.js)) as the rest of your app, then send it in here. Useful as eg. the connection pool then can be shared between the module and the rest of the application. Also useful if you want this module to use the native bindings of [pg](https://www.npmjs.org/package/pg) as this module itself only comes with [pg.js](https://www.npmjs.org/package/pg.js).
* **ttl** - the time to live for the session in the database – specified in seconds. Defaults to the cookie maxAge if the cookie has a maxAge defined and otherwise defaults to one day.
* **schemaName** - if your session table is in another Postgres schema than the default (it normally isn't), then you can specify that here.
* **tableName** - if your session table is named something else than `session`, then you can specify that here.
* **pruneSessionInterval** - sets the delay in seconds at which expired sessions are pruned from the database. Default is `60` seconds. If set to `false` no automatic pruning will happen. Automatic pruning weill happen `pruneSessionInterval` seconds after the last pruning – manual or automatic.
* **errorLog** – the method used to log errors in those cases where an error can't be returned to a callback. Defaults to `console.error()`, but can be useful to override if one eg. uses [Bunyan](https://github.com/trentm/node-bunyan) for logging.

## Useful methods

* **pruneSessions([callback(err)])** – will prune old sessions. Only really needed to be called if **pruneSessionInterval** has been set to `false` – which can be useful if one wants improved control of the pruning.

## Changelog

### 1.0.0

* First NPM-version of the script originally published as a Gist here: https://gist.github.com/voxpelli/6447728
