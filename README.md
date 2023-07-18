# Install

https://marketplace.visualstudio.com/items?itemName=donjayamanne.typescript-notebook

### Get started

```jsx
npm install @xmtp/xmtp-js ethers@5.7.0
```

### Import libraries

To start with XMTP, install the XMTP client SDK:

```jsx
const { Wallet } = require("ethers");
const { Client } = require("@xmtp/xmtp-js");
```

### Initialize the wallet

You'll want to replace this with a wallet from your application
// You'll want to replace this with a wallet from your application

```jsx
const wallet = Wallet.createRandom();
console.log("Wallet address: " + wallet.address);
```

### Create a client

A client is created that requires passing in a connected wallet that implements the Signer interface. Use client configuration options to change parameters of a client's network connection.

```jsx
const xmtp = await Client.create(wallet, { env: "dev" });
console.log(xmtp);
console.log("Client created", xmtp.address);
```

### Check if an address is on the network

First you need to check if the address you want to message is on the XMTP network. You can do this by calling client.canMessage with the address you want to message.

```jsx
//Message this XMTP message bot to get an immediate automated reply:
//gm.xmtp.eth (0x937C0d4a6294cdfa575de17382c7076b579DC176) env:production
const WALLET_TO = "0x20B572bE48527a770479744AeC6fE5644F97678B";
const isOnProdNetwork = await xmtp.canMessage(WALLET_TO);
console.log("Can message: " + isOnProdNetwork);
```

### Start a new conversation

You can create a new conversation with any EVM address activated on the XMTP network. For now we are only compatible with EVM wallets

```jsx
const conversation = await xmtp.conversations.newConversation(WALLET_TO);
console.log("Conversation created", conversation);
```

### Send a message

To send a message, the recipient must have already started their client at least once and consequently advertised their key bundle on the network.

```jsx
const message = await conversation.send("gm");
console.log("Message sent", message);
```

### Stream messages in a conversation

You can receive the complete message history in a conversation.

```jsx
for await (const message of await conversation.streamMessages()) {
  console.log(`New message from ${message.senderAddress}: ${message.content}`);
}
```
