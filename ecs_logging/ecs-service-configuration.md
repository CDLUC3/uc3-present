# ECS Service Configuration

- [ECS Logging in Merritt](README.md)

---

## Constructing a Merritt Service

Merritt Services are constructed using [Sceptre](https://docs.sceptre-project.org/latest/) and [AWS CloudFormation](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html).  While the Merritt Team strives to make most of our code public, our Sceptre code is kept in a private repository by default.

This documentation will attempt to extact and explain our fragments of our Sceptre configuration.

### Merritt Service Configuration Yaml

Each Merritt Environment (or "Stack") consists of numerous microservices.  Several of these services contain custom Merritt code.  Other services make use of open source software such as ZooKeeper.

The Merritt service operates a production service, a stage service and numerous developement "stacks".  In order to simplify the management of these configurations, it was desireable to organize as much configuration data as possible into a Yaml file.

Each Merritt Service is configured in a Yaml file named `service_data.yaml`.

This file allows us to define ports, healthchecks, and enviornment variables for each Merritt microservice.

Services with similar configurations are able to share configuration values using best practices for yaml files. 

Here is a simplified version of the file.

#### service_data.yaml

```yaml
ports:
  store: 
  - 8080
 
envvars:
  store:
    MRT_DOCKER_HOST: 'store'
    LOG_STORE: '/home/mrtHomes/logs'
    SERVICE: 'store'
    HOSTNAME: 'store'
    LOGLEVEL: 'info'
    CATALINA_OPTS: "-Dlog4j.configurationFile=/log4j2-ecs.xml -Djava.net.preferIPv4Stack=true -DAccessDaemon=0 -Dfile.encoding=UTF8 -Dorg.apache.tomcat.util.buf.UDecoder.ALLOW_ENCODED_SLASH=true"
  store_ecs-ephemeral:
    NODE_TABLE: nodes-pairtree-docker
    MERRITT_STORE_INFO: store-info-docker
    MRT_DOCKER_HOST: store
    MRT_MINIO_HOST: minio
    MRT_MINIO_PORT: 9000
    SSM_SKIP_RESOLUTION: Y
    << : *single_node
```

### Merritt Stack Configuration

Each Merritt Stack has a configuration file `config.yaml` that imports the `service_data.yaml` file.

This technique allows us to share the service_data.yaml file across environments or stacks.

```yaml
sceptre_user_data:
  services: !file 'config/service_data.yaml'
```

### Micro Service Config File

Each instance of a Merritt Service has config file that defines the docker image, tag and number of copies of the service to initiate.

For instance, `service_store.yaml`
```yaml
parameters:
  env: ecs-ephemeral
  count: '1'
  svcname: store
  image: '999999999.dkr.ecr.us-west-2.amazonaws.com/mrt-store'
  tag: 'ecs-dev'
```

### ECS Service

The template for each Merritt Service constructs a number of components

#### Parameter Definitions
```yaml
AWSTemplateFormatVersion: '2010-09-09'

# For CPU and memory values, see:
#   https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html#task_size

Parameters:
  env:
    Type: String
  svcname:
    Type: String
  count:
    Type: Number
    Default: 1

  # applies to all services
  loadbalancertg:
    Type: String
    Default: ''
  taskrole:
    Type: String
  executionrole:
    Type: String
  cluster:
    Type: String
  namespace:
    Type: String
  efsfileid:
    Type: String
    Default: ''
  # Common cpu/memory combinations
  # 1024/2048
  # 2048/4096
  # 4096/8192
  fargateCpu:
    Type: String
    Default: '1024'
  fargateMemory:
    Type: String
    Default: '2048'
  image:
    Type: String
    Default: ''
  tag:
    Type: String
    Default: ''
  serviceNameCf:
    Type: String
    Default: ''
  debugJava:
    Type: String
    Default: 'false'
```

#### Jinja2 Logic to Extract Settings from service_data.yaml

```yaml
{% set uc3_domain = sceptre_user_data.uc3_domain %}
{% set uc3_domain_data = sceptre_user_data.services.uc3_environments[uc3_domain] %}
{% set svcname = sceptre_user_data.stack_parameters.svcname %}
{% set serviceNameCf = sceptre_user_data.services.serviceNameCf[svcname] if sceptre_user_data.services.serviceNameCf[svcname] is defined else svcname %}
{% set svcname_env = svcname + '_' + sceptre_user_data.stack_parameters.env %}
{% set svcports = sceptre_user_data.services.ports[svcname] if sceptre_user_data.services.ports[svcname] is defined else [] %}
{% set fluentbit = true %}
{% set fluentbit_files = sceptre_user_data.services.fluentbit_files %}
{% set fluentbit_file = fluentbit_files[svcname] if fluentbit_files[svcname] is defined else '/fluent-bit/etc/no-customizations.conf' %}
{% set javadebug = false %}
{% if svcname in sceptre_user_data.services.javadebug %}
  {% set javadebug = true if 'debugJava' in sceptre_user_data.stack_parameters %}
{% endif %}
```

#### Define the ECS Service

Merritt utilizes ECS Service Discovery to allow the containers within a stack to communicate with each other using a simple container name.  This configuration requires every published port to be defined to service dicovery.

```yaml
Resources:
  Service{{serviceNameCf}}:
    Type: AWS::ECS::Service
    Properties:
      ServiceName: {{svcname}}
      Cluster: !Ref cluster
      DesiredCount: !Ref count
      TaskDefinition: !Ref TaskDefinition{{serviceNameCf}}
      EnableExecuteCommand: true

```
#### Configure ServiceConnect if the Service has exposed ports

```yaml
      {% if svcports | length > 0 %}
      ServiceConnectConfiguration:
        Enabled: true
        Namespace: !Ref namespace
        Services:
        {% for port in svcports %}
        {% if svcports | length > 1 %}
        - PortName: {{svcname}}{{port}}_port
          DiscoveryName: {{svcname}}{{port}}
          ClientAliases:
          - DnsName: {{svcname}}
            Port: {{port}}
        {% else %}
        - PortName: {{svcname}}{{port}}_port
          DiscoveryName: {{svcname}}
          ClientAliases:
          - DnsName: {{svcname}}
            Port: {{port}}
        {% endif %}
        {% endfor %}        
      {% endif %}
```
#### Configure a Deployment Strategy

A multi-node ZooKeeper cluster requires a carefully orchestrated deployment strategy.

```yaml
      DeploymentConfiguration:
        {% if svcname in sceptre_user_data.services.zkmultinode %}
        MaximumPercent: 100
        MinimumHealthyPercent: 0
        {% else %}
        MaximumPercent: 200
        MinimumHealthyPercent: 50
        {% endif %}
        DeploymentCircuitBreaker:
          Enable: true
          Rollback: true
      {% if 'loadbalancertg' in sceptre_user_data.stack_parameters %}

```

#### Configure a Load-Balancer for public-facing services

```yaml
      LoadBalancers:
        - ContainerName: {{svcname}}
          ContainerPort: {{svcports[0]}}
          TargetGroupArn: !Ref loadbalancertg
      {% endif %}
      CapacityProviderStrategy:
        - CapacityProvider: 'FARGATE'
          Weight: 1
      NetworkConfiguration:
        AwsvpcConfiguration:
          AssignPublicIp: 'ENABLED'
          Subnets:
            - {{sceptre_user_data.public_subnet_a}}
            - {{sceptre_user_data.public_subnet_b}}
            - {{sceptre_user_data.public_subnet_c}}
          SecurityGroups:
            - {{sceptre_user_data.merritt_sg}}
```

#### Create a CloudWatch Group for the Container

If Fluent Bit has been configured, most messages will be routed to OpenSearch.

The Log Router itself will log to CloudWatch. 

If Fluent Bit has not been configured, all output will got to CloudWatch.

```yaml
  CloudwatchLogsGroup{{serviceNameCf}}:
    Type: 'AWS::Logs::LogGroup'
    Properties:
      LogGroupName: !Sub "/mrt/ecs/${env}/${svcname}"
      RetentionInDays: 7
```

#### Create the TaskDefinition for the Merritt Service

This code snippet has special logic to configure ports for a node within a multi-node zookeeper cluster AND to define a port for debugging a tomcat application.

```yaml
  TaskDefinition{{serviceNameCf}}:
    Type: AWS::ECS::TaskDefinition
    Properties:
      NetworkMode: awsvpc
      ContainerDefinitions:
      - Image: "{{sceptre_user_data.stack_parameters.image}}:{{sceptre_user_data.stack_parameters.tag}}"
        Name: {{svcname}}
        {% if svcports | length > 0 %}
        PortMappings:
        {% for port in svcports %}
        - ContainerPort: {{port}}
          {% if svcname in sceptre_user_data.services.zkmultinode %}
          HostPort: {{port}}
          {% endif %}
          Protocol: 'tcp'
          Name: {{svcname}}{{port}}_port
        {% endfor %}
        {% if javadebug %}
        - ContainerPort: 8000
          HostPort: 8000
          Protocol: 'tcp'
          Name: {{svcname}}_debug_port
        {% endif %}
        {% endif %}
```

#### Configure the Fluent Bit Logger if applicable, otherwise use CloudWatch

```yaml
        {% if fluentbit %}
        LogConfiguration:
          LogDriver: awsfirelens
          Options:
            Name: opensearch
            Host: !Sub '{{sceptre_user_data.opensearch}}.${AWS::Region}.aoss.amazonaws.com'
            Port: 443
            Index: my_index_{{svcname}}
            Aws_Auth: 'On'
            Aws_Region: !Sub '${AWS::Region}'
            Aws_Service_Name: aoss
            Trace_Error: 'On'
            Trace_Output: 'On'
            Suppress_Type_Name: 'On'
            tls: 'On'
            retry_limit: 2
        {% else %}
        LogConfiguration:
          LogDriver: awslogs
          Options:
            awslogs-group: !Ref CloudwatchLogsGroup{{serviceNameCf}}
            awslogs-region: !Sub '${AWS::Region}'
            awslogs-stream-prefix: ecs
        {% endif %}
        StartTimeout: 30
        StopTimeout: 30
```

#### Configure Service Health Check and Entrypoint
```yaml
        {% set healthcheck = [] %}
        {% if svcname in sceptre_user_data.services.healthcheck %}
          {% set healthcheck = sceptre_user_data.services.healthcheck[svcname] %}
        {% endif %}
        {% if svcname_env in sceptre_user_data.services.healthcheck %}
          {% set healthcheck = sceptre_user_data.services.healthcheck[svcname_env] %}
        {% endif %}
        {% if healthcheck | length > 0 %}
        HealthCheck:
          Command: 
          {% for c in healthcheck %}
          - {{c}}
          {% endfor %}
        {% endif %}

        {% set entrypoint = [] %}
        {% if svcname in sceptre_user_data.services.entrypoint %}
          {% set entrypoint = sceptre_user_data.services.entrypoint[svcname] %}
        {% endif %}
        {% if svcname_env in sceptre_user_data.services.entrypoint %}
          {% set entrypoint = sceptre_user_data.services.entrypoint[svcname_env] %}
        {% endif %}
        {% if entrypoint | length > 0 %}
        EntryPoint:
        {% for c in entrypoint %}
        - {{c}}
        {% endfor %}
        {% elif javadebug %}
        EntryPoint:
        - catalina.sh
        - jpda
        - run
        {% endif %}

        {% set command = [] %}
        {% if svcname in sceptre_user_data.services.command %}
          {% set command = sceptre_user_data.services.command[svcname] %}
        {% endif %}
        {% if svcname_env in sceptre_user_data.services.command %}
          {% set command = sceptre_user_data.services.command[svcname_env] %}
        {% endif %}
        {% if command | length > 0 %}
        Command:
        {% for c in command %}
        - {{c}}
        {% endfor %}
        {% endif %}
```

#### Configure Service Environment Variables

Environment varibles may be defined
- At the service level
- At the service/stack level

The resulting set will be union of the 2 configurations.

```yaml
        Environment:
        - Name: MERRITT_ECS
          Value: {{sceptre_user_data.stack_parameters.env}}

        {% set services_envvars = sceptre_user_data.services.envvars %}
        {% set envvars = services_envvars[svcname] if svcname in services_envvars else {} %}
        {% for name, val in envvars.items() %}
        - Name:  "{{ name }}"
          Value: "{{ val }}"
        {% endfor %}

        {% set envvars = services_envvars[svcname_env] if svcname_env in services_envvars else {} %}
        {% for name, val in envvars.items() %}
        - Name:  "{{ name }}"
          Value: "{{ val }}"
        {% endfor %}

        {% if javadebug %}
        - Name: JPDA_TRANSPORT
          Value: dt_socket
        - Name: JPDA_ADDRESS
          Value: '*:8000'
        {% endif %}
```

#### Mount EFS (if applicable)
```yaml
        {% if 'efsfileid' in sceptre_user_data.stack_parameters %}
        MountPoints:
        - SourceVolume: ingest_efs
          ContainerPath: /tdr/ingest/queue
        {% endif %}
```

#### Create the Fluent Bit Side Car (if applicable)

Note that a custom config file can be specified within the container.

```yaml
      {% if fluentbit %}
      - Name: log_router
        Image: !Sub "${AWS::AccountId}.dkr.ecr.${AWS::Region}.amazonaws.com/mrt-fluent-bit:latest"
        Cpu: 0
        MemoryReservation: 51
        Essential: true
        User: 0
        LogConfiguration: 
          LogDriver: awslogs
          Options:
            awslogs-group: !Ref CloudwatchLogsGroup{{serviceNameCf}}
            mode: non-blocking
            max-buffer-size: 25m
            awslogs-region: !Sub '${AWS::Region}'
            awslogs-stream-prefix: firelens
        FirelensConfiguration:
          Type: fluentbit
          Options:
            config-file-type: file
            config-file-value: {{fluentbit_file}}
        Environment:
        - Name: MRT_ENV
          Value: !Ref env
        - Name: MRT_SERVICE
          Value: !Ref svcname 
      {% endif %}
```

#### Set Processing Resources for the Container

```yaml
      Cpu: !Ref fargateCpu
      Memory: !Ref fargateMemory
      RequiresCompatibilities:
        - FARGATE
      TaskRoleArn: !Ref taskrole
      ExecutionRoleArn: !Ref executionrole
```

#### If applicable, Mount EFS within the Container

```yaml
    {% if 'efsfileid' in sceptre_user_data.stack_parameters %}
    Volumes:
      - Name: ingest_efs
        EFSVolumeConfiguration:
          FilesystemId: {{sceptre_user_data.stack_parameters.efsfileid}}
          TransitEncryption: ENABLED
      {% endif %}
```

---

[NEXT](fluent-bit-customization.md)