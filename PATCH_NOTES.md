# xbailz hybrid patch

This package keeps the custom `Xrelly-stack/xbailz` features and applies the 1-to-1 target handling used by the comparison implementation.

## Change

`lib/Socket/messages-send.js` now normalizes message targets before message generation and relay:

- bare phone numbers become `<number>@s.whatsapp.net`
- legacy `<number>@c.us` targets become `<number>@s.whatsapp.net`
- group, LID, newsletter, status, and other valid JIDs are preserved
- invalid or empty targets fail early with a clear JID error

The normalization is applied both in `sendMessage()` and `relayMessage()` so helper methods that call `relayMessage()` receive the same behavior.

## Usage

```js
await sock.sendMessage('628123456789', { text: 'hello' })
await sock.sendMessage('628123456789@c.us', { text: 'hello' })
await sock.sendMessage('628123456789@s.whatsapp.net', { text: 'hello' })
```

This is a runtime JavaScript build patch; no `node_modules` directory is included.
