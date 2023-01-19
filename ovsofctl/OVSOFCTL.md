# Guide: `ovs-ofctl`

`ovs-ofctl` program is a command line tool for monitoring and administering OpenFlow switches. It can also show the current state of an OpenFlow switch, including features, configuration, and table entries. It should work with any OpenFlow switch, not just Open vSwitch.

---

## Guide `ovs-vsctl`

Specifically for Open vSwitch, this tool needs a brief mention. To use `ovs-ofctl`, you need to know the names of any OpenFlow enabled switches on the system. As SCC365 uses OvS switches with Mininet, you can use `ovs-vsctl` to get a list of switches useable by `ovs-ofctl` like so:

```bash
ovs-vsctl list-br
```

> 📖 There is more `ovs-vsctl` can do, see the [`ovs-vsctl` man page](https://manpages.ubuntu.com/manpages/focal/en/man8/ovs-vsctl.8.html) for more!

---

OvS-OFCTL can be used with the command "`ovs-ofctl`" followed by any support command.

  - **`dump-flows <switch name>`**: Dumps the flow table for a given supported switch
  - **`dump-ports-desc <switch name>`**: Dumps information about the ports on a given supported switch

> 📖 There is more `ovs-ofctl` can do, see the [`ovs-ofctl` man page](https://manpages.ubuntu.com/manpages/focal/en/man8/ovs-ofctl.8.html) for more!

---

## Example: Dumping a Flow Table

When controllers are using Flow Table modifications, checking on the Flow Table can be vital for debugging.

- To check the flow table for a given switch, for example `s1`, simply run the following command:
  ```
  ovs-ofctl dump-flows s1
  ```

> 💡 **Tip**: 
> In Mininet, by default switches are created in the same network namespace as the prompt starting Mininet. Thus, to get information via `ovs-ofctl`, another terminal window (or tmux pane) should be used.
