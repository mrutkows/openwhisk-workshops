# using action sequences

This exercise will explain how to use sequences to compose new "meta-actions" from existing actions on IBM Cloud Functions.

*Once you have completed this exercise, you will have…*

- **Understood what action sequences do and how they work.**
- **Created, deployed and invoked a sample action sequence.**
- **Used deliberate errors to stop processing the action sequence.**

Once this exercise is finished, we will be able to use action sequences on IBM Cloud Functions!

## Table Of Contents

* [Background](#background)
* [Action Sequences](sequences.md#action-sequences)
  * [Creating Sequence Actions](sequences.md#creating-sequence-actions)
  * [Handling Errors](sequences.md#handling-errors)

## Background

Developers often want to build actions as reusable components which can be combined to build "higher-order" serverless functions.

For example, what if you have serverless functions to implement an external API and want to enforce HTTP authentication? Rather than manually replicating the same authentication code across all action handlers, you can build an authentication action which can be invoked programmatically at runtime.

Unfortunately, this approach does incur additional charges. The application will be charged twice when the authentication function is called, as the calling action has to sit idle waiting for the response from the authentication function.

*OpenWhisk has a special type of action which resolves this problem...*
