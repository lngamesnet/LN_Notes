# Confirm unconfirmed funds

When c-lightning daemon starts, it rescans only some blocks back from current block height. If the daemon has stopped and restarted some time after, and during this time there was a tx to some node's address, what could happen is that daemon doesn't detect the confirmation for this tx. When the funds are listed, this tx is showed as unconfirmed, when it is really confimed on the blockchain.

## example:

-When we list funds, an old tx that should be confirmed is showed as unconfirmed:

```
lightning-cli listfunds

....
   "outputs": [
      {
         "txid": "5cd32ef767c434a941548c172d91ec2537bcddddbf28c957c094b5d96fa80ae0",
         "output": 1,
         "value": 843766,
         "amount_msat": "843766000msat",
         "address": "bc1qg4vlh2zffazruuzwy6jg6htp24v5wzcuc5u0jn",
         "status": "unconfirmed"
      }
   ],
....
....




