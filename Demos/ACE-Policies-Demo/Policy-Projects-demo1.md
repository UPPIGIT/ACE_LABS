Prepare an Integration Server


1. Open the policy named MyActivityLogPolicy.policyxml inside the SimplePolicyProject 
2. Edit the File name property of the policy:

◦If you are using a Windows platform, set the File name property value to C:\MyServer\ActivityLog.txt and save the policy.

◦If you are using a UNIX platform, set the File name property value to /MyServer/ActivityLog.txt and save the policy.

3. Create a Broker Archive file inside SimplePolicyProject named SimpleDeploy.bar 

4. Build the application and policy projects into the BAR file:
 - Select the Applications radio button and select the SimpleApp application
 - Select the Policies radio button and select the SimplePolicyProject
 - Click the 'Build and Save' button to generate the contents in the BAR file. 

5. If you are using Toolkit version 11.0.0.7 or later, then you can exploit the option to create a local integration server to test this tutorial. However the more detailed instructions which follow, work on earlier versions as well. Open an App Connect Enterprise Command Console and create a working directory by running the mqsicreateworkdir command:

◦If you are using a Windows platform: mqsicreateworkdir C:\MyServer

◦If you are using a UNIX platform: mqsicreateworkdir /home/exampleuser/MyServer


6. Start an Integration Server by typing the following command into the ACE Command Console:

◦If you are using a Windows platform: IntegrationServer --name MyServer --work-dir C:\MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log

◦If you are using a UNIX platform: IntegrationServer --name MyServer --work-dir /home/exampleuser/MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log

7. Connect to the Integration Server from the Integration Explorer view in the Toolkit, then drag and drop deploy the SimpleDeploy.bar 

----------------------------------------------------------------------------------------------------------------------------

Follow the following steps to complete the tutorial. 
1. Test the message flow by sending HTTP data to it using a client such as cURL:
curl -X POST http://localhost:7900/HTTPEcho -d HelloWorld
The message flow should echo back the same HelloWorld message to the HTTP client. Repeat this test a few times. 
2. Navigate to the directory location which was specified when editting the Activity Log policy:
◦If you are using a Windows platform, navigate to C:\MyServer and you should see a text file with the name ActivityLog.txt
◦If you are using a UNIX platform, navigate to /MyServer and you should see a text file with the name ActivityLog.txt

3. Open this file in your preferred text editor and you should see log entries formatted as comma separated data. Each log line corresponds to one of the curl requests which you just made. 
4. This concludes our simple example demonstrating how to use an Activity Log Policy.

-----------------------------------------------------------------------------------------
