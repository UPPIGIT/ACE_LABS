Exercise the administrative REST API

Interact with the Integration Server: 
------------------------------------------------------------------------------------------------------------------------------------------
1. In the web browser tab for your Integration Server (http://localhost:7600/apidocs), on the left side you will see all the available actions in the API. In this part of the tutorial we will view the details for this server, view details about the deployed application, stop the deployed application and then start it again; all from within the interactive documentation. 

2. Click on the GET /apiv2 tab in the list of actions. The body of the docs should change to present you with all the details on this particular action. You'll be presented with the description of the action, its parameters (and their structure), example requests from different languages (curl, python etc.) and examples of the standard responses. At the top of the web page, select the 'Try it' tab. 

3. Here you can run the action against your server as an example. Don't change anything from the defaults, and just click 'Send' in the bottom right. You will see the real request to your server and its response. 

4. Scroll down and inspect the JSON response, and you will see a few sections:
 - The name and type of your server at the very top
 
 - A section called 'actions' which shows you all the valid actions which can be conducted on the server
 - Further sections showing content which has been deployed to the server, such as 'applications', 'restApis', 'services' and 'policies', with details on whether they contain any further children. 
 
5. Run a GET /apiv2/applications to view the currently deployed applications. You will see the application which you deployed earlier, named 'ExampleApp' 

6. Run a GET /apiv2/applications/{application} (changing the Application parameter to be 'ExampleApp') to view details about a particular deployed application. 

7. Run a POST /apiv2/applications/{application}/stop (changing the Application parameter to be 'ExampleApp') to stop the ExampleApp application. 

8. Run a POST /apiv2/applications/{application}/start (changing the Application parameter to be 'ExampleApp') to start the ExampleApp application. 

--------------------------------------------------------------------------------------------------------------------------------------------------------------

Interact with the Integration Node: 

1. In the web browser tab for your Integration Node (http://localhost:4414/apidocs) for your integration node, on the left side you will see all the available actions in the API. In this part of the tutorial we will create a server under the node, view the server's properties and then delete the server; all from within the interactive documentation. 

2. Navigate to GET /apiv2/servers and proceed to the 'Try it' section. Send the request to the Integration Node and you will receive a response, showing that there are currently no child Integration Servers. 

3. Navigate to POST /apiv2/servers and proceed to the 'Try it' section. In the Body field use the following JSON to specify the name of an integration server which will be created: {"name":"MyNodeOwnedServer"}. Click 'Send' and then run another GET /servers to see the updated list of servers. 

4. Navigate to GET /apiv2/servers/{server} and the 'Try it' section. Copy the name MyNodeOwnedServer of your newly created server into the 'Path' field and click 'Send'. The response at the bottom of the page will show all the properties of the newly created server. 

5. Navigate to POST /apiv2/servers/{server}/stop and the 'Try it' section. Copy the name MyNodeOwnedServer of your newly created server into the 'Path' field and click 'Send'. This may take a while, so please be patient. 

6. Navigate to DELETE /apiv2/servers/{server} and the 'Try it' section. Input the name of the stopped server, MyNodeOwnedServer and click 'Send' to delete it. 

7. Run one last GET /apiv2/servers request to check that the server has been successfully deleted. 
