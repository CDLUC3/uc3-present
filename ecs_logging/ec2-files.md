# EC2 Logfiles to Migrate to a Containerized Environment

- [ECS Logging in Merritt](README.md)

---

## Merritt Tomcat Files

5 Merritt microservices run as tomcat applications.  These services produce 4 log files of interest to the Merritt team.

### Structured Log Files in JSON

These log entries have been engineered to be viewed in OpenSearch.  These entries follow [Elastic Common Schema](https://github.com/elastic/ecs-logging/blob/main/README.md) file conventions.

```json
  {
    "@timestamp": "2025-08-11T15:38:13.221Z",
    "log_level": "INFO",
    "message": "Start updateVersion",
    "ecs.version": "1.2.0",
    "event.dataset": "tomcat",
    "process.thread.name": "http-nio-8080-exec-7",
    "log_logger": "org.cdlib.mrt.store.storage.StorageService",
    "LocalID": "null",
    "PrimaryID": "ark:/99999/fk49926885",
    "node": "7777",
    "store.function": "update",
    "log_json": {
      "origin": {
        "file": {
          "name": "StorageService.java",
          "line": 144
        },
        "function": "updateVersion"
      }
    },
    "source": "stdout",
  }
```

### Tomcat Access Logs

Tomcat produces a structured text file for each web request that is processed.

```
127.0.0.1 - - [10/Jun/2025:00:01:32 -0700] "GET /state HTTP/1.1" 200 957
```

### Catalina Logs (Structured)

The tomcat server outputs structured log messages to the catalina.out file.

```
06-Aug-2025 19:42:47.893 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [25626] milliseconds
```

### Catalina Logs (Unstructured)

STDOUT from any application running in tomcat is streamed to STDOUT.  This content is may be difficult to parse.

```
[JerseyBase]>getStateResponse:xml - formatType=xml - mimeType=text/xml
BatchReportConsumerDaemon: Checking for additional tasks -  Current tasks: 0 - Max: 5
ProvisionConsumerDaemon: Checking for additional Job tasks for Worker: Current tasks: 0 - Max: 2
```

## Other Merritt Services

Other Merritt services generate structured log files that will require a unique parsing rule.

The Merritt UI is a Ruby on Rails application.  It produces a structured json long file and an unstructured narrative log file.

---

[NEXT](log-files-to-STDOUT.md)