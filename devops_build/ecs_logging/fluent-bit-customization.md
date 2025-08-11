# Fluent Bit Customization V1: Create OpenSearch Records

- [ECS Logging in Merritt](README.md)

---

The Merritt System builds a number of docker images that support the Merritt system.

The [mrt-fluent-bit](https://github.com/CDLUC3/merritt-docker/tree/main/mrt-inttest-services/mrt-fluent-bit) image contains customized fluent-bit configurations for each Merritt service.

## Dockerfile
```Dockerfile
FROM public.ecr.aws/aws-observability/aws-for-fluent-bit

RUN yum -y update

COPY *-customizations.conf /fluent-bit/etc/

RUN cat /fluent-bit/etc/parsers.conf /fluent-bit/etc/parsers-customizations.conf > /fluent-bit/etc/parsers-merged.conf 
```

## parsers-customizations.conf
```conf
# Merritt parser customization

[PARSER]
    Name mrt-json
    Format regex
    Regex ^\[(?<reqid>[0-9a-f]{8,8}-[0-9a-f]{4,4}-[0-9a-f]{4,4}-[0-9a-f]{4,4}-[0-9a-f]{12,12})\]\s+(?<log>.*)$

[PARSER]
    Name tomcat-access
    Format regex
    Regex ^(?<host>[^ ]*) - (?<user>[^ ]*) \[(?<time>[^\]]*)\] "(?<method>\S+)( +(?<path>[^\"]*?)(?: +\S*)?)?" (?<response_code>[^ ]*) (?<size>[^ ]*).*$

[PARSER]
    Name catalina
    Format regex
    Regex ^(?<time>\d\d-(Jan|Feb|Mar|Apr|May|Jun|Jul|Aug|Sep|Oct|Nov|Dec)-\d\d\d\d \d\d:\d\d:\d\d.\d\d\d) (?<log_level>INFO|WARN|ERROR|SEVERE|DEBUG) \[(?<thread_name>[^ \]]+)\] (?<log>.*)$

[PARSER]
    Name sinatra
    Format regex
    Regex ^(?<host>[^ ]*) - (?<user>[^ ]*) \[(?<time>[^\]]*)\] "(?<method>\S+)( +(?<path>[^\"]*?)(?: +\S*)?)?" (?<response_code>[^ ]*) (?<size>[^ ]*) (?<duration>[^ ]*).*$

[PARSER]
    Name zoo
    Format regex
    Regex ^(?<time>[0-9\- :,]+) \[(?<worker>[a-zA-Z0-9:]+)\] - (?<log_level>(INFO|WARN|ERROR|SEVERE|DEBUG)) (?<message>.*)$

# used by command line test driver
[PARSER]
    Name testdriver
    Format regex
    Regex ^(?<log>.*)$
```

## java-customizations.conf

```conf
# mrt-fluent-bit concatenates standard parsers with Merritt parsers
[SERVICE]
    Parsers_file /fluent-bit/etc/parsers-merged.conf

# Parse one of three input types: Tomcat Access log, Tomcat Catalina Log, Log4j ECS json output
[FILTER]
    Name Parser
    Match *
    Parser docker
    Parser tomcat-access
    Parser catalina
    Key_Name log
    Reserve_Data On
    # Preserve_Key On

# Handle log.* properties that might conflict with a TEXT data type for log
[FILTER]
    Name modify
    Match *
    Condition Key_exists log.level
    Rename log.level log_level

[FILTER]
    Name modify
    Match *
    Condition Key_exists log.logger
    Rename log.logger log_logger

# If log contains a json object, no regex will match
[FILTER]
    Name modify
    Match *
    Condition Key_value_does_not_match log .
    Rename log log_json

# Suppress health check
[FILTER]
    Name   grep
    Match  *
    Exclude path "state\?t=json"

# Identify the source log file based on properties that are present
[FILTER]
    Name modify
    Match *
    Condition Key_exists ecs.version
    Add log_source mrt-log4j

[FILTER]
    Name modify
    Match *
    Condition Key_exists host
    Add log_source tomcat-access

[FILTER]
    Name modify
    Match *
    Condition Key_exists log_level
    Add log_source catalina

[FILTER]
    Name modify
    Match *
    Condition Key_does_not_exist method
    Condition Key_does_not_exist host
    Condition Key_does_not_exist log_level
    Add log_source stdout
```

## AWS Firelens Generated Configuration

By default, AWS firelens provides a default output action.  

If you connect to the Docker container and `cat /fluent-bit/etc/fluent-bit.conf`, you will see the following default action.

Records that are processed with this workflow have been tagged with `myservicename-firelens`.  By default, these records will be sent to Open Search.

```conf
[INPUT]
    Name tcp
    Listen 127.0.0.1
    Port 8877
    Tag firelens-healthcheck

[INPUT]
    Name forward
    Mem_Buf_Limit 25MB
    unix_path /var/run/fluent.sock

[INPUT]
    Name forward
    Listen 127.0.0.1
    Port 24224

[FILTER]
    Name record_modifier
    Match *
    Record ecs_cluster mrt-ecs-dev-stack
    Record ecs_task_arn ...
    Record ecs_task_definition ...

@INCLUDE /fluent-bit/etc/FLUENTBIT_FILE-customizations.conf

[OUTPUT]
    Name null
    Match firelens-healthcheck

[OUTPUT]
    Name opensearch
    Match myservicename-firelens*
    Aws_Auth On
    Aws_Region us-west-2
    Aws_Service_Name aoss
    Host ....us-west-2.aoss.amazonaws.com
    Index my_index_myservicename
    Port 443
    Suppress_Type_Name On
    Trace_Error On
    Trace_Output On
    retry_limit 2
    tls On
```


## What Happens to Log Records in

---

[NEXT](what-is-happening-in-fluent-bit.md)