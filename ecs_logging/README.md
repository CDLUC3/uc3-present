# ECS Logging in Merritt with Fluent Bit

The [Merritt System](https://github.com/CDLUC3/mrt-doc/blob/main/README.md) is migrating from EC2-based microservices to ECS-base microservice containers. As a result, the Merritt team will no longer be able to rely on long-living log files.

This site describes how the Merritt team migrated our logging infrastructure from log files to cloud-based logging solutions in AWS.

- [EC2 Logfiles to Migrate to a Containerized Environment](ec2-files.md)
- [Log4j and Tomcat Overrides to Send Logs to STDOUT](log-files-to-STDOUT.md)
- [AWS Firelens](aws-firelens.md)
- [ECS Service Configuration](ecs-service-configuration.md)
- [Fluent Bit Customization V1: Create OpenSearch Records](fluent-bit-customization.md)
- [What is Happending in Fluent Bit?](what-is-happening-in-fluent-bit.md)
- [Fluent Bit Customization V2: Create OpenSearch Records AND CloudWatch Logs](fluent-bit-customization-with-cloudwatch.md)
- [Command Line Testing of FluentBit Configs](fluent-bit-command-line-testing.md)