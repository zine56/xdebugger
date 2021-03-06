# XDebugger :postbox:

**A very lightweight library to create a production debugger with custom errors readable for humans. Includes errors in table format, logger and search methods with dynamic filters.**

This library was destinated for debug in development or production environment and obtain errors in background, with a readable format for any developer, also can disable console logs any time.

1. [Demos](https://github.com/matiaslopezd/xdebugger#demos)
2. [Installation](https://github.com/matiaslopezd/xdebugger#installation-cd)
3. [How to use](https://github.com/matiaslopezd/xdebugger#how-to-use-fire)
  - [Basic Initialization](https://github.com/matiaslopezd/xdebugger#basic-initialization-wrench)
  - [Log](https://github.com/matiaslopezd/xdebugger#how-log-inbox_tray)
  - [Listen global errors](https://github.com/matiaslopezd/xdebugger#how-to-listen-errors-fax)
  - [Get logs](https://github.com/matiaslopezd/xdebugger#how-to-get-logs-mailbox_with_mail)
  - [Search / Filter logs](https://github.com/matiaslopezd/xdebugger#how-to-search--filter-logs-mailbox_closed)
    - [$eq](https://github.com/matiaslopezd/xdebugger#eq-search)
    - [$cnt](https://github.com/matiaslopezd/xdebugger#cnt-search)
    - [$lte](https://github.com/matiaslopezd/xdebugger#lte-search)
    - [$lt](https://github.com/matiaslopezd/xdebugger#lt-search)
    - [$gte](https://github.com/matiaslopezd/xdebugger#gte-search)
    - [$gt](https://github.com/matiaslopezd/xdebugger#gt-search)
  - [Send logs to API](https://github.com/matiaslopezd/xdebugger#how-set-and-send-log-per-log-to-a-api-outbox_tray)
  - [Export and dowload logs](https://github.com/matiaslopezd/xdebugger#how-export-and-download-logs-outbox_tray)
  - [View logs in console](https://github.com/matiaslopezd/xdebugger#how-console-logs-in-readable-format-bar_chart)
  - [Clean logger](https://github.com/matiaslopezd/xdebugger#how-to-clean-debugger-mailbox_with_no_mail)
  - [Default data of logs](https://github.com/matiaslopezd/xdebugger#how-set-default-data-bookmark_tabs)
  - [Set max logs and size](https://github.com/matiaslopezd/xdebugger#how-set-max-records-and-size-of-values-log-straight_ruler)

## Demos
- [Simple initialization](https://matiaslopezd.github.io/xdebugger/examples/generate)
- [Custom initialization](https://matiaslopezd.github.io/xdebugger/examples/custom_init)

## Installation :cd:
```html
<!-- In development environment -->
<script src="xdebugger.js"></script>

<!-- In production environment -->
<script src="xdebugger.min.js" async></script>

```

## How to use :fire:

Use XDebugger is really easy and very flexible, it's possible define `debug`, `log`, `datatypes`, `action`, `default` and `max` variables for different logs requirements.

Can use for debug development or in production website. Also if you want for example can load debug parameters via API, like `{ debug: true, log: false }` and obtain errors or custom logs if you set.

## Basic initialization :wrench:

```javascript
// In development environment
const debug = new XDebugger({ debug: true, log: true });

// In production environment
const debug = new XDebugger({ debug: true });

```

Set `{ log: false }` disable completely console. That's mean XDebugger, clean console for not show any log, info, warn, table and error. If try log anything console show:

```javascript
  Console was cleared
> console.log("Try write something like this");
"Developer mode is disabled."
```


## How log :inbox_tray:

You can log any data you want, also XDebugger add by default `time` as _String_ and `timestamp` format as _Number_.

```javascript
// Obviously you need initialize before!!
// Example log =>

debug.log({
  message: `${value} is not a valid data type`,
  code: 150,
  explanation: `The variable was evaluated and is not valid, typeof ${value}: ${typeof value}.`,
  response: `Change to ${this._datatypes.toString()} data type.`,
  error: `Value expect a ${this._datatypes.toString()} and recieve ${typeof value}`,
})
```

## How to listen errors :fax:
```javascript
window.onerror = (message, url, line, col, err) => debug.log(debug.onerror(message, url, line, col, err));
```

## How to get logs :mailbox_with_mail:

```javascript
debug.logged

// Output =>
> [{..}]
```

## How to search / filter logs :mailbox_closed:

Search was based in MongoDB queries for filter. XDebugger have 6 types of filters:

 - `$eq`: Match value of log key with query value key
 - `$cnt`: Contains value of log key with query value key
 - `$lte`: Less or equal value of log key with query value key
 - `$gte`: More or equal value of log key with query value key
 - `$lt`: Less value of log key with query value key
 - `$gt`: More value of log key with query value key
 
### `$eq` search

```javascript
// Implicit search
debug.search({
  timestamp: 1556528447311,
  code: 105
});

// Explicit search
debug.search({
  timestamp: {
    $eq: 1556528447311
  },
  code: 401
});
```

### `$cnt` search

```javascript
debug.search({
  error: {
    $cnt: "filter"
  },
  internalCode: 9224
});
```

### `$lte` search

```javascript
debug.search({
  timestamp: {
    $lte: 1556528447311
  },
  code: 105
});
```

### `$lt` search

```javascript
debug.search({
  timestamp: {
    $lt: 1556528447311
  },
  browser: "Google Chrome"
});
```

### `$gte` search

```javascript
debug.search({
  timestamp: {
    $gte: 1556528447311
  },
  version: 12.1
});
```

### `$gt` search

```javascript
debug.search({
  timestamp: {
    $gt: 1556528447311
  },
  name: "John Doe",
  idUser: "507f191e810c19729de860ea"
});
```

## How set and send log per log to a API :outbox_tray:
```javascript
const debug = new Debugger({ debug: true, action: (log) => {
    // Here your code to POST log
  } 
});
```

## How export and download logs :outbox_tray:

You can export all logs and download in a `JSON` file.

```javascript
debug.export();
```

Also can export filtered logs as follow:

```javascript
debug.export(debug.search({
  code: 105,
  browser: "Brave"
}));
```

## How console logs in readable format :bar_chart:

The `view` function accept `Array` or `Object`, that mean one or more logs.

```javascript
debug.view(debug.logged);
```
## How to clean debugger :mailbox_with_no_mail:

This clean logger and console.

```javascript
debug.clean();
```
## How set default data :bookmark_tabs:

`default` key in object initialization parameter allow set default data like browser, version, internalCode, etc.

```javascript
// 'library' as third party source functionality

const debug = new XDebugger({ debug: true, default: {
  browser: library.browser.name,
  version: library.browser.version,
  language: library.browser.lang,
  ...
}});
```

Also you can rewrite default `time` and `timestamp`:

```javascript
const debug = new XDebugger({ debug: true, default: {
  time: mycustomtime.toString(),
  timestamp: mycustomtime.getTime(),
}});
```

### How set max records and size of values log :straight_ruler:

- `length`: set the max records logger can save
- `size`: set the max size value of log allowed in MB, Ex: `{ key: "value value value value value value value " } => 80 Bytes`

```javascript
const debug = new XDebugger({ debug: true, default: {
  max: {
    length: 60,
    size: 100
  }
}});
```

## `datatypes` Not tested yet!! :heavy_exclamation_mark:

Define allowed data type of log value.

Not tested with functions or other data type.

> By default it allow `number`, `string`, `object`.


**Try not use complex schema with console.table, that lose the readable format**
