# TAKT heartbeat

A signed statement, published at least every 7 days, that TAKT (ABX DEVELOPMENT LLP, registered in
Scotland, SO301872) operates its service. TAKT's source escrow (Terms of Service, clause 8,
https://taktcycles.com/legal/terms.html) reads it here.

## How to check a heartbeat

```sh
# TAKT signed it (allowed_signers lists the main heartbeat key and the backup key kept offline)
ssh-keygen -Y verify -f allowed_signers -I heartbeat@taktcycles.com -n takt-heartbeat \
  -s latest.txt.sig < latest.txt

# Optional: it existed no later than a Bitcoin block (OpenTimestamps, https://opentimestamps.org)
ots verify latest.txt.ots
```

For the escrow a heartbeat counts when it lies at heartbeats/<YYYY>/<YYYY-MM-DDTHHMMSSZ>.txt, is
signed by a key in `allowed_signers`, states that same time as "Issued (UTC)", and is not dated in
the future. A heartbeat signed for the future, if ever present here, releases the escrow.
