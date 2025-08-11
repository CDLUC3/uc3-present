# Command Line Testing of FluentBit Configs

- [ECS Logging in Merritt](README.md)

The code directory for `mrt-fluent-bit.conf` contains a few files to support command line testing of fluent bit rules.  These files have been constructed to enable testing without relying on OpenSearch or CloudWatch.

## test-driver-stdin.conf

The following config file is designed to take command line input and make it look as if it has been processed by AWS Firelens.


```conf
[SERVICE]
    Parsers_file /fluent-bit/etc/parsers-merged.conf

# Simulate input using the 'dummy' input provider
[INPUT]
    Name    stdin
    Tag     stdin
    Parser  testdriver

[FILTER]
    Name parser
    Match *
    Key_Name log
    Parser docker

[FILTER]
    Name modify
    Match *
    Add container_name mysrvc
    Add container_id 0001

@INCLUDE /fluent-bit/etc/java-customizations.conf

[OUTPUT]
    name        stdout
    match       *
```

## Special handling of STDOUT records

At the bottom on the `java-customizations.conf` file there is an include that is commented out.

In order to test, toggle the commenting in these 2 lines.
```
# @INCLUDE /fluent-bit/etc/log_source_stdout-to-stdout-customizations.conf
@INCLUDE /fluent-bit/etc/log_source_stdout-to-cw-customizations.conf
```

The modified include file `log_source_stdout-to-stdout-customizations.conf` looks like this.

```conf
[FILTER]
    Name rewrite_tag
    Match stdin
    Rule $log_source ^stdout$ cw.$container_name.$container_id false
    emitter_mem_buf_limit 512MB

[OUTPUT]
    Name                    stdout
    Match                   cw.*
    Format                  json_lines
```

This rule will send the special handling of STDOUT records to to STDOUT without attempting to send to CloudWatch.

## Testing at the command line

Run `docker compose build` to build a local copy of the docker file for testing.

Run the following command to start an interactive session.

```
docker run -it --rm -e MRT_SERVICE=aaa -v ./test-driver-stdin.conf:/fluent-bit/etc/fluent-bit.conf ${ECR_REGISTRY}/mrt-fluent-bit
AWS for Fluent Bit Container Image Version 2.33.1
Fluent Bit v1.9.10
* Copyright (C) 2015-2022 The Fluent Bit Authors
* Fluent Bit is a CNCF sub-project under the umbrella of Fluentd
* https://fluentbit.io
```

### Mimic a JSON logged record
```
{"ecs.version": 22, "message": "hello"}
```

Note that the record is categorized as `mrt-log4j`
```
[0] stdin: [1754954664.201771042, {"ecs.version"=>22, "message"=>"hello", "container_name"=>"mysrvc", "container_id"=>"0001", "log_source"=>"mrt-log4j"}]
```

### Mimic a tomcat access record

```
127.0.0.1 - - [10/Jun/2025:00:01:32 -0700] "GET /state HTTP/1.1" 200 957
```

Note that the fields are parsed and the record is tagged as `tomcat-access`
```
[0] stdin: [1754954762.399392629, {"host"=>"127.0.0.1", "user"=>"-", "time"=>"10/Jun/2025:00:01:32 -0700", "method"=>"GET", "path"=>"/state", "response_code"=>"200", "size"=>"957", "container_name"=>"mysrvc", "container_id"=>"0001", "log_source"=>"tomcat-access"}]
```

### Mimic a Structured catalina.out message

```
06-Aug-2025 19:42:47.893 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [25626] milliseconds
```

Note that the fields are parsed and tagged as `catalina`
```
[0] stdin: [1754954999.202767961, {"time"=>"06-Aug-2025 19:42:47.893", "log_level"=>"INFO", "thread_name"=>"main", "log"=>"org.apache.catalina.startup.Catalina.start Server startup in [25626] milliseconds", "container_name"=>"mysrvc", "container_id"=>"0001", "log_source"=>"catalina"}]
```

### Mimic an unstructured catalina.out message
```
The quick brown fox jumped over the lazy dogs
```

Pay particular attention to this line, note the `cw.mysrvc.*`.  This is a simulation of the record being processed by the cloudwatch output rule.
```
[0] cw.mysrvc.0001: [1754955126.690142465, {"log"=>"The quick brown fox jumped over the lazy dogs", "container_name"=>"mysrvc", "container_id"=>"0001", "log_source"=>"stdout"}]
```

## Additional Testing Notes

Note: Fluent Bit documentation recommends [Rubular](https://rubular.com/) for regex testing.

---


