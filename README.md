# Official fluentd docker image customized

[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/scott-seo/dvdstore-api/blob/master/LICENSE)

## Purpose
* Provide easy docker logs forwarding in json using customized fluentd

## How to use this image
### Start the database docker image scottseo/custom-fluentd
```
$ docker run -it --rm -p 24224:24224 --name custom-docker-fluent-logger -e TOKEN=${LOGGLY_TOKEN} custom-fluentd:latest
```
## How to test this image
```
docker run --log-driver=fluentd --link custom-docker-fluent-logger:logger --log-opt fluentd-address=0.0.0.0:24224  ubuntu echo -e "{\"name\":\"scott\", \"log\":{\"message\":\"scott\"}}"
```

## Configuration

```xml
<source>
  @type forward
  port 24224
  bind 0.0.0.0
</source>

<filter **>
  type parser
  format multi_format
  <pattern>
    format json
  </pattern>
  <pattern>
    format none
  </pattern>
  key_name log
  hash_value_field message
</filter>

<filter **>
  type record_transformer
  <record>
    fluent_tag ${tag}
  </record>
</filter>

<match **>
  type loggly
  loggly_url "https://logs-01.loggly.com/inputs/#{ENV['TOKEN']}/tag/fluentd"
</match>
```
