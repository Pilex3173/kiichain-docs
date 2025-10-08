# ðŸ§© Troubleshooting for KiiChain Validator Nodes

This document helps **KiiChain validators** troubleshoot common node issues â€” such as sync failures, unstable peer connections, or configuration errors.

---

## âš™ï¸ Node Unable to Sync

### ðŸ§  Symptoms
- Node freezes at a specific block height.  
- Log messages such as:
  ```
  ERR failed to fetch block
  state sync failed: no suitable peers
  ```

### ðŸ› ï¸ Solution
1. Ensure a stable internet connection and that the RPC port (default `26657`) is open.  
2. Delete old node data:
   ```bash
   kiichaind unsafe-reset-all
   ```
3. Add persistent peers in `config/config.toml`:
   ```bash
   persistent_peers = "peer1@1.2.3.4:26656,peer2@5.6.7.8:26656"
   ```
   > ðŸ’¡ Use the peer list from the official documentation or Discord community.
4. Restart the node:
   ```bash
   systemctl restart kiichaind
   ```

---

## ðŸ” Node Stuck in State Sync

### ðŸ§  Symptom
Sync stops even though the status is `StateSync: true`.

### ðŸ› ï¸ Solution
1. Ensure the `trust_height` and `trust_hash` parameters are valid (use a block from the previous hour).  
2. Temporarily disable state sync to allow a full sync:
   ```bash
   [statesync]
   enable = false
   ```
3. Restart the node to force full synchronization.

---

## ðŸŒ No Peers Connected

### ðŸ§  Symptoms
Logs show:
```
No peers connected
```
The node is not receiving new blocks.

### ðŸ› ï¸ Solution
1. Ensure the P2P port (`26656`) is open and not blocked by a firewall.  
2. Check `config/config.toml`:
   - `seeds` and `persistent_peers` are not empty.  
   - `addr_book_strict = false` (recommended for testing mode).  
3. Add public peers manually:
   ```bash
   kiichaind unsafe-reset-peers
   ```

---

## ðŸ“œ How to View Node Logs

To view node activity and errors in real time:
```bash
journalctl -fu kiichaind -o cat
```

To view only the last 50 lines:
```bash
journalctl -u kiichaind -n 50 --no-pager
```

---

## âš ï¸ Common Configuration Errors

| Component       | Common Error                        | Solution |
|-----------------|--------------------------------------|-----------|
| `config.toml`   | RPC or P2P port is wrong             | Use defaults: 26656 (P2P), 26657 (RPC) |
| `app.toml`      | Minimum gas price is empty           | Set `minimum-gas-prices = "0.025ukii"` |
| `client.toml`   | Wrong chain ID                       | Use `kiichain-testnet-1` or the current active network |
| Data cache      | Not cleared before resync            | Run `kiichaind unsafe-reset-all` before resync |

---

## ðŸ’¡ General Tips

- Run the node with:
  ```bash
  systemctl enable kiichaind
  ```
  to automatically start it on system reboot.

- Use **Grafana + Prometheus** for monitoring validator performance.  
- Securely back up your `priv_validator_key.json`.  
- Always update your binary from the official releases.

---

## ðŸ’¬ Need Help?

If you're still having trouble:

- **Open a new issue on GitHub:**  
  `Issues â†’ New issue â†’ Validator Troubleshooting`

- **Join the official KiiChain Discord** for community support.

---

_Last updated: October 2025_  
**Contributor:** @Pilex3173
