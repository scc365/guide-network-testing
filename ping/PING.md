# Guide: `ping`

Ping is perhaps the simplest of the tools recommended for use in SCC365. This tools uses ICMP Echo Request/Reply packets to determine connectivity between 2 point in a network that should support ICMP (such as hosts and routers).

Ping can be used with the command "`ping`" followed by any flags.

  - **`-c`**: The number of echo requests to make before automatically exiting
  - **`-i`**: The time to wait between each request (in seconds)

> 📖 There is more `ping` can do, see the [`ping` man page](https://manpages.ubuntu.com/manpages/trusty/man8/ping.8.html) for more!

---

## Example: Making Requests

- To run a ping to a given host, simply run the command and specify the host afterwards:
  ```bash
  ping lancaster.ac.uk
  ```
  This will continue to run until exited (typically `Ctrl-C`)
- To run the same action but limited to 5 requests, the `-c` flag can be used:
  ```bash
  ping -c 5 lancaster.ac.uk
  ```
  Now after the fifth request that program will automatically be terminated. This is particularly useful when automating tests though scripts.
- To run the same action as above but make each request be sent after each 0.5s rather than 1s:
  ```bash
  ping -c 5 -i 0.5 lancaster.ac.uk
  ```

> 💡 **Tip**: 
> In the Mininet prompt, a ping can easily be sent between hosts using host command prefixing and auto name-to-IP tools.
> For example, the following command in the Mininet prompt would ping between `h1` and `h9`:
> **`mininet>`** `h1 ping -c 3 -i 0.75 h9` 

## Example: Parsing Results

The output from `ping`, if targeting a reachable host, should look something like:
```
PING lancaster.ac.uk (148.88.65.80): 56 data bytes
64 bytes from 148.88.65.80: icmp_seq=0 ttl=49 time=45.561 ms
64 bytes from 148.88.65.80: icmp_seq=1 ttl=49 time=52.544 ms
64 bytes from 148.88.65.80: icmp_seq=2 ttl=49 time=83.972 ms
64 bytes from 148.88.65.80: icmp_seq=3 ttl=49 time=46.751 ms
64 bytes from 148.88.65.80: icmp_seq=4 ttl=49 time=46.945 ms

--- lancaster.ac.uk ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 45.561/55.155/83.972/14.610 ms
```

- There should be a line produced for each request (provided it is not being suppressed) that contains stats for that single request:
  ```
  64 bytes from 148.88.65.80: icmp_seq=0 ttl=49 time=45.561 ms
  ```
  For example, this shows that:
  - `64`bytes were received from the resolved IP for `lancaster.ac.uk` (`148.88.65.80`)
  - The packet's TLL was decremented to `49` from its default
  - The round trip time for the ping was `45.561`ms
- The stats at the end are summarizing all requests (including failed requests if any):
  ```
  5 packets transmitted, 5 packets received, 0.0% packet loss
  round-trip min/avg/max/stddev = 45.561/55.155/83.972/14.610 ms
  ```
  This tells us that:
  - As 5 packets were sent and 5 received, no packets were lost
  - The average RTT was `55.155`ms (and other RTT info)
