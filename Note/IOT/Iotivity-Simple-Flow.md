---

title: Iotivity之Simple回调流程图
date: 2018-04-25 18:32:50
tags: [ IOT, C ]
categories: [ Note ]

---

```
=================Server====================================================================Client===================
                                                                                                                            
  +---------------------------+                                      +----------------------+                               
  |    OCResourceRequest      |                                      |   OC::OCResource     |                               
  |---------------------------|      +----------------------+        |----------------------|      +----------------------+ 
  |  messageID,representation |      | InProcServerWrapper  |        |   m_clientWrapper ---+----->| InProcClientWrapper  | 
  |  devAddr, query, options  |      |----------------------|        |  +---------------+   |      |----------------------+ 
  |  payload                  |      |    start/stop        |        |  |  OCResource   |   |      | start/stop           | 
  |  resourceHandle           |      |    registerResource  |        |  |---------------|   |      | ListenForResource    | 
  |  requestHandle            |      |                      |        |  | uri ...       |   |      |                      | 
  +------|--------------------+      |    bindxxxToResource |        |  | actionsetHead |   |      | xxxRepresentation    | 
         |                           |    [type/interface]  |        |  | childresHead  |   |      |   [Get/Put/Post]     | 
         v                           |                      |        |  | observerHead  |   |      |                      | 
  +---------------------+            |    startPresence     |        |  | entityHandler |   |      | ObserveResource      | 
  |  OCServerRequest    |            |    sendResponse      |        |  +---------------+   |      | SubscribePresence    | 
  |---------------------|            |----------------------|        |----------------------|      |----------------------| 
  |  ehResponseHandler  |            |    processFunc       |        | get/put/post/observe |      | listeningFunc        | 
  +--------+------------+            +----------------------+        |  subscribe/publish   |      +----------------------+ 
           |                                                         +----------------------+                               
           +--> HandleSingleResponse (for OCDoResponse)                                                                                                           
                                                                                                                            

                           entityHandler                 stack                   FoundResource                              
                                cb  uri-1                  |                          cb   call-1                           
        simpleserver            |                          |                           |        simpleclient                
                                |                          |                           |                                    
   Platform::registerResource   |                          |                           |    Platform::findResource          
              |                 |                          |                           |                |                   
              |  "/a/light"     |         OCCreateResource |                           |                |                   
              +-----------------+------------------------> | OCDoResource              |  "/oic/res"    |                   
                                | OCResourceRequest        | <-------------------------+----------------+                   
                                | <----------------------- |                           |                                    
                                |            OCDoResponse  |                           |  curResource                                 
         Platform::sendResponse | -----------------------> |              OCResource   |      ||                             
                                |                          | ------------------------> | "/a/light"                                  
                                |                          | OCDoResource              |                                    
                                | OCResourceRequest        | <------------------------ | GetResourceRepresentation
                          doGet | <----------------------- |                    onGet  |
                                |            OCDoResponse  |                     cb    |
         Platform::sendResponse | -----------------------> |         OCResource   |
                                |                          | -------------------> |
                                |                          | OCDoResource         |
                                | OCResourceRequest        | <--------------------| PutResourceRepresentation
                          doPut | <----------------------- |              onPut   |
                                |            OCDoResponse  |               cb     |
         Platform::sendResponse | -----------------------> |    OCResource  |
                                |                          | -------------> |
                                |                          | OCDoResource   |
                                | OCResourceRequest        | <------------- | PostResourceRepresentation
                         doPost | <----------------------- |        onPost  |
                          /     |                          |          cb    |
                         /      |                          |           |  
                        /       |                          |           |         FoundResource
                       /        |     entityHandler        |           |              cb  call-2
                      /         |          cb  uri-2       |           |               |
   Platform::registerResource   |          |               |           |               |    Platform::findResource
              |                 |          |               |           |               |                |
              |  "/a/light1"    |          |               |           |               |                |
              +-----------------+----------|-------------> |           |               |                |
                                |          |               | <---------|---------------+----------------+
                                | <--------|-------------- |           |               |
                                |          | OCDoResponse  |           |               |  curResource
         Platform::sendResponse | ---------|-------------> |           |               |      ||
                                |          |               | ----------|-------------> | "/a/light1"
                                |          |               |           |               |
         Platform::sendResponse | ---------|-------------> | --------> |               |
                                |          |               |           |                
                                |          |               |           |  "/a/light1" 
                                           |OC_REST_OBSERVE| <-------- | observeResource <--- OC::OCRecource::observe
                                           | <------------ | OCDoResource                         
                          Observers.insert |               |                             onObserve 
                                           |               |                                 cb    
                                           |               |                                 |     
                       |                   |               |                                 |     
         loop   +----- |                   |               |                                 |
                |      |                   |               |                                 |
                |      |                   | OCDoResponse  |                                 |
notifyListOfObservers  | ------------------|-------------> |      wrapper switch             |
                |      |                   |               | ------------------------------> |
                |      |                                                                           
                |      | -------------------------------------------------------------------  
                +----> |                 |--------------+     +-------------|                 
                       |                 |              |     |             |                 
                       |                 |              |     |             |                               
                       |                 |     loop    OCProcess    loop    |                               
                       |                 |              |     |             |                               
                       |                 |<-------------+     +------------>|                               
                       |                 |                                  |                               
  ChangeLightRepresentation           processFunc                       listeningFunc                       
         thread3                        thread2                           thread2                                              
                                                                                                            

                                                                                                            
```                                                                                                         
                                                                                                            
                                                                                      
                                                                                      
                                                                                      
                                                                                      
                                                                                      
                                                                                      
                                                                                      
                                                                                      
