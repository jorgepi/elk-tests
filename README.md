# elk-tests
Testing ELK solution with Docker


## Quick build:

`$ export ELK_VERSION=7.2.0`
`$ docker-compose up`

## Adding extensions

APM:

`$ docker-compose -f docker-compose.yml -f extensions/apm-server/apm-server-compose.yml up`

The most basic configuration to send traces to apm server. Is to specify the SERVICE_NAME and SERVICE_URL. Here is an example Python FLASK configuration:

```
import elasticapm
from elasticapm.contrib.flask import ElasticAPM

from flask import Flask

app = Flask(__name__)
app.config['ELASTIC_APM'] = {
    # Set required service name. Allowed characters:
    # a-z, A-Z, 0-9, -, _, and space
    'SERVICE_NAME': 'PYTHON_FLASK_TEST_APP',

    # Set custom APM Server URL (default: http://localhost:8200)
    'SERVER_URL': 'http://apm-server:8200',

    'DEBUG': True,
}
```

Curator:

`$ docker-compose -f docker-compose.yml -f extensions/curator/curator-compose.yml up`

Logspout:

`$ docker-compose -f docker-compose.yml -f extensions/logspout/logspout-compose.yml up`

In your Logstash pipeline configuration, enable the udp input and set the input codec to json:

```
input {
  udp {
    port  => 5000
    codec => json
  }
}
```
