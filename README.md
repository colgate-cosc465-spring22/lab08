# Lab 08: Congestion control simulator 

## Overview
In this lab, you will augment a simulated sender with support for slow start and fast retransmit.

### Learning objectives
After completing this lab, you should be able to:
* Explain how a sender reacts upon timeouts and duplicate ACKs
* Identify periods of additive increase, multiplicative decrease, slow start, fast retransmit, fast recovery, and timeouts on a graph of the congestion window (CWND) over time
* Explain how delay, bandwidth, and queue size impact congestion control

## Getting started
Clone your git repository on the `tigers` servers. 

The `Sender` class in `congestion_control.py` implements an additive increase multiplicative decrease (AIMD) congestion control algorithm. In particular:
* `_recv` increases the congestion window (`CWND`) by `1/CWND` each time new data is acknowledged
* `_timeout` decreases `CWND` to `CWND/2` when a timeout occurs

To run a simulation:
1. Start a server (replacing `PORT` with a random integer > 1024)
    ```bash
    ./server.py -p PORT
    ```
2. In anothe terminal, start a client (replacing `PORT` with the same port number)
    ```bash
    ./client.py -p PORT
    ```
The client will output each data packet it transmits and each ACK it receives. The server will output each line of data it receives. The also client generates and updates the file `cwnd.png` with a graph of `CWND vs time`; the graph is re-generated every 2 seconds. (_Sometimes you need to close and reopen the file to force VS Code to refresh the image preview._)

By default the simulator uses the following settings:
* Bandwidth: 1 packet / second
* Round trip time (RTT): 1 second
* Queue capacity: infinite

You can pass the `-b` and `-d` command-line options on `client.py` and `server.py` to change the bandwidth and RTT, respectively. You can pass the `-q` command-line option to `client.py` to set a fixed queue size. For example, the following commands a simulator with a bandwidth of 10 packets/second, an RTT of 0.5 seconds, and a queue size of 10:
```bash
./server.py -p PORT -b 10 -d 0.5
./client.py -p PORT -b 10 -d 0.5 -q 10
```

As you work on this lab, you may want to consult [Section 6.3](https://book.systemsapproach.org/congestion/tcpcc.html) of _Computer Networks: A Systems Approach_.

## Implement slow start
Your first task is to modify the `Sender` class in `congestion_control.py` to add support for slow start. The modified behavior should only be enabled when `self._use_slow_start` is `True`; otherwise, the client should simply use AIMD.

Hint: You'll need to modify the `__init__`, `_timeout` and `_recv` methods.

To test your code, include the `-s` command-line option when you run `client.py`. You can observe the graph in `cwnd.png` to visually confirm slow-start is correctly implemented.

## Implement fast retransmit and fast recovery
Your second task is to modify the `Sender` class in `congestion_control.py` to add support for fast retransmit and fast recovery. The modified behavior should only be enabled when `self._use_fast_retransmit` is `True`; otherwise, the client should simply use timeouts.

Hint: You'll need to modify the `__init__` and `_recv` methods.

To test your code, include the `-f` command-line option when you run `client.py`. You can observe the log messages in the terminal and the graph in `cwnd.png` to confirm fast retransmit and fast recovery and correctly implemented.

## Submission instructions
When you are done, you should commit and push your modified `congestion_control.py` file to GitHub.
