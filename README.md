# API Documentation

Reverse-engineered Discord API documentation.

---

## Gateway (WebSocket)

the **gateway** is discord's way of sending data, it is a (`WebSocket`), it is used for sending realtime data, voice status updates, etc.

## Gateway URL (WebSocket)

to init a websocket connection you would connect to the URL

`wss://gateway.discord.gg/?v=10&encoding=json`

- `v=10` is the current major api version (as of the time i document this)
- `encoding=json` indicates that the payload will be in json format

---

### connection flow

1 *connect* to the gateway url
2 *recieve* the `op 10` hello event
3 *start* sending a `heartbeat` at the given interval (ex: 1/6)
4 *send* an `identify` paylod to authenticate
5 *recieve* `dispatch` events (e.g, `READY` `MESSAGE_CREATE`, etc..)

### structure
most websocket messages will follow this format

```json
{
  "op": 0,
  "d": {},
  "s": 42,
  "t": "EVENT_NAME"
}
```
- `op` (int): opcode, defines message type
- `d` (object): event data
- `s` (int|null): sequence number, normally used for resuming sessions
- `t` (string|null): event name (for `dispatches`)


### mostly important opcodes you need to know;
- 0 (dispatch) (event from discord, a msg)
- 1 (heartbeat) (heartbeat ping to keep the websocket connection alive)
- 2 (identify) (sent to login with a user or bot token)
- 7 (reconnect) (server requests client to reconnect to the websocket)
- 9 (invalid session) (re identify or resume required, happens more of the time when your session is invalidated due to an expired token or an older client version)
- 10 (hello) (used to make sure the client can communicate correctly, sent instantly on connection)
- 11 (heartbeat ack) (response to the heartbeat msg)

### other sections coming soon
- REST api (e.g., GET /channels/:id/messages)
- auth and tokens
- ratelimiting
- voice sockets

This documentation is released under the MIT License.
