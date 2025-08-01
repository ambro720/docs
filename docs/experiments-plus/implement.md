---
title: Implement an experiment
sidebar_label: Implement
slug: /experiments-plus/implement
keywords:
  - owner:vm
last_update:
  date: 2024-10-31
---

To deploy an experiment, you'll need to:

1. Pull the experiment configurations in your application
2. Log the events you'll want in your experiment results
3. Test your experiment in development or a lower environment
4. Click "Start"!

Every experiment needs to expose users into more than one bucket (#1) and log metrics on their behavior after exposure (#2, called "log events"). Statsig automates many of the annoying parts of setting up an experiment, like writing the code you can use to assign buckets, and conducting analysis on the exposures and log events. The experimenter's job is to devise the experiments - and use our SDKs to accomplish #1 and #2.

## Pulling experiment configurations from Statsig

In the code snippets below, we illustrate experimenting on a product demo flow, where you might experiment to improve conversion through the funnel to demo completion. For full implementation details, check out the [SDK documentation](/sdks/getting-started) for the language you'll be using, or walkthrough our example guide for [your first a/b test](/guides/abn-tests).

```js
const user = { userID: loggedInUserID };
const demoConfiguration = statsig.getExperiment(user, "demo_experience");

// use parameters to control the experience
if (demoConfiguration.get("show_banner", false) {
  showBanner();
}

const title = demoConfiguration.get("title", "Start Demo");
banner.setTitle(title);
```

You can also look at a code snippet for your particular experiment by clicking into the code snippet button on the experiment page and selecting the right SDK
![experiment code snippet button](https://graphite-user-uploaded-assets-prod.s3.amazonaws.com/CbjKvuo40oMU45psWLvG/c0f22d41-46e9-4be8-87b0-99bcf1f9a5c5.png)

## Logging events for your scorecard

In order to get experiment results for the events and metrics you care about, you should instrument the experience with the proper event logging (or set up an event integration/data warehouse import to send events to Statsig experimentation stats engine). If you'd like to use our SDKs, your code might look like this:

```

statsig.logEvent(user, "demo_started");
...
statsig.logEvent(user, "demo_completed");
```

Just a few simple events can help you measure how people are moving through a certain funnel in your product, and enable you to experiment on those flows to increase conversion.

## Testing in a lower environment

Once experiments are launched, you can't edit the groups without restarting the experiment, as users are already being allocated to each group. We therefore recommend testing each experiment in lower environments before starting. You can do this by clicking the "Test" button in the experiment setup page, then selecting "Enable for Environments". These environments should match your [SDK environment setup](/guides/using-environments/#configuring-environments). Testing in a lower environment and [overrides](/experiments-plus/overrides) can help you manually set your experiment "group" to properly test each variant.

![image](/img/experiment_test_button.png)

## Starting your experiment

Once your experiment has metrics, parameters, and a hypothesis - and you've tested it in a lower environment, you're ready to launch! Click the "Start" button and your experiment will be immediately live in Production.
