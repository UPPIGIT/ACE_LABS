File Examples:
------------------------------------
scenario 1 :Delimited record detection using the FileInput node

Note : The delimiter can be set to either DOS or UNIX Line End, or Custom Delimiter (Hexadecimal):

Assume the input file in this example has the following content. Each line ends with a line terminator (on Windows this is
X'0D0A', on Unix this is X'0A').

Sample Msg:
<AccNo>12345</AccNo>
<AccNo>34567</AccNo>
<AccNo>56789</AccNo>


Records and elments tab:

Detection :Delimted
Delimente :DOS or UNIX line end
Delimeter type : Postfix

Output :

The FileInput node detects records that end with a DOS or UNIX line end and propagates a message for each one that it finds.
The DOS or UNIX line end is not part of any of the messages. The result is the propagation of three messages, as follows:

Message 1: <AccNo>12345</AccNo>
Message 2: <AccNo>34567</AccNo>
Message 3: <AccNo>56789</AccNo>
-------------------------------------------------------------------------------------------------------------------------------------------------------
Example 2:
Example of reading a file where records are separated by a custom delimiter

Assume the input file in this example has the following content. There is no line terminator at the end of the file.
<AccNo>12345</AccNo>,<AccNo>34567</AccNo>,<AccNo>56789</AccNo>

FileInput node Records and Elements
Detection :Delimted
Delimentr :Custom
Custom delimiter :2C
Delimeter type : infix

Note :The hexadecimal X'2C' represents a comma character in ASCII. On other systems, a different hexadecimal code might need to
be used

Fileoutput node Records and Elements

Record defination :Record is Delimited data

Output:
Message 1: <AccNo>12345</AccNo>
Message 2: <AccNo>34567</AccNo>
Message 3: <AccNo>56789</AccNo>

---------------------------------------------------------------------------------------------------------------------------------

Example 3:

File Input:
UnitedKingdom,USA,Germany,France,India

Output:
<Message><RecordNumber>1</RecordNumber><RecordData>UnitedKingdom</RecordData></Message>
<Message><RecordNumber>2</RecordNumber><RecordData>USA</RecordData></Message>
<Message><RecordNumber>3</RecordNumber><RecordData>Germany</RecordData></Message>
<Message><RecordNumber>4</RecordNumber><RecordData>France</RecordData></Message>
<Message><RecordNumber>5</RecordNumber><RecordData>India</RecordData></Message>

	SET OutputRoot.XMLNSC.Message.RecordNumber=InputLocalEnvironment.File.Record;
	SET OutputRoot.XMLNSC.Message.RecordData=CAST(InputRoot.BLOB.BLOB AS CHARACTER CCSID InputProperties.CodedCharSetId);
------------------------------------------------------------------------------------------------------------------------------------------------------		

Example 4:

Transforming ws request messages to a file

The three HttpInput nodes (which have the Message Domain property set to XMLNSC) propagate messages to a Compute node.
Because the FileOutput node is receiving input from three sources, a message needs to be sent to the Finish File terminal to
close the file. 
It doesn't matter what the content of this message is, as long as the Finish File terminal receives a message.The file is then closed and moved from the mqsitransit directory to the output directory.

msg1:
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>

msg2:
<Dept><DeptId>124513</DeptId><Sale>44.99</Sale><ItemCode>54852744</ItemCode></Dept>

msg3:

<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>

Output:

<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>

code:

SET OutputRoot =InputRoot;
		BEGIN ATOMIC 
			SET source_count =source_count + 1;
		END;
		
		IF source_count =3 THEN

			PROPAGATE TO TERMINAL 'out' DELETE NONE;
			PROPAGATE TO TERMINAL 'out1' DELETE NONE;
			
			SET source_count =0;
			
		ELSE
			
			PROPAGATE TO TERMINAL 'out' DELETE NONE;
			
		END IF;
		
		RETURN FALSE;

----------------------------------------------------------------------------------------------------------------------------------------------------

Example 5:

Parsed Record Sequence

Input file :
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>
<Dept><DeptId>124512</DeptId><Sale>29.99</Sale><ItemCode>24873586</ItemCode></Dept>
<Dept><DeptId>124513</DeptId><Sale>44.99</Sale><ItemCode>54852744</ItemCode></Dept>
<Dept><DeptId>124514</DeptId><Sale>99.99</Sale><ItemCode>85222843</ItemCode></Dept>
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>


Output:

file1:
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>
file2:
<Dept><DeptId>124512</DeptId><Sale>29.99</Sale><ItemCode>24873586</ItemCode></Dept>
file3:
<Dept><DeptId>124513</DeptId><Sale>44.99</Sale><ItemCode>54852744</ItemCode></Dept>
file4:
<Dept><DeptId>124514</DeptId><Sale>99.99</Sale><ItemCode>85222843</ItemCode></Dept>
file5:
<Dept><DeptId>124511</DeptId><Sale>34.99</Sale><ItemCode>19449539</ItemCode></Dept>

	SET OutputRoot = InputRoot;
		DECLARE now CHARACTER ;
		SET now =CAST(CURRENT_TIMESTAMP AS CHARACTER FORMAT 'yyyyMMdd-HHmmss') ;
		SET OutputLocalEnvironment.Destination.File.Directory='C:\Users\Shrisha\Desktop\FileDemo\MultipleFIles';
		SET OutputLocalEnvironment.Destination.File.Name='File_'||'_'||now;
		
-----------------------------------------------------------------------------------------------------------------------------------------------------

examle 6:

file input : Binary output :xml

use cpy file for msget.

check : parsed detection and fixed length

code;

	CALL CopyMessageHeaders();
		-- CALL CopyEntireMessage();
		CALL CopyEntireMessage();
		SET OutputRoot.Properties.MessageFormat='XML1';

----------------------------------------------------------------------------------------------------------------------------------------------------------
DO below changes to above any example or create new flow

Example 7:

FTP demo :

Read file from remote server and write to remote server

Prereq:

AWS:

1. Lunch AWS Linux ec2 instance .Open Inbound access to all
2. SSH to ec2

switch to root user
passwd ec2-user
enter new password
sudo su             //to access as root.
yum update -y       //to update your server to latest stable release
yum install vsftpd  // to install the ftp plug-ins.

sudo vi /etc/vsftpd/vsftpd.conf

Then add the following lines to the bottom of the vsftpd.conf file:
pasv_enable=YES
pasv_min_port=1024
pasv_max_port=1048
pasv_address=<Public IP of your instance>

Open command propmt 

C:\Users\Shrisha>ftp <Public IP of your instance>
Connected to 54.152.57.131.
220 (vsFTPd 3.0.2)
User (54.152.57.131:(none)): ec2-user
331 Please specify the password.
Password:
230 Login successful.
ftp> mkdir FTPInput
257 "/home/ec2-user/FTPInput" created
ftp> cd FTPInput
250 Directory successfully changed.



Setting a security identity for the broker

C:\Program Files\IBM\ACE\11.0.0.8>mqsisetdbparms MyNode -n ftp::myFtpIdentity -u
 ec2-user -p Aadhya@123
BIP8071I: Successful command completion.


Set FTP Properties to FTP nodes.

Chek Remote FTP Transfer

server and port :  <Public IP of your instance>:21
securityIdentity : myFtpIdentity
server directory :/home/ec2-user/FTPInput  for out put node /home/ec2-user/FTPOutput 

Note Create FTPInput and FTPOutput folders on remote server

------------------------------------------------------------------------------------------------------------------------------------------------









 








