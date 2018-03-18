## Batnode Prototype

## Specification

#### Upload Single File

kd = kad node
bt = bat node
tkd = target kad node
tbt = target bat node

1. kd performs an iterativeFindNode for <= k kd nodes w/ closest ids to file id
2. kd returns those nodes contacts to bt
3. bt iterates through contacts until the following subroutine succeeds:
4. For each target kad node (tkd):
5. bt asks kd to send a broker_connectionRPC to tkd. Broker connection's payload is bt public conect tuple
6. tkd receives RPC and tells its target bat node (tbt) to send establish_connection tcp request to bt tuple
7. tkd simultaneously sends a success RPC to bt, containing tbt's public tuple
8. bt sends establish_connection tcp request to tbt
9. if bt receives the establish_connection request, it sends a standard store_file message in response
10. if tbt is instead the one to receive establish_connection request, it sends a ready_to_store message in response, and bt responds to that with standard store_file message
11. if bt does not receive a response with X amount of time, move onto the next tkd and try again, else, cease iteration
12. when tbt finishes receiving file, it initiates an iterative store on its tkd

Edge cases: 
 - The node doesn't have enough allocated storage
 - The nodes are already connected from previous exchange, so both establish_connection requests go through, causing the file to be sent over twice

 

#### Retrieve Single File



#### Redefining a file as a group of shards: New versions of Upload and Retrieval




#### Shards and Avoiding Exceeding High Water Marks for JSON-based requests



#### BatNode's public ip/port changes



#### KadNode disconnects and reconnects



#### BatNode-KadNode Pair joins the network


#### BatNodes keep eachother alive



#### Discussion of Chosen NAT Traversal Strategies




### Demos

#### 1: Write a file across two nodes in a LAN.

`node1` will write to `node2`, which resides in another directory.

Starting condition:  
- `demo/batnode1/hosted` has an `example.txt` file.
- Delete any files in `demo/batnode2/hosted`.

1) cd into `demo/batnode1`
2) run `node batnode.js`
3) open a new terminal session and `cd` into `demo/batnode2`
4) run `node batnode.js`

Ending condition: `demo/batnode2/hosted` has a `example.txt-1` file.

