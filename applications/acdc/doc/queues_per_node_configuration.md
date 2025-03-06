# ACDC Node-Specific Account Configuration

## Overview
ACDC supports running specific accounts on specific nodes using a configuration document in the ACDC database.

## Configuration Document
The configuration document should be created in the ACDC database (`system_config/acdc`) with the ID `nodes_accounts`.

### Document Structure

{
    "id": "nodes_accounts",
    "nodes": {
        "acdc@host1.example.com": {
            "accounts": [
                "account1_id",
                "account2_id"
            ]
        },
        "acdc@host2.example.com": {
            "accounts": [
                "account3_id",
                "account4_id"
            ]
        }
    },
    "default": {
        "accounts": [
            "default_account1_id",
            "default_account2_id"
        ]
    }
}


### Fields
- `nodes`: Object containing node-specific configurations
  - Keys are the full node names (e.g., "acdc@host1.example.com")
  - Values contain an `accounts` array listing account IDs for that node
- `default`: Optional default configuration
  - `accounts`: Array of account IDs to run on nodes without specific configuration

## Behavior
1. When ACDC starts on a node, it checks for node-specific configuration
2. If found, only the specified accounts are initialized on that node
3. If no node-specific config exists, checks for default accounts
4. If no configuration exists at all, falls back to all accounts in accounts_listing

## Example Usage
To configure account "abc123" to run only on node "acdc@host1":


{
    "id": "nodes_accounts",
    "nodes": {
        "acdc@host1.example.com": {
            "accounts": [
                "abc123"
            ]
        }
    }
}


## Maintenance Functions
The existing maintenance functions already show only what's running on the current node:
- `acdc_maintenance:status()` - Shows agents on this node
- `acdc_maintenance:queues_status()` - Shows queues on this node
- `acdc_maintenance:queue_detail(AccountId, QueueId)` - Shows queue details if running on this node