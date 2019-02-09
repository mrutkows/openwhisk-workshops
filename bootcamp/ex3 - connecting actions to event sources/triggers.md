### Creating Triggers

*Triggers* represent a named "channel" for a stream of events.

Let's create a trigger to send *location updates*:

```
$ ibmcloud wsk trigger create locationUpdate
ok: created trigger locationUpdate
```

You can check that the trigger has been created like this:

```
$ ibmcloud wsk trigger list
triggers
locationUpdate                         private
```

So far we have only created a named channel to which events can be fired.

Let's now fire the trigger by specifying its name and parameters:

```
$ ibmcloud wsk trigger fire locationUpdate -p name "Donald" -p place "Washington, D.C"
ok: triggered locationUpdate
```

Triggers also support default parameters. Firing this trigger without any parameters will pass in the default values.

```
$ ibmcloud wsk trigger update locationUpdate -p name "Donald" -p place "Washington, D.C"
ok: updated trigger locationUpdate
$ ibmcloud wsk trigger fire locationUpdate
ok: triggered locationUpdate
```

Events you fire to the `locationUpdate` trigger currently do not do anything. To be useful, we need to create a rule that associates the trigger with an action.

🎉🎉🎉 **That was easy? Let's keep going by connecting actions to triggers…** 🎉🎉🎉
