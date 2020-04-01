# NS Lab 1: *Internet Routes and Measurement of Round Trip Times*


### Overview

In this lab exercise, you will learn how to use `ping` and `traceroute` to measure round trip times and find network routes.


### Learning objectives

At the end of this lab exercise, you should be able to:

  - Understand how the `ping` and `traceroute` utilities work.
  - Use the `ping` utility to measure network round trip times.
  - Use the `traceroute` utility to find network routes.
  - Observe and understand the effects of varying packet sizes on delays experienced.


### Preparation

You will need ping and traceroute to be installed on your Ubuntu virtual machine. Most Ubuntu installations should already include ping by default. You can install traceroute by running `sudo apt-get install traceroute` from the command line.




# Part 1: Measurement of round trip times using `ping`

The `ping` utility is one of the most widely-used network utilities. It enables you to measure the time that it takes for a packet to travel through the Internet to a remote host and back.

The `ping` utility works by sending a short message, known as an echo-request, to a remote host using the Internet Control Message Protocol (ICMP). When a host that supports ICMP receives an echo-request message, it replies by sending an echo-response message back to the originating host.

In the first part of this lab exercise, you will use the ping utility to send echo requests to a number of different hosts. In many of the exercises, you will be referring to hosts using their DNS names rather than their IP addresses. For more information about ping, you can look up its manual page by running `man ping` from the command line.


## Round trip times

Use `ping` to send 10 packets to each of the following hosts. Each packet should have a size of 56 bytes, and there should be an interval of 5 seconds between each packet sent.

  1. `www.csail.mit.edu`
  2. `www.berkeley.edu`
  3. `www.usyd.edu.au`
  4. `www.kyoto-u.ac.jp`

Note: The size of each packet is 56 bytes by default, but you may observe that the actual size of the packet is larger than 56 bytes. You can look up the manual for ping to understand why such a discrepancy exists.


### Question 1

*For each host, record the percentage of packets sent that resulted in a successful response. Record also the minimum, average, and maximum round trip times for the packets that resulted in a response.*

- **Command used:** `ping -i 5 -c 10 [HOST]`

| Host                | Pass | Min RTT | Avg RTT | Max RTT |
| ------------------- | ---- | ------- | ------- | ------- |
| `www.csail.mit.edu` | 100  | 27.380  | 48.116  | 62.211  |
| `www.berkeley.edu`  | 100  | 227.565 | 325.692 | 433.604 |
| `www.usyd.edu.au`   | 100  | 118.433 | 146.782 | 201.896 |
| `www.kyoto-u.ac.jp` | 100  | 107.024 | 147.541 | 312.626 |


### Question 2

*Describe and explain the differences in the minimum round trip time to each of these hosts.*

- The hosts with the fastest to the slowest minimum RTTs were `www.csail.mit.edu`, `www.kyoto-u.ac.jp`, `www.usyd.edu.au`, and `www.berkeley.edu`. 
- The differences in minimum RTTs may have been due to various factors such as queueing delay, routing delay, and propagation delay (since the packet size is constant and small, transmission delay should be relatively negligible).
- For instance, it only took 50 hops to reach `www.csail.mit.edu`, but it took 226 hops to reach `www.berkeley.edu`, which could have led to increased routing and propagation delays. 
- However, it took 238 hops to reach `www.usyd.edu.au`, even though its minimum round trip time was shorter than `www.berkeley.edu`. 
- Also, it took only 49 hops to reach `www.kyoto-u.ac.jp`, even though its minimum round trip time was longer than `www.csail.mit.edu`.
- These differences could have been caused by a higher queueing delay due to increased traffic in the local network around `www.berkeley.edu` and `www.kyoto-u.ac.jp`.


### Question 3

*Repeat the exercise using packet sizes of 56, 512 and 1024 bytes. Record the minimum, average, and maximum round trip times for each of the packet sizes. Why are the minimum round-trip times to the same hosts different when using 56, 512, and 1024–byte packets?*

- **Command used:** `ping -i 5 -c 10 -s [PACKETSIZE] [HOST]`

| Host                | Size | Pass | Min RTT | Avg RTT | Max RTT |
| ------------------- | ---- | ---- | ------- | ------- | ------- |
| `www.csail.mit.edu` | 56   | 100  | 29.474  | 56.157  | 74.407  |
|                     | 512  | 100  | 33.812  | 46.693  | 82.605  |
|                     | 1024 | 100  | 33.749  | 70.090  | 166.446 |
| `www.berkeley.edu`  | 56   | 100  | 274.919 | 376.738 | 465.444 |
|                     | 512  | 100  | 276.569 | 365.118 | 446.207 |
|                     | 1024 | 100  | 250.129 | 337.968 | 437.085 |
| `www.usyd.edu.au`   | 56   | 100  | 118.587 | 137.397 | 156.619 |
|                     | 512  | 100  | 129.669 | 153.646 | 169.284 |
|                     | 1024 | 100  | 129.846 | 147.961 | 177.669 |
| `www.kyoto-u.ac.jp` | 56   | 100  | 100.129 | 130.817 | 159.965 |
|                     | 512  | 100  | 111.190 | 135.064 | 157.106 |
|                     | 1024 | 100  | 119.524 | 136.616 | 152.112 |

- From the results, it can be observed that in general, the larger the packet size, the greater the minimum RTT.
- This is most likely caused by increased transmission delay due to the increase in number of bytes to send.
- However, the minimum RTT for 1024-byte packets from `www.berkeley.edu` was shorter than 56 and 512-byte packets.
- This could suggest that other factors such as variations in queuing delay may have affected the minimum RTT more significantly.
- Since the effect of increasing packet size on minimum RTT is small (only a few milliseconds increase on average), it can be deduced that transmission delay does not have a very significant effect on minimum RTT.


## Unanswered pings

Use `ping` to send 100 packets to the following host. Each packet should have a size of 56 bytes, and there should be an interval of 5 seconds between each packet sent.

`www.wits.ac.za`


### Question 4

*Record the percentage of the packets sent that resulted in a successful response. What are some possible reasons why you may not have received a response? (Be sure to check the host in a web browser.)*

- **Command used:** `ping -i 5 -c 100 www.wits.ac.za`

| Host             | Pass |
| ---------------- | ---- |
| `www.wits.ac.za` | 0    |


- This was received between `icmp_seq 36` and `icmp_seq 37`:

> ```
> 92 bytes from 192.168.43.1: Destination Net Unreachable
> Vr HL TOS  Len   ID Flg  off TTL Pro  cks       Src           Dst
>  4  5  00 5400 3a40   0 0000  40  01 b4b7 192.168.43.74  146.141.13.50
> ```

- Possible reasons that `www.wits.ac.za` could not be reached: 

  1. Echo/ICMP requests could be disabled on a router or the server itself to prevent Denial-of-Service (DoS) attacks such as ping flooding or for other security concerns such as ICMP tunneling.

  2. A firewall installed on the server could be blocking echo/ICMP requests.

  3. The server could have been misconfigured to handle echo/ICMP requests or does not support ICMP.




# Part 2: Understanding Internet routes using `traceroute`

The `traceroute` utility is another useful network utility. It enables you to trace the route taken by a packet from your machine to a remote host.

Here is an example of the output produced when `traceroute` is used to trace the route taken by a packet to `www.mit.edu`.

```
traceroute to www.mit.edu (118.215.81.86), 30 hops max, 60 byte packets
1 192.168.9.2 (192.168.9.2) 0.221 ms 0.193 ms 0.107 ms
2 10.12.0.1 (10.12.0.1) 3.363 ms 2.555 ms 3.253 ms
3 172.16.1.106 (172.16.1.106) 3.072 ms 3.416 ms 3.418 ms
4 172.16.1.210 (172.16.1.210) 4.977 ms 4.712 ms 4.921 ms
5 192.168.22.27 (192.168.22.27) 4.806 ms 6.521 ms 6.451 ms
6 103.24.77.1 (103.24.77.1) 7.172 ms 3.590 ms 3.187 ms
7 201.210-193-8.qala.com.sg (210.193.8.201) 4.312 ms 9.056 ms
7.870 ms
8 137.203-211-158.unknown.qala.com.sg (203.211.158.137) 8.904 ms
6.690 ms 6.555 ms
9 213.203-211-158.unknown.qala.com.sg (203.211.158.213) 7.710 ms
5.423 ms 5.193 ms
10 203.116.10.125 (203.116.10.125) 6.783 ms 6.705 ms 6.440 ms
```

Each line in the output begins with a host on the route from your computer to `www.mit.edu`, followed by the round-trip times for 3 packets sent to that host.

For more information about `traceroute`, you can look up its manual page by running `man traceroute` from the command line.


## Basics of `traceroute`


### Question 5

*Explain how `traceroute` discovers a path to a remote host. (Hint: The `traceroute` manual will be helpful for answering this question.)*

- `traceroute` works by sending batches of three ICMP packets onto the Internet.
- After each batch of ICMP packets are sent out, the **TTL** (Time To Live) of each packet is incremented.
- When an ICMP packet reaches the next router, its TTL will be decremented. The packet will continue to be routed as long as its TTL is more than 0.
- Once the TTL reaches 0, it will not be forwarded anymore and an `ICMP Time Exceeded` message will be sent back to the source. 
- After the source receives the `ICMP Time Exceeded` message, the round trip time will be captured.
- However, if an the `ICMP Time Exceeded` message is not received after a certain timeout, traceroute will skip ahead to the next batch of ICMP packets.
- When the ICMP packet is successfully routed to the destination, it will attempt to connect to an unused port, which will elicit an `ICMP Port Unreachable` response. 
- `traceroute` terminates once it has received the `ICMP Port Unreachable` message, or if the TTL has been incremented past the maximum number of hops.


## Route asymmetries

In this exercise, you will run traceroute in two opposite directions. First, you will run traceroute on a remote host to see the route taken to your network. You will also run traceroute from your computer to see the route taken to that host. 

Steps:
  1. Find out your computer’s public IP address. (Hint: You can use a website like http://www.whatismypublicip.com/, or search for “what is my ip” using Google’s search engine.)
  2. Visit https://www.uptrends.com/tools/traceroute in your web browser. Enter your com-puter’s public IP address, select the “from Location” and click “Start Test” to start a traceroute to your computer. Follow the steps shown below for at least three locations namely: New York, Amsterdam, Tokyo.
  3. After traceroute finishes running, you should be able to view the route taken from specified location to your network. Record the IP address of the first hop, which will be used in the next step.
  4. On your computer, run traceroute using the IP address recorded in the previous step as the remote destination.


### Question 6

*Record the output of traceroute when run in both directions above.*

My IP address: `111.65.56.89`

> **https://www.uptrends.com/tools/traceroute**  
> Test: `111.65.56.89`  
> From: New York
> 
> | Step | Time | Time | Time | Hostname                                  | IP address      |
> |------|------|------|------|-------------------------------------------|-----------------|
> | 1    | 1    | <1   | <1   | 72-9-99-137-cust-gw.reverse.ezzi.net      | 72.9.99.137     |
> | 2    | 2    | 3    | 3    | ads-psc-cr01.ezzi.net                     | 96.45.77.1      |
> | 3    | 1    | 1    | 1    | ads-psc-ir01-v261.ezzi.net                | 72.9.111.109    |
> | 4    | 1    | <1   | 1    | ads-psc-ir02-te2-1.ezzi.net               | 72.9.111.206    |
> | 5    | 2    | 1    | 1    |                                           | 38.32.124.49    |
> | 6    | 2    | 1    | 1    | te0-3-1-12.rcr51.ewr06.atlas.cogentco.com | 154.24.13.241   |
> | 7    | 2    | 2    | 2    | be3791.rcr21.ewr02.atlas.cogentco.com     | 154.24.61.177   |
> | 8    | 3    | 2    | 3    | be2272.ccr42.jfk02.atlas.cogentco.com     | 154.54.83.201   |
> | 9    | 3    | 3    | 3    | be3496.ccr31.jfk10.atlas.cogentco.com     | 154.54.0.142    |
> | 10   | 3000 | 2    | 2    | zayo.jfk04.atlas.cogentco.com             | 154.54.9.63     |
> | 11   | 69   | 69   | 69   | ae16.cs1.lga5.us.zip.zayo.com             | 64.125.25.56    |
> | 12   | -    | -    | -    |                                           |                 |
> | 13   | 69   | 70   | 69   | ae12.mcs1.lhr11.uk.eth.zayo.com           | 64.125.28.225   |
> | 14   | 69   | 69   | 69   | ae3.mpr3.lhr8.uk.zip.zayo.com             | 64.125.26.151   |
> | 15   | 69   | 68   | 68   | ae5.mpr1.lhr8.uk.zip.zayo.com             | 64.125.26.74    |
> | 16   | 68   | 68   | 68   |                                           | 213.161.72.106  |
> | 17   | 242  | 240  | 240  |                                           | 203.208.166.249 |
> | 18   | 245  | 246  | 245  |                                           | 203.208.192.190 |
> | 19   | 242  | 239  | 239  | ae2-0.qt-cr04.singnet.com.sg              | 202.166.120.202 |
> | 20   | 242  | 241  | 241  |                                           | 202.166.119.73  |
> | 21   | 242  | 241  | 241  |                                           | 202.166.126.35  |
> | 22   | -    | -    | -    |                                           |                 |
> | 23   | -    | -    | -    |                                           |                 |
> | 24   | -    | -    | -    |                                           |                 |
> | 25   | -    | -    | -    |                                           |                 |

> **macOS traceroute**  
> Test: `72.9.99.137`  
> From: `111.65.56.89`  
> ```
> traceroute to 72.9.99.137 (72.9.99.137), 64 hops max, 52 byte packets
>  1  192.168.43.1 (192.168.43.1)  4.400 ms  2.016 ms  1.492 ms
>  2  192.168.213.15 (192.168.213.15)  65.537 ms  43.710 ms  262.059 ms
>  3  10.252.11.141 (10.252.11.141)  88.541 ms
>     10.252.11.137 (10.252.11.137)  142.637 ms
>     10.252.11.141 (10.252.11.141)  115.378 ms
>  4  10.252.11.50 (10.252.11.50)  24.854 ms
>     10.252.11.54 (10.252.11.54)  125.706 ms
>     10.252.11.50 (10.252.11.50)  87.891 ms
>  5  202.166.126.34 (202.166.126.34)  38.833 ms  32.120 ms  135.389 ms
>  6  202.166.119.72 (202.166.119.72)  36.657 ms  27.808 ms  88.223 ms
>  7  202.166.124.37 (202.166.124.37)  130.380 ms  131.466 ms  28.098 ms
>  8  ae6-0.tp-er03.singnet.com.sg (202.166.126.97)  141.252 ms  139.758 ms  30.664 ms
>  9  203.208.191.197 (203.208.191.197)  136.269 ms
>     203.208.191.113 (203.208.191.113)  143.647 ms
>     203.208.145.233 (203.208.145.233)  28.223 ms
> 10  203.208.172.210 (203.208.172.210)  200.966 ms
>     203.208.171.186 (203.208.171.186)  206.955 ms
>     203.208.173.138 (203.208.173.138)  198.681 ms
> 11  palo-b22-link.telia.net (62.115.8.200)  261.864 ms  277.956 ms  224.238 ms
> 12  cogent-ic-344188-palo-b22.c.telia.net (62.115.174.65)  304.406 ms
>     203.208.172.234 (203.208.172.234)  301.798 ms
>     cogent-ic-344188-palo-b22.c.telia.net (62.115.174.65)  269.678 ms
> 13  be2016.ccr22.sfo01.atlas.cogentco.com (154.54.0.177)  308.295 ms
>     be2015.ccr21.sfo01.atlas.cogentco.com (154.54.7.173)  203.946 ms  306.994 ms
> 14  cogent-ic-344188-palo-b22.c.telia.net (62.115.174.65)  205.107 ms
>     be3109.ccr21.slc01.atlas.cogentco.com (154.54.44.138)  307.269 ms
>     be3110.ccr32.slc01.atlas.cogentco.com (154.54.44.142)  306.479 ms
> 15  be3037.ccr21.den01.atlas.cogentco.com (154.54.41.146)  271.612 ms
>     be3038.ccr22.den01.atlas.cogentco.com (154.54.42.98)  275.737 ms
>     be2015.ccr21.sfo01.atlas.cogentco.com (154.54.7.173)  273.257 ms
> 16  be3035.ccr21.mci01.atlas.cogentco.com (154.54.5.90)  306.806 ms
>     be3036.ccr22.mci01.atlas.cogentco.com (154.54.31.90)  280.173 ms
>     be3035.ccr21.mci01.atlas.cogentco.com (154.54.5.90)  367.534 ms
> 17  be3038.ccr22.den01.atlas.cogentco.com (154.54.42.98)  306.481 ms
>     be2832.ccr42.ord01.atlas.cogentco.com (154.54.44.170)  409.086 ms
>     be3037.ccr21.den01.atlas.cogentco.com (154.54.41.146)  271.456 ms
> 18  be3035.ccr21.mci01.atlas.cogentco.com (154.54.5.90)  306.640 ms
>     be2718.ccr22.cle04.atlas.cogentco.com (154.54.7.130)  307.605 ms
>     be3036.ccr22.mci01.atlas.cogentco.com (154.54.31.90)  272.880 ms
> 19  * be2890.ccr42.jfk02.atlas.cogentco.com (154.54.82.246)  323.980 ms  374.576 ms
> 20  be2717.ccr21.cle04.atlas.cogentco.com (154.54.6.222)  305.187 ms
>     be2271.rcr21.ewr01.atlas.cogentco.com (154.54.83.166)  378.425 ms
>     be2272.rcr21.ewr02.atlas.cogentco.com (154.54.83.202)  273.122 ms
> 21  be3792.rcr51.ewr06.atlas.cogentco.com (154.24.61.182)  282.572 ms
>     be3791.rcr51.ewr06.atlas.cogentco.com (154.24.61.178)  307.500 ms
>     be3227.rcr21.ewr06.atlas.cogentco.com (154.24.56.154)  384.237 ms
> 22  te0-0-2-0.nr11.b002887-1.ewr06.atlas.cogentco.com (154.24.13.222)  282.084 ms  380.079 ms  307.143 ms
> 23  38.32.124.50 (38.32.124.50)  307.100 ms  281.801 ms
>     be3227.rcr21.ewr06.atlas.cogentco.com (154.24.56.154)  305.711 ms
> 24  ads-psc-cr02.ezzi.net (72.9.111.106)  307.337 ms
>     te0-0-2-0.nr11.b002887-1.ewr06.atlas.cogentco.com (154.24.13.222)  404.682 ms
>     ads-psc-cr02.ezzi.net (72.9.111.106)  268.941 ms
> 25  72-9-99-137-cust-gw.reverse.ezzi.net (72.9.99.137)  315.487 ms
>     38.32.124.50 (38.32.124.50)  307.062 ms  307.469 ms
> ```

> **https://www.uptrends.com/tools/traceroute**  
> Test: `111.65.56.89`  
> From: Amsterdam
> 
> | Step | Time | Time | Time | Host name                      | IP address      |
> |------|------|------|------|--------------------------------|-----------------|
> | 1    | 12   | <1   | <1   | gateway.as64425.com            | 5.182.210.1     |
> | 2    | 1    | 1    | <1   | ae2-226.RT.IR9.AMS.NL.retn.net | 87.245.246.38   |
> | 3    | 1    | 4    | 3    | ae18-2.RT.TC2.AMS.NL.retn.net  | 87.245.232.122  |
> | 4    | 2    | 1    | 1    | er1.ams1.nl.above.net          | 80.249.208.122  |
> | 5    | 6    | 6    | 6    | ae3.cs1.ams10.nl.eth.zayo.com  | 64.125.31.104   |
> | 6    | 6    | 6    | 6    | ae2.cs1.lhr15.uk.eth.zayo.com  | 64.125.29.17    |
> | 7    | 6    | 6    | 6    | ae1.mcs1.lhr15.uk.eth.zayo.com | 64.125.29.129   |
> | 8    | 6    | 7    | 6    | ae12.mpr3.lhr8.uk.zip.zayo.com | 64.125.27.241   |
> | 9    | 11   | 9    | 6    | ae5.mpr1.lhr8.uk.zip.zayo.com  | 64.125.26.74    |
> | 10   | 6    | 6    | 6    |                                | 213.161.72.106  |
> | 11   | 6    | 6    | 6    |                                | 203.208.178.222 |
> | 12   | 168  | 168  | 168  |                                | 203.208.153.117 |
> | 13   | 168  | 168  | 168  |                                | 203.208.182.253 |
> | 14   | 165  | 165  | 165  |                                | 203.208.192.30  |
> | 15   | 165  | 165  | 165  | ae2-0.qt-cr04.singnet.com.sg   | 202.166.120.202 |
> | 16   | 165  | 167  | 165  |                                | 202.166.119.73  |
> | 17   | 166  | 165  | 166  |                                | 202.166.126.35  |
> | 18   | -    | -    | -    |                                |                 |
> | 19   | -    | -    | -    |                                |                 |
> | 20   | -    | -    | -    |                                |                 |
> | 21   | -    | -    | -    |                                |                 |
> | 22   | -    | -    | -    |                                |                 |
> | 23   | -    | -    | -    |                                |                 |
> | 24   | -    | -    | -    |                                |                 |
> | 25   | -    | -    | -    |                                |                 |

> **macOS traceroute**  
> Test: `5.182.210.1`  
> From: `111.65.56.89`
> ```
> traceroute to 5.182.210.1 (5.182.210.1), 64 hops max, 52 byte packets
>  1  192.168.43.1 (192.168.43.1)  3.107 ms  1.921 ms  1.771 ms
>  2  192.168.213.15 (192.168.213.15)  460.535 ms  408.868 ms  409.485 ms
>  3  10.252.11.141 (10.252.11.141)  37.791 ms
>     10.252.11.137 (10.252.11.137)  18.413 ms
>     10.252.11.141 (10.252.11.141)  36.998 ms
>  4  10.252.11.50 (10.252.11.50)  21.888 ms
>     10.252.11.54 (10.252.11.54)  29.061 ms
>     10.252.11.50 (10.252.11.50)  54.704 ms
>  5  202.166.126.34 (202.166.126.34)  25.735 ms  36.150 ms  22.174 ms
>  6  202.166.119.72 (202.166.119.72)  29.961 ms  27.438 ms  21.996 ms
>  7  202.166.124.37 (202.166.124.37)  30.020 ms  28.767 ms  22.379 ms
>  8  ae6-0.tp-er03.singnet.com.sg (202.166.126.97)  30.053 ms  39.225 ms  29.759 ms
>  9  203.208.191.113 (203.208.191.113)  37.632 ms
>     203.208.145.233 (203.208.145.233)  30.240 ms
>     203.208.191.197 (203.208.191.197)  38.247 ms
> 10  203.208.166.202 (203.208.166.202)  189.991 ms  194.295 ms  181.955 ms
> 11  linx-224.retn.net (195.66.224.193)  181.323 ms  193.664 ms  179.820 ms
> 12  ae0-2.rt.ir9.ams.nl.retn.net (87.245.232.123)  184.498 ms  193.349 ms  190.067 ms
> 13  * * *
> 14  * * *
> 15  * * *
> 16  * * *
> 17  * * *
> 18  * * *
> 19  * * *
> 20  * * *
> 21  * * *
> 22  * * *
> 23  * * *
> 24  * * *
> 25  * * *
> ```

> **https://www.uptrends.com/tools/traceroute**  
> Test: `111.65.56.89`  
> From: Tokyo
> 
> | Step | Time | Time | Time | Host name                                  | IP address      |
> |------|------|------|------|--------------------------------------------|-----------------|
> | 1    | <1   | <1   | <1   | hosted-by.i3d.net                          | 31.204.145.130  |
> | 2    | 1    | <1   | 1    | ce-0-18-0-0.r01.tokyjp08.jp.bb.gin.ntt.net | 61.120.146.13   |
> | 3    | 2    | 1    | 1    | ae-19.r30.tokyjp05.jp.bb.gin.ntt.net       | 129.250.6.130   |
> | 4    | 43   | 43   | 43   | ae-4.r24.tkokhk01.hk.bb.gin.ntt.net        | 129.250.2.51    |
> | 5    | 44   | 44   | 44   | ae-1.r02.tkokhk01.hk.bb.gin.ntt.net        | 129.250.6.92    |
> | 6    | 48   | 48   | 49   | ae-2.a01.newthk03.hk.bb.gin.ntt.net        | 129.250.6.125   |
> | 7    | 44   | 44   | 44   | ae-0.singtel.newthk03.hk.bb.gin.ntt.net    | 129.250.66.154  |
> | 8    | 82   | 79   | 79   |                                            | 203.208.183.250 |
> | 9    | -    | -    | -    |                                            |                 |
> | 10   | 270  | 275  | 283  |                                            | 203.208.192.30  |
> | 11   | 268  | 274  | 283  | ae2-0.qt-cr04.singnet.com.sg               | 202.166.120.202 |
> | 12   | 264  | 270  | 278  |                                            | 202.166.119.73  |
> | 13   | 271  | 277  | 286  |                                            | 202.166.126.35  |
> | 14   | -    | -    | -    |                                            |                 |
> | 15   | -    | -    | -    |                                            |                 |
> | 16   | -    | -    | -    |                                            |                 |
> | 17   | -    | -    | -    |                                            |                 |
> | 18   | -    | -    | -    |                                            |                 |
> | 19   | -    | -    | -    |                                            |                 |
> | 20   | -    | -    | -    |                                            |                 |
> | 21   | -    | -    | -    |                                            |                 |
> | 22   | -    | -    | -    |                                            |                 |
> | 23   | -    | -    | -    |                                            |                 |
> | 24   | -    | -    | -    |                                            |                 |
> | 25   | -    | -    | -    |                                            |                 |

> **macOS traceroute**  
> Test: 31.204.145.130  
> From: 111.65.56.89
> ```
> traceroute to 31.204.145.130 (31.204.145.130), 64 hops max, 52 byte packets
>  1  192.168.43.1 (192.168.43.1)  37.555 ms  1.149 ms  1.120 ms
>  2  192.168.213.15 (192.168.213.15)  192.071 ms  198.165 ms  201.911 ms
>  3  10.252.11.141 (10.252.11.141)  24.713 ms
>     10.252.11.137 (10.252.11.137)  31.677 ms
>     10.252.11.141 (10.252.11.141)  27.437 ms
>  4  10.252.11.50 (10.252.11.50)  29.395 ms
>     10.252.11.54 (10.252.11.54)  30.812 ms
>     10.252.11.50 (10.252.11.50)  28.139 ms
>  5  202.166.126.34 (202.166.126.34)  62.687 ms  16.404 ms  37.963 ms
>  6  202.166.119.72 (202.166.119.72)  22.342 ms  29.455 ms  30.228 ms
>  7  202.166.124.37 (202.166.124.37)  30.077 ms  30.921 ms  29.213 ms
>  8  ae6-0.tp-er03.singnet.com.sg (202.166.126.97)  30.152 ms  28.710 ms  43.837 ms
>  9  203.208.191.197 (203.208.191.197)  27.058 ms  29.371 ms
>     203.208.191.113 (203.208.191.113)  118.212 ms
> 10  203.208.182.249 (203.208.182.249)  31.151 ms  30.129 ms  30.120 ms
> 11  203.208.172.233 (203.208.172.233)  298.040 ms
>     203.208.158.46 (203.208.158.46)  197.035 ms
>     203.208.151.218 (203.208.151.218)  196.937 ms
> 12  203.208.172.234 (203.208.172.234)  233.678 ms
>     ix-xe-0-1-2-0.tcore2.pdi-palo-alto.as6453.net (66.198.144.41)  306.643 ms  381.862 ms
> 13  if-ae-2-2.tcore1.pdi-palo-alto.as6453.net (66.198.127.1)  410.220 ms  483.776 ms  409.623 ms
> 14  if-ae-7-2.tcore2.sv1-santa-clara.as6453.net (209.58.86.73)  511.956 ms * *
> 15  if-ae-0-2.tcore1.sv1-santa-clara.as6453.net (63.243.251.1)  423.147 ms
>     if-et-5-2.hcore1.kv8-chiba.as6453.net (209.58.86.143)  469.997 ms
>     if-ae-0-2.tcore1.sv1-santa-clara.as6453.net (63.243.251.1)  409.583 ms
> 16  * * if-et-6-2.hcore2.kv8-chiba.as6453.net (63.243.250.57)  1364.502 ms
> 17  if-ae-2-2.tcore2.tv2-tokyo.as6453.net (180.87.180.2)  503.912 ms *
>     if-ae-24-2.tcore2.tv2-tokyo.as6453.net (180.87.181.72)  530.407 ms
> 18  if-ae-21-2.tcore1.tv2-tokyo.as6453.net (120.29.217.66)  410.390 ms *
>     180.87.181.70 (180.87.181.70)  323.988 ms
> 19  * * *
> 20  * * *
> 21  * * *
> 22  * * *
> 23  * * *
> 24  * * *
> 25  * * *
> ```


### Question 7

*Describe anything unusual you might observe about the output. Are the same routers traversed in both directions? If no, why might this be the case?*

- One observation is that the uptrends traceroutes could never reach my IP address, and always terminated at `202.166.*.*`.
  - `202.166.*.*` are most likely regional routers hosted by Singtel as seen in the host name **ae2-0.qt-cr04.singnet.com.sg**.
  - The macOS traceroutes shows that `202.166.*.*` is always reached within 5 hops via `10.252.11.*`.
  - This suggests that routers at `10.252.11.*`, which probably belong to local ISPs, have disabled incoming ICMP requests.

- Another interesting observation from the macOS traceroutes is that the first 8 hops to reach **ae6-0.tp-er03.singnet.com.sg** are always the same.
  - This implies that the local network is not dense and has low traffic. That makes sense as the tests were conducted using mobile data at 1am local Singapore time.

- It can also be seen that incoming and outgoing requests at `202.166.119.*` and `202.166.126.*` are strangely routed to address that differ by 1.
  - For instance, in the uptrends traceroutes, incoming ICMP requests are always routed to `202.166.120.202` (**ae2-0.qt-cr04.singnet.com.sg**) -> `202.166.119.73` -> `202.166.126.35` and terminate.
  - However, in the macOS traceroutes, outgoing ICMP requests are always routed to `10.252.11.50` -> `202.166.126.34` -> `202.166.119.72` -> `202.166.124.37` -> `202.166.126.97` (**ae6-0.tp-er03.singnet.com.sg**).
  - It can be inferred that `202.166.126.35`/`202.166.126.34` and `202.166.119.73`/`202.166.119.72` are coupled routers serving as incoming/outgoing regional gateways.
  
- For New York traceroute, the macOS traceroute passed through `154.54.*.*` seven times, whereas the uptrends traceroute only passed through `154.54.*.*` three times.
  - This implies that the incoming traffic at the time was probably greater than the outgoing traffic, which led to higher queue delays and inefficient routing paths.
  - This could be because uptrends hosts a traceroute web service that is available globally, which led to high incoming ICMP traffic from active traceroutes around the world.
  
- For the Amsterdam traceroute, the macOS traceroute was able to reach `87.245.232.123` within 2 hops from `203.208.166.202`.
  - It took 9 hops for the uptrends traceroute to reach `203.208.178.222` from `87.245.232.122`.
  - This implies that the outgoing traffic was higher than the incoming traffic at the time.
  - Perhaps this is due to increased usage during the day in Amsterdam, especially because of increased stay-at-home measures in this COVID-19 season.

- For the Tokyo traceroute, the macOS traceroute failed miserably as it was not able to even reach `129.250.*.*`, where the uptrends traceroute passed through at steps 3 to 7.
  - The macOS traceroute took a long detour to routers in America as seen in the host names **if-ae-2-2.tcore1.pdi-palo-alto.as6453.net** and **if-ae-0-2.tcore1.sv1-santa-clara.as6453.net**.
  - In the process, it incurred long delays of up to 1364.502 ms. This is caused by propagation delay from the packet being sent across the Pacific Ocean.
  - This behavior is likely caused by some misconfigured routers at `203.208.*.*`, which were unable to find `129.250.*.*` successfully.
  - The ICMP packets were rerouted to `66.198.*.*` (**ix-xe-0-1-2-0.tcore2.pdi-palo-alto.as6453.net**), where they passed through Santa Clara and then back across the Pacific Ocean to Chiba, before finally arriving in Tokyo.
  - Even after reaching Tokyo, the traceroute could not resolve to the intended destination and terminated at `180.87.181.70`/`120.29.217.66`.
  - This is very unusual, because in the uptrends traceroute, it only took 8 hops from `31.204.145.130` to reach `203.208.*.*`.
  - One possible explanation is that the network infrastructure in Tokyo is very complicated, such that routers face difficulty in resolving to a given host belonging to another ISP.



















































































