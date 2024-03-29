# usbguard-logstash-pipeline

## Input and Output ##

This pipeline does not provide inputs or outputs so you can configure whatever you need. Files named `input.conf` and `output.conf` will not interfere with updates via git, so name your files accordingly.

Here are examples how your files could look if you want to use a local Redis instance.

```
input {
  redis {
    host => localhost
    key => "detector"
    data_type => list
  }
}

output {
  redis {
    key => "forwarder"
    data_type => list
    host => localhost
  }
}
```
## Additional In-Between-Filtering

In between input and output, you might want to use an in between filter like this:

```
 else if [log][file][path] == "/var/log/detector.log" {
    redis {
      host => "localhost"
      data_type => "list"
      key => "detector"
      congestion_threshold => 12000
    }
```

## Filebeat

Filebeat needs to be configured appropriately: Usbguard can write to syslog, or to the default usbguard-log (which is chosen here as an example):

```
- type: log
  # Change to true to enable this input configuration.
  enabled: true
  # Paths that should be crawled and fetched. Glob based paths.
  paths:
    - /var/log/detector/detector.log
```
