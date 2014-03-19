# Steno
steno(速记)是轻量级的日志组件库，专门用于支持CF

## Concepts-概念
loggers，sinks，formatters是它的三个主要类。
Loggers是入口，它负责接收client的消息，并创建为结构化消息，再将结构化消息传递给sinks。
sinks负责接收结构化的消息，并调用foratter将它们格式为字符串，然后传输给另外的输出端。
## Configuration-配置

To use steno, you must configure one or more 'sinks', a 'codec' and a 'context'.
If you don't provide a codec, steno will encode your logs as JSON.


For example:

    config = Steno::Config.new(
      :sinks   => [Steno::Sink::IO.new(STDOUT)],
      :codec   => Steno::Codec::Json.new,
      :context => Steno::Context::ThreadLocal.new)

### from YAML file

Alternatively, Steno can read its configuration from a YAML file in the following format:

```yaml
# config.yml
---
logging:
  file: /some/path            # Optional - path a log file
  max_retries: 3              # Optional - number of times to retry if a file write fails.
  syslog: some_syslog.id      # Optional - only works on *nix systems
  eventlog: true              # Optional - only works on Windows
  fluentd:                    # Optional
    host: fluentd.host
    port: 9999
  level: debug                # Optional - Minimum log level that will be written.
                              # Defaults to 'info'
```
Then, in your code:
```ruby
config = Steno::Config.from_file("path/to/config.yml")
```

With this configuration method, if neither `file`, `syslog` or `fluentd` are provided,
steno will use its stdout as its sink. Also, note that the top-level field `logging` is required.

### from Hash

As a third option, steno can be configured using a hash with the same structure as the above
YAML file (without the top-level `logging` key):
```ruby
config = Steno::Config.from_hash(config_hash)
```

## Usage

    Steno.init(config)
    logger = Steno.logger("test")  
    logger.info("Hello world!")

### Log levels

    LEVEL	NUMERIC RANKING 
    off		0
    fatal	1
    error	5
    warn	10
    info	15
    debug	16
    debug1	17
    debug2	18
    all		30

