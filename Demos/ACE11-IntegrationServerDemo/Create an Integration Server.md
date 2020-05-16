Create an Integration Server


1. From the Integration Explorer view in the bottom left corner of the Toolkit, right-click the Integration Servers folder and select the option to Create a local integration server. 
 Leave all the options with their default values and click Finish.
 The local Integration Server will be started, using a project in your Toolkit's workspace as the work directory. Click OK to dismiss the Success dialog. 
 Well done, you've just created a standalone Integration Server hosted locally on your own machine!


2. This step is only required if you are using a Toolkit earlier than version 11.0.0.7. If you're using a Toolkit earlier than version 11.0.0.7, then you will not find a menu option to Create a local integration server. 
 Instead, you will need to open an App Connect Enterprise Command Console and start an integration server in that console session
 
 Open an App Connect Enterprise Command Console and start an integration server using the following command:
-----------------------------------------------------------------------------------------------------------------------------------------------
 ◦If you are using a Windows platform: 
 IntegrationServer --work-dir C:\MyServer 
				  --name MyServer 
				  --admin-rest-api 7600 
				  --http-port-number 7900 
				  --console-log 
				  
 where C:\MyServer is a folder on your file system that the server will use for its working directory.
---------------------------------------------------------------------------------------------------------------------------
◦If you are using a UNIX platform: 
IntegrationServer --work-dir /home/exampleuser/MyServer 
				--name MyServer --admin-rest-api 7600 
				--http-port-number 7900 
				--console-log 
				
				
where /home/exampleuser/MyServer is a folder on your file system that the server will use for its working directory. 

------------------------------------------------------------------------------------------------------------------------------------

After a few seconds the server should report that it has finished initialization and that its HTTP Listener has started listening for connections. Well done, you've just created a standalone Integration Server hosted locally on your own machine!

-------------------------------------------------------------------------------------------------------------------------------------

Create a second Integration Server which is owned by an Integration Node 
1. Open a new App Connect Enterprise Command Console and create an integration node using the following command:
	
	mqsicreatebroker MyNode 

2. Start the Integration Node using the following command:
	
	mqsistart MyNode

3. Create an Integration Server which is owned by the Integration Node using the following command:
	
	mqsicreateexecutiongroup MyNode -e MyNodeOwnedServer

4. Make a note of the administration port which the Integration Node has opened using the mqsilist command.
The response should report something like:

BIP1325I: Integration node 'MyNode' with administration URI 'http://YourHostName:4414' is running.

---------------------------------------------------------------------------------------------------------------------------------