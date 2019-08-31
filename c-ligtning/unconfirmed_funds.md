# Confirm unconfirmed funds

When c-lightning daemon starts, it rescans only some blocks back from current block height. If the daemon has stopped and restarted some time after, and during this time there was a tx to some node's address, what could happen is that daemon doesn't detect the confirmation for this tx. When the funds are listed, this tx is showed as unconfirmed, when it is really confimed on the blockchain.

##example:





