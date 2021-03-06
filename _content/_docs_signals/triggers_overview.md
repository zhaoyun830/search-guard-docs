---
title: Triggers Overview
html_title: Triggers Overview
slug: elasticsearch-alerting-triggers-overview
category: triggers
order: 0
layout: docs
edition: community
description: A trigger specfies when and how often a watch for Elasticsearch Alerting i executed.
---

<!--- Copyright 2020 floragunn GmbH -->

# Triggers overview
{: .no_toc}

{% include toc.md %}

## What is a trigger

Every watch has to define a trigger. A trigger specifies when a watch gets executed ("triggered"). Currently the following trigger types are supported:

* Date and time
  * for example, every Wednesday at 2pm 
* Interval
  * for example, every 10 minutes 
* cron
  * gives you the full power of cron expressions

Example:

```json
{
	"trigger": {
		"schedule": {
			"weekly": {
				"on": "thursday",
				"at": "14:40:45"
			}
		}
	},
	"checks": [ ... ],
	"actions": [ ... ]
}
```


## Trigger execution

Each trigger gets registered with the Trigger Execution Engine. The execution engine makes sure that

* Each trigger is executed on exactly one node at a time
  * You can specify node filters to define on which nodes Signals Alerting should run
* Triggers created in different tenants will not interfere whith each other
  * This applies only when you are using [Multi Tenancy](elasticsearch-alerting-security-multi-tenancy).   
   
## Time zones

Signals supports different [time zones](triggers_timezones.md). If no time zone is specified, the default JVM time zone is used. 
