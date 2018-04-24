---

title: Iotivity之请求响应流程图
date: 2018-04-24 16:40:06
tags: [ IOT, C ]
categories: [ Note ]

---

```
                                                                                                   CAQueueingThreadBaseRoutine
                                                                                                              | thread3 (send)
                                                                                                              |
                                                                                           +---------> queue_get_element ---------+
                                                                                           |                                      |
                                                                                           |          +----------------+          |
                                                                                           |          |      next      |          |
                                                                                           |          |  +----------+  |          |
                                                                                           |          |  | msg,size |  |          |
                                                                                           |          |  +----------+  |          |
                                                                                           |          +----------------+          |
                                                                                           |                   ^                  |
                      +--------------------------------------------------------+           |                   |                  |
                      |     thead5        +-------------+                      |           |                   |                  |
                      |         \         |             |                      |           |          +----------------+          |
                      v          \        v             |                      |           |          |      next      |          |
                 sendDataToAll    ---- sendData  CASendUnicastData     CASendMulticastData |          |  +----------+  |          |
                      ^                   ^             ^                      ^           |          |  | msg,size |  |          |
                      | g_adapterHandler  |             |                      |           |          |  +----------+  |          |
                      +---------+---------+             +-----------+----------+           |          +----------------+          |
                                |                                   |                      |                   ^                  |
                                |                                   |                      |                   |                  |
                                | +---------------------------------|-------------+        |                   +------+           |
                                | | (x)                    |                      |        |                          |           |
                                | | CAReceiveThreadProcess | CASendThreadProcess  | <---+  |    CAQueueingThread      |           |
                                | |                        |     thread3          |     |  |    +---------------------|-------+   |
                                | +-----------------------------------------------+     |  |    |            |                |   |
                                |                                   ^                   +-------- threadTask |   dataQueue    |   |
                                |    thread4           r-3          |                      |    |            |                |   |
                                |    CAReceivedPacketCallback ------+-------\              |    +-----------------------------+   |
                                |         (parse pdu)               |        \             |                                      |
                                |                                   |         \            +---------- queue_add_element <--------+
                                |        g_receiveThread       g_sendThread    \                              |
                                |               ^                   ^           \       r-4        ((5))      |      [[3]]
                                |               | CAQueueingThread  |            ----------------> CAQueueingThreadAddData
                                |               +---------+---------+                                         ^
                                |                         |                                                   |
+-------------------------------+-------------------------+---------------------------------------------------+--------------+
| DIR: connectivity             |                         |                                                   |              |
|                               |                         |                                                   |              |
|                       interfacecontroller               |                   retransmission                  |              |
|                               |                         |                          |                        |              |
|                               |                         |                          |                        |              |
| connectivitymanager           |                  messagehandler                    |                 queueingthread        |
|        |                      |                         |                          |                        |              |
+--------+----------------------+-------------------------+--------------------------+------------------------+--------------+
         |                      |                         |                          |                        |
    h-1  |                      |              h-2        |                          |             h-4        |
    CAInitialize                |              CAInitializeMessageHandler            |             CAQueueingThreadInitialize
                                |                                                    |             CAQueueingThreadStart
                                |                                                    |                     (create)
                      h-3       |      r-1                                           |
                      CASetPacketReceivedCallback                                    |
                      CASetErrorHandleCallback                                       |
                                                                         h-5         |
                         g_adapterHandler  --------------\               CARetransmissionInitialize
                                                          \              CARetransmissionStart
                                                           \ 1...n
             CAInitializeRA             CAInitializeNFC     ------------------------\
                       \                  /                                          \             +--------------------------+
                        \                /                                            -----------> |   CAConnectivityHandler  |
                      h-6\  transtype   /                                 r-2                      |--------------------------|
                      CAInitializeAdapters                   +--> CAReceivedPacketCallback  <--------startAdapter  (thread4)  |
                      /         |        \                   |                                     | stopAdapter              |
                     /          |         \                  +--> CAAdapterChangedCallback         | startListenServer        |
                    /           |          \          params |                                     | stopListenServer         |
       CAInitializeTCP   CAInitializeEDR   CAInitializeIP ---+--> CAConnectionChangedCallback      | startDiscoveryServer     |
                                                             |                                     | sendData      (unicast)  |
                                                         non +--> CAAdapterErrorHandleCallback     | sendDataToAll (muticast) |
    h-7                                                      |                                     | getNetInfo               |
    CASelectNetwork                                          +--> CARegisterCallback  -----------> | readData                 |
                                                                                         handler   | terminate                |
                                                                                                   | transportType            |
    h-8                                        h-9                                                 +--------------------------+
    CARegisterHandler                          CASetInterfaceCallbacks
      /     |     \                             /         |         \
     /      |      \                           /          |          \
    /       |       \     call                /           |           \
Request  Response  Error  <--- g_requestHandler   g_responseHandler   g_errorHandler
 cb-a     cb-b     cb-c             cb-a                cb-b               cb-c


=====================================================================================================================


                                               h-11                                    r-6
+--------------------------------------------> CAHandleRequestResponseCallbacks (queue_get_element)
|                                                       thread2
| +---------------------------------+
| | HandleVirtualResource           |   call               cb-1
| | HandleResourceWithEntityHandler |  -----> resource->entityHandler()
| | HandleDefaultDeviceEntityHandler|
| +---------------------------------+  /----> resouce = FindResourceByUri(request->resourceUrl)
|       ^                             /    +---------------------------------------------------------------+
|  h-12 |                            / +-->|                       OCServerRequest                         |<-----------------+
|  ProcessRequest     +-------------/  |   |---------------------------------------------------------------|                  |
|       ^             |                |   |   method, resourceUrl, acceptFormat, payloadFormat, devAddr   |                  |
|       | (by uri)    |                |   |   numResponses, qos, options, observeResult, delayedResNeeded |                  |
|DetermineResourceHandling             |   |   ehResponseHandler, requestId, requestToken, coapID, query   |                  |
|       ^                              |   +---------------------------------------------------------------+                  |
|       |          AddServerRequest----+              |                                                                       |
|       | +3        +2 ^                      [[2]]   v       cb-3                                                            |
|       |              |                      HandleSingleResponse                                                            |
|      HandleStackRequests                                   |                                                                |
|             ^  | +1                                        v                                                                |
|             |  |                                   OCSendResponse --->  CASendResponse  --->  CAQueueingThreadAddData       |
|             |  |                                                                                                            |
|             |  +--> GetServerRequestUsingToken         +---->  GetClientCBUsingToken  <-----------+                         |
|             |                                          |               |                          |                         |
|             |                                          |               |                          |                         |
|       OCHandleRequests                          OCHandleResponse       |      cb-2                |                         |
|             ^                                          ^             client->callback()           |                         |
|       cb-a  |                                   cb-b   |                                  cb-c    |                         |
|       HandleCARequests                          HandleCAResponses                         HandleCAErrorResponse             |
|             ^                                          ^                                          ^                         |
|             |   r-7                                    |                                          |                         |
|             +------------------------------------------.------------------------------------------+                         |
|                                                receive | handle                                                             |
|                                                        |                                                                    |
|                                                       CSDK                                                                  |
|                                          +-----------------------------+                                                    |
|                                          |                             |                                                    |
|                                          |    OCInitializeInternal     |                                                    |
|           server sample                  |             ^               |                   client sample                    |
|     +-----------------------+            |             |               |             +----------------------+               |
|     |                       |            |             |               |             |                      |               |
|     |Platform::start()   ----------------+--------> OCInit2 <----------+-----------------  Platform::start()|               |
|     |                       |            |                             |             |                      |               |
|     |                       |            |                             |             |                      |               |
|     |     +--------------+  |            |                             |             |   +--------------+   |               |
|     |     |     start    |  |            |                             |             |   |     start    |   |               |
|     |     |              |  |            |                             |             |   |              |   |               |
|     |     |              |  |thread2     |           r-5               |      thread2|   |              |   |               |
|     |     |  processFunc  ---------------+-------> OCProcess <---------+-----------------  listeningFunc|   |               |
|     |     |              |  |            |             |               |             |   |              |   |               |
|     |     +--------------+  |            |             |               |             |   +--------------+   |               |
|     |    wrapper            |            +-------------+---------------+             |   wrapper            |               |
|     |                       |                          |                             |                      |               |
|     +-----------------------+                          |                             +----------------------+               |
|     main thread                               /--------+                                          main thread               |
|                                              /                                                                              |
|                          /------------------/                                                                               |
|                         /                                                                                                   |
|   h-10                 v                                                                                                    |
+-- CAHandleRequestResponse                                                                                                   |
                                                                                                                              |
=================server==================================sdk========================================================          |
                                                                                                                              |
     {{1}}                                                                                                                    |
     Platform::registerResource                                                                                               |
              |                           cb-1                                                                                |
              |(uri,type,interface,attr,eHandler)    {{2}}                                                                    |
              +---------------------------------->   OCCreateResource                                                         |
                                                      insertResource         headResource                                     |
                                                                                 |                                            |
                                                    +----------------+           |                                            |
                                                    |   OCResource   | <---------+                                            |
                                OCResourceType      |----------------|                                                        |
                               +-------------+      |  uri           |      OCResourceInterface                               |
                               | next | name | <----|  resType       |       +-------------+                                  |
                               +-------------+      |  resInterface  |-----> | name | next |                                  |
                           OCResourceProperty       |  resAttributes |-- --+ +-------------+                                  |
                    +------+--------+-----+---------|  resProperties |     |                                                  |
                    |      |        |     |         |  actionsetHead |     |       OCAttribute                                |
                    |      v        |     v   +-----|  childresHead  |---+ |   +---------------------+                        |
                    |  discoverable |  active | +---|  observerHead  |   | +-> | name | value | next |                        |
                    |               |         | |   |  entityHandler |   |     +---------------------+                        |
                    v               v         | |   |  uuid  (cb-1)  |   |                                                    |
                  slow         observable    /  |   |----------------|   |       OCChildResource                              |
                                            /   |   |       next     |   |     +-----------------+                            |
                                     +------    |   +----------------+   +---> | resource | next |                            |
                    OCActionSet      v          |                              +-----------------+                            |
               +-------------------------+      |                                                                             |
               | name | timesteps | type |      v ResourceObserver                                                            |
               |-------------------------|    +-------------------------------------------------+                             |
               |     head   |   next     |    | id | uri | query | token | devAddr | qoc | next |                             |
               +-------------------------+    +-------------------------------------------------+                             |
                                                                                                                              |
=========================================================sdk======================================client============          |
                                                                                                                              |
                                                                                           ((1))                              |
                                                                                           Platform::findResource             |
                                                                                                        |                     |
                                                     ((2))                                              |                     |
                         gen: resHandle & token      OCDoResource    <----------------------------------+                     |
                       /---------------------------- OCDoRequest       (host, uri, conntype, callback)                        |
                      /                                                                          | cb-2                       |
               ((3)) /                                                                           |                            |
               AddClientCB                                                                  FindCallback                      |
                                                       +--------------------+                                                 |
                g_cbList ----------------------------> |    ClientCB        |                                                 |
                                                       |--------------------|      +--> con                                   |
                                    text <---+         |    callback(cb-2)  |      |                                          |
                                             |         |    handle (random) |      +--> non                                   |
                                    xml  <---+         |    type            |------|                                          |
                                             |         |    token  (random) |      +--> ack                                   |
                                    json <---+         |    options         |      |                                          |
                                             |         |    payload         |      +--> reset            discover             |
                                    cbor <---+---------|    payloadFormat   |                               ^                 |
                 ip <---+                              |    context         |                               |                 |
                        |   +---------------+          |    method          +---+-----+-----+-----+-----+---+                 |
          bluetooth <---+   |   OCDevAddr   |          |    sequenceNumber  |   |     |     |     |     |                     |
                        |   |---------------|          |    requestUri      |   v     v     v     v     v                     |
                nfc <---+---|    adapter    |<---------|    devAddr         |  get   put  post  delete observe                |
                        |   |    flags      |          |--------------------|                                                 |
                nfc <---+   |    port       |          |    next            |                                                 |
                        |   |    addr       |          +--------------------+                                                 |
               xmpp <---+   |---------------|                                                                                 |
                            |   routeData   |                                                                                 |
                            |   remoteId    |        ((4))                                                                    |
                            +---------------+        OCSendRequest  --->  CASendRequest  --->  CAQueueingThreadAddData        |
                                                                                                                              |
                                                                                                                              |
=================server==================================sdk========================================================          |
                         entityHandler (request)                                                                              |
                            / [[0]]                                                                                           |
     [[1]]                 /                                                       Request  ---->  Response                   |
     Platform::sendResponse                                                                                                   |
                |                                                                                                             |
                |  (response)                        [[2]]                                                                    |
                +--------------------------------->  OCDoResponse   -----------\ call                                         |
                                                                                \                                             |
                   +-------------------+ c++     c +------------------------+    -----> requestHandle->ehResponseHandler      |
                   |OCResourceResponse |           | OCEntityHandlerRequest |                            cb-3                 |
                   |-------------------|           |------------------------|                                                 |
                   |  newResourceUri   |           |    method, messageID   |                                                 |
                   |  interface        |           |    devAddr, query      |                                                 |
                   |  headerOptions    |<----------|    options             |                                                 |
                   |  representation   |           |    payload             |     requestHandle                               |
                   |  requestHandle    |           |    requestHandle       |-------------------------------------------------+
                   |  resourceHandle   |           |    resourceHandle      |                                                 |
                   +-------------------+           +------------------------+                                                 |
                            |                                                                                                 |
                            |                    c +------------------------+                                                 |
                            |                      |OCEntityHandlerResponse |                                                 |
                            +--------------------> |------------------------|     requestHandle                               |
                                                   |     requestHandle      |-------------------------------------------------+
                                                   |     ehResult           |
                                                   |     resourceUri        |
                                                   |     resourceHandle(x)  |
                                                   |     payload            |
                                                   +------------------------+
 ```
