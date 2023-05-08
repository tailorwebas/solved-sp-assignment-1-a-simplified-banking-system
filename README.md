Download Link: https://assignmentchef.com/product/solved-sp-assignment-1-a-simplified-banking-system
<br>
Problem Description

In assignment 1, you are expected to implement a simplified “​<strong>Banking System</strong>​”. You need to complete the simple servers which we already handled a lot of works such as internet connection, memory allocation, etc. The provided source code can be compiled and run as very simple read/write servers, which can only serve one request per time. Things you need to do is to modify it so that it can support ​<strong>I/O multiplexing</strong>​, deal with ​<strong>many requests at the same time</strong>​, rather than be blocked by only one request, and to action on the same account by using different servers.

To summarize, there are​<strong> three</strong>​ tasks for you:

<ol>

 <li>Use ​<strong>select() to do a multiplexing banking system</strong>​. There may be multiple clients/accounts that connect to servers and send request at the same time.</li>

 <li>Please use​<strong> file lock </strong>​to guarantee the correctness when there are multiple read/write requests occurring on one account at the same time.</li>

 <li>Implement the ​<strong>Account structure</strong>​. The structure of an account is a struct in C which have two attributes : id and balance. Each of them belongs to type int. There is a file called ​<strong><em>account_list </em></strong>​including 20 continuous account structures with id from 1~20, and randomly specified price at beginning.</li>

</ol>

​ typedef  struct{ int    id; int    balance;

} Account;

2.How to Run the Sample Servers

<strong> </strong>Compile

To produce execution files, you have to compile ​<em>server.c </em>​. If you take a look on makefile, you will see that the execution file write_server is compiled directly. However, if you computing it with​<em> -D READ_SERVER,</em>​ the execution file <em>read_server </em>​will be produce. On the first attempt, you can type the command, make, after extracting the files on the workstation.

Run

$ ./read_server {port_num}

(Take read_server for example, where {port_num} is the port you want assign to the server.)

3.How to Test Servers at Client Side

<strong> </strong>Use telnet to connect to your servers

$ telnet  {host}  {port_num}

Where {host} is the location of your servers run, i.e., linux1.csie.ntu.edu.tw. Note that the {port_num} must be the same with one you run the read/write server. If you’re testing the sample server, you can type anything, pressing enter and the content of your input will immediately send back to you. However, if you’re testing your banking system, there may be different cases described below. After connecting to the read server, you need to send an account-id to query the details of the correspondings account. And then type single enter to indicate the end of the input. There have​<strong> two </strong>​different situations.

Case 1 –  You can query this account

The banking system should return the details of the account and the connection will be closed.

Case 2 –  Anyone else is changing this account

The banking system should return “This account is locked!” and the connection will be closed.

Connect to a writer server

On the other hand, if you connect to a write server, you should send an account-id to specify the account that you want to change at the first line. You might get​<strong> two</strong>​ different types of response immediately.

Case 1 –  You get the right to do some action on this account

The banking system should return “This account is modifiable.” Next, you should type an action command, i.e., save, withdraw, transfer or balance. There are four actions which can be done by the banking system.

<strong>save</strong>​<strong>:</strong>​ ​save the money into account. The balance of the account should be

increased. For example, “save 20”.

<strong>withdraw</strong>​<strong>: </strong>​withdraw money from account. The balance of the account

should be decreased. For example “withdraw 20” <strong>transfer: </strong>​Transfer money from account A to account B. The balance of

account A should be decreased while the balance of account B should be increased. There are two integers in this command. The first integer is the id of account B and the second integer is the money that need to be transfered. For example, “transfer 1 20”.

<strong>b</strong>​<strong>alance</strong>​<strong>: </strong>​Set the account balance to a specify value.

If you succeed in changing the details of the account, the connection will be closed.

Case 2 –  You get the right to do the actions but you failed




The banking system should return “This account is modifiable.” Next, if you specify an invalid command. The operation will fail and the banking system should return “Operation failed.” And then the connection will closed. Below shows the definition of invalid command in each command:

<strong>save:</strong>​ The integer in this command should be bigger than or equal to 0. Otherwise, it is an invalid command. For example, “save -20” is invalid command and “save 0” is a valid command.

<strong>withdraw: </strong>​The integer in this command should be bigger than or equal to 0 and less than or equal to the balance of the account . Otherwise, it is an invalid command. For example, if the balance of the account is 100, then “withdraw -100” and “withdraw 200” are invalid commands while “withdraw 0” and “withdraw 100” are valid command.

<strong>transfer: </strong>​The second integer should be bigger than or equal to 0 and less

than or equal to the balance of the account A. Otherwise, it is an invalid command. For example, if the balance of account A is 200, then “transfer 1 201” and “transfer 1 -100” are invalid commands. “transfer 1 0” and “transfer 1 200” is valid commands. You can assume the first integer is always valid and the account A and account B does not have same account id.

<strong>balance:</strong>​ The specify value should greater than 0. Otherwise, it is an

invalid command. For example, balance 100 is valid command and balance -100 is invalid command.

Case 3 –  Anyone else is changing the same account

For all the command, make sure there is no anyone else is changing the same account. In transfer command, you can always assume that there is no anyone else changing the account B. Otherwise, the banking system should return “This account is locked.” and the connection will be closed.

4.Input and Output format

The following text with [standard input] is entered by user, while [standard output] is printed by the program. Suppose the host is at ​<em>linux1.csie.org </em>

Read

Server Side

$ ./read_server 12345

<em>[standard output] starting on linux1, port 12345, fd 3, maxconn 65536 </em>

Client Side

<em>$ telnet linux1.csie.ntu.edu.tw 12345 </em>

<em>[standard output] Trying 140.112.30.32….. </em>

<em>[standard output] Connected to linux1.csie.ntu.edu.tw. </em>

<em>[stabdard output] Escape character is ‘^]’. </em>

Case 1 –  You can query this account

<em>[standard input] 2 </em>

<em>[standard output] 2 4108 </em>

<em>[standard output] Connection closed by foreign host. </em>

Case 2  –  Anyone else is changing this account

<em>[standard input] 2 </em>

<em>[standard output] This account is locked. </em>

<em>[standard output] Connection closed by foreign host. </em>

Write

Server Side

$ ./write_server 12346

<em>[standard output] starting on linux1, port 12346, fd 3, maxconn 65536</em>

Client Side

<em>$ telnet 140.112.30.32 12346 </em>

<em>[standard output] Trying 140.112.30.32….. </em>

<em>[standard output] Connected to linux1.csie.ntu.edu.tw. </em>

<em>[standard output] Escape character is ‘^]’.           </em>

<em> </em>Case 1  –  You get the right to do some action on this account

<em>[standard input] 2 </em>

<em>[standard output] This account is modifiable </em>

<em>[standard input] save 200 </em>

<em>[standard output] Connection closed by foreign host.      </em>

Case 2  –  You get the right to do the actions but you failed

(a)save

<em>[standard input] 2 </em>

<em>[standard output] This account is modifiable. [standard input] save -200 </em>

<em>[standard output] Operation failed. </em>

<em>[standard output] Connection closed by foreign host. </em>

(b)withdraw

<em>[standard input] 2 </em>

<em>[standard output] This account is modifiable. [standard input] withdraw 200000 </em>

<em>[standard output] Operation failed. </em>

<em>[standard output] Connection closed by foreign host. </em>

(c)transfer

<em>[standard input] 2 </em>

<em>[standard output] This account is modifiable. </em>

<em>[standard input] transfer 1 -100 </em>

<em>[standard output] Operation failed. </em>

<em>[standard output] Connection closed by foreign host. </em>

Case 3 –  Anyone else is changing the same account

<em>[standard input] 2 </em>

<em>[standard output] This account is locked. </em>

<em>[standard output] Connection closed by foreign host. </em>

















