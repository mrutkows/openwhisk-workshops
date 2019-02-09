### API Gateway

OpenWhisk web actions can benefit from being exposed using the API gateway.

The API Gateway acts as a proxy to [Web Actions](https://github.com/apache/incubator-openwhisk/blob/master/docs/webactions.md) and provides them with additional features including HTTP method routing , client id/secrets, rate limiting and CORS. For more information on API Gateway feature you can read the [api management documentation](https://github.com/apache/incubator-openwhisk-apigateway/blob/master/doc/v2/management_interface_v2.md)

#### example

Let's look a short example of using the API Gateway service…

1. Ensure the `hello` action is enabled as a web action.

```
$ ibmcloud wsk action update hello --web true
ok: updated action hello
```

2. Create a new API Gateway endpoint for the web action.

```
$ ibmcloud wsk api create /api/hello get hello --response-type json --apiname "hello-world"
ok: created API /api/hello GET for action /_/hello
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello
```

3. Check HTTP API returns JSON response.

```
$ curl "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello?name=Bernie"
{
  "payload": "Hello, Bernie from Vermont"
}
```

4. Check other HTTP methods are not supported.

```
$ curl -XPOST "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello?name=Bernie"
{"status":404,"message":"Error: Whoops. Verb not supported."}
```

5. Enable other endpoints and verbs.

```
$ ibmcloud wsk api create /api/hello/world get hello --response-type json --apiname "hello-world"
ok: created API /api/hello/world GET for action /_/hello
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello/world
$ curl "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello/world?name=Bernie"
{
  "payload": "Hello, Bernie from Vermont"
}
```

```
$ ibmcloud wsk api create /api/hello post hello --response-type json --apiname "hello-world"
ok: created API /api/hello POST for action /_/hello
https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello
$ curl -XPOST "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello?name=Bernie"
{
  "payload": "Hello, Bernie from Vermont"
}
```

#### saving & restoring

1. List all the exposed API endpoints.

```
$ ibmcloud wsk api list
```

2. Export the API definitions to a Swagger file.

```
$ ibmcloud wsk api get / > swagger.json
```

3. Delete all the API definitions.

```
$ ibmcloud wsk api delete /
ok: deleted API /
```

4. Check there are no more APIs defined.

```
$ ibmcloud wsk api list
ok: APIs
Action                            Verb             API Name  URL
```

5. Restore from Swagger file.

```
$ ibmcloud wsk api create --config-file swagger.json
```

6. Confirm the APIs have been re-created.

```
$ ibmcloud wsk api list
```

### API Management Features

IBM Cloud Functions supports using the IBM API Management service to support more advanced features including authentication, rate limiting, CORS and more. These features are available when using the IBM Cloud Functions Web UI to create and modify the API endpoints.

Let's look at setting up some of these features for the API endpoints we have already defined…

1. Open the [IBM Cloud Functions](https://console.bluemix.net/openwhisk/) homepage.
2. Navigate to the [APIs](https://console.bluemix.net/openwhisk/apimanagement) page.
3. Click the "hello world" API in the table.
4. Select the "Definition" link from the API details menu.

![API Management Web UI](images/apis.gif)

#### Authentication

Let's turn on custom authentication for our APIs to ensure only authorised users use them.

1. Under the "*Security and Rate Limiting*" section, toggle the switch to enable authentication.
2. Check "*Method*" is "*API key only*" and "*Location*" is "*Header*".
3. Set the "*Parameter name of API key*" to "*X-Auth-Key*"
4. Click the "*Save*" button to update the API definition.

![Authentication](images/auth-on.png)

Once we have enabled API authentication, we need to create the API keys on the "*Sharing*" panel.

1. Under the "*Sharing Outside of Cloud Foundry organization*" section, select "*Create API key*"

![Authentication](images/api-keys.png)

2. Set the key name as "*sample-key*" and make a note of the API key value.
3. Click the "*Create*" button.

If the key has been created correctly the table should now display the newly enabled key.

Let's try out calling one of our API endpoints without an API key.

```
$ ibmcloud wsk api list
ok: APIs
Action                                Verb     API Name  URL
/user@host.com_dev/hello     get  hello-world  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello
/user@host.com_dev/hello    post  hello-world  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello
/user@host.com_dev/hello     get  hello-world  https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello/world
$ curl https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello
{"status":401,"message":"Error: Unauthorized"}
```

If we now call the same API endpoint with the authentication header, it should succeed.

```
$ curl -H X-Auth-Key:<INSERT_YOUR_KEY> "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello?name=Bernie"
{
  "payload": "Hello, Bernie from Vermont"
}
```

*Hurrah it works!*

IBM API Management also provides authentication support including keys with secrets and OAuth integration.

#### Rate Limiting

Let's enable rate limiting on our APIs to ensure we don't have to pay too much! Rate limiting is only supported when authentication is enabled. Limits are on a per-key basis.

1. Under the "*Security and Rate Limiting*" section, toggle the "*Rate Limiting*" switch to on.
2. Set "*Maximum Calls*" to 1 and "*Unit of time*" to "*Minutes*"
3. Click the "*Save*" button to update the API definition.

![Authentication](images/rate-limit.png)

Let's check rate limiting is working.

1. Call the authenticated endpoint twice in succession.

```
$ curl -H X-Auth-Key:<INSERT_YOUR_KEY> "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello?name=Bernie"
{
  "payload": "Hello, Bernie from Vermont"
}
$ curl -H X-Auth-Key:<INSERT_YOUR_KEY> "https://service.us.apiconnect.ibmcloud.com/gws/apigateway/api/<UUID>/api/hello?name=Bernie"
{
  "status":429,
  "message":"Error: Rate limit exceeded"
}
```

On the first call, the response is returned as normal. On the second call, the rate limiting error should be returned. If you wait sixty seconds and try again, the request will be processed as normal.

🎉🎉🎉 **The API Gateway service add services like request routing, based on method and paths, rate limiting and pluggable authentication. It is perfect for high-traffic public APIs.** 🎉🎉🎉
