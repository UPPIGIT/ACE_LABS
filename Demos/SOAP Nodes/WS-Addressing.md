Routing (WS-Addressing)
----------------------------------------------------------------------
This section describes the concepts of WS-Addressing as they apply to the SOAP nodes.

WithoutWS-Addressing, a response to a Web service request is always returned to the sender. This is often exactly what you want, but is not very flexible. With WS-Addressing, address information is carried explicitly in the message itself, providing support for:

•More complex message exchange patterns, involving multiple endpoints. A response no longer has to go to the original sender, and separate destinations can be specified for normalresponses and SOAP faults.

•Asynchronous and "extended duration" communication patterns.

•A message correlation mechanism, which is used by other protocols such as WS-ReliableMessaging.

In essence, WS-Addressing defines a number of SOAP headers that your message flow may need to inspect or define at runtime. 

Listing 1 shows an example SOAP Header using WS-Addressingheaders. If WS-Addressing is enabled, then inbound WS-Addressing headers are processedspecially and appropriate headers are generated for outbound messages.

Listing 1. SOAP message example: WS-Addressing Headers

<SOAP-ENV:Header>
 <wsa:To>http://localhost:6080//myAppServer/services/myService</wsa:To> 
 <wsa:Action>myOperation1</wsa:Action> 
 <wsa:MessageID>uuid:515704D6-0111-4000-E000-82267F000001</wsa:MessageID> 
 <wsa:ReplyTo> 
 <wsa:Address>http://www.w3.org/2005/08/addressing/anonymous</wsa:Address> 
 </wsa:ReplyTo>
 <wsa:FaultTo> 
 <wsa:Address>http://www.w3.org/2005/08/addressing/anonymous</wsa:Address> 
 </wsa:FaultTo>
 </SOAP-ENV:Header>
 
 You tell a SOAP node to use WS-Addressing by checking Use WS-Addressing on theWS Extensions properties tab. 
 
 The property can be set on the SOAPInput node and the SOAPRequest node (it is not set by default). It is implicitly selected for the SOAPAsyncRequestnode.
 
 
 
WS-Addressing defines the following key headers:

•wsa:To, wsa:ReplyTo, wsa:FaultTo, wsa:Fromthe endpoints to receive the message and its replies

•wsa:MessageID, wsa:RelatesTomessage correlators

•wsa:Actiona URI indicating how the message should be processed

The ReplyTo, FaultTo and From headers are defined as EPRs. (The To header is a simple URI,but Message Broker allows an EPR to be used for this too in some circumstances.) An EPR is astandard way of specifying an endpoint and other associated information.


WS-Addressing also defines two special addresses:
•"http://www.w3.org/2005/08/addressing/anonymous", which means send the response backover the connection that the request was received on. The response is returned to the sender.

•"http://www.w3.org/2005/08/addressing/none", which means no message should be sentbecause all messages to this address will be discarded.



 
 
 
 The following ESQL shows the WS-Addressing To field being set to a specific endpoint and the ReplyTo field being set to anonymous.
Listing 2. Setting WS-Addressing Headers

SET OutputLocalEnvironment.Destination.SOAP.Request.WSA.To = 'http://localhost:7800/test1';

SET OutputLocalEnvironment.Destination.SOAP.Request.WSA.ReplyTo = 'http://www.w3.org/2005/08/addressing/anonymous';

The WS-Addressing header information is set in the LocalEnvironment. If you attempt to set theinformation under SOAP.Header as follows, then the information will not be used:

DECLARE wsa NAMESPACE 'http://www.w3.org/2005/08/addressing';SET OutputRoot.SOAP.Header.wsa:To = 'http://localhost:7800/test1'; -- DO NOT DO THIS

WS-Addressing makes use of the following folders under LocalEnvironment. Each folder containsfields applicable to the node that will use the information. If you need to see all the fields, select the checkbox Place WS-Addressing headers into LocalEnvironment on the node. 

If this checkboxis not ticked then the addressing information is processed and discarded when the message isreceived (at the SOAPInput node for a request, or at a SOAPRequest or SOAPAsyncResponsenode for a response message).



Table 1. WS-Addressing and LocalEnvironment

Folder Name
Populated by
Used by

SOAP.Input.WSA
SOAPInput (if checkbox ticked)
message flow

Destination.SOAP.Reply.WSA
message flow
SOAPReply

Destination.SOAP.Request.WSA
message flow
SOAPRequest (outbound),SOAPAsyncRequest

SOAP.Response.WSA
SOAPRequest (inbound),SOAPAsyncResponse (if checkbox ticked)
message flow


If the ReplyTo field is an explicit (non-anonymous) EPR then the response is sent to that endpointusing a new connection. If the ReplyTo field has the special value anonymous (the default) then theresponse is sent to the originator on the same connection ).
