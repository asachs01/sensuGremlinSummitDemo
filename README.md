## Sensu + Gremlin Summit Demo

This repo contains the configs for laying out the patterns discussed in the "Monitoring Graceful Failure" talk by Lorne Kligerman & Aaron Sachs at Sensu Summit 2019.

### Config

Config listing:

```
|-- assets
|   |-- check-hook-process-filter-asset.yml
|   |-- cpu-check-asset.yml
|   |-- fatigue_check.yml
|   |-- has-contact-asset.yml
|   |-- remediation-asset.yml
|   |-- sensu-aggregate-check.yml
|   |-- sensu-email-handler.yml
|   |-- sensu-influxdb-handler.yml
|   `-- sensu-slack-handler.yml
|-- checks
|   |-- aggregate-check.yml
|   |-- cpu-check-basic.yml
|   |-- cpu-check-best.yml
|   |-- cpu-check-better.yml
|   |-- cpu-check.yml
|   |-- get-top-processes.yml
|   `-- gremlin-kill-attacks.yml
|-- filters
|   |-- check-hook-process-filter.yml
|   |-- fatigue-check-filter.yml
|   |-- has-contact-helpdesk.yml
|   `-- has-contact-ops.yml
|-- handlers
|   |-- email-basic.yml
|   |-- email-better.yml
|   |-- email-helpdesk.yml
|   |-- email-ops.yml
|   |-- email-remediation.yml
|   |-- email-set.yml
|   |-- influxdb.yml
|   |-- remediation.yml
|   `-- slack.yml
|-- hooks
|   |-- get_top_processes.yml
|   `-- gremlin-hook.yml
`-- templates
    |-- basic-email-template
    |-- remediation_template
    `-- standard_email_template
```

## Patterns

### Let Monitoring Know: CPU check + check_hook + email 

To implement this pattern, you'll need the following components:

* `assets/cpu-check-asset.yml`
* `checks/cpu-check-basic.yml`
* `hooks/get_top_processes.yml`
* `email-basic.yml`

The goal here is just implementing a simple check that will check the CPU utilization and fire off an email if we hit a warning/critical state

### Let The Right People Know: CPU check w/ contact labels + check_hook + handler set 

TODO

### Close the Loop: CPU check + aggregate check w/ remediation annotations, contact labels, fatigue annotations + email 

TODO
