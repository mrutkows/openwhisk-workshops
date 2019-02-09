# creating and invoking actions

This exercise will introduce the concepts needed to create and use actions with IBM Cloud Functions.

*Once you have completed this exercise, you will have…*

- **Created and invoked actions.**
- **Understood how to pass parameters to actions.**
- **Created actions which return asynchronous results.**

Once this exercise is finished, we will be able to create simple serverless functions using IBM Cloud Functions!

## Table Of Contents

* [Background](#background)
* [Creating And Invoking Actions](create_invoke.md#creating-and-invoking-actions)
  * [Creating Node.js actions](create_invoke.md#creating-node.js-actions)
  * [Invoking actions](create_invoke.md#invoking-actions)
* [Using Action Parameters](parameters.md#using-action-parameters)
  * [Passing parameters to an action (Node.js)](parameters.md#passing-parameters-to-an-action-(node.js))
  * [Setting default parameters](parameters.md#setting-default-parameters)
* [Retrieving Action Logs](logs.md#retrieving-action-logs)
  * [Creating Activation Logs](logs.md#creating-activation-logs)
  * [Accessing Activation Logs](logs.md#accessing-activation-logs)
  * [Polling Activation Logs](logs.md#polling-activation-logs)
* [Calling Other Actions](openwhisk.md#calling-other-actions)
  * [Proxy Example](openwhisk.md#proxy-example)
* [Asynchronous Actions](async.md#asynchronous-actions)
  * [Returning asynchronous results (Node.js)](async.md#returning-asynchronous-results-(node.js))
  * [Using actions to call an external API](async.md#using-actions-to-call-an-external-api)


### Background

Actions are stateless code snippets that run on the OpenWhisk platform. An action can be written as a JavaScript, Swift, PHP, or Python function, a Java method, static binary or a custom executable packaged in a Docker container. For example, an action can be used to detect the faces in an image, respond to a database change, aggregate a set of API calls, or post a Tweet.

Actions can be explicitly invoked, or run in response to an event. In either case, each run of an action results in an activation record that is identified by a unique activation ID. The input to an action and the result of an action are a dictionary of key-value pairs, where the key is a string and the value a valid JSON value. Actions can also be composed of calls to other actions or a defined sequence of actions.
