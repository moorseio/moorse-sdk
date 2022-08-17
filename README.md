# Moorse's software development kit

## Quickstart

Create a node js project, and install the package.

```bash
$ npm i moorse
```

At typescript use import to get the moorse class with all the methods, the same can be done at es6.

```typescript
import { Moorse } from "moorse";

async function useMoorse(pack) {
    const moorse: Moorse = new Moorse({
        channelType: pack.channel,
        integrationId: pack.integrationId
    });
    await moorse.login(pack.email, pack.password);
    await moorse.sendText({
        to: pack.waNumber,
        body: "wow, it's a moorse message!"
    });
}
```

Here, pack is an object like: 

```typescript
const pack: any = {
    integrationId: "some-moorse-integration-id",
    channel: "whatsapp",// <-- Unique communication channel for now
    email: "sample.email@gmail.com",
    password: "strongpass123",
    waNumber: "5583996843637"
};
```