

Getting started with ACEv11 - Policy Projects and Policies with Overrides


OVERVIEW 


App Connect Enterprise v11 introduces the concept of a Policy Project which can be created in the Toolkit, and is used to hold one or more policies. Policies are used to control connection properties and operational properties which are required by the ACE runtime. 

A policy can be used to provide values which override the properties of message flow nodes. This is helpful for properties which are likely to differ across different runtime environments such as Dev / Test / Production as it avoids having to change the message flow itself when promoting it between environments. 

A policy can be used to abstract the definition of message flow node properties which may include sensitive data that a developer (unlike an administrator) may not have access to. 

Policies can be used to provide global properties that have wider scope than a message flow node. For example, a user can define an Activity Log policy which defines the name of a file to which activity log records should be written. 

This tutorial uses a simple message flow which receives data over HTTP and sends an Email output. The EmailOutput message flow node is configured to use an SMTP Policy which is deployed using a BAR file. The tutorial demonstrates how to refer to the policy from the EMailOutput message flow node, how you can override policies, and the concept of a default policy project. 
-----------------------------------------------------------------------------------------------------------------------------------------------------------
Import an application and create a policy project with an SMTP policy


1. Click Import and the HTTPInputEmailOutput application will be imported into your workspace. 

2. Open the HTTPInputEmailOutput message flow and look at the EmailOutput node's properties. Note that the Basic property tab has a property named SMTP Server and Port which addresses {MyPolicyProject}:MySMTPPolicy 

3. On the Email property tab, note that the property named To Addresses specifies a value of anyobscurenameyouwant@mailinator.com. You can optionally change this if you want. This is the email address to which an email will be sent (from your SMTP email provider) 

4. On the Email property tab, change the property named From Address to be your email address from which you intend the email to be sent (when ACE connects to your SMTP server) 

5. Create a Policy project named MyPolicyProject using the New link in the top right corner of the Application Development view. 

6. Create a Policy named MySMTPPolicy of type SMTP (click the New link which appeared inside the empty project you just created). Set the SMTP server name to be your SMTP server. For example, smtp.hursley.ibm.com:25 Set the Security identity to be the hardcoded string GoodIdentity (this value will be mapped to your actual identity using the mqsisetdbparms command in the later steps) 

------------------------------------------------------------------------------------------------------------------------------------------------------------

Create and Configure an Integration Server


1. Open an App Connect Enterprise Command Console and run the mqsicreateworkdir command to prepare a work directory for an integration server:

◦If you are using a Windows platform: mqsicreateworkdir C:\MyServer

◦If you are using a UNIX platform: mqsicreateworkdir /home/exampleuser/MyServer

2. Run the mqsisetdbparms command to define the identity which ACE will use when communicating with your SMTP server:

◦If you are using a Windows platform:

 mqsisetdbparms -w C:\MyServer -n smtp::GoodIdentity -u YourSMTPUserId -p YourSMTPPassword
where C:\MyServer is a folder on your file system, created in the command above, that the server will use for its working directory.

◦If you are using a UNIX platform: 
mqsisetdbparms -w /home/exampleuser/MyServer -n smtp::GoodIdentity -u YourSMTPUserId -p YourSMTPPassword
where /home/exampleuser/MyServer is a folder on your file system, created in the command above, that the server will use for its working directory.

3. Now, start the integration server using the following command:

◦If you are using a Windows platform:

 IntegrationServer --work-dir C:\MyServer --name MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log

◦If you are using a UNIX platform: 

IntegrationServer --work-dir /home/exampleuser/MyServer --name MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log

4. After a few seconds the server should report that it has finished initialization and that its HTTP Listener has started listening for connections. 

-----------------------------------------------------------------------------------------------------------------------------------------------------

Run three separate examples to demonstrate policy identification and overrides

Learn how the {PolicyProject}:Policy syntax can address a deployed policy 
1. In the App Connect Enterprise Toolkit, connect to your Integration Server MyServer on localhost:7600. Create a Broker Archive file, for example named HTTPInputEmailOutput.bar inside the HTTPInputEmailOutput application project which you imported earlier. Once you have created the BAR file, you need to add to it both the HTTPInputEmailOutput application and the MyPolicyProject policy project. Remember to click the Build and Save button on the BAR editor's prepare tab. 
2. Drag and drop deploy the HTTPInputEmailOutput.bar BAR file onto MyServer. 
3. Test the message flow by sending HTTP data to it using a client such as cURL: 
curl -X POST http://localhost:7900/HTTPInputEmailOutput -d HelloWorld

4. The message flow should echo back the same message to the HTTP client, but you should find that the flow's email Output node has successfully found the SMTP policy you deployed, which in turn used the abstracted userid and password which was defined using mqsisetdbparms, and an email will have been successfully sent. Check this is the case. If you are using mailinator for this purpose, you can open the public mailbox using a web browser and a URL like this:
http://www.mailinator.com/v2/inbox.jsp?zone=public&query=anyobscurenameyouwant 

Learn how unqualified policy names won't be resolved, if you haven't defined a default policy project 
1. Delete all resources which have previously been deployed. Open the HTTPInputEmailOutput message flow and on the EmailOutput node's Basic property tab edit the property named SMTP Server and Port to take the value MySMTPPolicy (so it is no longer qualified by the name of the policy project). 

2. Open the HTTPInputEmailOutput.bar BAR file and click the Build and Save button on the BAR editor's prepare tab. Drag and drop deploy the updated BAR file. 

3. Repeat the test of the message flow by sending HTTP data to it using a client such as cURL, and this time you will note that an exception is reported in the command console window where the IntegrationServer is running. This exception occurs because having deleted the Policy Project qualifier, the EmailOutput node's policy can no longer be located. 

Learn how unqualified policy names will be resolved, if you have defined a default policy project 
--------------------------------------------------------------------------------------------------------------------------------------------

1. Delete all resources which have previously been deployed and then stop the Integration Server using CTRL-C in the command console window where it is running. 

2. Look inside your integration server's work directory on the file system (at C:\MyServer on Windows and at /home/exampleuser/MyServer on UNIX) and edit the server.conf.yaml file. Find this line:
 #policyProject: 'DefaultPolicies' # Name of the Policy project that will be used for unqualified Policy references
... and change it to become ...
 policyProject: 'MyPolicyProject' # Name of the Policy project that will be used for unqualified Policy references  

3. From your App Connect Enterprise Toolkit's workspace directory on the file system, copy the MyPolicyProject project (which is a directory on your file system) into your Integration Server's overrides directory. If you have followed this tutorial's suggested naming conventions then this directory will end up at C:\MyTestServer\overrides\MyPolicyProject on Windows and at /home/exampleuser/MyServer/overrides/MyPolicyProject on UNIX. Restart your integration server. 

4. Back in the App Connect Enterprise Toolkit, create a new BAR file, for example named HTTPInputEmailOutput2.bar, and build it to just contain the application HTTPInputEmailOutput and NOT the policy project MyPolicyProject. Deploy the new BAR file. 

5. Retest the message flow using cURL just as you have done before and note that it will now send the email again successfully. This has demonstrated the fact that on message flow node properties, App Connect Enterprise will look for unqualified policies in the default policy project which you can configure for your server using the server.conf.yaml file. 
------------------------------------------------------------------------------------------------------------------------------------------------------

