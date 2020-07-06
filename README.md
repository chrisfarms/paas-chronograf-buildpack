# paas-chronograf-buildpack

Cloudfoundry buildpack for deploying a [Chronograf][chronograf] instance
connected to an `influxdb` service instance.

* Visualise your application logs and metrics by adding [paas-telegraf-buildpack][] sidecars
* Alert on metric conditions by combining with [paas-kapacitor-buildpack][]

Designed for use with GOV.UK PaaS cloudfoundry, PRs welcome for other
cloudfoundry's with influxdb service support.

Chronograf does not provide any metrics itself, to get
your application logs and metrics ingested into your
InfluxDB instance, add the [paas-telegraf-buildpack][]
sidecar to each of your applications.

## Usage

Create a manifest.yml describing your application:

```
---
applications:
- name: chronograf
  memory: 250M
  instances: 1
  stack: cflinuxfs3
  health-check-type: process
  buildpacks:
  - https://github.com/chrisfarms/paas-kapacitor-buildpack
  - https://github.com/chrisfarms/paas-chronograf-buildpack
  services:
  - my-influx-db-service-instance    # bind to an influxdb instance
```

Push your application:

```
cf push -f manifest.yml
```

Note: This buildpack does not require any other files, but if `cf` complains
you don't have any files during push, you may need to add _something_ to keep
it happy (for example: `touch Chronograf`).

[chronograf]: https://www.influxdata.com/time-series-platform/chronograf/
[paas-kapacitor-buildpack]: https://github.com/chrisfarms/paas-kapacitor-buildpack
[paas-telegraf-buildpack]: https://github.com/chrisfarms/paas-telegraf-buildpack
