# ECS Service Configuration

- [ECS Logging in Merritt](README.md)

---

## AWS Firelens and Fluent Bit

Per recommendations in AWS documentation, ECS log output is processed with AWS Firelens.

AWS Firelens runs a sidecar container within an ECS Task that processes STDOUT with [Fluent Bit](https://docs.fluentbit.io/manual/about/what-is-fluent-bit).

- When run separately from AWS Firelens, Fluent Bit can process [many input types](https://docs.fluentbit.io/manual/data-pipeline/inputs).  In the case of AWS Firelens, the [Forward Input Processor](https://docs.fluentbit.io/manual/data-pipeline/inputs/forward) is automatically configured.  
- Fluent Bit can apply a set of [parsers](https://docs.fluentbit.io/manual/data-pipeline/parsers) against input entries.  Merritt log entries consist of [JSON records](https://docs.fluentbit.io/manual/data-pipeline/parsers/json) and [text strings that can be parsed by regular expressions](https://docs.fluentbit.io/manual/data-pipeline/parsers/regular-expression)
- Fluent Bit can apply a set of [filters](https://docs.fluentbit.io/manual/data-pipeline/filters) against log entries as they are processed.  These filters typically add, remove and restructure JSON representations of log entries.
- Fluent Bit applies [output rules](https://docs.fluentbit.io/manual/data-pipeline/outputs) to forward log entries to places where they can be persisted.  When AWS Firelens is configured, a default output destination is specified.

---

[NEXT](ecs-service-configuration.md)