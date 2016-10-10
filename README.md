# Official fluentd docker image customized

[![license](https://img.shields.io/github/license/mashape/apistatus.svg?maxAge=2592000)](https://github.com/scott-seo/dvdstore-api/blob/master/LICENSE)

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
