Connect your App Connect Enterprise Toolkit to your Integration Servers

Connect to the Integration Server MyServer 
1. The steps in this section are only needed if you are using a Toolkit earlier than version 11.0.0.7. If you created a local integration server in your Toolkit then it is already displayed to you without you needing to explicitly connect to it. 
2. In the ACE Toolkit, find the Integration Explorer view. By default, this is located directly below the Application Development tab in the bottom left corner. 
3. Right click on the Integration Servers label and select Connect to an Integration Server 
4. Enter the Host Name as localhost and the port as 7600. Remember this is the '--admin-rest-api' port which was used when starting the server. Click Finish 
5. Your server labelled MyServer should appear with a green arrow showing that the server is currently running. 

Connect to the Integration Node MyNode 
1. If you are using App Connect Enterprise v11.0.0.5 or a later fix pack, then local Integration Nodes defined on the same machine as your Toolkit will already be displayed automatically! If the runtime you are using is on a different machine to your Toolkit, or if you are on App Connect Enterprise v11.0.0.4 or earlier then you will need to follow the next three instructions to connect to an integration node. 
2. Still in the Integration Explorer view in the ACE Toolkit, right click on the Integration Nodes label and select Connect to an Integration Node 
3. Enter the Host Name as localhost and the port number which was reported to you using the mqsilist command which you ran in the Prepare section. 
4. Your node labelled MyNode, and its server labelled MyNodeOwnedServer should appear with green arrows showing that they are currently running. 

Deploy a simple message flow to your server 
1. Locate the Broker Archive file named SimpleDeploy.bar inside the Application project which you imported earlier named SimpleApp, then drag and drop it onto your standalone server named MyServer. 
2. Try testing the message flow SimpleFlow by sending it data using an HTTP client such as cURL: 
curl -X POST http://localhost:7900/Echo -d HelloWorld
