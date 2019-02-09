

### Creating And Invoking Actions

#### Creating Node.js actions

Review the following steps and examples to create your first JavaScript action.

1. Create a JavaScript file with the following content. For this example, the file name is 'hello.js'.

```javascript
function main() {
    return {payload: 'Hello world'};
}
```

The JavaScript file might contain additional functions. However, by convention, a function called `main` must exist to provide the entry point for the action.

2. Create an action from the following JavaScript function. For this example, the action is called 'hello'.

```
$ ibmcloud wsk action create hello hello.js
ok: created action hello
```

3. List the actions that you have created:

```
$ ibmcloud wsk action list
actions
hello       private
```

You can see the `hello` action you just created.


#### Invoking Actions

**After you create your action, you can run it on IBM Cloud Functions with the 'invoke' command.** 

You can invoke actions with a *blocking* invocation (i.e., request/response style) or a *non-blocking* invocation by specifying a flag (`—blocking`) on the command-line. A blocking invocation request will *wait* for the activation result to be available. The wait period is the lesser of 60 seconds or the action's configured [time limit](https://github.com/apache/incubator-openwhisk/blob/master/docs/reference.md#per-action-timeout-ms-default-60s). The result of the activation is returned if it is available within the wait period. Otherwise, the activation continues processing in the system and an activation ID is returned so that one may check for the result later, as with non-blocking requests (see [here](https://github.com/apache/incubator-openwhisk/blob/master/docs/actions.md#watching-action-output) for tips on monitoring activations).

1. Invoke the `hello` action using the command-line as a blocking activation.

```
$ ibmcloud wsk action invoke --blocking hello
```

```
ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
```

```
{
    "result": {
        "payload": "Hello world"
    },
    "status": "success",
    "success": true
}
```

The command outputs two important pieces of information:

- The activation ID (`44794bd6aab74415b4e42a308d880e5b`)
- The invocation result if it is available within the expected wait period

The result in this case is the string `Hello world` returned by the JavaScript function. The activation ID can be used to retrieve the logs or result of the invocation at a future time.

If you don't need the action result right away, you can omit the `—blocking` flag to make a non-blocking invocation. You can get the result later by using the activation ID.

2. Invoke the `hello` action using the command-line as a non-blocking activation.

```
$ ibmcloud wsk action invoke hello
ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
```

3. Retrieve the activation result

```
$ ibmcloud wsk activation result 6bf1f670ee614a7eb5af3c9fde813043
{
    "payload": "Hello world"
}
```

To access the most recent activation record, activation results or activation logs, use the `--last` or `-l` flag. 

4. Run the following command to get your last activation result.

```
$ ibmcloud wsk activation result --last
{
    "payload": "Hello world"
}
```

Note that you should not use an activation ID with the flag `--last`.

5. If you forget to record the activation ID, you can get a list of activations ordered from the most recent to the oldest. Run the following command to get a list of your activations:

```
$ ibmcloud wsk activation list
activations
44794bd6aab74415b4e42a308d880e5b         hello
6bf1f670ee614a7eb5af3c9fde813043         hello
```

🎉🎉🎉 **Great work, you have now learned how to create, deploy and invoke your own serverless functions on IBM Cloud Functions. What about passing data into actions? Let's find out more…** 🎉🎉🎉
