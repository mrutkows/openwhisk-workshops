# Using Rules

Rules are used to associate a trigger with an action. Each time a trigger event is fired, the action is invoked with the event parameters.

## Creating rules

As an example, create a rule that calls the `hello` action whenever a location update is triggered.

1. Check the `hello` action exists and responds to the correct event parameters.

```text
$ ibmcloud wsk action invoke --result hello --param name Bernie --param place Vermont
{
    "payload": "Hello, Bernie from Vermont"
}
```

1. Check the trigger exists.

```text
$ ibmcloud wsk trigger get locationUpdate
ok: got trigger a
{
    "namespace": "user@host.com_dev",
    "name": "locationUpdate",
    "version": "0.0.1",
    "limits": {},
    "publish": false
}
```

1. Create the rule using the command-line. The three parameters are the name of the rule, the trigger, and the action.

```text
$ ibmcloud wsk rule create myRule locationUpdate hello
ok: created rule myRule
```

1. Retrieve rule details to show the trigger and action bound by this rule.

```text
$ ibmcloud wsk rule get myRule
ok: got rule myRule
{
    "namespace": "user@host.com_dev",
    "name": "myRule",
    "version": "0.0.1",
    "status": "active",
    "trigger": {
        "name": "locationUpdate",
        "path": "user@host.com_dev"
    },
    "action": {
        "name": "hello",
        "path": "user@host.com_dev"
    },
    "publish": false
}
```

## Testing rules

1. Fire the `locationUpdate` trigger. Each time that you fire the trigger with an event, the `hello` action is called with the event parameters.

```text
$ ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
ok: triggered /_/locationUpdate with id 5c153c01d76d49dc953c01d76d99dc34
```

1. Verify that the action was invoked by checking the activations list.

```text
$ ibmcloud wsk activation list --limit 2
activations
5ee74025c2384f30a74025c2382f30c1 hello
5c153c01d76d49dc953c01d76d99dc34 locationUpdate
```

We can see the trigger activation \(`5c153c01d76d49dc953c01d76d99dc34`\) is recorded, followed by the `hello` action activation \(`5ee74025c2384f30a74025c2382f30c1`\).

1. Retrieving the trigger activation record will show the actions and rules invoked from this activation.

```text
$ ibmcloud wsk activation result 5ee74025c2384f30a74025c2382f30c1
{
   "payload": "Hello, Donald from Washington, D.C."
}
```

You can see that the hello action received the event payload and returned the expected string.

Activation records for triggers store the rules and actions fired for an event and the event parameters.

```text
$ ibmcloud wsk activation result 5c153c01d76d49dc953c01d76d99dc34
{
    "name": "Donald",
    "place": "Washington, D.C."
}
$ ibmcloud wsk activation logs 5c153c01d76d49dc953c01d76d99dc34
{"statusCode":0,"success":true,"activationId":"5ee74025c2384f30a74025c2382f30c1","rule":"user@host.com_dev/myRule","action":"user@host.com_dev/hello"}
```

You can create multiple rules that associate the same trigger with different actions.

**Can you create another trigger and rule that calls the** `hello` **action? 🤔**

You can also use rules with sequences. For example, one can create an action sequence `recordLocationAndHello`that is activated by the rule `anotherRule`.

```text
$ ibmcloud wsk action create recordLocationAndHello --sequence /whisk.system/utils/echo,hello
$ ibmcloud wsk rule create anotherRule locationUpdate recordLocationAndHello
```

## Disabling rules

Rules are enabled upon creation but can be disabled and re-enabled using the command-line.

1. Disable the rule connecting the `locationUpdate` trigger and `hello` action.

```text
$ ibmcloud wsk rule disable myRule
```

1. Fire the trigger again.

```text
$ ibmcloud wsk trigger fire locationUpdate --param name Donald --param place "Washington, D.C."
ok: triggered /_/locationUpdate with id 53f85c39087d4c15b85c39087dac1571
```

1. Check the activation list there are no new activation records.

```text
$ ibmcloud wsk activation list --limit 2
activations
5ee74025c2384f30a74025c2382f30c1 hello
5c153c01d76d49dc953c01d76d99dc34 locationUpdate
```

The latest activation records were from the previous example.

_Activation records for triggers are only recorded when they are bound to an active rule._

🎉🎉🎉 **Right, now we have a way to connect actions to events in OpenWhisk, how do we connect triggers to event sources like messages queues? Enter trigger feeds…** 🎉🎉🎉

