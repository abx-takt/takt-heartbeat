# TAKT heartbeat

A signed, time-anchored statement, published at least every 7 days, that TAKT (ABX DEVELOPMENT LLP,
registered in Scotland, SO301872) operates its service. The escrow terms in TAKT's Terms of Service
(clause 8, https://taktcycles.com/legal/terms.html) treat the absence of a valid heartbeat for 90 days
both here and at https://taktcycles.com/.well-known/takt-heartbeat.txt as TAKT having ceased to
provide the service.

## How to check a heartbeat

```sh
# 1. TAKT signed it (the key line is in allowed_signers here and on taktcycles.com/trust/)
ssh-keygen -Y verify -f allowed_signers -I heartbeat@taktcycles.com -n takt-heartbeat \
  -s latest.txt.sig < latest.txt

# 2. It existed no later than a Bitcoin block (OpenTimestamps, https://opentimestamps.org)
ots verify latest.txt.ots

# 3. It was written no earlier than the drand round it names (https://drand.love, quicknet):
#    fetch that round and compare the randomness
curl -s https://api.drand.sh/52db9ba70e0cc0f6eaf7803dd07447a1f5477735fd3f661792ba94600c84e971/public/ROUND
```

For TAKT escrow vaults a heartbeat counts when it is signed by a key in `allowed_signers` (the
main heartbeat key or the backup key, which is kept offline) and its "Issued (UTC)" time is not in
the future; vaults read it here, on GitHub, only. The drand round and the OpenTimestamps proof are
additional evidence of its time. Every heartbeat is kept in `heartbeats/`.
