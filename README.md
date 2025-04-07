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



