# discord api documentation
reverse-engineered discord api reference for developers
---

## gateway (websocket)

the **gateway** is discord's real-time communication system. it's a websocket connection used for receiving events, maintaining presence, and handling voice updates.

### gateway url

connect to:
```
wss://gateway.discord.gg/?v=10&encoding=json
```

- `v=10` specifies api version 10
- `encoding=json` tells the gateway you want json-formatted messages

---

### connection flow

1. *connect* to the gateway url
2. *receive* the `op 10` hello event
3. *start* sending heartbeats at the interval provided in hello
4. *send* an `identify` payload with your auth token
5. *receive* `dispatch` events (`READY`, `MESSAGE_CREATE`, etc.)

### message structure

websocket messages follow this format:

```json
{
  "op": 0,
  "d": {},
  "s": 42,
  "t": "EVENT_NAME"
}
```

where:
- `op` (int): opcode that defines the message type
- `d` (object): event data payload
- `s` (int|null): sequence number for resuming sessions
- `t` (string|null): event name for dispatch events

### important opcodes

| opcode | name | direction | description |
|--------|------|-----------|-------------|
| 0 | dispatch | server → client | event notification (message created, etc.) |
| 1 | heartbeat | client → server | ping to keep connection alive |
| 2 | identify | client → server | authentication with token |
| 7 | reconnect | server → client | server asks you to reconnect |
| 9 | invalid session | server → client | session invalidated, need to re-identify |
| 10 | hello | server → client | sent immediately on connecting |
| 11 | heartbeat ack | server → client | confirms your heartbeat was received |

### coming soon
- rest api endpoints
- auth and token handling
- ratelimit guidelines
- voice connections

---

*this documentation is released under the mit license.*
