# genesys-dashboard-metric

An example how use the Genesys Cloud Platform API to build in custom metrics into a dashboard. In this use case I am using the "Estimated Wait Time" of a queueId with selecting a specfic mediaType. Details about the endpoints can be found in the [API Explorer](https://developer.genesys.cloud/devapps/api-explorer) the endpoint I am using in this example is

```
/api/v2/routing/queues/{queueId}/mediatypes/{mediaType}/estimatedwaittime

```

## Setup

The overall framework that im using is the same as most of my other client side use cases and is built on top of my [user snippets](https://github.com/mcphee11/vscode-user-snippets) So if you know how this works then skip straight to the "Dashboard" section of this README.md

## Create an OAuth

This example uses "Implicit Grant" OAuth2 to your Genesys Cloud ORG environment. The first thing you will need to do is to get a ClientID from your Genesys Cloud ORG with the ability to connect to the API endpoints. This will also need to have a redirect to the Hosting location of the server url for example: "http://localhost:8080/index.html". You will also need add the required "scopes" for the endpoints that you want to use. In this example we are using "routing".

![](/docs/images/oauth.png?raw=true)

Once saved you will need to copy the:

```
clientId
```

as you will need this later for the authentication.

## Get the QueueId

Now you will also need to find out the "QueueId" for which queue you want to check the EWT on... To do this the easiest way is to simply go to Admin > Queues then copy the queueId from the URL bar

![](/docs/images/queueId.png?raw=true)

NOTE: your queueId will be different this this of course...

## Dashboard

Now you can add the component to the Dashboard of your choosing. To do this simply create a "Web Content" widget type. Give it a "Widget Title" based on the data you are going to surface. Then in the URL put the information as URL Params.

```
http://localhost:8080/index.html?gc_region=YOUR_REGION&gc_clientId=YOUR_CLIENTID&gc_redirectUrl=http://localhost:8080/index.html&gc_queueId=YOUR_QUEUEID&gc_mediaType=call&gc_refresh=30000
```

Remember to update the items with your information

```
YOUR_REGION = eg: mypurecloud.com.au
YOUR_CLIENTID = from the copy in OAuth step
YOUR-QUEUEID = from QueueId step
gc_mediaType = call, chat, callback, email, videoComm, message
gc_refresh = the rate at which it refreshes the data in ms
```

If you look in the code I ahve set a ">=" check on the gc_refresh timer in ms so that if its less than 15000 (15sec) it will display "err" and not do the API request... this is to stop high API requests causing a 429 rate limit as well as to many API requests can drive users of the fair useage. You should NOT do GET requests every few ms

Now you can see the "Estimated Wait" Widget looks like one of the default windgets which is giving me "Interactions Count" next to it.

![](/docs/images/dashboard.png)

## NOTE

As time goes on Genesys are releasing more and more metrics to the default dashboard. This design pattern is only for cases where there is NOT a default option of what you need. You can also use this approach for more advanced Analytics Queries from the Analytics API.
