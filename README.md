# Test Env for Prometheus/Pushgateway/Grafana

I needed a quick, easy and fast way as a starting point to test things with Prometheus, Grafana and the Pushgateway. This repo contains a simple `docker-compose.yml` file which will do the following things:

* Create a docker network at `10.222.222.0/24`
* Start a Prometheus instance (running at port 9090, also forwarded to the host) on IP `10.222.222.2`
* Start a Pushgateway instance, which is being scraped by Prometheus (idem port 9091) on IP `10.222.222.3`
* Start a Grafana instance (idem port 3000) at IP `10.222.222.4`

You can push metrics using most Prometheus client libraries to the Pushgateway, or you can alter the (simplistic) `prometheus.yml` to add your own configuration of Prometheus.

To make things easy and to avoid needing a service discovery mechanism, the containers are hard wired to IP addresses in the `docker-compose.yml` file.

### Usage

Run

```
$ docker-compose up
```

Browse to

* [localhost:9090](http://localhost:9090) for Prometheus
* [localhost:3000](http://localhost:9090) for Grafana (username/password is `admin`/`admin`)

### Setting up Grafana to use Prometheus

Add a data source in Grafana,

* Name: `prometheus`
* Type: `Prometheus`
* URL: `http://10.222.222.2:9090`

Leave all other settings, then "Add". It should be successful; now you can use Prometheus as a data source for your dashboards.

## Adding your own containers

If you want to test the scraping of your own containers, the easiest way is to add the container to the `docker-compose.yml` and alter the `prometheus.yml` file in the [`config`](config) directory to match your container.

You should then be able to see the metrics at [localhost:9090](http://localhost:9090).

## Links

* https://prometheus.io

## License

Copyright 2016 Haufe-Lexware GmbH & Co. KG

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
