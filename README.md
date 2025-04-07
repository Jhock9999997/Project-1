# Project-1
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

