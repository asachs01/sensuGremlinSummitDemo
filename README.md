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

This pattern builds on the last pattern and adds a few things:

* Filtering
  * [Contact routing using labels](https://bonsai.sensu.io/assets/sensu/sensu-go-has-contact-filter)
  * [Hook processes](https://bonsai.sensu.io/assets/asachs01/sensu-go-hook-has-process-filter)(currently hardcoded to `gremlin` for the demo)
* Handler sets
* Templates for email

To implement this pattern, you'll need:

* Uses the following filters/handlers
  * Filters
    * `filters/has-contact-helpdesk.yml`
    * `filters/has-contact-ops.yml`
  * Handlers
    * `handlers/email-set.yml`
    * `handlers/email-helpdesk`
    * `handlers/email-ops`
  * Templates
    * `templates/standard_email_template`

There are some key bits required to make this component work:

* The check must have the necessary labels
* The check must also be performing a check_hook
* The handlers must have `hook_process_gremlin` and `has_contact_{ops,helpdesk}`

You'll notice in the templates that we can wrap our template with some HTML to format things and make it a bit prettier.

### Close the Loop: CPU check + aggregate check w/ remediation annotations, contact labels, fatigue annotations + email 

This pattern takes what we've implemented previously and builds on it by adding:

* Fatigue check filtering
* Automated remediation
* Unscheduled/unpublished checks
