# Guide: `traceroute`

Packets move though IP networks by hoping though various appliances. Perhaps the most important appliance of note in terms of packet forwarding are routers. Knowing if 2 points in a network can communicate is something that can easily be determined with tools such as [`ping`](../ping/PING.md) and [`hping3`](../hping3/HPING3.md), but knowing any details about the route they traversed the network via is typically less visible. Traceroute uses the IP header's TTL field and the expected ICMP `TIME_EXCEEDED` responses to determine each hop a packet takes on its journey.

Traceroute can be used with the command "`traceroute`" followed by any flags.

  - **`-f`**: The TTL of the first probe (each probe increments this by `1`)
  - **`-m`**: The maximum TTL to increment to

> 📖 There is more `traceroute` can do, see the [`traceroute` man page](https://manpages.ubuntu.com/manpages/trusty/man1/traceroute.db.1.html) for more!

---

## Example: Tracing a Journey

- To determine the IP addresses of any routers between your device and `lancaster.ac.uk`, use `traceroute` like so:
  ```
  traceroute lancaster.ac.uk
  ```
- The output should look something like:
  ```
  traceroute to lancaster.ac.uk (148.88.65.80), 30 hops max, 60 byte packets
   1  gateway (X.X.X.X)  0.228 ms  0.187 ms  0.181 ms
   2  X.X.X.X (X.X.X.X)  0.401 ms  0.347 ms  0.375 ms
   3  jips-ipv6.lancs.ac.uk (194.80.33.17)  5.399 ms  5.601 ms  5.375 ms
   4  isag03.is-core01.rtr.lancs.ac.uk (148.88.253.153)  5.341 ms  6.026 ms  5.527 ms
   5  www.lancs.ac.uk (148.88.65.80)  0.695 ms !X  0.697 ms !X  0.653 ms !X
  ``` 
  This output shows each router encountered by the packet on the way to `lancaster.ac.uk`, including the local gateway.
