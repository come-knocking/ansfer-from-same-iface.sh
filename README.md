###
### get the default gateway IP address.
### get the current device's IP address.
### Checks if the routing table entry exists in the iproute2 tables file, and if not, adds it.
### Adds a default route to the specified routing table.
### Adds a routing rule for the current device's IP address.

```bash
#!/usr/bin/env bash

TABLE_ID=100 
TABLE_NAME=real
TABLE_STRING="$TABLE_ID $TABLE_NAME"
IP_ROTE_FILE="/etc/iproute2/rt_tables"

DEFAULT_ROUTE=$(ip route show default | awk '/default/ {print $3}')
MY_IP=$(ip route get $DEFAULT_ROUTE | awk '{print $5}')
if ! grep -qF "$TABLE_STRING" "$IP_ROTE_FILE"; then
    echo "$TABLE_STRING" >> "$IP_ROTE_FILE"
fi
ip route add default via $DEFAULT_ROUTE table $TABLE_NAME
ip rule add from $MY_IP table $TABLE_ID
```