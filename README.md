# docker-compose for ELK

Simple docker-compose config for ELK stack.

### Getting started
- Start it up by running `docker-compose up -d`

### Example
- Using node.js, winston-logstash-transport and winston

create a new file (winston.js)
```
const appRoot = require('app-root-path');
const winston = require('winston');
const LogstashTransport = require('winston-logstash-transport').LogstashTransport;

const options = {
  logstash: {
    port: 28777,
    host: 'localhost'
  },
};

const logger = winston.createLogger({
  transports: [
    new LogstashTransport(options.logstash),
  ],
  format: winston.format.combine(
    winston.format.json(),
    winston.format.timestamp()
  ),
  exitOnError: false,
});

logger.stream = {
  write: function (message, encoding) {
    logger.info(message);
  },
};

module.exports = logger;
```

in your `app.js`, add the following:
```
...
const winston = require('./winston.js'); // this is the logger created above
app.use(morgan('combined', { stream: winston.stream }));
...
```
