---

title: Iotivity之请求响应流程图
date: 2018-04-24 16:40:06
tags: [ IOT, DrawIt ]
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
                 ((7))v          \        v             |                      |((6))      |          |      next      |          |
                 sendDataToAll    ---- sendData  CASendUnicastData     CASendMulticastData |          |  +----------+  |          |
                      ^                   ^             ^                      ^           |          |  | msg,size |  |          |
                      | g_adapterHandler  |             |                      |           |          |  +----------+  |          |
                      +---------+---------+             +-----------+----------+           |          +----------------+          |
                                |                                   |                      |                   ^                  |
                                |                                   |                      |                   |                  |
  get  put post delete          | +---------------------------------|-------------+        |                   +------+           |
   |    |    |    |             | | (x)                    |                      |        |                          |           |
   ----------------             | | CAReceiveThreadProcess | CASendThreadProcess  | <---+  |    CAQueueingThread      |           |
           ^                    | |                        |     thread3          |     |  |    +---------------------|-------+   |
           |                    | +-----------------------------------------------+     |  |    |            |                |   |
    CA_REQUEST_DATA   <--|      |                                   ^                   +-------- threadTask |   dataQueue    |   |
                         | code |    thread4           r-3          |                      |    |            |                |   |
    CA_SIGNALING_DATA <--|------+--- CAReceivedPacketCallback ------+-------\              |    +-----------------------------+   |
                         |      |     |  get code/token from PDU    |        \             |                                      |
    CA_RESPONSE_DATA  <--|      |     |                             |         \            +---------- queue_add_element <--------+
                                |     |  g_receiveThread       g_sendThread    \                              |
                                |     |         ^                   ^           \       r-4        ((5))      |      [[3]]
                                |     +-----+   | CAQueueingThread  |            ----------------> CAQueueingThreadAddData
                                |      set  |   +---------+---------+                                         ^
                                |           |             |                                                   |
+-------------------------------+-----------+-------------+---------------------------------------------------+--------------+
| DIR: connectivity             |           |             |                                                   |              |
|                                           |             |                                                   |              |
|                       interfacecontroller |             |                   retransmission                  |              |
|                               |           |             |                          |                        |              |
|                               |           |                                        |                                       |
| connectivitymanager           |           |      messagehandler                    |                 queueingthread        |
|        |                      |           |             |                          |                        |              |
+--------+----------------------+-----------+-------------+--------------------------+------------------------+--------------+
         |                      |           |             |                          |                        |
    h-1  |                      |           |  h-2        |                          |             h-4        |
    CAInitialize                |           |  CAInitializeMessageHandler            |             CAQueueingThreadInitialize
                                |           |      /                                 |             CAQueueingThreadStart
                                |           |     /CAReceivedPacketCallback          |                 (thread create)
                      h-3       |      r-1  v    /                                   |
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
    h-7                                                      |                                     | getNetInfo        ((8))  |
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
| +---------------------------------+                                                     g_serverRequestTree
| | HandleVirtualResource           |   call               cb-1                                   |
| | HandleResourceWithEntityHandler |  -----> resource->entityHandler()                           |GetServerRequestUsingToken
| | HandleDefaultDeviceEntityHandler|                                                             |
| +---------------------------------+  /----> resouce = FindResourceByUri(request->resourceUrl)   v
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
|       cb-a  |   thread2                         cb-b   |                                  cb-c    |                         |
|       HandleCARequests [get|put|post|delete]    HandleCAResponses                         HandleCAErrorResponse             |
|             ^                                          ^                                          ^                         |
|             |   r-7                                    |  r-7                                     |  r-7                    |
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
                                                                                                                              |
====================================================================================================================          |
                                                   |                                                                          |
                                                   |                                                                          |
                               RPC      <--------- | --------->      RPC                                                      |
                                                   |                                                                          |
                                         a         |          1                                                               |
                                OCDoResponse       |         OCDoRequest                                                      |
                                        \                      /                                                              |
                            +------------\--------------------/------------+                                                  |
                            |             \  b          2    /             |                                                  |
                   SERVER   |      CASendResponse     CASendRequest        |   CLIENT                                         |
                            |              \                /              |                                                  |
                     ^      |               \ c         3  /               |     ^                                            |
                     |      |             CADetachSendMessage              |     |                                            |
                     |      |                      ^                       |     |                                            |
                 ---------  |                      |                       | ---------                                        |
                     |      |            d         v         4             |     |                                            |
                     |      |           CAHandleRequestResponse            |     |                                            |
                     v      |                /            \                |     v                                            |
                            |         e     /              \     5         |                                                  |
                   CLIENT   |   HandleCAResponses     HandleCARequests     |   SERVER                                         |
                            |             /                  \             |                                                  |
                            +------------/--------------------\------------+                                                  |
                                   f    /                      \     6                                                        |
                                OCHandleResponse         OCHandleRequests                                                     |
                                                                                                                              |
                                                                                                                              |
=================server==================================sdk========================================================          |
                                                                                                                              |
     {{1}}                                                                                                                    |
     Platform::registerResource                                                                                               |
              |                           cb-1                                                                                |
              |(uri,type,interface,attr,eHandler)    {{2}}                                                                    |
              +---------------------------------->   OCCreateResource                                                         |
                                        thread2       insertResource         headResource                                     |
                                                                                 |                                            |
                                                    +----------------+           |                                            |
                                                    |   OCResource   | <---------+ (resourceHandler)                          |
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
               |     head   |   next     |    | id | uri | query | token | devAddr | qos | next |                             |
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
               AddClientCB                                                          [Find|Get|Put|Post]Callback               |
                                                       +--------------------+                                                 |
                g_cbList ----------------------------> |    ClientCB        |                                                 |
                                                       |--------------------|      +--> con                                   |
                                    text <---+         |    callback(cb-2)  |      |                                          |
  GetClientCBUsingUri                        |         |    handle (random) |      +--> non                                   |
  GetClientCBUsingToken             xml  <---+         |    type            |------|                                          |
  GetClientCBUsingHandle                     |         |    token  (random) |      +--> ack                                   |
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
  Wrapper:                                                                      \                                             |
                                                                                 -----> requestHandle->ehResponseHandler      |
    +---------------------------+ c++              c +------------------------+                        cb-3                   |
    |    OCResourceRequest      |                    | OCEntityHandlerRequest |                                               |
    |---------------------------|                    |------------------------|                                               |
    |  messageID,representation |                    |    method, messageID   |                                               |
    |  devAddr, query, options  | formResourceRequest|    devAddr, query      |                                               |
    |  payload                  |<-------------------|    options             |                                               |
    |  requestHandle            |                    |    payload             |     requestHandle                             |
    |  resourceHandle           |                    |    requestHandle       |-----------------------------------------------+
    +---------------------------+                    |    resourceHandle      |                                               |
                |                                    +------------------------+                                               |
                |  get/put/post                                                                                               |
                v                                  c +------------------------+                                               |
             +-------------------+ c++               |OCEntityHandlerResponse |                                               |
             |OCResourceResponse |                   |------------------------|     requestHandle                             |
             |-------------------| form              |     requestHandle      |-----------------------------------------------+
             |  newResourceUri   |------------------>|     ehResult           |
             |  interface        |                   |     resourceUri        |
             |  headerOptions    |                   |     resourceHandle(x)  |
             |  representation   |                   |     payload            |
             |  requestHandle    |                   +------------------------+
             |  resourceHandle   |
             +-------------------+
```
