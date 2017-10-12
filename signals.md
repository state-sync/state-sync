#Signals

Lets talk about signals. Change client model and sync it to server is not always best scenario. Sometimes you need transactional change on server side.

That is signal feature designed for.

Signal is an event with parameters sent to server. Signal has name and parameters.

When server receives signal, it checks user permissions, handle signal, sync changes made by signal logic back to client and send confirmation or error.

Signal handler mutate state according it logic and parameters. Signal handler is also can mutate other things like db entities.

After processing server sends model patch and confirmation to the client.

Typical cases to use signals.
- Context switch like edit entity with specified id.
- Data mutation actions like create entity from area data.

Signals used to interacts with things outside of state.

Pay attention that signals never return value. Client only get confirmation on success (in this case all changes in client state already) or fail.

**Why?**

If we allow signals to return value it will create second protocol of data delivery (in addition to server patches).

