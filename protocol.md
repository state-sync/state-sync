# State sync protocol

Sync protocol based on events fying between client and server.

## <a name="clientRequests"></a>Client requests
All client requests have unique numeric id autoincremented on each client event.

### <a name="subscribeRequest"></a>Subscribe request
Client can subscribe part of client state to be synchronized with  area on server side. Server respond with [Subcribe response](#subscrbeResponse) with all information required to start using area by ui components.

```javascript
{
  id: 1, // unique id
  type: "subscribe", // event type
  area: "myArea", // area name
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




