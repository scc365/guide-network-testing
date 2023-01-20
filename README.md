# Guide: Network Testing

This tutorial looks at testing networks using some popular Linux network testing tools. Being able to test a network, emulated or real, can be a challenge. Networks typically don’t just work or not work, but they have many traits can be impacted by a wide variety of factors both within and outside the network itself. Here you will find guides to the network testing tools recommended for use in SCC365 and how they can be used to determine values for specific metrics.

Some testing is covered in the [`Mininet Tutorial`](https://github.com/scc365/tutorial-mininet), so if you have not yet completed that tutorial, please do so before starting this.

> ⏰ **Estimated Completion Time**: 2 Hours

> 📖 This can be used a guide to refer back to when completing other SCC365 tasks

## Getting Started

### **MyLab VM**

> required ‼️

A VM named "SCC 365" is available on the university hosted MyLab service. This is the best way of running the provided VM and works from within your browser or the VMWare Horizon desktop client.

It is this option that will be used by TAs when your work is being marked, so be sure to test any assessed work on this platform prior to submission.

You can access MyLab [`here`](https://mylab.lancaster.ac.uk/).

### **Docker**

> optional 🐳

If you would prefer to complete the practical work without a VM, this is partially possible. Provided are also docker base images for the core software used by the module. These are usable on MacOS and Linux.

Although the use of docker is not required, there will be some related help in the tutorials and coursework specs.

You can find the base images on the GitHub page here .

## Network Metrics

A good start when testing is to know what it is your testing for, and why! Networks can be very complicated webs of interconnect hardware and software all operating within physical constraints. To understand how a network is operating, you need to look holistically at a wide variety of metrics over time. This stage will describe some metrics (many of which you might be familiar with) that can help you understand a network.

 - **latency**: This is a measure of time, specifically how long it takes for data to get from a given point in the network to another. Especially if you play games, you are probably familiar with latency as it is not just a metric used in networks, but often used as a core metric where any data needs to be transferred between 2 points (e.g. the mouse sensor data to be sent to the PC and be processed). In networks, this metric can be used to gain insight as to how end-to-end performance may be, especially with real time applications, or it can be used to find latency bottlenecks within the network. Many factors can impact latency including in-network queues and simple geography. Typically, this is a value that should be minimized where possible.
 - **round trip time**: Where latency is the time cost between two points in a network, round trip time (rtt) is the time it takes for data to go from a point in the network to another, and back again. This metric is important to collect separately to latency as the time it takes for the data to move between 2 points might be different in reverse. Again, this is typically minimized where possible.
 - **bandwidth**: Also described as link capacity, bandwidth is a measure of how much data can traverse a link per second, and so measured in bits per second (`Mbps`,`Gbps`). If you've ever executed a speed test or purchased a data package from an ISP, then you've probably encountered this before. Although it can appear somewhat simple, often just dubbed "speed", it can have its quirks. For example, when a speed test is run, it can only evaluate the bandwidth between your system (specifically the app on your system) and the specific selected server (again, specifically the application) and only for the type of traffic used. There is typically a lot of network infrastructure between a client device and a remote server, and the configuration on each of these devices can have large consequences. As soon as a single link/device reduces the bandwidth, it is bottlenecked to that value. In many circumstances a larger bandwidth is better, but it is worth considering the device's ability to process the packets being received.
 - **packet loss**: This is the number of packets in a given set of packets that do not reach the destination (or when testing, are not _known_ to reach the destination). This is typically shown as a percentage.
 - **jitter**: Here is where monitoring over time starts to play more of a role. Jitter describes the variance in latency over time calculated by taking the average difference between each latency measurement. For example, if there were 10 latency measurements, there would be 9 differences to average to determine jitter. High jitter can be particularly impactful to applications such as VoIP and online gaming and is best minimized.

Testing over time isn't just useful for jitter calculations, but looking at how values change on each measurement can be insightful. Using just single values or averages can be deceptive and the details of network behavior can be lost. The example tests in the [`Mininet Tutorial`](https://github.com/scc365/tutorial-mininet) show this as by noticing that the first ping in a series after creating the topology has a larger rtt than the others indicates the processing that is taking place.

## Tools

Firstly, before any testing can be done, information on the visible network should be gathered. For this you should use `iproute2`:

|     Name     | Command |                         Description                          |            Guide             |
| :----------: | :-----: | :----------------------------------------------------------: | :--------------------------: |
| **IPRoute2** |  `ip`   | Show/manipulate routing, devices, policy routing and tunnels | [here](./iproute/IPROUTE.md) |

> 💡 **Tip**: Remember, if using Mininet you can run commands as hosts in the topology by prefixing the command with the host's name in the Mininet prompt. For example, to list all interface addresses on `h1`:
> **`mininet>`** `h1 ip a`

### Testing Tools

When testing networks, it is important to gather the metrics mentioned above as they can provide a quick indication as to the general state of the network. There are many tools deigned to gather those, but for SCC365 the following CLI tools should be used:

|    Name    | Command  |                                               Description                                               |           Guide            |
| :--------: | :------: | :-----------------------------------------------------------------------------------------------------: | :------------------------: |
|  **Ping**  |  `ping`  | Using ICMP echos, ping can evaluate metrics such as connectivity, latency, rtt, jitter, and packet loss |   [here](./ping/PING.md)   |
| **IPerf**  | `iperf3` |        With an iperf client and server, this can determine the bandwidth between 2 the 2 points         |  [here](./iperf/IPERF.md)  |
| **HPing3** | `hping3` | Using ICMP echos, ping can evaluate metrics such as connectivity, latency, rtt, jitter, and packet loss | [here](./hping3/HPING3.md) |


### Inspection Tools

As mentioned in [stage 1](#network-metrics), debugging a network can sometimes be more complicated as devices interact with one another in ways not naturally observable to the network operator. These inspection tools can uncover different details that networks do not naturally make observable. In turn, these can be useful when finding the cause of an issue as opposed to the [testing tools](#testing-tools) that can be useful to determine if there is an issue.

|      Name      |   Command    |                                             Description                                             |               Guide                |
| :------------: | :----------: | :-------------------------------------------------------------------------------------------------: | :--------------------------------: |
| **Traceroute** | `traceroute` | Determine each router that a packet traverses between the traceroute client and a given destination | [here](./traceroute/TRACEROUTE.md) |
|   **TShark**   |   `tshark`   |                       Inspect and filter all packets moving though a network                        |     [here](./tshark/TSHARK.md)     |

### OpenFlow Tools

When using OpenFlow, tools such as TShark can be vital for debugging. However, this only lets you view the interaction between a controller and switch. Tools also exist that allow you to perform specific OpenFlow actions manually:

|     Name      |        Command         |                                 Description                                  |                                     Guide                                      |
| :-----------: | :--------------------: | :--------------------------------------------------------------------------: | :----------------------------------------------------------------------------: |
| **OvS OFCTL** |      `ovs-ofctl`       | Manually control OvS switches using various control types including OpenFlow |                         [here](./ovsofctl/OVSOFCTL.md)                         |
| **OvS DPCTL** |      `ovs-dpctl`       | Manually administer OvS datapaths                                            |                         [here](./ovsdpctl/OVSDPCTL.md)                         |
|   **PTCP**    | `controller ptcp:6633` |                       Run a simple OpenFlow controller                       | Included in the [Mininet Tutorial](https://github.com/scc365/tutorial-mininet) |

### P4 Tools

Right now SCC365 does not use P4, thus does not recommend any P4 related tools. This might not be the same forever though, so feel free to check back in the future 🔮
