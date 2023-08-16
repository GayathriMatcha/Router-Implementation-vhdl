# Router Implementation vhdl
### IMPLEMENTATION OF ROUTER ON NETWORK-ON- CHIP  (NOC)                      
Network-on-Chip (NoC) is an advance design method of communication network into System-on-Chip (SoC). It provides solution to the problems of traditional bus-based SoC.  It is widely considered that NoC will take the place of traditional bus-based design and will meet the communication requirements of next SoC design. A router is the key component and called as the communication backbone in NoC. The work presented in this Report is  concerned with the design of a router as a basic component in the network on chip using Very High speed integrated circuit Hardware Description Language (VHDL). The design is implemented in VHDL and simulated in Xilinx ISE Design Suite 14.7.The designed router deals with packet based data transfer. A  8- bit packets are used in each router port. The architecture proposed in this report supports five connections simultaneously without any communication bottleneck, and improve the speed of communication on NOC router. 
### MICROARCHITECTURE OF A ROUTER:
![image](https://github.com/GayathriMatcha/Router-Implementation-vhdl/assets/98030485/19d7baca-0fb2-4887-9b54-9afa19129120)

THEORY:-
 Today’s industry is moving towards multi core system on chip (SOC). System on Chip (SOC) is a new trend technology which includes various computer components in a single chip, such as a smartphone or wearable computer. Conventional bus based interconnection between these multi core system became impractical as the number of cores increase because of more power consuming, more size occupied and poor performance due to data queue. For providing reliable communication between these cores, Network-on-Chip (NOC) is required . The use of a NOC as alternative SOC communication technology has several advantages-
(1) Adaptive as required from bandwidth segment wires points of view
(2) Reliable performance in high loads with flexible dynamic range .                                        


                                      NETWORK ON CHIP(NOC)
                             
                                                    
                                          The heart of an on-chip network is the router, which undertakes the essential task of guiding and coordinating the data flow. Performance of Network-on-chip is determined by the router architecture to a large extend and virtual-channel router is said to be a promising choice for NoC. NoC architectural style, the chip is broken into a set of interconnected nodes where each node can be a processor and is commonly known as processing element (PE).   In the conventional router architecture, each router has five ports, one for each direction: North, South, East and West, and a local port that is connected to the local processing element. Each port has one output Arbiter,  FIFO Buffers ,Virtual Channel Allocator Finally, all ports are connected to each other using Switch allocator, which contains crossbar switch for depicting physical connections.

MODEL:- 
ROUTER MICRO ARCHITECTURE AND ITS FUNCTIONALITY:-
The below figure is the microarchitecture of a state-of –the-art credit based virtual channel(VC) router to explain how typical routers work. The router has five input and output ports .The major components which constitute the router are the input buffers,route computation  logic,  virtual channel allocator(VC allocator) , switch allocator  and the crossbar switch.Most on-chip network routers are input-buffered, in which packets are stored in buffers only at the input ports, input buffered as input buffering permits the use of single-ported memories.Here, we assumed four VCs at each input port,each with its own buffer queue that is four flits deep.
                                 MICROARCHITECTURE OF A ROUTER 
                                           
 A head ﬂit,up on arriving at an input port,is ﬁrst decoded and buffered  according  to its input VC in the buffer write (BW) pipeline stage. In the next stage, the routing logic performs route buffer write computation (RC) to determine the output port for the packet.The header then arbitrates for a VC corresponding to its output in the vc allocation(VA) stage.Upon successful allocation of a VC the header  flit proceeds to the switch allocation(SA) stage where it arbitrates for the switch input and output ports.On winning the output port, the flit is then read from the buffer and proceeds to the switch traversal(ST)stage,where it  traverses the crossbar.Finally,the flit is passed through the output port.
   COMPONENTS OF A ROUTER:-
	ARBITER:-

 An arbiter is required to determine how the physical channel can be shared amongst many requestors. Here Round robin  arbiter is used.  It operates on the principle that a request that was just served should have the lowest priority on the next round of arbitration. Arbiter controls the arbitration of the ports and resolves contention problem. It keeps the updated status of all the ports and knows which ports are free and which ports are communicating with each other. Packets with the same priority and destined for the same output port are scheduled with a Round-Robin Arbiter. Use of an arbiter in an output channel is to tackle the problem of a single port receiving multiple input requests. Arbiter matches N requests to 1 resource. The circuit required for a round robin arbiter is shown in the figure below. A set of requests from 4 different requestors are shown .The last request serviced prior to this set of requests was from Requestor A. As a result, B has the highest   priority initially. With the round-robin arbiter,  requests are satisﬁed in the following order: B1,C1,D1,A1, D2, A2.
                                               
                                                     ROUND ROBIN ARBITER
                                                

                   




                              
  REQUEST QUEUE FOR ARBITER

                                                                        


	VC SEPARABLE ALLOCATOR:-

 Due to the need for pipeline able allocators, simple separable allocators are typically used, with an allocator being composed of arbiters, where an arbiter is one that chooses one out of N requests to a single resource. Figure below shows how each stage of a 3:4 separable allocator (an allocator matching 3 requests to 4 resources), is composed of arbiters. For instance, a separable VA will have the ﬁrst stage of the   allocator(comprised of our 3:1arbiters)select one of the eligible output VCs, with the winning requests from the ﬁrst stage of allocation then arbitrating for an output VC in the second stage (comprising three 4:1arbiters). Different arbiters have been used in practice, with round-robin arbiters being the most popular due to their simplicity. Figure  shows one potential outcome from a separable allocator. Figure  shows the request matrix. Each of the 3:1 arbiters selects one value of each row of the matrix; these ﬁrst stage results of the allocator are shown in the matrix in Figure below. The second set of 4:1 arbiters will arbitrate among the requests set in the intermediate matrix. The ﬁnal result  shows that only one of the initial requests was granted. Depending on the arbiters used and the initial states, more allocations could result.
 SEPARABLE   3:4 ALLOCATOR (3 requestors, 4 resources)                                                                  
	VIRTUAL CHANNEL:- 

When a physical channel is divided into a multiple number of logic channels, these logic channels are called as virtual channels. A virtual channel has its own queue, but it shares the bandwidth of the physical channel in a time multiplexed fashion. Virtual channels offer flexibility, better channel utilization and improve network throughput and reduce the effect of blocking. 

	FIFO BUFFERS:- 

The FIFO memory buffer is used as a memory buffer to store the incoming data temporarily. It is used for synchronizing the speeds of operation of the receiver and sender. The receiver and sender have different frequencies, and if sender sends at a higher frequency compared to receiver, receiver will get swamped with incoming data. In order to avoid this, and also for temporary storage of incoming data, we use FIFO (first-in-first-out) memory buffers. Buffer organization has a large impact on network throughput, as it heavily inﬂuences how efﬁciently packets share link bandwidth. Essentially, buffers are used to house packets or ﬂits when they cannot be forwarded right away onto output links. Flits can be buffered on the input ports and on the output ports.
             Figure a shows an input-buffered router where there is a single queue in each input port, i.e. there are no VCs. Incoming ﬂits are written into the tail of the queue, while the ﬂit at the head of the queue is read and sent through the crossbar switch and onto the output links (when it wins arbitration). 
                                                
	CROSSBAR:-

 A crossbar depicts the physical connections inside the switch allocator. It is a collection of switches arranged in a matrix configuration. The crossbar switch has multiple input and output lines that form a crossed pattern of interconnecting lines between which a connection may be established by closing a switch located at each intersection, the elements of the matrix. It switches bits from input ports to output ports, performing the essence of a router’s function. Figure below describes a crossbar switch, where input select signals to each multiplexer set up the connections of the switch, i.e. which input port(s) should be connected to which output port(s). Synthesizing this will lead to a crossbar composed of many multiplexers, multiplexer crossbar such as that illustrated in Figure .Most low-frequency router designs will use such synthesized crossbars.  
                                                                 DIAGRAM OF A CROSSBAR
                                                    
                              



ROUTING ALGORITHM IN NOC:
Network on Chip is another version of System on Chip which is implemented in the form of Network, so called as  NoC. The On Chip communication in NoC will take place through packet switching. Routing Algorithm determines in which path the packet can be transmitted from source to destination, for this function routers are used between nodes in the network, this routers will direct the packets depending upon the routing algorithm implemented in the design of NoC . So, it is in the hands of the designer to select a suitable network topology and routing algorithm to achieve best performance of the system that overcomes scalability and performance limitations. The routing algorithm of the designed NoC results in the performance of the system. Latency, throughput and load distribution are very important parameters to be considered while designing.

XY ROUTING ALGORITHM:
In network on chip (NoC), routing is done to interface distinctive components together in the most effective way. For instance selecting a short way might be imperative in one circumstance, while making a proper activity load adjusting may be of more prominent significance in other circumstance. A limitless number of routing algorithm have been proposed and every one of them have a few advantages and disadvantages contrasted with each other. One routing may be beneficial in one NoC while the same may not suit for the other NoC.
The XY routing algorithm is one of the distributed deterministic routing   algorithm . Deadlock or live lock never occurs in XY routing. XY routing algorithm is most suitable for networks implemented using mesh topology. The packets which occurs at the nodes are first routed in X-direction and then in the Y-direction. X-direction indicates movement of packets in horizontal direction and Y- direction indicates movement of packets in vertical direction to the receiver.  Routers address  are placed in XY coordinates.
XY ROUTING ALGORITHM :
![image](https://github.com/GayathriMatcha/Router-Implementation-vhdl/assets/98030485/2ff43a3b-f5d5-414b-880d-a66ef6e23a9c)

                                            
