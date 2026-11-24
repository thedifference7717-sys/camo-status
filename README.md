# CAMO network status (Seal Watch feed)

Live status of the CAMO network from the Seals, CAMO's watch team. Each seed node runs Seal Watch
(`camo_health.py` in [the CAMO repository](https://github.com/thedifference7717-sys/camo/tree/main/contrib/camo/tools))
and publishes its view here as one small JSON file. Seed nodes can write to this repository only.

People: see **https://camocoin.org/status.html**.

## Files

| File | Seed |
| --- | --- |
| `seed1.json` | seed 1 |
| `seed2.json` | seed 2 |

Raw URLs to poll (every few minutes):

```
https://raw.githubusercontent.com/thedifference7717-sys/camo-status/main/seed1.json
https://raw.githubusercontent.com/thedifference7717-sys/camo-status/main/seed2.json
```

## Format

```json
{
 "feed": "camo-seal-watch/1",
 "network": "mainnet",
 "seed": "seed1",
 "status": "OK",
 "updated": 1797400000,
 "updated_utc": "2026-12-15T18:00:00Z",
 "until": null,
 "reasons": [],
 "height": 12345,
 "advice": "normal"
}
```

| `status` | Meaning | Suggested action |
| --- | --- | --- |
| `OK` | Nothing unusual | Normal confirmations |
| `CAUTION` | A reorganization, a burst of blocks, a big hashrate swing, odd timestamps, or a transaction that left the chain in a reorg | Require extra confirmations (30 or more). Clears 30 minutes after the last sign |
| `HOLD` | A possible double-spend, a chain split, or a deep reorganization | Do not credit CAMO deposits until it clears. Lasts at least 2 hours, or until the team has checked the network |

`until` is when the current CAUTION or HOLD ends unless something new happens (unix time). `reasons` holds the Seal
Watch findings in plain words; a possible double-spend names both transaction IDs, so a service can check whether it
credited the replaced one.

A file is pushed when its status changes and at least every 30 minutes otherwise. **If `updated` is more than
45 minutes old, treat that seed as not reporting** (require extra confirmations).

Suggested rule: if either seed says `HOLD`, pause deposits; if either says `CAUTION` or neither is fresh, require
extra confirmations. The feed is an early warning, not a replacement for running your own CAMO node.

## What it is not

The feed cannot pause, change or reverse the CAMO chain; nobody can. It contains only public chain data (heights,
transaction IDs) and never identifies anyone.
