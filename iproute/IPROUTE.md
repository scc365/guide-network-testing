# Guide: `iproute`

IPRoute is a fairly complex tool, but is still recommended in SCC365. For the course you can use `iproute` to list addresses of interfaces, and to bring links up/down.

> 📖 There is more `iproute2` can do, see the [`ip` man page](https://manpages.ubuntu.com/manpages/focal/en/man8/ip.8.html) for more!

---

## Example: Listing Addresses

- To list all addresses using `ip`, simply run:
  ```bash
  ip a
  ```
  A response in the same format as below should be returned:
  ```
  1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
  2: ip6tnl0@NONE: <NOARP> mtu 1452 qdisc noop state DOWN group default qlen 1000
    link/tunnel6 :: brd ::
  3: h1-eth0@if21: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc htb state UP group default qlen 1000
    link/ether 7a:77:bc:5f:6e:e5 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 10.0.0.1/8 brd 10.255.255.255 scope global h1-eth0
       valid_lft forever preferred_lft forever
    inet6 fe80::7877:bcff:fe5f:6ee5/64 scope link 
       valid_lft forever preferred_lft forever
  ```
- This example response shows that this given device has 3 interfaces. The points of note are the interfaces names (e.g. `h1-eth0`), the IP addresses (e.g. `10.0.0.1/8`), the MAC addresses (e.g. `7a:77:bc:5f:6e:e5`), and the link state (e.g. `UP`). However, the other information may also be useful in some scenarios.


> 💡 **Tip**: 
> In Mininet, interfaces for a specific host can't be seen from the machine running Mininet, so run the `ip a` command from the host via the Mininet CLI:
> **`mininet>`** `h1 ip a`
> Also, interfaces on switches should be visible from the machine running Mininet. 


## Example: Setting Link State

- To set an interface visible via the `ip a` command to a given state, you can use the `link set` command. For example, to set an interface called `s1-eth0` to the `DOWN` state:
  ```
  ip link set s1-eth0 down
  ```
- The result of this can be checked via the `ip a` command.
