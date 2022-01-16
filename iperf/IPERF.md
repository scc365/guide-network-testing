# Guide: `iperf`

IPerf is a CLI tool that determines the bandwidth between 2 points in a network. Unlike the other tools recommended in SCC365, it requires commands to be executed on both nodes for a single test.

Iperf can be used with the command "`iperf3`" followed by any flags.

  - **`-s`**: Specify that the instance is to act as the server node
  - **`-c`**: Specify that the instance is to act as the client node connecting to a given server
  - **`-p`**: The port number to listen/connect to

The following flags can be used alongside the `-c` flag:

  - **`-u`**: Opt for testing with UDP rather than the default that is TCP
  - **`-t`**: Time to run the test for (in seconds)
  - **`-P`**: Number of simultaneous streams to test over (useful for testing multiple path routes)

> 📖 There is more `iperf3` can do, see the [`iperf3` man page](https://manpages.ubuntu.com/manpages/focal/en/man1/iperf3.1.html) for more!

---

## Example: Testing Bandwidth

- Firstly, run an `iperf` server on one of the nodes. For example, on a machine with the publically visible IP `10.50.50.30`:
  ```bash
  iper3 -s -p 5201
  ```
- Next to run the test, on the other machine run an `iperf` client that connects to prior created server:
  ```bash
  iperf -c 10.50.50.30 -p 5201 -t 5
  ```
  This command will run an `iperf` test over TCP for 5 seconds, determining the bandwidth between the client and `10.50.50.30`
- The output from on client then might look somewhat like:
  ```
  Connecting to host 10.50.50.30, port 5201
  [  4] local 10.0.0.2 port 50628 connected to 10.50.50.30 port 5201
  [ ID] Interval           Transfer     Bandwidth       Retr  Cwnd
  [  4]   0.00-1.00   sec  4.94 MBytes  41.4 Mbits/sec    0    495 KBytes       
  [  4]   1.00-2.00   sec  4.73 MBytes  39.6 Mbits/sec    0    684 KBytes       
  [  4]   2.00-3.00   sec  4.22 MBytes  35.4 Mbits/sec    0    857 KBytes       
  [  4]   3.00-4.00   sec  5.00 MBytes  41.9 Mbits/sec    0   1.04 MBytes       
  [  4]   4.00-5.00   sec  5.00 MBytes  41.9 Mbits/sec    0   1.24 MBytes       
  - - - - - - - - - - - - - - - - - - - - - - - - -
  [ ID] Interval           Transfer     Bandwidth       Retr
  [  4]   0.00-5.00   sec  23.9 MBytes  40.1 Mbits/sec    0             sender
  [  4]   0.00-5.00   sec  18.5 MBytes  31.1 Mbits/sec                  receiver
  ```
  The summary at the bottom of the output contains the most useful information. 
  - The `sender` line shows that the upload speed from the `iperf` client to the server is `40.1`Mbps
  - The `receiver` line shows that the download speed from the `iperf` server to the client is `31.1`Mbps


## Example: Testing Bandwidth in Mininet

Using `iperf` in Mininet can be easily done though the command given in the Mininet prompt like so:

- The Mininet prompt provides an `iperf` command that can be very easily used to test the bandwidth between 2 nodes. For example, to test the bandwidth between `h1` and `h2`, the following command can be executed:
  ```bash
  mininet> iperf h1 h2
  ```

This however does not have the configurability of using the `iperf3` command recommended:

- To repeat the `h1` to `h2` `iperf` test done above, you can use the node name prefixing to run the `iperf3` command:
  ```
  mininet> h1 iperf3 -s -p 5201 &
  ```
  This starts an `iperf` server in the background on `h1`. You can check this by checking the jobs on `h1` like so:
  ```
  mininet> h1 jobs
  ```
- Next, you need to run the client on `h2` and connect to the server that is running on `h1`:
  ```
  mininet> h2 iperf3 -c h1 -p 5201 -t 5
  ```
  Then the test should execute, and the client side results should be displayed
- Finally, to clean up the `iperf3` server should by terminated. Using the job ID (in the square brackets `[X]`), run the kill command on `h1`:
  ```
  mininet> h1 kill %1
  ```
  (assuming the job ID was `1`)

## Example: Parallel Streams

An IPerf test will appear to devices in the network as a single "flow". So, when testing the bandwidth between 2 nodes that have multiple load balanced routes, the `-P` flag should be used on the client.

- Simply running the same command as in the prior examples for the server is okay (using the setup from the first [example](#example-testing-bandwidth)):
  ```
  iper3 -s -p 5201
  ```
- However, for the client, the `-P` flag should be used to indicate how many streams should be used. Each stream will present itself as a separate flow. It is worth noting that the load balancing algorithm may bin multiple flows down the same path. For example, the use 5 parallel streams:
  ```
  iperf -c 10.50.50.30 -p 5201 -P 5
  ```
