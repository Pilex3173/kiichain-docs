# 🧩 Troubleshooting for KiiChain Validator Nodes

This document helps KiiChain validators troubleshoot common node issues, such as sync failures, unstable peer connections, or configuration errors.

---

## ⚙️ Node Unable to Sync

### Symptoms
- Node freezes at a specific block height.
- Log messages such as: ERR failed to fetch block or state sync failed: no suitable peers

- ### Solution
1. Ensure a stable internet connection and an open RPC port (default `26657`).
2. Delete old node data:
```bash
kiichaind unsafe-reset-all

3.Add persistent peers to the config/config.toml file: persistent_peers = "peer1@1.2.3.4:26656,peer2@5.6.7.8:26656"

(Use the peer list from the official documentation or the Discord community.)

Restart the node:

systemctl restart kiichaind


Node Stuck in State Sync

Symptom

Sync stops even though the status is StateSync: true.

Solution

Ensure the trust_height and trust_hash parameters are valid (use a block from the previous hour).

Try temporarily disabling state_sync and allowing full sync:

[statesync]enable = false

Restart the node to force a full sync.

No Peers Connected

Symptoms

Log shows:

No peers connected

The node is not receiving new blocks.


Solution

Ensure the P2P port (26656) is open and not blocked by the firewall.

Check the config/config.toml file:

seeds and persistent_peers are not empty.

addr_book_strict = false for testing mode.

Add public peers manually: kiichaind unsafe-reset-peers

How to View Node Logs

To view node activity and errors:

journalctl -fu kiichaind -o cat

Or just the last 50 lines:

journalctl -u kiichaind -n 50 --no-pager

⚠️ Common Configuration Errors

ComponentsCommon ErrorsSolutionconfig.tomlRPC or P2P port is wrongMake sure default: 26656 (P2P), 26657 (RPC)app.tomlMinimum gas price is emptySet minimum-gas-prices = "0.025ukii"client.tomlChain ID is not appropriateUse kiichain-testnet-1 or appropriate active networkDataCache is not clearedRun unsafe-reset-all before resync
General Tips

Run the node with systemctl enable kiichaind to automatically start on reboot.

Use monitoring tools like Grafana + Prometheus if the validator is active.

Securely back up priv_validator_key.json.

Always update the binary version from official releases.

💬 Need Help?

If you're still having trouble:

Open a new issue on GitHub: Issues → New issue → Validator Troubleshooting

Or join the official KiiChain Discord community.
Last updated: October 2025
Contributor: @Pilex3173
