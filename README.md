# ICS-721 Token Transfer
Welcome to the ICS-721 Token Transfer Feature Documentation on [HaloTrade](https://inft.halotrade.zone). This document serves as a comprehensive guide for developers and users interested in understanding and implementing the transfer of non-fungible tokens over IBC channels between Aura Network and other chains.

## Supported Chains
ICS-721 Token Transfer on HaloTrade currently supports the following chains:
- [Stargaze](https://stargaze.zone)
  
## Transfer NFTs from Aura Network to Other Chains
Users must prepare their NFTs before transferring to other chains. This includes:
- Creating a new NFT: there are many ways to create a new NFT on Aura Network. For example, users can create a new collection and mint a new NFT, or buy NFTs from other users on [Seekhype Marketplace](https://beta.seekhype.io/),...
- Wrapping NFT (if nessesary): some NFTs on Aura Network are not compatible with other chains. Users must wrap their NFTs before transferring to other chains. For example, some NFTs on Aura Network have metadata uri in `http` protocol, which is not supported by Stargaze. Users must wrap their NFTs and change the metadata uri to `ipfs` protocol before transferring to Stargaze. The source code of the wrapping contract can be found [here](https://github.com/aura-nw/mstr-ics721-wrapper)

After preparing NFTs, users can transfer them to other chains using [ArkProtocol's guidelines](https://github.com/arkprotocol/ark-cw-ics721).
We have deployed contracts both on Aura Network mainnet and testnet to support the transfer of NFTs to other chains:
- [Stargaze](./Stargaze.md)

To transfer a NFT from Aura to other chain, user must send NFT to the `RATE_LIMITER_OUTGOING_PROXY` with attached `ibc_msg`:
```javascript
let ibc_msg = {
    "receiver": RECEIVER_ON_OTHER_CHAIN,
    "channel_id": CHANNEL,
    "timeout": {
        "block": {
            "revision": 1,
            "height": block_height,
        },
        "timestamp": null,
    },
    "memo": null
}

let send_msg = {
    "send_nft": {
        "contract": RATE_LIMITER_OUTGOING_PROXY_AURA,
        "token_id": TOKEN_ID,
        "msg": btoa(JSON.stringify(ibc_msg)),
    }
}
```
  
To get more information about the IBC channels on Aura Network mainnet, please refer to [Aura Network IBC Channels](https://aurascan.io/ibc-relayer)

## Transfer NFTs from Other Chains to Aura Network
### Transfer NFTs from Stargaze
Now, Aura network supports metadata of NFTs in both `http` and `ipfs` protocols. Therefore, users can transfer NFTs from Stargaze to Aura Network without wrapping them.

The information of contracts on Staragze to support the transfer of NFTs to Aura Network in [Stargaze.md](./Stargaze.md)

To transfer a NFT from Stargaze to Aura Network, user must send NFT to the `RATE_LIMITER_OUTGOING_PROXY` with attached `ibc_msg`:
```javascript
let ibc_msg = {
    "receiver": RECEIVER_ON_AURA_NETWORK,
    "channel_id": CHANNEL,
    "timeout": {
        "block": {
            "revision": 1,
            "height": block_height,
        },
        "timestamp": null,
    },
    "memo": null
}

let send_msg = {
    "send_nft": {
        "contract": RATE_LIMITER_OUTGOING_PROXY_AURA,
        "token_id": TOKEN_ID,
        "msg": btoa(JSON.stringify(ibc_msg)),
    }
}
```
