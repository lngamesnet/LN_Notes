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
```
-If we check the tx (5cd32ef767c434a941548c172d91ec2537bcddddbf28c957c094b5d96fa80ae0) in the [blcok explorer](https://blockstream.info/tx/5cd32ef767c434a941548c172d91ec2537bcddddbf28c957c094b5d96fa80ae0) that it was confirmed, in the block 582840.

-We check wich is the current block height:
```
lightning-cli getinfo
{
.....
   "blockheight": 592582,
...
}
```
-The tx was confirmed 592582 - 582840 = 9742 blocks back. So the daemon needs to rescan at least 9742 blocks back to confirm the unconfirmed tx. 

-Edit lightning config file (.lightning/config). 'rescan' options indicates to the daemon the number of blocks to rescan when it starts. In this example, we added:
```
rescan=9800
```
-Restart the daemon and wait until the daemon is synced again (NOTE: if the number of blocks to rescan is very high, the s ync process can be very long!!).

```
lightning-cli getinfo
{
...
   "blockheight": 588642,
...
   "warning_lightningd_sync": "Still loading latest blocks from bitcoind."
..
}
```
-Once the node's blockheight is higher than tx block height, the tx should be showed as confirmed by c-lightning daemon:

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
         "status": "confirmed"
      }
   ],

....
```
-When the node is synced again, edit and comment (or delete) the 'rescan' option in config file, and restart the daemon.



