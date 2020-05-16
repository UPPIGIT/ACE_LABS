Prepare a server to host the ACE Dashboard for administering multiple servers and nodes

1. Open a third ACE Command Console, which we will use to start another Integration Server whose only purpose is to host a variant of the ACE Web UI referred to as the ACE Dashboard which allows you to administer multiple Integration Nodes and Servers in your network. 

This special server behavior is configured by setting an environment variable named ACE_DASHBOARD. 
Given that the role of the web UI which will be served by this Integration server is to provide information about multiple servers and nodes running in your network, you will most likely want to configure this form of the web UI to be served using HTTPS.

In the example below, we will demonstrate using a keystore which has been populated with a self-signed certificate. For production usage, you are advised not to use self-signed certificates and instead import a personal certificate from a certificate authority. 

2. In Command Console, create a working directory by running the mqsicreateworkdir command:

◦If you are using a Windows platform: mqsicreateworkdir C:\MyDashboardServer
◦If you are using a UNIX platform: mqsicreateworkdir /home/exampleuser/MyDashboardServer

3. Edit the server.conf.yaml file which the above command will have created in the directory C:\MyDashboardServer on Windows, or /home/exampleuser/MyDashboardServer on UNIX, with the following details in order to secure the ACE Web UI. 

For the sslCertificate property, use a file path which is relevant for your operating system, 

for example on Windows C:\MyDashboardServerKeyStore\keystore.pfx 
but on UNIX /home/exampleuser/MyDashboardServerKeyStore/keystore.pfx:

RestAdminListener:
  port: 7643
  sslCertificate: C:\MyDashboardServerKeyStore\keystore.pfx
  sslPassword: adminRestApi::sslpwd
Remember to save the server.conf.yaml file. 

4. Create the directory C:\MyDashboardServerKeyStore on Windows or /home/exampleuser/MyDashboardServerKeyStore on UNIX, and then use keytool to create a keystore file containing your self-signed certificate:

◦If you are using a Windows platform: keytool -genkey -alias ACE_alias -keystore C:\MyDashboardServerKeyStore\keystore.pfx -storepass change1t -storetype pkcs12 -keyalg RSA

◦If you are using a UNIX platform: keytool -genkey -alias ACE_alias -keystore /home/exampleuser/MyDashboardServerKeyStore/keystore.pfx -storepass change1t -storetype pkcs12 -keyalg RSA

When prompted, provide suitable values for your self-signed certificate's values. For example, CN=aceserver.hursley.ibm.com, OU=IBM ACE, O=ibm, L=Hursley, ST=Hampshire, C=GB 

5. Use the mqsisetdbparms command to specify the password for the keystore to the Integration Server:

◦If you are using a Windows platform: mqsisetdbparms -w C:\MyDashboardServer -n adminRestApi::sslpwd -u ignore -p change1t
◦If you are using a UNIX platform: mqsisetdbparms -w /home/exampleuser/MyDashboardServer -n adminRestApi::sslpwd -u ignore -p change1t

6. Set the environment variable ACE_DASHBOARD=true by typing:
	set ACE_DASHBOARD=true 

7. Finally, start a new integration server by typing the command:

◦If you are using a Windows platform: IntegrationServer --name MyDashboardServer --work-dir C:\MyDashboardServer --console-log
◦If you are using a UNIX platform: IntegrationServer --name MyDashboardServer --work-dir /home/exampleuser/MyDashboardServer --console-log

8. We will use a web browser to explore the Web UI in the Run section of this tutorial. 

----------------------------------------------------------------------------------------


1. Open your web browser and connect to the URL https://localhost:7643. 
Your browser will ask you if you are happy to connect based upon the information which is in the self-signed certificate which you created earlier. 
Once connected, in the top right corner of this web UI you should see a button which allows you to toggle your view between Integrations, Servers and Nodes. 
To the left of this you will see the option to Edit Connections:

 - Click on 'Edit Connections'
 - Under the Connections tab, click 'Add'
 - Select 'Integration Server', use the name 'MyServer' (or any name you like to uniquely identify the standalone server), set the address to 'http://localhost:7600', the Username to 'webuser' and the Password to 'password123'
 - Click 'Save' in the bottom right of the window and the Servers view should show your standalone integration server. 

2. Repeat the process to Edit Connections:

 - Click on 'Edit Connections'
 - Under the Connections tab, click 'Add'
 - Select 'Integration Node', use the name 'MyNode' (or any name you like to uniquely identify the node which you created earlier), set the address to 'http://localhost:4414' (for this connection you don't need a Username and Password)
 - Click 'Save' in the bottom right of the window and in the Servers view, you should now be able to see tiles representing both the standalone server, and the servers which are owned by the Integration Node all together in the same Web UI. 

