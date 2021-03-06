﻿# Installation

This will walk you through the way towards setting up the Rubix Environment.

### Prerequisites

-   **Java 10.0.2 JDK** - We currently support version 10.0.2. Further support will be available soon.

    Download Link: [Java](https://www.oracle.com/java/technologies/java-archive-javase10-downloads.html)
    
-   **go-ipfs v0.4.22** - IPFS, a distributed file system is used for storage. We support go-ipfs of specific versions (0.4.18 - 0.4.22) 

    Download Link: [IPFS](https://dist.ipfs.io/go-ipfs)
    
After installing IPFS, follow the steps mentioned to setup [Private Network](#setup-private-ipfs-network)

### Setup Private IPFS Network 
Before setting up the Private Network, make sure IPFS is set as global **PATH** for Windows.

> **NOTE** 📋 For Mac and Linux Systems, the path is automatically set as global during the time of installation.

**I. Automated Script:** Download the [script and swarm key](https://github.com/rubixchain/rubixnetwork/tree/master/setupscripts) depending on the platform.

**MAC / Linux Users**
1. Open a terminal in the downloaded location and execute the following:
	```
	> chmod +x init.sh
	> ./init.sh
	```
2.  Open a new terminal and run the following command to start the daemon
	```
	> ipfs daemon
	```
**Windows Users**
1. Double click the `init.bat` file
2.  Open a new terminal and run the following command to start the daemon
	```
	> ipfs daemon
	```
**II. Manual Setup:**

Execute the command `ipfs init` in your terminal. This generates a unique `PeerID` for the node.

Sample Peer Identity: "QmeRAkURreUeWsZ5yovKSpNEC4U7UcAd91cYpbNhx4ovY"

1.  The IPFS Daemon should be running till the setup process is finished. To run the daemon, execute `ipfs daemon` in the terminal.
2.  Download the [Swarm Key](https://github.com/rubixchain/rubixnetwork/blob/master/setupscripts/LinScript/swarm.key) and place it in the IPFS repo directory. 
   
     **Windows:** /Users/<username\>/.ipfs/
     
     **Mac:** /Users/<username\>/.ipfs/
     
     **Linux:** /home/<username\>/.ipfs/


3.  The default bootstrap nodes should be replaced with Rubix bootstrap nodes.

     Execute the following commands in your terminal :
	```
	> ipfs bootstrap rm --all
	> ipfs bootstrap add /ip4/52.172.51.38/tcp/4001/ipfs/QmRdExYiDHN6VtFmYN24nCimmAMz83FQmDuZmtpnteURiq
	> ipfs bootstrap add /ip4/52.172.51.38/tcp/4002/ipfs/QmenpqsPquKTWbwuV48fgSYz7XhoDkgAfABEtc3xF7Jsry
	> ipfs bootstrap add /ip4/52.172.51.38/tcp/4003/ipfs/QmPRX8w7xng24soPJTPSWcY2LEkPyc4WfUAayiymb9Ndcu
	> ipfs bootstrap add /ip4/13.71.70.168/tcp/4001/ipfs/QmemdPdzwZ1VYY9xEFpiENDWqpL3crbVGT1X4TderdyYhi
	> ipfs bootstrap add /ip4/13.71.70.168/tcp/4002/ipfs/QmNcNqoLcAH224EekfVKX22b9dvYcK6pSYw74bfaL2P6Km
	> ipfs bootstrap add /ip4/13.71.70.168/tcp/4003/ipfs/QmcdaAtoDFCxY73qYqa1EuQXxP4q9ZJLk5oKD1YsLo4PvF
	> ipfs bootstrap add /ip4/13.71.82.252/tcp/4002/ipfs/QmPTShTj5FDttPbxiMSkcm1SHFwsyCmyk6YhkDZWHmWQjt
	> ipfs bootstrap add /ip4/13.71.82.252/tcp/4003/ipfs/Qme4EQV4YGgtiAoTy3n1RB2sHMtW4KQq97ydnpV2Hh6npr
	```
4.  For peers within the network to discover and communicate with each other, Auto Relay and LibP2P functionalities should be enabled.

     Do the following in your terminal:
	   ```
     > ipfs config --json Swarm.EnableAutoRelay true
     > ipfs config --json Experimental.Libp2pStreamMounting true
	 ```
     
5.  Once the above steps are finished, terminate IPFS using
    `ipfs shutdown` command in the terminal.

6.  Now, restart the daemon and check for the presence of unique fingerprint.

    > **NOTE** 📋 The nodes that have same swarm key will have same fingerprint.

IPFS Private Network Setup is Successfully Finished ✅

# Rubix DID Creation

1.  Make sure IPFS Daemon is running. If not, execute the command `ipfs daemon `
2.  Download the DID Creator jar from here: [Link]()
3.  Open a terminal. Go to the downloaded location and run the following command
    ` java -jar DIDCreation.jar `
4.  Send an API request to you node to create a new DID. 
	 ``` java
	  $ curl -X POST http://localhost:9501/create -d 
	  '{  
		"data": "9989198712,user@rubix.network"  
	   }'
	  ```  
For more on Rubix API, visit [Rubix API Docs](https://github.com/rubixchain/rubixnetwork/blob/master/Rubix%20API.md).

You have successfully created a Decentralized Identity for your node ✅

# Rubix Token Application

1.  Make sure IPFS Daemon is up and running. If not, execute the command `ipfs daemon`
2.  Download the Token Transfer jar from here: [Link]()
3.  Open a terminal. Go to the downloaded location and run the following command `java -jar TokenTransfer.jar`

Just like how we just created a node and DID, there are millions in the network with their identities that has to be synced with other peers in the network.

1.  Send an API request to your node to sync the nodes in the network.
	```java
	 $ curl -X GET http://localhost:8881/sync -d
	/**
	* Returns: None
	*/
	```
2.  Now that the nodes are synced, with the knwoledge of receiver's DID tokens can be transffered.
	```java
	 $ curl -X POST http://localhost:8881/start -d  
	 {
	    "receiver": "75239D03C9A8BD9381C16B0D78A21A1493B1D2AB982DA8824EF068471FF96020",
	    "tokenCount":1,
	    "quorum":"[]",
	    "threadExt":"t1",
	    "comment":"transaction comments"
	 }
	/**
	* This API call transfer tokens with the given input parameters
	* Inputs: receiver (String), tokenCount (Integer), quorum (JSONArrayList), threadExt (String), comment (String)
	* Returns: Transaction ID (String), Success / Failure (Boolean)
	*/
	```
For more on Rubix API, visit [Rubix API Docs](https://github.com/rubixchain/rubixnetwork/blob/master/Rubix%20API.md).
