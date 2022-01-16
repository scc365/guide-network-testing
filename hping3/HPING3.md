# Guide: `hping3`

HPing3 is similar to ping in that it is useful when determining if 2 nodes can connect. However, whereas `ping` uses just ICMP packets, `hping3` is more configurable, allowing more connectivity traits to be discovered. For example, `hping3` can be useful when testing connectivity though a firewall.

HPing3 can be used with the command "`hping3`" followed by any flags.

  - **`-c`**: The number of echo requests to make before automatically exiting
  - **`-i`**: The time to wait between each request (in seconds, or if the number is prefixed with `u`, microseconds)
    - **`--fast`**: Same as `-i u10000`
    - **`--faster`**: Same as `-i u1`
    - **`--fastest`**: Just sends packets as fast as possible
  - **`-1`**: Use ICMP rather than TCP
  - **`-2`**: Use UDP rather than TCP

> 📖 There is **lots** more `hping3` can do, see the [`hping3` man page](https://manpages.ubuntu.com/manpages/focal/en/man8/hping3.8.html) for more!

---

## Example: Test a Firewall

If a firewall were to be situated on the path between 2 hosts, for example `h1` and `h2`, simply using `ping` would only be useful on testing rules that impact ICMP packets (or the lower layers). `hping3` can be used to generate more specific packets, thus be useful when testing firewall rules deigned to block or allow the specific packets. Below are some examples:

- The firewall is intending to block normal web traffic by blocking any TCP packet with the destination port `80` or `443`. To test if this is working with `hping3`, the following could be executed:
  ```
  hping3 -p 80
  ```
  and:
  ```
  hping3 -p 443
  ```
  Failed responses should be given if the firewall is successfully blocking traffic.
> 💡 **Tip**: When testing a service like a firewall with `hping3`, it is important to remember that the firewall is not the only appliance that could be impacting traffic. Using the logs of the firewall or a deeper inspection tool such as [`tshark`](../tshark/TSHARK.md) is needed to fully verify.
- The firewall is intending to block normal QUIC traffic by blocking any UDP packet with the destination port `443`. To test if this is working with `hping3`, the following could be executed:
  ```
  hping3 -2 -p 443
  ```
- The firewall is intending to block stop TCP SYN flood attacks by blocking a node that sends too many TCP syn packets in a small time window. To test if this is working with `hping3`, the following could be executed:
  ```
  hping3 --syn --fastest
  ```

> 💡 **Tip**: 
> In the Mininet prompt, `hping3` can easily be used with host command prefixing and auto name-to-IP tools.
> For example, the following command in the Mininet prompt would send UDP packets between `h1` and `h9`:
> **`mininet>`** `h1 hping3 -2 -c 3 h9` 

