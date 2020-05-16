---
title: Slack Actions
html_title: Creating Slack Actions for Signals Alerting
slug: elasticsearch-alerting-actions-slack
category: actions
order: 400
layout: docs
edition: community
description:
---

<!--- Copyright 2020 floragunn GmbH -->

# Slack Action
{: .no_toc}

{% include toc.md %}


Use Slack actions to send notifications via Slack. You can use Mustache templates to define dynamic content for the Slack message.

## Prerequisites

In order to use Slack actions, you need to configure a Slack webhook using the accounts registry of Signals. See the [accounts registry documentation](accounts.md) for more on that.

## Basic Functionality

A basic Slack action looks like this:

<!-- {% raw %} -->
```json
 {
	"actions": [
		{
			"type": "slack",
			"name": "my_slack_action",
			"throttle_period": "1h",
			"account": "internal_slack",
			"text": ":warning:\n**Bad destination weather** for {{data.bad_weather_flights.hits.total.value}} flights over last {{data.constants.window}}"
		}
	]
}
```
<!-- {% endraw %} -->

The basic configuration attributes are:

**name:** A name identifying this action. Required.

**throttle_period:** The throttle period. Optional. Specify the time duration using an *amount*, followed by its *unit*. Supported units are m (minutes), h (hours), d (days), w (weeks). For example, `1h` means one hour.

**checks:** Further checks which can gather or transform data and decide whether to execute the actual action. Optional.

**account:** Identifies the Slack application which shall be used for sending the message. See the [accounts registry documentation](accounts.md). Required.

**text:** Defines the content of the message. Mustache templates can be used to render attributes from the watch runtime data. Optional. See the [Slack documentation](https://api.slack.com/messaging/composing/formatting) for details on how to format the message.

**blocks:** Defines the content of the message in the [Slack Blocks format](https://api.slack.com/block-kit/building). Mustache templates can be used to render attributes from the watch runtime data. Optional. See the [Slack documentation](https://api.slack.com/messaging/composing/formatting) for details on how to format the message.

It is required to have at least a message `text` when there is no `blocks` defined otherwise. Please note it is not mandatory to provide a `text` in addition to `blocks` but it is recommended by Slack in order to have a safe fallback.

## Slack Blocks

Slack Blocks allow you to add complex data to the message payload. Slack's [Block Kit Builder](https://api.slack.com/tools/block-kit-builder) provides an intuitive web UI to design Blocks.

```json
 {
	"actions": [
		{
			"type": "slack",
			"name": "my_slack_action",
			"throttle_period": "1h",
			"account": "internal_slack",
            "text": ":warning:\n**Bad destination weather** for {{data.bad_weather_flights.hits.total.value}} flights over last {{data.constants.window}}",
			"blocks": [
              {
  			      "type": "section",
  			      "text": {
  				    "type": "mrkdwn",
  				    "text": "Hey there 👋 A quick warning **Bad destination weather** for {{data.bad_weather_flights.hits.total.value}} flights over last {{data.constants.window}}"
                  }
  		      }
            ]
		}
	]
}
```