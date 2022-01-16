# Guide: `tshark`

TShark is a command line tool that allows a user to inspect packets based on given filters and dissections. It is perhaps the most verbose tool recommended for use in the SCC365 course and thus should be used in 2 contexts: to find the cause of a problem identified with a tool that provides a higher-level summary (e.g. `ping`), or to simply learn more about networked interaction.

The `tshark` CLI tool can be used with the command "`tshark`" followed by any flags.

> 💡 **Tip**: `sudo` may be required

  - **`-i`**: The network interface to monitor
  - **`-d`**: Specify how a layer type should be dissected
  - **`-O`**: A list of protocols that will be printed in detail
  - **`-f`**: A filter expression for which packets to show
  - **`-Y`**: A display filter options for which packets to show

> 📖 There is more `tshark` can do, see the [`tshark` man page](https://www.wireshark.org/docs/man-pages/tshark.html) for more!

---

## Example: Generic Monitoring

- Firstly, to see a list of available interfaces to `tshark`, you can run:
  ```bash
  tshark -D
  ```
- Next, simply run `tshark` on that interface (for example: `s1-eth1`):
  ```bash
  tshark -i s1-eth1
  ```
  Now all packets moving though `s1-eth1` should be displayed.
- Now to find a specific packet type you are looking for, you can use a _display filter_. Say, to only show `icmp` packets, you can run the following:
  ```bash
  tshark -i s1-eth1 -Y icmp
  ```
  Now only `icmp` packets should be displayed.

## Example: OpenFlow Debugging

This example assumes there is an OpenFlow-enabled switch (such as OvS in Mininet) using a remote controller situated on the same machine as the switch on port `6633`. This should be a common scenario for those completing SCC365 tasks.

- Firstly, as the controller communication takes place through the loopback (`lo`) interface, this should be specified:
  ```bash
  tshark -i lo
  ```
- However, this will now show all packets that are traversing the `lo` interface. To make it easier to debug OpenFlow communication `tshark` should dissect TCP packets on port `6633` as OpenFlow, and only OpenFlow packets should be printed in detail:
  ```bash
  tshark -i lo -d tcp.port==6633,openflow -O openflow_v4
  ```
  > 💡 **Note**: The `_v4` specifies OpenFlow v1.3, the version used by SCC365
- This should now be useful in debugging, occasionally printing output that looks somewhat like:
  ```
  OpenFlow 1.3
    Version: 1.3 (0x04)
    Type: OFPT_FEATURES_REQUEST (5)
    Length: 8
  ```
  Note the `(5)` for the type, this is the ID for the features request packet type. To further specify your target packets, you can add display filters such as:
  ```bash
  tshark -i lo -d tcp.port==6633,openflow -O openflow_v4 -Y "openflow_v4.type==5"
  ```
  To find out the other type IDs, play around with `tshark` when using `ryu` or `ptcp` controllers with Mininet.
  > 🎁 **Bonus**: ID `0` is the OpenFlow "Hello" type. This is used on first contact between a switch and a controller

---

## Using Docker 🐳

You can use `tshark` in docker too! This can be useful if you are running all the SCC365 work in docker, or just using your own device. To do so, simply replace the command `tshark` with the following docker command:

```bash
docker run --rm -it --privileged --network host ghcr.io/scc365/tshark:latest
```

> 💡 **Tip**: You can also alias "`tshark`" to that command in your shell if you are familiar with aliases.

You can then use this much like a regular `tshark` install, for example to monitor all traffic on the loopback:

```bash
docker run --rm -it --privileged --network host ghcr.io/scc365/tshark:latest -i lo
```
