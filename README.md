# How to log anything
This repository contains recommended practices for setting up logging infrastructure in the most common web frameworks.

PRs are more than welcome!

## Contents
- [NodeJS with ExpressJS](#nodejs-with-expressjs)

## NodeJS with ExpressJS
### Requests logging
Use [morgan](https://github.com/expressjs/morgan) to log requests to your service:
```javascript
var express = require('express')
var morgan = require('morgan')

var app = express()
app.use(morgan(`:date[iso] [method=:method] [endpoint=:url] [status=:status] [duration_in_ms=:response-time]`))
// outputs '2018-09-08T23:22:21.653Z [method=GET] [endpoint=/requested-endpoint] [status=200] [duration_in_ms=35.134]
```

### Application logging
Use [winston](https://github.com/winstonjs/winston) to log application messages:
```javascript
const { createLogger, format, transports } = require('winston');
const { combine, timestamp, label, printf } = format;

const myFormat = printf(info => {
  return `${info.timestamp} [${info.label}] ${info.level}: ${info.message}`;
});

const logger = createLogger({
  format: combine(
    label({ label: 'right meow!' }),
    timestamp(),
    myFormat
  ),
  transports: [new transports.Console()]
});

logger.info('I am an info message!')
// outputs '2018-09-08T23:22:21.611Z [right meow] info: I am an info message!
```

