# Fluent Bit Customization V2: Create OpenSearch Records AND CloudWatch Logs

- [ECS Logging in Merritt](README.md)

---

The following line exists at the end of the `java-customizations.conf` file

```yaml
@INCLUDE /fluent-bit/etc/log_source_stdout-to-cw-customizations.conf
```

## log_source_stdout-to-cw-customizations.conf

```conf
[FILTER]
    Name rewrite_tag
    Match *firelens*
    Rule $log_source ^stdout$ cw.$container_name.$container_id false
    emitter_mem_buf_limit 512MB

[OUTPUT]
    Name                    cloudwatch
    Match                   cw.*
    auto_create_group       true
    log_group_name          /mrt/ecs/${MRT_ENV}/${MRT_SERVICE}
    log_stream_prefix       ecs-stdout-       
    region                  us-west-2
    retry_limit             2
    log_key                 log
    log_retention_days      7
```

## What is this doing?

If a record has `log_source == stdout`, then fluent-bit re-tags the record as `cw.*`.

By re-tagging the record, the default output rule (send to opensearch) of `*firelens*` is not applied.

The new output rule that is defined in the file above will apply to records tagged as `cw.*`.  This output rule will send the records to CloudWatch.

---

[NEXT](fluent-bit-command-line-testing.md)