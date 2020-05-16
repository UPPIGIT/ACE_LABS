Prepare an Integration Server

Other Ref:
https://prasadvadnala.blogspot.com/p/managing-resources-using-rest-api.html?spref=bl

1. From the Integration Explorer view in the bottom left corner of the Toolkit, right-click the Integration Servers folder and select the option to Create a local integration server. Leave all the options with their default values and click Finish. The local Integration Server will be started, using a project in your Toolkit's workspace as the work directory. Click OK to dismiss the Success dialog. Well done, you've just created a standalone Integration Server hosted locally on your own machine! 

2. This step is only required if you are using a Toolkit earlier than version 11.0.0.7. If you're using a Toolkit earlier than version 11.0.0.7, then you will not find a menu option to Create a local integration server. Instead, you will need to open an App Connect Enterprise Command Console and start an integration server in that console session. This alternate option is also detailed in the Knowledge Center. Open an App Connect Enterprise Command Console and start an integration server using the following command:

◦If you are using a Windows platform:

 IntegrationServer --work-dir C:\MyServer --name MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log 

where C:\MyServer is a folder on your file system that the server will use for its working directory.

◦If you are using a UNIX platform: IntegrationServer --work-dir /home/exampleuser/MyServer --name MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log 

where /home/exampleuser/MyServer is a folder on your file system that the server will use for its working directory.

After a few seconds the server should report that it has finished initialization and that its HTTP Listener has started listening for connections. 

3. You can deploy the provided BAR file to your Integration Server using the Toolkit. This technique is described in several other tutorials, but to demonstrate the administrative REST API (which the App Connect Enterprise Toolkit and administrative Web UI both utilise), in this example we will deploy using the following cURL command:

◦If you are using a Windows platform:


 curl -X POST http://localhost:7600/apiv2/deploy --data-binary @C:\YourToolkitWorkspace\ExampleApp\ExampleDeploy.bar -H "Content-Type: application/octet-stream" 

 where C:\YourToolkitWorkspace is the directory location of your App Connect Enterprise Toolkit's workspace.

◦If you are using a UNIX platform: 

curl -X POST http://localhost:7600/apiv2/deploy --data-binary /YourToolkitWorkspace/ExampleApp/ExampleDeploy.bar

 where /YourToolkitWorkspace is the directory location of your App Connect Enterprise Toolkit's workspace.

4. Open up your preferred web browser and go to this URL:
http://localhost:7600/apidocs
You will be presented a page containing comprehensive documentation on the administrative REST API, which can be used to control your Integration Server. 

5. Launch a separate App Connect Enterprise Command Console. Create and start an Integration Node by typing the following commands:

mqsicreatebroker MyNode
 mqsistart MyNode
 mqsilist

The last mqsilist command should return a message like this:

BIP1325I: Integration node 'MyNode' with administration URI 'http://YourHostName:YourPortNumber' is running. 
where YourHostName is your machine's hostname and YouPortNumber is an available port, starting from 4414. 


6. Back in your preferred web browser, open a new tab and go to your URL which will be similar to this:
http://localhost:4414/apidocs

You will be presented a similar documentation page, however there are a few key differences in the actions provided by the administrative REST API when dealing with a node as opposed to a server, which we will explore in the Run stage of the tutorial. 
