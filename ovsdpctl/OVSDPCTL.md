# Guide: `ovs-dpctl`

`ovs-dpctl` program is a command line tool for monitoring and administering OpenFlow datapaths. It can also show the current state of an OpenFlow datapath, including features, configuration, and table entries. 

---

OvS-DPCTL can be used with the command "`ovs-dpctl`" followed by any support command.

  - **`dump-flows`**: Dumps the flow table for all supported datapaths
  - **`dump-flows <datapath name>`**: Dumps the flow table for a given supported datapath
  - **`dump-ports-desc <datapath name>`**: Dumps information about the ports on a given supported datapath

> 📖 There is more `ovs-dpctl` can do, see the [`ovs-dpctl` man page](https://manpages.ubuntu.com/manpages/focal/en/man8/ovs-dpctl.8.html) for more!

---

## Example: Dumping a Flow Table

When controllers are using Flow Table modifications, checking on the Flow Table can be vital for debugging.

- To check the flow table for a given switch, for example `s1`, simply run the following command:
  ```
  ovs-dpctl dump-flows s1
  ```
