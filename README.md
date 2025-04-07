![image](https://github.com/user-attachments/assets/6ee7d0da-32f9-49c9-b607-b0930a63c5bc)# Project-1
In this project I took on the role of a network administrator to investigate the situation, analyze the potential cause, suggest solutions, and outline the limitations of the solution. At the end I provided a detailed report on my findings. 

#Scenario
As a network administrator working for XYZ company, your manager has received reports that the company’s website is not working. Users attempting to access the website report that they receive a ‘page cannot be displayed’ error in their browser. Your manager asks you to investigate the situation, analyse the potential cause or causes of the website service disruption, suggest solutions, and outline the limitations of the solution. They have also asked that you provide a detailed report of your findings.
You have begun to look into the situation and confirm the interrupted service issue by visiting the website from your browser—the error message is displayed. Following this, you decide to perform some basic troubleshooting. You attempt to visit the website using the IP address 192.168.170.8 directly with no success.
Based on this preliminary investigation, you suspect that there may be a problem with the Web Server and you determine that the server has been subject to some form of attack. You investigate further so that you understand what happened and can suggest appropriate solutions.

# Task 1
1. The normal process when accessing a webpage, I have divided this task into three distinct steps.

   Step 1: Create a local server
![image](https://github.com/user-attachments/assets/720d279f-9889-4355-a4b8-dd02a4de0349)
When accessing this URL, it displays “Hello World” in the browser. The objective of this step is to capture packets exclusively from the Loopback interface and exclude any irrelevant packets.
 Step 2: Access the local server on Chrome browser
 After pressing the Enter key on the address bar, the “Hello World” message was successfully printed,indicating the successful establishment of access to the local web server
![image](https://github.com/user-attachments/assets/b9054c62-39d2-4b6d-9cd3-830421c0fbd7)

Step 3: Analyse the captured packets on Loopback interface
![image](https://github.com/user-attachments/assets/023de4c5-98cc-4087-98e6-4700cf8fdada)

*Establishment of TCP connection on client port 62310
On Seq No.1: The client sends a SYN packet to initiate a connection which includes an initial sequence 
number. 
On Seq No.2: The server responds with SYN-ACK to acknowledge the client's sequence number and 
includes a new sequence number.
On Seq No.3: The client sends an ACK to the server.
 *HTTP Request
 On Seq No. 5: The client sends sends GET /index to the server.
 *HTTP Response

 
 On Seq No. 7: The server responds with "Hello World", following the som ACK TCP packet to ensure delivery.
*Termination of TCP connection
 On Seq No. 13: The server responded with FIN-ACK to end the connection.
 On Seq No. 14: The client confirms with ACK to termination. 

** Abnormal behaviours identified
![image](https://github.com/user-attachments/assets/da9bbbbd-74b0-44b4-abd2-930acc088f4c)
As the screenshot suggests, there are 3 abnormal behaviours which is suspicious and identified,
-> Incrementing source ports
 The varied source IP addresses with port increments from 2430 to 2449 all the way by the difference of 1, this is rare to see in the normal situation. And this could be led by some scripts or tools running on the client side, in iterative way to configure the proxy source IP ports.
-> Destination port is always 0
![image](https://github.com/user-attachments/assets/8ff68c7f-59fa-40ca-86eb-5f84eda3ac55)
The request with same destination IP address which is 192.168.170.8 where its destination port is always 0, in a normal way, port 0 on the system is reserved and not used for application-layer communication. While there are massive of requests configured with destination port 0, only malformed packets can have such pattern, which means these requests could be done by intentionally designed and configured, so that the server could not respond but still waiting for reply.
 There is only SYNC packet, the client never gets replied
 ![image](https://github.com/user-attachments/assets/b43ba0d8-476e-450a-95f2-842a2f0361ad)
The SYNC packet is the first step to establish connection with server, while with destination port 0, this means the handshake could never be done, instead, this will consume the available resources on the server and the other valid requests could not be handled correctly.

** ANALYSIS of compromised security issues and goals
-> Denial of Service attack
![image](https://github.com/user-attachments/assets/7cb8286d-7f28-4136-b213-f4d78a246ef0)

-> Exhausted server resources

Regarding the comprised security objectives, the following compromised goals have been identified
-> TCP packet integrity
-> Server response availability 
-> Port usability testing

 Addressing the SYN flood attack requires a combination of preventative, detective, and corrective strategies. Preventative measures, such as enabling HTTPS and restricting access to essential ports, strengthen the system. Detective strategies like attack detection systems and traffic alerts ensure suspicious activity is quickly identified. Corrective actions, including IP blocking and terminating malicious connections, minimize the impact of ongoing threats. Together, these strategies create a layered defence, enhancing the network's resilience and safeguarding it from both current and future threats

****Task 2: Network Performance Evaluation***
This task involves analyzing an ns-3 trace file generated from a simulation of a four-node network: n0 (source), n1 (router), and n2/n3 (sinks). Traffic flows include TCP segments from n0 to both n2 and n3, with payload sizes of 50 bytes. Node IPs are:
n0: 192.168.1.1
n1: 192.168.1.2 / 192.168.2.1 / 192.168.3.1
n2: 192.168.2.2
n3: 192.168.3.2
I have chosen n0 as the source and n3 as the sink for this analysis.
The following performance metrics are calculated using grep and awk in Linux:
Round trip delay over time
Minimum round trip delay
Average round trip delay
Maximum round trip delay
Although I am unable to attach the ns-3 trace file, the following shows my full working and calculations.

![image](https://github.com/user-attachments/assets/24d05348-b213-4934-adf9-18219a2119da)

Node n0 is a source node. Nodes n2 and n3 are sink nodes. Node n1 is a router.  
The network traffic flows are as follows:
 n0 => n2 TCP segments (payload size = 50 bytes)
 n0 => n3 TCP segments (payload size = 50 bytes)
 The IP addresses for the four nodes in the network are configured as follows:
 n0: 192.168.1.1
 n1: 192.168.1.2 and 192.168.2.1 and 192.168.3.1
 n2: 192.168.2.2
 n3: 192.168.3.2
 Data preparation using supplied trace file
 ** Step 1 - Extract relevant data for analysis from the trace file.
 As we are measuring round trip performance from n0 to n3, we can extract the relevant trace file packets using a combination of the source node identifier for n0 (NodeList/0/) and the destination IP address for n3 (192.168.3.2).
 grep "NodeList/0/" Assignment-1.tr | grep "192.168.3.2" > 
Assignment3_Task2_n0_n3_nodelist0.tr
** Step 2 - Retrieve the enqueued packets from Assignment3_Task2_n0_n3_nodelist0.tr and write to a new file 
round_trip1.tr
 grep ^"+ " Assignment3_Task2_n0_n3_nodelist0.tr > round_trip1.tr
 ** Step 3 - Retrieve the received ACK packets from Assignment3_Task2_n0_n3_nodelist0.tr and append to previous file round_trip1.tr
 grep ^"r " Assignment3_Task2_n0_n3_nodelist0.tr >> round_trip1.tr
 ** Step 4 - Sort round_trip1.tr by packet id number (field 19) and write to new file round_trip2.tr
 sort -gk 19 round_trip1.tr > round_trip2.tr
 ** Step 5 - From round_trip2.tr, calculate the difference (d) between field one receiving ACK (r) and enqueue time (+) in seconds and write the results to field three for each packet (field 19) to new file round_trip3.tr
 awk '$1~/[+]/{tx=$2; idt=$19} $1~/^r/{rx=$2; idr=$19; if(idt==idr) d=rx-tx; $1=$1 " (d= " d  
" )"} 1' round_trip2.tr > round_trip3.tr
 ** Step 6 - Extract the received packets containing the results of this difference calculation from round_trip3.tr to a new file round_trip4.tr 
grep ^"r " round_trip3.tr > round_trip4.tr
** Step 7 - Save a copy of round_trip4.tr as a text file round_trip4.txt to use as an input dataset for gnuplot
 cp round_trip4.tr round_trip4.txt
 **Round Trip Delay Over Time**
 ** Step 1 - Sort round_trip4.tr by elapsed time (field five) and write to new file rtdot.tr
 sort -gk 5 round_trip4.tr > rtdot.txt
 ** Step 2 - Using rtdot.txt as the input file for gnuplot, plot the Round Trip Delay Over Time using field five (elapsed time) as the X-axis and field three (round trip delay) as the Y-axis value

![image](https://github.com/user-attachments/assets/25ee4a75-4a2c-4379-a382-7fa2f151163e)

**Minimum Round Trip Delay**
Step 1 - Using round_trip4.tr as the input file, use cat and awk to find the minimum value of field three (difference)
 cat round_trip4.tr | awk 'min>$3 || NR==1{ min=$3 } END{ print min }'
 = 0.01264 seconds
 Step 2 - Using round_trip4.txt as the input file for gnuplot, plot the Round Trip Delay using field 22 (Packet ID) as the X-axis and field three (round trip delay) as the Y-axis value.
 ![image](https://github.com/user-attachments/assets/12cd9128-f758-440e-ba8a-4376b11244cc)

**Average Roound Trip Delay**
** Step 1 - Using round_trip4.tr, add all the difference values in field three to get a total round trip time for all packets (in seconds) and print the total to screen.
 cat round_trip4.tr | awk '{total+=$3} END {print total}'
 Total seconds = 9969.32
** Step 2 - Using round_trip4.tr, count the number of received packets using field one (r) and print to screen.
 grep -c ^"r " round_trip4.tr
 Total packets = 1792
** Step 3 - Using the two figures calculated above, perform the following calculation to calculate the average 
round trip delay:
 Total seconds / total packets = seconds Average Round Trip Delay
 9969.32 / 1792 = 5.5632366 seconds Average Round Trip Delay

 **Maximum Round Trip Delay**
 ** Step 1 - Using round_trip4.tr as the input file, use cat and awk to find the maximum value of field three (difference)
 cat round_trip4.tr | awk 'max<$3 || NR==1{ max=$3 } END{ print max }'
 = 15.2163 seconds
 ** Step 2 - Using round_trip4.txt as the input file for gnuplot, plot the Round Trip Delay using field 22 (Packet ID) as the X-axis and field three (round trip delay) as the Y-axis value. Note that this provides the same result as for the Minimum Round Trip Display, so that plot is also referenced here.

![image](https://github.com/user-attachments/assets/ef9f33cd-0736-4f55-b420-c8c5ae00f3bb)
