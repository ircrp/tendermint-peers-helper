# Sort and filter addrs array by last_success field

```
jq '(now - 3 * 60 * 60) as $earliest_date
  | .addrs                                            # Select addrs array
  | sort_by(.last_success)                            # Sort array by last_success field
  | reverse                                            # Reverse the sort order
  | .[]                                               # Select all elements in the array
  | select(.last_success != "0001-01-01T00:00:00Z")   # Filter out records with "0001-01-01T00:00:00Z" last_success field
  | select(.last_success | strptime("%Y-%m-%dT%H:%M:%S.%Z") | mktime > $earliest_date)   # Filter out records with last_success older than 3 hours
  | .addr.id + "@" + .addr.ip + ":" + ( .addr.port | tostring )' addrbook.json   # Construct output string as src_id@addr_ip:addr_port
```
