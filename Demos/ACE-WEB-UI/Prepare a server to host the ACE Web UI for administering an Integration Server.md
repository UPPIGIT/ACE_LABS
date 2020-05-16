Prepare a server to host the ACE Web UI for administering an Integration Server 

1. Open an App Connect Enterprise Command Console and create a working directory by running the mqsicreateworkdir command:

◦If you are using a Windows platform: mqsicreateworkdir C:\MyServer

◦If you are using a UNIX platform: mqsicreateworkdir /home/exampleuser/MyServer

2. Edit the server.conf.yaml file which the above command will have created in the directory C:\MyServer on Windows, or /home/exampleuser/MyServer on UNIX. To turn on Basic Authentication for users logging in to the ACE Web UI, find the line   #basicAuth: true and uncomment the line by removing the leading # symbol. Remember to save the server.conf.yaml file. 

3. Use the mqsiwebuseradmin command to specify a web userid and password which will be used to login to the Web UI:

◦If you are using a Windows platform: mqsiwebuseradmin -w C:\MyServer -c -u webuser -a password123

◦If you are using a UNIX platform: mqsiwebuseradmin -w /home/exampleuser/MyServer -c -u webuser -a password123

4. Start an integration server using the following command: 

◦If you are using a Windows platform: IntegrationServer --name MyServer --work-dir C:\MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log

◦If you are using a UNIX platform: IntegrationServer --name MyServer --work-dir /home/exampleuser/MyServer --admin-rest-api 7600 --http-port-number 7900 --console-log



---------------------------------------------------------------------------------------

1. Open a tab in a web browser and connect to the URL http://localhost:7600. 
This will present the WebUI for your standalone integration server, which we configured to use Basic Authentication. 
When challenged type the username webuser and password password123. Log-in should succeed and you should see several tabs along the top of the page (eg Contents), and a Deploy button on the right.

2. Explore the other areas of the Web UI yourself

 
