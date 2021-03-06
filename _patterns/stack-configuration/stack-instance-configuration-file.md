---
layout: pattern
title:  "Stack Instance Configuration File Pattern"
date: 2019-03-28 10:07
category: Stack Configuration Patterns
order: 23
published: false
---

Values can be provided to instances of [template stacks](/patterns/stack-replication/template-stack.html) by putting them into files, which are then checked into version control.


## Also Known As

- Environment configuration files
- Stack configuration files


## Motivation

Creating configuration files for a stack's instances is straightforward and easy to understand. Because the file is committed to the source code repository, it is easy to see what values are used for any given environment ("what is the maximum cluster size for production?"), to trace the history for debugging ("when did the maximum cluster size change?"), and for auditing ("who changed the maximum cluster size?").


## Applicability

Keeping configuration files with a stack project is useful when the specific list of stack instances is known, and when instances don't need to be created dynamically.


## Consequences

Because creating a new stack instance involves creating a new configuration file in the stack project repository, it may be difficult to automatically create stack instances on demand. This may be an issue when using ephemeral test instances, for example. This can often be worked around, for example by creating temporary configuration files or by sharing a generic configuration file across instances.

Another drawback of stack parameter files comes when a stack is managed through a Continuous Delivery pipeline. In this case, making a change to the configuration for production involves committing a change to the parameter file in the stack's source code, and then waiting for the update to flow through the full pipeline to production. This can be painful, particularly for changes which are needed quickly.


## Implementation

[Template stacks](/patterns/stack-replication/template-stack.html) typically define parameters which can be set differently for different instances of the stack. For example, a stack that is used to create a web server cluster may have different values for the sizing of the cluster in different environments:


| Stack Instance | environment_id | cluster_minimum | cluster_maximum |
|-------|--------|---------|
| webserver_test | test | 1 | 1 |
| webserver_staging | staging | 1 | 2 |
| webserver_production | production | 2 | 5 |


These can be defined in stack parameter files, typically one file for each stack instance:


~~~ console
   ├── src/
   │   ├── cluster.infra
   │   └── networking.infra
   ├── environments/
   │   ├── staging.properties
   │   └── production.properties
   └── test/
~~~


The parameter file for an environment then defines the values to use for the relevant stack instance:


~~~ properties
environment_id  = test
cluster_minimum = 1
cluster_maximum = 1
~~~


The parameter file for the environment is passed to the stack management tool when running it to provision or update the relevant stack instance:

~~~ console
stack up --config=../environments/test.properties
~~~


When environments are split into multiple infrastructure stacks, managing the configuration for all of the stacks in all of the environment can become messy. There are two common ways of arranging parameter files in these cases. One is to put configuration files for all of the environments  with the stack code:


~~~ console
   ├── webserver/
   │   ├── src/
   │   │   ├── cluster.infra
   │   │   └── networking.infra
   │   └──environments/
   │       ├── test.properties
   │       ├── staging.properties
   │       └── production.properties
   └── appserver/
       ├── src/
       │   ├── server.infra
       │   └── networking.infra
       └──environments/
           ├── test.properties
           ├── staging.properties
           └── production.properties
~~~


The other is to centralize the configuration for all of the stacks in one place:


~~~ console
   ├── webserver/
   │   ├── cluster.infra
   │   └── networking.infra
   ├── appserver/
   │   ├── server.infra
   │   └── networking.infra
   └── environments/
       ├── test/
       │   ├── webserver.properties
       │   └── appserver.properties
       ├── staging/
       │   ├── webserver.properties
       │   └── appserver.properties
       └── production/
           ├── webserver.properties
           └── appserver.properties
~~~


Both approaches can become messy and confusing, from different directions. When you need to make a change to all of the things in an environment, making changes to configuration files across dozens of stack projects is painful. When you need to change the configuration for a single stack across the various environments it's in, trawling through a tree full of configuration for dozens of other stacks is also not fun.

## Related Patterns

Other patterns for configuring stack instances include [command line parameters](command-line-parameters.html), [stack instance configuration files](stack-instance-configuration-file.html), [wrapper stacks](wrapper-stack.html), [pipeline-defined parameters](pipeline-defined-parameters.html), and [a stack parameter registry](stack-parameter-registry.html).
