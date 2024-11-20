---
title: Chapter 3 - Running the UDP Transmit Test
description: This chapter provides instructions for running the Iperf sample.
---


Assuming the host browser is displaying the NetX Duo Iperf Demonstration web page as shown previously and Jperf is running on the host, this chapter describes how to execute each Iperf test.

## Running the UDP Transmit Test

The UDP Transmit Test determines the performance of NetX Duo UDP transmission to the host. In this test, the NetX Duo target is the client and the Jperf host is the server. First, select **Server** and **UDP** in the Jperf IDE. Next, select **Run IPerf!** to initiate the Iperf server, as shown below.

![Running the UDP transmission test.](media/picture3.jpg)

Now, from the NetX Duo Iperf Demonstration web page, select the **Start UDP Transmit Test** button to initiate the test. You should now observe performance statistics in the Jperf IDE and the NetX Duo web page updated, as shown below.

![UDP transmission test stats.](media/Picture4.jpg)

To complete the test, select **here** link on the NetX Duo Iperf Demonstration web page. You should now observe the performance results of the test. In this example, the UDP transmission performance on the NetX Duo target to the Iperf host was 94Mbps on the NetX Duo target, as shown below.

![UDP transmission test results.](media/Picture5.jpg)

## Running the UDP Receive Test

The UDP Receive Test determines the performance of NetX Duo UDP reception on the NetX Duo target. In this test, the NetX Duo target is the server and the Jperf host is the client. First, select **Client** and **UDP** in the Jperf IDE. Next, select **Start UDP Receive Test** on the NetX Duo Iperf Demonstration web page, as shown in the following illustration.

![Running the UDP receive test.](media/picture6.jpg)

Now select **Run IPerf!** from the Jperf IDE and observe statistics in the Jperf IDE, as shown below.

![UDP receive test stats.](media/picture7.jpg)

To complete the test, select the **here** link on the NetX Duo Iperf Demonstration web page. You should now observe the performance results of the test. In this example, the UDP reception performance on the NetX Duo target was 95Mbps, as shown below.

![UDP receive test results](media/picture8.jpg)

## Running the TCP Transmit Test

The TCP Transmit Test determines the performance of NetX Duo TCP transmission to the host. In this test, the NetX Duo target is the client and the Jperf host is the server. First, select **Server** and **TCP** in the Jperf IDE. Next, select **Run IPerf!** to initiate the Iperf server, as shown below.

![Running the TCP transmit test.](media/picture9.jpg)

Now, from the NetX Duo Iperf Demonstration web page, select the **Start TCP Transmit Test** button to initiate the test. You should now observe performance statistics in the Jperf IDE and the NetX Duo Iperf Demonstration web page updated, as shown below.

![TCP transmit test stats.](media/picture10.jpg)

To complete the test, select the ***here*** link on the NetX Duo Iperf Demonstration web page. You should now observe the performance results of the test. In this example, the TCP transmission performance on the NetX Duo target was 91Mbps, as shown below.

![TCP transmit test results.](media/picture11.jpg)

## Running the TCP Receive Test

The TCP Receive Test determines the performance of NetX Duo TCP reception on the NetX Duo target. In this test, the NetX Duo target is the server and the Jperf host is the client. First, select **Client** and **TCP** in the Jperf IDE. Next, select **Start TCP Receive Test** on the NetX Duo web page, as shown.

![Running the TCP receive test](media/picture12.jpg)

Now select **Run IPerf!** from the Jperf IDE and observe statistics in the Jperf IDE, as shown below.

![TCP receive test statistics.](media/picture13.jpg)

To complete the test, select the ***here*** link on the NetX Duo Iperf Demonstration web page. You should now observe the performance results of the test. In this example, the TCP reception performance on the NetX Duo target was 71Mbps, as shown below.

![TCP receive test results.](media/picture14.jpg)
