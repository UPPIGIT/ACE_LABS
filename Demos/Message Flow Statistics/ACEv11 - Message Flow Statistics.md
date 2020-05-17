

Getting started with ACEv11 - Message Flow Statistics

OVERVIEW 


This tutorial shows how to turn on message flow statistics and then view the statistics information in real-time using the ACE administrative Web UI. This tutorial uses a simple message flow deployed to an integration server in order to demonstrate these concepts. 

A configuration file, known as 'server.conf.yaml', is used to control the integration server's statistics settings. In this example we will only focus on the statistics settings in this file, but many other options are available in order to control the server's configuration. 

Once an integration server has been created with statistics enabled, a simple flow will be deployed. A few messages will be sent to the message flow over HTTP, before launching the product's administrative Web UI where the statistics can be viewed in real-time. 

------------------------------------------------------------------------------------------------------------------------------------------------------------------
Prepare an Integration Server


1. Open an App Connect Enterprise Command Console and run the mqsicreateworkdir command to create a working directory which will be used to store the files which the runtime integration server needs:

◦If you are using a Windows platform: 

mqsicreateworkdir C:\StatsServer

where C:\StatsServer will be the working directory for your integration server.

◦If you are using a UNIX platform:
 
 mqsicreateworkdir /home/exampleuser/StatsServer
 
 where /home/exampleuser/StatsServer will be the working directory for your integration server.

2. Navigate to your server's working directory (C:\StatsServer in Windows and /home/exampleuser/StatsServer on UNIX) and locate the file server.conf.yaml. Open it using your preferred text editor, locate the Statistics section and edit the settings (the comments in the sample help with allowed values, and you will need to remove some # comment characters). Depending on the precise version of App Connect Enterprise v11 which you are using you will find that these settings are slightly different. In version 11.0.0.8 for example, the settings were changed to have statistics turned on by default:
Statistics: 
  Snapshot: 
    accountingOrigin: 'basic' 
    nodeDataLevel: 'advanced' 
    outputFormat: 'json' 
    publicationOn: 'active' 
    threadDataLevel: 'basic' 
 Save and close the file. 

3. Back in the App Connect Enterprise Command Console window, start the server:
◦If you are using a Windows platform: IntegrationServer --work-dir C:\StatsServer --name StatsServer --admin-rest-api 7600 --http-port-number 7800 --console-log
◦If you are using a UNIX platform: IntegrationServer --work-dir /home/exampleuser/StatsServer --name StatsServer --admin-rest-api 7600 --http-port-number 7800 --console-log

4. In the App Connect Enterprise Toolkit, locate the Integration Explorer view (by default this is located in the bottom left corner), right click on the Integration Servers label and select Connect to an Integration Server. Enter the Host Name as localhost and the port as 7600 and click Finish. 

5. Locate the Broker Archive file named StatsBar.bar inside the Application project which you imported earlier named StatsApp, then drag and drop it onto your server named StatsServer. 

6. Right click on the server in the Toolkit and select Start Web User Interface 
-------------------------------------------------------------------------------------------------------

Follow these steps to complete the tutorial

Send some messages to the message flow and view statistics in the Web User Interface 

1. Navigate to a Command Prompt and run this command a handful of times:
curl -X POST http://localhost:7800/request -d HelloWorld
Each execution should receive the same fixed reply message:
{"OutputMessage":"This is a fixed reply message!"} 

2. Return to the administrative WebUI for the server which you opened in the Prepare stage. 

3. When you first open the WebUI you will be looking at information at the Server level. Select the Flow Statistics tab on the navigation bar at the top. 

4. After a brief pause you should see a tabular Message flow statistics view which summarises activity at the flow level. Notice that the value in the Total input messages column corresponds to the number of curl requests you made to the server. 

5. To get a more detailed view of how data is passing through your flows, change to the Flow node statistics view to see a breakdown for each node in the flow. 

6. In both views, the message flow name links you to another page with information about the message flow. This also has a Flow Statistics tab which includes some graphs which you may like to explore. 

7. When you're ready to conclude this tutorial, stop the Integration Server in the Command Console and close your browser. 
