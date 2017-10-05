# State sync protocol

Sync protocol is simple

## **Subscribe**

Client can subscribe to sync area with server. Only area name is required. Server respond with init event. Init event provides all information required to start using area by ui components.

## **Unsubscribe**

Client can unsubscribe from sync at any time. If you need to pause/resume just unsubscribe and subscribe again.

## **Client patch**

next

