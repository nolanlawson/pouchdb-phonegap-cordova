# pouchdb-phonegap-cordova

PouchDB runs great on PhoneGap and Cordova. Here's how to get started.

## Sample apps

* [Cordova PouchDB "Hello world" app](https://github.com/nolanlawson/pouchdb-cordova-hello-world)
* [Cordova PouchDB "Hello world" app, with the SQLite Plugin](https://github.com/nolanlawson/pouchdb-cordova-hello-world-with-sqlite-plugin)

## Installation

Just download [`pouchdb.js`](http://pouchdb.com/guides/setup-pouchdb.html) and include it in your `index.html`:

```html
<script src="./path/to/pouchdb.js"></script>
```

By default, PouchDB will use either WebSQL or IndexedDB &ndash; whatever is available on the device. PouchDB supports:

| Platform | Supported version |
| ----- | --- |
| Android | 4+ |
| iOS | 7+ |
| Windows Phone | 8+ |
| Firefox OS | all |
| BlackBerry | 10+ |

Android 2.3 and Windows Phone 7 are not supported.

## SQLite Plugin

The Cordova [SQLite Plugin](https://github.com/brodysoft/Cordova-SQLitePlugin/) provides a WebSQL-like API that gives direct access to the same SQLite API used by native apps. 

You may want it for a few reasons:

* To avoid [storage limits](http://pouchdb.com/faq.html#data_limits).
* To run in a background thread instead of the UI thread (e.g. if the UI slows down).
* To use SQLite on Windows Phone 8 (instead of IndexedDB).
* To provide more direct control over the SQLite database (e.g. so you can prepopulate the database using native code).

It's recommended to avoid the SQLite Plugin unless you really need it, since it adds extra complexity. But if you do need it, here's what you should do:

**First**, be sure to include `pouchdb.js` after `cordova.js` and/or `SQLitePlugin.js`, so that PouchDB will know to wait for the `'ondeviceready'` event and use the SQLite Plugin:


```html
<script src="cordova.js"></script>
<script src="SQLitePlugin.js"></script>
<script src="./path/to/pouchdb.js"></script>
```

**Second**, create a PouchDB that prefers WebSQL to IndexedDB (rather than the reverse, which is the default). You can use this code:

```js
var db = new PouchDB('my_database', {adapter: 'websql'});
if (!db.adapter) {
  // websql/sqlite not supported (e.g. FirefoxOS)
  db = new PouchDB('my_database');
}
```

The adapter is called `'websql'`, but as long as you have the SQLite Plugin installed (i.e. you have `navigator.sqlitePlugin` or `window.sqlitePlugin` available as global variables), then PouchDB will use the SQLite Plugin.

See the sample apps above if you get stuck.