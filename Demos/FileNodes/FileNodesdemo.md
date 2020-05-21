
FileNodes
-------------------------------------------------------------------------------------------------------------------------------------------------------

FileInput node reads files from the local file system or FTP server

FileOutput node writes files to the local file system or FTP server

Focus on message processing rather than message delivery

Supported on all broker platforms


Terminals:
---------------------------------------------------------------------------------------------------------------------------------------------------------

Here are the two nodes that provide file support in Message Broker version 6.1. Both nodes have an out terminal (used after successful processing), and a failure terminal (used after processing error occurs in the node). 

As an input node, File Input also has a catch terminal that is used to flow caught errors that occur downstream of the node.

There are specific terminals, both on the File Input and File Output Nodes, for handling the end of the file.

On the File Input Node, the “End of Data” terminal is given control when the Broker detects that it has reached the end of the input file.

On the Output Node, the “Finish File” terminal is driven to indicate that the file should be written out. 

Assuming this is completed successfully, the message sent to the “Finish File” terminal is propagated to the “End of Data” terminal.


File Nodes Basic Algoritham:
--------------------------------------------------------------------------------------------------------------------------------------------------------

The File Input node is able to read through a pre-configured directory on the broker’s file system. 

The directory name is configured on the Basic tab of the node and must be entered in the format expected by the broker file system. This directory will be scanned by the Broker for files. 

Subdirectories of this directory are not scanned. The files to be processed must be present in the specified directory.

The file name can be specified as a wildcard. Acceptable wildcards on the file name pattern are ‘asterisk*’ which matches multiple characters, and ‘question mark’, which matches a single character.

The value for the directory must either be a fully qualified directory path, or as a directory relative to $MQSI_WORKPATH/file. For example, in a Windows® environment the default is C:\ Documents and Settings \ All Users \ Application Data \ IBM \ MQSI \ file.

If there is an operating system lock on a file it will not be read until the lock is removed.

 
Once the file is read and parsed, an option on the node specifies the action to take; either the input file is deleted, or moved into an archive subdirectory of the input directory. If the archive directory does not exist, it will be created.

The archive directory will be named “mqsiarchive”

If an archive file already exists with the same name, an option exists on the node to overwrite it. If you do not set this option, the node will throw an exception if it tries to move a successfully-processed file with the same name into the archive sub-directory.

Alternatively, you can opt to rename the file to include a timestamp value as it is moved into the archive directory, which will prevent duplicate files in the directory. The timestamp includes the time to a millisecond granularity, ensuring uniqueness of file names in the archive directory.

When the file is placed into the archive directory, the contents of the file will not be altered.

Once the file is processed, the next file in the directory is processed.

Once all files have been processed, the directory is rescanned. If any files that were previously locked are now unlocked, they will be processed by the Broker.

If a directory scan yields no matches, the node waits for a preconfigured amount of time before rescanning. The default value for this interval is 5 seconds.


Environment:
-------------------------------------------------------------------------------------------------------------------------------------------------------------

Each time the contents of a file are propagated, the File Input node stores additional information on the file inside the Local Environment, and this table describes those fields.

For example, the Local Environment Directory element contains the absolute directory name that contains the file that is being processed. Other element values are as shown in the table on this slide.

Also, the File Input node copies the characters in the file name matched by wild cards, together with any intermediate characters, to the Properties tree “WildcardMatch” element.

So if the match expression was “file*.txt” and the file read was “file02.txt”, then the value of WildcardMatch would be “02”. Or if the match expression was “file??type.*”, and the file read was “file02type.xml”, the value of WildCardMatch would be “02type.xml”. This information can be used by an output node, where the output name can be derived from the input name.

FIle Input Node -File Parsing
-----------------------------------------------------------------------------------------------------------------------------------------------

Before covering how the file information is parsed, note that the Input Message Parsing tab on the File Input Node has two non-standard fields (CCSID and encoding). This is similar to the equivalent properties on an MQ Input Node.

The File Input node reads raw bytes from the file system before passing on to the parser.

These parameters make it possible to parse information from a system other than the one in which the broker is running. This system may have a different Character Set than the one hosting the Broker, and this property allows these files to be processed correctly.

Record Detection
-----------------------------------------------------------------------------------------------------------------------------------------------------
The File Input node allows you to split up each file into multiple records that are propagated separately.

One particular benefit of record detection is that, depending on which parser you’re using, the File Input node does not need to store the whole file in memory at any one time. Only the records currently being processed are stored in memory. 

This streaming allows for very large files to be processed. Such files could be several gigabytes in size. Thecapability is supported by the MRM and XMLNSC parsers.

Record detection allows files to be split into individual parts using several techniques.

Alternatively, the whole file can be processed in one go. 
When processing records individually, the maximum size of an individual record is 100MB, which is the same as the maximum size of an MQ message. Note however, that processing records of this size will need careful planning of the Broker environment and in particular the size of the Broker Java Virtual Machine.

Note that a file can only be processed by a single thread within the Broker. Therefore,individual records can only be processed sequentially, not in parallel.

---------------------------------------------------------------------------------------------------------------------------------------------------------------

five options available for record detection.

“Whole File” reads the entire file’s contents into memory and parses it in its entirety according to the Inbound Message Parsing options. If this option is used, then you must be sure that the available memory in the Broker is sufficient to accommodate the whole file.

“Fixed Length” breaks the file up into a set number of bytes and propagates each chunk as a separate message. The last message in any file may be of a size smaller than the specified length. If you use this option, you specify the record size on the File Input Node

“Delimited” allows you to specify a delimiter that is used to break up the records in a file. In this case, you can specify a standard DOS or UNIX “end of line” delimiter, or you can specify you own custom delimiter. This is done by specifying the hexadecimal characters which indicate the end of line for the particular file.

If you specify Delimiter type=Infix and the last data in the file ends with a delimiter, the FileInput node will propagate an empty record to indicate the delimiter's presence.

However, if you specify Delimiter type=Postfix the FileInput node will not propagate an empty record, whether the last data in the file ends with a delimiter or not.

There is a more advanced option for record detection.
“Parsed Record Sequence” breaks the file into messages, each of which conforms to a single structure as defined on the Inbound Message Parsing options. For example, this could be used to process a file which contains multiple instances of a piece of XML data.

Transictionality
---------------------------------------------------------------------------------------------------------------------------------------------

When you include a File Input node in a message flow, the value that you set for Transaction Mode defines whether messages are sent under sync point. The File Input node is not transactional, and does not itself process files under sync point.

If you set “Transaction Mode” to Yes, the message is received under sync point (that is,within a WebSphere MQ unit of work).
 Any messages subsequently sent by an output node in the same instance of the message flow are put under sync point, unless the output
node has explicitly overridden this. If the File Input node backs-out the transaction, allchanges made under sync point are backed out.

If you set “Transaction Mode” to No, the file is not received under sync point. Anymessages subsequently sent by an output node in the flow are not put under sync point,unless an individual output node has specified that the message must be put under sync point. Changes not made under sync point will not be backed-out if the File Input node backs out the transaction.


FileInput Node failure scenario
--------------------------------------------------------------------------------------------------------------------------------------------------
Each FileInput node has a configurable retry mechanism. There are three options for “Retry Mechanism” .

The first option is Failure, which is the default action. If the file could not be read, then it should be processed as a failure.

The second option is Short Retry. In this case, the Broker retries several times before processing as a failure. The number of times to retry is given by “Retry threshold” and the “Short retry interval” specifies the length of time to wait between failures, in seconds.

The third option is “Short retry then Long retry”. In this case, once short retries have been exhausted, the node will continue to retry indefinitely, waiting for the Long retry interval period between retries.

Failure processing is determined by the “Action on failing file” option. This provides three options.

The first option is “Move to Backout Subdirectory”. In this case, the file is written to a back out subdirectory, which is named “mqsi-backout”. This directory is created as a subdirectory of the input directory. This is created automatically if it doesn’t already exist. 

If a file of that name already exists in the back out directory, the message flow fails; the failure will be reported in the normal manner, for example using the Event Log in a Windows environment.
The second option is “Delete”. In this case, the input file is deleted and the next file is processed
-------------------------------------------------------------------------------------------------------------------------------------------------------------
Local environment file related information.
https://www.ibm.com/support/knowledgecenter/SSMKHH_10.0.0/com.ibm.etools.mft.doc/ac55410_.htm
-------------------------------------------------------------------------------------------------------------------------------------------------------

FTP:

FileInput node – using FTP
When active, FTP settings cause the node to periodically
transfer files on a remote server to the local directory for input.
Security Identity ‘UserMapping’ set using runtime command:
mqsisetdbparms BROKER –n ftp::UserMapping –u ftpUser –p ftpPassword

It is possible to use the File Input to read files from an FTP server. FTP support is enabled
through a single Boolean flag on the FTP tab of the File Input node. When using the FTP
function, the Broker operates in a similar way to normal file processing. The FTP server
directory is scanned for new files, and any files that are detected are moved into the local
file directory specified on the File Input Node. These files are then processed by the
Broker as a local file.
The security authorization for the FTP server can be specified using the “Security Identity”
value of the FTP tab. The value specified in here should match the parameter that is
specified on the “mqsi-set-db-parms” command.
When using the “mqsi-set-db-parms” command, note the format of the “ftp” parameter.
This must be followed by a double colon, and then the name of the Security Identity
property, which is used on the FTP tab of the File Node.
IPv6 format addresses can be used as the FTP server address, although the address
must be enclosed in square brackets.

FileInput node – Algorithm with FTP enabled

FileInput node scans a pre-configured directory for files that match a given specification. Upon successful processing,file is either deleted or moved to an mqsiarchive subdirectory

If no files are found during the directory scan, the node transfers files from the configured FTP server to the local directory being used for file input. Transferred files are deleted from the FTP server.

Transferred files are processed in the usual way.

If no files are found on the FTP server, the node waits for the period of time configured on the FTP server panel “scandelay” before trying again.

Additional instances handling
---------------------------------------------------------------------------------------------------------------------------------
Each file is processed by at most one instance and node
Additional instances and nodes can concurrently process other files in the same directory.
Option to allocate threads from a node-specific pool rather than the flow pool
Additional instances tuning is particularly important when FTP is involved


FIle Output Node:
------------------------------------------------------------------------------------------------------------------------------------------------------

The File Output Node is similar to the File Input node, in that in its simplest form you can specify the directory to which files are placed and a file name expression that describes the file name.

The file name expression can include a single wildcard character ‘*’, which will be replaced with the value of the LocalEnvironment.Wildcard.WildcardMatch element in the tree.
For example, in a File Input to File Output scenario, the Wildcard-Match element is set up during the File Input node processing. This allows you to preserve the name of the input file on the File Output node. 
You can also use standard tree manipulation logic to set the value of the Wildcard-Match element to whatever you want, regardless of whether you
used a File Input node or not. For example, this could be used to specify the name of the output file.


Options if the file already exists

 Replace the existing file
 Create (Fail if already exists – go down Failure terminal)
 Archive previous file
Optionally replacing any existing file in the archive
 Archive previous file, with timestamp

Appending records

 “Records and Elements” tab defines how multiple writes to the same file are
handled
 Record definition options:
Record is Whole File – close file automatically after first write
Record is Unmodified Data – the message bit-stream appended to file
Record is Fixed Length Data – specify length in bytes and padding character
Record is Delimited Data – specify delimiter and infix/postfix option
 Unless “Record is Whole File” is selected, the file will be closed when the
“Finish File” terminal is triggered
The Finish File message is propagated on to the FileOutput’s “End Of Data” terminal
 The mqsitransit subdirectory holds all files that have not yet been closed

FileOutput - FTP support
---------------------------------------------------

As with the File Input node, FTP support is configurable on a single ‘FTP’ tab and enabled through the use of a single check box on the FTP Properties tab.
FTP transfer only happens after a file is closed on the local file system. After the file is transferred to the FTP server, it can be retained locally, or deleted from the local file system.

The FTP process happens synchronously, and can take an extended time, depending on the available network bandwidth, and the size of the file to be transferred. As with the FTP Input function, you should make use of the “additional instances” property, to make sure that thread starvation does not occur.

File input and output – Hiding FTP user names and passwords

 ‘Server identification’ may be an alias to a service already configured on the broker
mqsisetdbparms BROKER –n ftp::UserMapping –u USER –p PASS
mqsicreateconfigurableservice BROKER –c FtpServer –o MYSERVER
mqsichangeproperties BROKER –c FtpServer –o MYSERVER –n serverName –v
IPADDRESS
mqsichangeproperties BROKER –c FtpServer –o MYSERVER –n securityIdentity –v
UserMapping
 This done, a server identification of MYSERVER on the node will cause the preconfigured
service definition to take precedence over the node properties
