# State sync protocol

Sync protocol based on async events.

# <a name="eventInit"></a>Initialization
Client subscribes to stomp '/root' channel and receive initialization event. Initialization event provides protocol version and access tokens for user and session. These tokens used internally to route events between user sessions.

```javascript
{
  version: "1.0", // unique id
  userToken: "92beb", // user token
  sessionToken: "e0242", // session token
}
```
After receiving init event, client subscribes to additional Stomp channels in order to receive session and user events.

- /session/e0242 server will send session specific events to this channel.
- /account/92beb server will send user specific events to this channel.

# <a name="requestResponse"></a>Request / Response model
All client requests have unique, numeric and auto incremented **id** property. Response for request have **forId** property.

```javascript
// client request
{
  id: "12",
  ...
}
// server response
{
  forId: "12",
  ...
}

```

# <a name="subscription"></a>Area subscription
All client requests have unique numeric id auto incremented on each client event.

## <a name="subscribeRequest"></a>Request
Client subscribes area of client state to be synchronized with area on server side. Server respond with [Subscribe response](#subscribeResponse) with all information required to start using area by ui components.

```javascript
{
  id: 1, // unique id
  type: "subscribe", // event type
  area: "myArea", // area name
}
```

## <a name="subscribeResponse"></a>Response
Server respond to successful client subscription request with subscription response. 

```javascript
{
  forId: 1, // unique id of request
  type: "areaSubscription", // event type
  area: "myArea", // area name,
  config: {
    clientLocalPrefix: "$"; // client do not push changes in properties started with '$'
    clientPush: [ "/" ]; // prefixes for data pushed to server
    timeout: 6000 //  signals timeout
  },
  model: {
    // initial area model
  }
}
```

## <a name="subscribeResponse"></a>Error
Server respond with error if area do not exists of access denied. 

```javascript
{
  forId: 1, // unique id of request
  type: "areaSubscriptionError", // event type
  area: "myArea", // area name,
  error: "accessDenied"
}
```

Error codes
- 'unknownArea' - Area is unknown
- 'accessDenied' - Access is denied for user
- 'alreadySubscribed' - Area is already subscribed in this session

# <a name="areUnsubscribe"></a>Area unsubscribe
Client can unsubscribe area and stop synchronization at any time. If you need to pause/resume just unsubscribe and subscribe again. Please note that there is no error message. Area always can be unsubscribed.

## <a name="areaUnsubscribeRequest"></a>Request
Client send unsubscribe event to server.

```javascript
{
  id: 231, // unique id
  type: "unsubscribe", // event type
  area: "myArea", // area name
}
```
## <a name="areaUnsubscribeResponse"></a>Response
Server respond with confirmation on unsubcribe request.

```javascript
{
  forId: 1, // unique id of request
  type: "areaUnsubscription", // event type
  area: "myArea", // area name,
}
```

# Client patch
State sync detects client changes and send [json patch](https://tools.ietf.org/html/rfc6902) event to server. Server 
respond with server patch event followed by patch response. Two events are required to simplify client sync logic.

## Patch request 

```javascript
{
  id: 4002, // unique id
  type: "patch", // event type
  area: "myArea", // area name
  patch: [
    {op:"replace", path:"/settings/watch"}
  ]
}
```

# Signal
Client sends signals to server with parameters. Server handle signal, sync area by [server patch](#serverPatch) event followed by [signal response](#signalResponse).

```javascript
{
  id: 123, // unique id
  type: "signal", // event type
  signal: "update", // signal name
  parameters: {
    name: "John"
  }
}
```





