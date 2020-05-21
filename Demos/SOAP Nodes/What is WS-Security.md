What is WS-Security?
--------------------------------------------------------------------------------------------------------------------------------------------------------------
Web Services Security describes enhancements to SOAP messaging to provide quality of protection through message integrity, message confidentiality, and single message authentication. 

WS-Security mechanisms can be used to accommodate a wide variety of security models and encryption technologies.

Integration Bus focuses on the part of the WS-Security specification that is known as WSSecurityPolicy.

This policy is implemented by using the properties of the SOAP Input or SOAP Request nodes.

Key areas that the Integration Bus encompasses include authentication, message-level protection, and message part protection.

WS-Security defines a <Security> element that can be placed in the header section of a SOAP message.

Transport level security and message security

A primary security task is to keep the SOAP message secure through intermediate systems until it reaches the ultimate recipient.

At the transport level, you can use SSL and HTTPS.

At the message level, you can use WS-Security. WS-Security is based on securing SOAP messages through an XML digital signature, confidentiality through XML encryption, and credential propagation through security tokens. 

The WS-Security specification defines the core facilities for protecting the integrity and confidentiality of a message and provides mechanisms
for associating security-related claims with the message.

