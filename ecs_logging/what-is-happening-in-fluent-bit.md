# What is Happening in Fluent Bit?

- [ECS Logging in Merritt](README.md)

---

Scenario: we have a service named app that logs in the following format
```
127.0.0.1 /foo/bar 200
127.0.0.1 /foo/bar?hi=x 200
127.0.0.1 /bar/foo 404
```

After firelens processing, the following will be generated.

Note that the fluentd logging driver adds: container_name, container_id.

> [!NOTE]
> The record seems to get tagged with `appname-firelens` somewhere in the workflow although it is not clear where that happens.

```

{
  "container_name": "...",
  "container_id": "...",
  "ecs_cluster": "...",
  "ecs_task_arn": "...",
  "ecs_task_definition": "...",
  "log": "127.0.0.1 /foo/bar 200"
}
{
  "container_name": "...",
  "container_id": "...",
  "ecs_cluster": "...",
  "ecs_task_arn": "...",
  "ecs_task_definition": "...",
  "log": "127.0.0.1 /foo/bar?hi=x 200"
}
{
  "container_name": "...",
  "container_id": "...",
  "ecs_cluster": "...",
  "ecs_task_arn": "...",
  "ecs_task_definition": "...",
  "log": "127.0.0.1 /bar/foo 404"
}

```

Add a custom parser for app `parser-customizations.conf`
```
[PARSER]
    Name app
    Format regex
    Regex ^(?<host>[^ ]*) (?<path>[^\"]*?) (?<code>[^ ]*)$
```

Add a custom rule to `app-customizations.conf`
```
[SERVICE]
    Parsers_file /fluent-bit/etc/parsers-merged.conf

[FILTER]
    Name Parser
    Match *
    Parser app
    Key_Name log
    Reserve_Data On
```

After running this customization, the following is generated
```
{
  "container_name": "...",
  "container_id": "...",
  "ecs_cluster": "...",
  "ecs_task_arn": "...",
  "ecs_task_definition": "...",
  "host": "127.0.0.1",
  "path": "/foo/bar",
  "code": 200
}
{
  "container_name": "...",
  "container_id": "...",
  "ecs_cluster": "...",
  "ecs_task_arn": "...",
  "ecs_task_definition": "...",
  "host": "127.0.0.1",
  "path": "/foo/bar?hi=x",
  "code": 200
}
{
  "container_name": "...",
  "container_id": "...",
  "ecs_cluster": "...",
  "ecs_task_arn": "...",
  "ecs_task_definition": "...",
  "host": "127.0.0.1",
  "path": "/bar/foo",
  "code": 404
}
```

This data will be sent to the Merritt OpenSearch collection.

### Other notes
- Fluent bit provides a json parser named `docker` that will preserve json input.
- Custom filters can be applied based on the presence/abscense of json properties.
- Regex tests will return false if a property contains a json object
- Open search will reject records if type conflicts are introduced
  - `{"foo": "bar"}`
  - `{"foo": {"cat": "meow"}}`

---

[NEXT](fluent-bit-customization-with-cloudwatch.md)
