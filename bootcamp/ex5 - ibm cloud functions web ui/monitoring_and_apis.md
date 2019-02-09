### Monitoring

IBM Cloud Functions Web UI comes with a [comprehensive visualisation dashboard](https://console.bluemix.net/openwhisk/dashboard) for monitoring serverless applications.

This dashboard shows activations within a region, org and space. Developers can see activation results, invocation times and logging output through the dashboard. Activations displayed can be filtered by name or time window.

![](images/monitoring.png)

### APIs

HTTP endpoints for web actions can be created and managed through the IBM Cloud Functions Web UI. Using this interface is often a lot more intuitive than using the CLI tool for managing more complex APIs.

1. Select "APIs" from the left-hand menu panel on the homepage.

![API homepage](images/apis-homepage.png)

#### Details Overview

The API details page will show properties for the chosen API, including an API monitoring page showing invocations.

![API homepage](images/api-details.png)

Using the menu on the left-hand side, different properties for the API can be accessed and modified.

- *"Summary"* - API overview page and monitoring dashboard for API invocations.
- *"Definition"* - API configuration properties, allows updating properties live.
- *"Sharing"* - Configure exposing API with our internal users.
- *"API Explorer"* - API documentation for your endpoints.

#### Creating APIs

1. Click the "Create API" from the [APIs homepage](https://console.bluemix.net/openwhisk/apimanagement).

In this page, API details can either be filled out manually or imported from an existing Swagger file.

2. Fill out the "API name" field as "myapi"
3. Fill out the "Base path for API" field as "/api"


4. Click on the "Create Operation" button to add new HTTP endpoints to this API.
5. Fill out the "Path" field as "/hello"
6. Choose an action from the drop-down list.
7. Click the "Create" button.

Using the API management create page, security, rate limiting, CORS or oauth support can be configured.

8. Click the "Save" button to create your API.

![Creating an API](images/create-apis.gif)