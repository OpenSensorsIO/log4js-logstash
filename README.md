opensensors-log4js-logstash
===============

This is a very simple log4js appender that can talk to logstash instances. This version is still very specific to Gembly Games B.V. but feel free to fork and adjust =)

Installation
------------
You can install install opensensor-log4js-logstash by adding this .git url to your package.json

Usage: logstash configuration
-----------------------------
In the "input" part of the logstash server conf :

    tcp {
        'codec' => 'json_lines'
        'data_timeout' => '10'
        'port' => '5959'
        'type' => 'tcp-input'
    }



Usage: log4js configuration
---------------------------
Plain javascript
```javascript
    var log4js = require('log4js');
    log4js.configure({
        "appenders": [
            {
                "category": "TEST",
                "type": "opensensor-log4js-logstash",
                "host": "localhost",
                "port": 5959,
                "fields": {
                    "instance": "MyAwsInstance",
                    "source": "myApp",
                    "environment": "development"
                }
            },
            {
                "category": "tests",
                "type": "console"
            }
        ],
        "levels": {
            "tests":  "DEBUG"
        }
    });

    var log = log4js.getLogger('tests');

    log.error('hello hello');
```

Or in YAML
```yaml
appenders:
  [
    {
        type: 'console',
        category:
          [
            'WEBSERVER','TEST'
          ]
    },
    {
        type: 'opensensors-log4js-logstash',
        host: 'localhost',
        port: 5959,
        batch: {
            size: 200,
            timeout: 10
        },
        fields: {
            instance: 'MyAwsInstance',
            source: 'myApp',
            environment: 'development',
        },
        category:
          [
            'WEBSERVER','TEST'
          ]
    }
  ]
replaceConsole: true
```
