# State sync protocol

Sync protocol based on async events.

## <a name="eventInit"></a>Initialization
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

## <a name="requestResponse"></a>Request / Response model
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

## <a name="subscription"></a>Area subscription
All client requests have unique numeric id auto incremented on each client event.

### <a name="subscribeRequest"></a>Subscribe request
Client subscribes area of client state to be synchronized with area on server side. Server respond with [Subscribe response](#subscribeResponse) with all information required to start using area by ui components.

```javascript
{
  id: 1, // unique id
  type: "subscribe", // event type
  area: "myArea", // area name
}
```

### <a name="subscribeResponse"></a>Subscribe response
Server respond to successful client subscription request with subscription response. 

```javascript
{
  forId: 1, // unique id of request
  type: "areaSubscription", // event type
  area: "myArea", // area name,
  config: {
    clientLocalPrefix: "$";
    clientPush: [ "/" ]; // prefixes
    timeout: 6000 //  signals timeout
  },
  model: {
    // initial area model
  }
}
```


### <a name="unsubscribeRequest"></a>**Unsubscribe**
Client can unsubscribe area and stop syncronization at any time. If you need to pause/resume just unsubscribe and subscribe again.

```javascript
{
  id: 231, // unique id
  type: "unsubscribe", // event type
  area: "myArea", // area name
}
```

### **Client patch**
State sync detects client changes and send [json patch](https://tools.ietf.org/html/rfc6902) event to server.

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
### **Client signal**
Client sends 

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

## Server events
Server respond with event on client 




