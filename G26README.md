#													ASSIGNMENT 01

### TEAM DETAILS:

|     NAME              |      USN      |
|-----------------------|---------------|
| 1.R.PRATIKSHA         |  1KS18CS076   |
| 2.SINDU.A.S           |  1KS18CS096   |
| 3.VARIDHI MADHURANATH |  1KS18CS111   |
<br>
### CONTRIBUTION OF EACH TEAM MEMBER:
|     NAME             |      CONTRIBUTION      |
|----------------------|------------------------|
| R.PRATIKSHA          |Implementation of code part and went through various library functions to appraoch the program. |
| SINDU.A.S            |Implementation of UDP Client program and including in the main part of code.Went through nc and tcp dump commands.|
| VARIDHI MADHURANATH  |Error correction and debugging.Tried to implement the program in Java.|
<br>
### INSTRUCTIONS TO EXECUTE THE PROGRAM:

#### Run as:
*On server terminal: nc -l -u -vv -p <server port number>
*On client terminal: ./G26.py -s <server ip address> -c <client ip address> -p <server port num> -P <client port num> -d <data>
*Use tcpdump command to verify the computation of checksum for the data sent from client to server.
<br>
### DETAILS ON EXAMPLE INVOCATION AND OUTPUT:
Example:
#### Client Side:
```bash
$ ./G26.py -s 192.168.0.108 -c 192.168.0.108 -p 5678 -P 5689 -d TESTING
Checksum: 0x1958
The unique code to be appended with data along with the first two characters from input is: V]UQ
Checksum after appending is: 0x1958
(b' TESTINGTESTING\n', ('192.168.0.108', 5678))
```
#### Server Side:
```bash
$ nc -l -u -vv -n -p 5678
Listening on any address 5678
Received packet from 192.168.0.108:60649 -> 0.0.0.0:5678 (local)
TESTING TESTINGTESTING
```
<br>
### DESCRIPTION ON HOW THE PROGRAM WORKS:
* Creates a socket connection between UDP client and server.
* Functions and their description:
    * get_input():
        * Getting command line arguments.
    * int_conv():
        * Converts hex value to num.
    * hex_IP():
        * Uses spilt function to remove '.' from IP address and computes hex value of IP address by splitting into 2 parts.
     * valid_ip():
        * Checks whether the entered IP is valid not not.I snot valid then it exits the program..
    * process_input():
        * Parse's inputs.
        * Converts source prt and dest port to hex.
        * Data is encoded to convert to hex.
        * content list contains the hex values of data in pairs
    * hex_add_final():
        * Adds the sum and rem(carry) obtained form hex_add().
    * onescomplement():
        *Computes the ones complement of the sum obtained from hex_add_final.
    * hex_add():
        * Adds the hex values of src IP,dest IP, UDP protocol(val),length,src port ,dest port and data.
    * checksum():
        * Calculates checksum by adding hex values,wrapping carry around and performing ones complement.
        * Prints the checksum calculated.
    * append_characters():
        * Appends the 6 characters to original data inlcuding the two from input.
    * checksum_append():
        * Checksum of data after appending is calculated.
        * Prints the checksum calculated.
<br>
### CHALLENGES FACED AND HOW DID YOU ADDRESS THESE:
* As the networking concepts are new and weren't familiar we faced a lot of challenges to overcome those by using many resources online and under the guidance of the teacher.
* Adding of hex values was quite challenging and also adding the hex values after appending new characters increased the size of the code.
* Finding one's complement of hex values became difficult hence that part of the code was referred online.
* Learnt parsing of command line arguments.
* The separation of '.' from the IP address part was learnt under the guidance of the teacher.
* The 6 characters which had to be appended was quite challenging.
* Using netcat(nc) tool was a new concept. Became familiar with the implementation and application of netcat(nc) utility tool
* Understanding the problem statement and developing the code for UDP client-server implementation was questioning. 
* Learning about and using the packet capture utility tcpdump was challenging.
* While trying to implement the code many errors were arised and we found it quite difficult dealing with those.
<br>
# RESULT DETAIL AND EXPLANATION:
* The checksum is computed for the data using the following steps:
    * Let us consider an example for data as TESTING
    * And the program is invoked as : ./G26.py -s 10.30.26.1 -c 10.30.26.11 -p 16384 -P 53 -d TESTING
    * STEP 1:
        * Add the hex values of server IP address,client IP address,sever port num,client port num,data,UDP protocol(17) and the length of data i.e., twice.
        * The sum obtained is 0x1c093
        * Hence the carry obtained is 1.
        * This carry shld be wrapped around,hence the sum would become  0xc094
        * The one's complement would be 0x3f6b
        * This is the checksum for original data.
    * STEP 2:
        * 6 characters should be appended such that that checksum would remain the same.
        * In the 6 characters to be appended the first two would be from the inputs i.e., 'TE' and the other two characters are fixed to 'UQ'
        * The other two charcters are calculated such that upon adding the 6 characters the resulting sum should be FFFC.
        * Hence,the remaining two characters would be calculated using,
            * hex_append_data = hex(0xfff3-(0x5445+0x5551))
            * where,
                * 0x5445 is the hex values of TE i.e., the first two characters.
                * 0x5551 is the hex value of UQ.
            * Hence the other two charaters to be appended can be found.
        * Now the data after appending would be TEUQV]TESTING.
        * The computation of checksum would follow the same steps as STEP1 which resuts in 0x3f6b.
    * Hence the checksum matches with the original data and the data after appending 6 characters.
* The netcat(nc) utility tool is used to mimic the server side.
* The data sent by the client side is received and displayed on the server side. 
	* For example,in this case TESTING is the data which will be sent from client to server.
* The server echoes back the content by duplicating it which will be received by the client.
	* The response from the server is typed manually on the terminal by the user
		* For example,in this case the server would send TEESTINGTESTING which will be received by the client.
* The tcpdump command when executed would capture this traffic on the network and will display the UDP checksum which can be used to verify the checksum computed by the program.
