

Getting started with ACEv11 - Policy Projects and Policies

OVERVIEW 


This tutorial demonstrates how to create a simple Policy within a Policy Project. The example guides you through the creation of an Activity Log Policy which is deployed alongside a simple flow. The Activity Log Policy specifies how App Connect Enterprise should log activity associated with the deployed flow, saved directly to a local file. 

App Connect Enterprise v11 introduces the concept of a Policy Project which can be created in the Toolkit, and is used to hold one or more policies. 

Policies are used to control connection properties and operational properties which are required by the ACE runtime. 

A policy can be used by an administrator to override or abstract some specific property values. 

For example sensitive data which might differ between runtime environments such as Dev / Test / Production. 

Policies can also be used to provide global properties that have wider scope than a message flow node, such as the Activity Log example in this tutorial. To understand more about policy overrides, a separate specific tutorial on this topic has been provided. 

--------------------------------------------------------------------------------------------------------------------------------------------------------

Activity Log information helps you to understand what your message flows are doing by providing a high-level overview of interactions with external resources. Activity Log messages are concise and avoid technical complexity, although more information is provided in the message detail. Because the log entries are short, uncomplicated, and focused on single activities, they can be quickly scanned and understood. Patterns of behavior and changes to such patterns are easier to identify than in more extensive product trace. Activity Logs can be written to a circular file system. The ActivityLog policy is used to set up file logging if you want continuous logging of activities over a long period. In the next section we will deploy the policy and message flow and observe their behavior