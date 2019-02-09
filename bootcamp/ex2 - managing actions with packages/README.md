# managing actions with packages

This exercise will introduce the concepts needed to create and use packages with IBM Cloud Functions.

*Once you have completed this exercise, you will have…*

- **Learnt how to find public packages.**
- **Understood how to use public package actions and bindings.**
- **Created and shared custom packages.**

Once this exercise is finished, we will be able to create and share actions using packages using IBM Cloud Functions!

## Table Of Contents

* [Background](#background)
* [Browsing packages](existing.md#browsing-packages)
* [Invoking actions in a package](existing.md#invoking-actions-in-a-package)
* [Creating and using package bindings](existing.md#creating-and-using-package-bindings)
* [Creating new packages](custom.md#creating-new-packages)
* [Sharing packages](custom.md#sharing-packages)

### Background

In IBM Cloud Functions, you can use *packages* to bundle together related actions and even share them with others. Packages can only contain *actions*. *Triggers* and *rules* are not supported at the moment. Package nesting is not allowed, i.e. packages cannot contain other packages.

Packages also support default parameters. Package parameters are automatically passed into actions during invocations. This provides a convenient method to manage service credentials needed with multiple actions.
