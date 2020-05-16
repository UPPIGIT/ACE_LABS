Prepare a server to host the ACE Web UI for administering an Integration Node 
1. Now create an Integration Node and create three integration servers which will be owned by it. Open a new ACE Command Console and type:

	mqsicreatebroker MyNode -P 4414
	
	mqsistart MyNode
	
	mqsicreateexecutiongroup MyNode -e MyNodeOwnedServer1
	
Repeat the final command and replace the 'MyNodeOwnedServer1' to create a 'MyNodeOwnedServer2', and 'MyNodeOwnedServer3'.
 
2. We will use a web browser to explore the Web UI in the Run section of this tutorial. 


------------------------------------------------------------------------------------

1. Open your web browser and connect to the URL http://localhost:4414. 
This will show you information about the Integration Node named MyNode, and its three servers MyNodeOwnedServer1, MyNodeOwnedServer2 and MyNodeOwnedServer3 which you created earlier. 
You should see a tile representing each server, which you can click into to display their content, which will be empty to start with until you deploy something,which you might like to try! 

2. In the server view of the Integration Node Web UI, on the right you will find a button labelled 'Create a Server'. You can use this option to create another integration server which is owned by the node

