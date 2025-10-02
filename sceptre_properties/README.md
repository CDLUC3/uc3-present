# Rationale for Placement of Sceptre Properties

---

## As a sceptre stack parameter in a resource config file
```
# /config/dev/my_resource.yaml

parameters:
  param: !stack_output_external ...
  foo: bar
```
### Limitations
- No support for hash structures

### Benefits
- Establishes a clear set of cloud formation dependencies
- Can be accessed via sceptre_user_data.stack_parameters
- Default values can be inferred (unless you use sceptre_user_data.stack_parameters)
- Can enumerate allowable values
- Can be used in CloudFormation dependencies

---

##  As sceptre_user_data in a specific resource config file
```
# /config/dev/my_resource.yaml

sceptre_user_data:
  param: !stack_output_external ...
  foo:
    my_array:
    - cat
    - dog
    color: green
```
### Limitations
- Cannot be used in conjunction with an environmental config.yaml.  This supersedes that structure.

### When to use it
- Complex hashes that apply to only one resource

## As sceptre_user_data in an environmental config.yaml
```
# /config/dev/config.yaml

sceptre_user_data:
  param: !stack_output_external ...
  foo:
    my_array:
    - cat
    - dog
    color: green
  bar:
     dog: puppy
```
### Limitations
- Does not establish resource dependencies
- Cannot define 100% of properties this way, cloud formation needs at least a minimum of one property to establish identity

### When to use it
- Values that are being re-used across multiple resources
- Complex hash data

---

## As a value in a yaml file (imported with a !file resolver)
```
# /config/my_yaml.yaml
foo:
  my_array:
  - cat
  - dog
  color: green
bar:
   dog: puppy

# /config/dev/config.yaml

sceptre_user_data:
  param: !stack_output_external ...
  my_data: !file '/config/my_yaml.yaml'
```

### Limitations
- Must be static values (no parameter substitution from CF or Jinja)

### When to use it
- Values stored in complex hashes (CF does not handle hashes well)
- Re-usable values that benefit from yaml anchors and aliases
- Complex settings that benefit from hash key lookups

---

## Stack Group Config parameters

```
# /config/dev/config.yaml

foo: bar

parameters:
  param: !stack_output_external ...
```

The value can then be referenced in a config file or in a template.

```
# /config/dev/my_resource.yaml

parameters:
  param: !stack_output_external ...
  foo: {{stack_group_config.foo}}
```

### Limitations
- TBD

### When to use
- TBD

---

## Sceptre Variable Files 

- https://docs.sceptre-project.org/latest/docs/cli.html?highlight=--var-file#variable-handling
- Barbara and the PAD team use this approach

### When to Use
- TBD

### Limitations
- variable file must always be provided with the sceptre command
- TBD

---

## Hard Coded in a Template

```
  Properties:
    Memory: 1024
```

### When to Use
- TBD

### Limitations
- Not re-usable

### When to use it
- Value is not likely to change and has little significance apart from the template
