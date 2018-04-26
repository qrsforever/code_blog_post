---

title: Iotivity之Presence&Observe流程图
date: 2018-04-26 11:00:21
tags: [ IOT, DrawIt ]
categories: [ Note ]

---

```
========Server===============================SDK===========================================Client============

    SERVER

    ((1))
    Platform::start                         OCInit2

                                            ((2))            "/oic/ad"  +------------------+
                                            OCCreateResource  --------> |   OCResource     |
                                                                  ----> |------------------|
                                +-----------------+              /      | uri,type,if,uuid |  (no active)
                                |PresenceResource |             /       | attrs,propers    |----> OC_OBSERVABLE
                                |-----------------|  handle    /        | actionsetHead    |  ^
                                |    handle       | ----------/         | childresHead     |  |
                                |    presenceTTL  |              /----- | observerHead     |  |
                                +-----------------+             /       | entityHandler    |  |
                                                               /        |------------------|  |
    ((3))                                      observerHead   /         |     next         |  |
    startPresence             /-------------------------------          +------------------+  |
                             /              ((4))                                             |
                            /               OCStartPresence                                   |
                           v                                                 OC_ACTIVE        |
              +-------------------+               ChangeResouceProperty  ---------------------+ (active)
              | ResourceObserver  |    TTL=0
              |-------------------| <------------ AddObserver
              | observeId         |
              | resUri            |                   OCPresenceTrigger
 see    ? <---+-TTL               |                           |
 ocobserve.h  | acceptFormat      |                  +--------+--------+
              | query,token       |                  |        |        |
              | devAddr           |                  v        v        v
              | lowQosCount  (NON)|                CREATE   CHANGE   DELETE
              | forceHighQos (CON)|                  |        |        |
              |-------------------|        OCCreateResource   |   OCDeleteResource
              |      next         |         +-----------------------------------+
              +-------------------+         | OCBindResource (collection)       |
 +------------+                             | OCBindResourceTypeToResource      |
 | OCPresence |                             | OCBindResourceInterfaceToResource |
 |------------|                             +-----------------------------------+
 |    TTL     |                                                                            CLIENT
 | timeout[l] |
 |   level    |        g_cbList                                                          [[1]]
 +------------+           |                                                thread2       Platform::start
      ^                   v                 OCInit2                        listeningFunc
      |   +-------------------+                                                   |
      |   |     ClientCB      |                                   "/oic/ad"       |      [[2]]
      |   |-------------------|\                          /-----------------------+----- subscribePresence
      |   |  requestUri       | \            [[3]]       /   SubscribeCallback    |       (Unicast)
      |   |  sequenceNumber   |  \          OCDoResource              |           |
      +---|  presence, TTL    |   \           /                       +-----------+--------\     thread2
          |  payload, method  |    \         /                                    |         ---> presenceHandler
          |  callback (cb-1)  |      AddClientCB                                  |                   cb-1
          +-------------------+    ( Don't Call OCSendRequest )                   |                    |
                                                                                  | loop:              |
     ((5))                                                                        |        thread2     |
     createResource                                                               | --> OCProcess      |
                                             ((6))                                |          |         |
                                            OCCreateResource                                 |         |
                            Trigger: CREATE          |                                       |         |
                        +----------------------------+                                       |         |
                        v                                                                    |         |
 +--------> SendPresenceNotification                                                         |         |
 |          SendAllObserverNotification                                                      |         |
 |            |         |                                                                    |         |
 |            v         |   "/oic/ad"       ((7))               "/oic/ad"                    |         |
 |  AddServerRequest    +-----------------> OCSendResponse  -------------------------------> |         |
 |                \                                                                          |         |
 |                 \                                                                         |         |
 |                  \                       [[4]]     ((8))                                  |         |
 |  g_svrRequestTree \                      OCHandleResponse <------------------------------ |         |
 |         |      +-------------------+        |                                             |         |
 |         +----> |  OCServerRequest  |        |  HandlePresenceResponse                     |         |
 |                |-------------------|        |                                             |         |
 |                | resourceUrl,reqID |        |  client = GetClientCBUsingUri("/oic/ad")    |         |
 |                | reqToken, query   |        |                                             |         |
 |                | payload, method   |        |  ResetPresenceTTL                           |         |
 |                | ehResponseHandler |        |                                             |         |
 |                +-------------------+        |  client->callback                           |         |
 |                                             |  -------------------------------------------|-------> | [[5]]
 |                                                                                           |         |
 |          thread2                                                                          |         |
 |          processFunc                                                          [[6]]       |         |
 |             |                                                                 OCProcessPresence     |
 |             | loop:                                                                  |              |
 |             |                                                                        | check ttl    |
 |             | --> OCProcess                                                          |              |
 |             |        |                                                               |              |
 |             |        |     "/oic/ad"     [[7.1]]             check timeout[l]        |              |
 |             |        | <---------------- OCSendRequest <---------------------------- | (Unicast)    |
 |             |        |                                     timeout    "/oic/ad"      |              |
 |             |        |                   ((9))      [[8]]                            |  check level |
 |             |        | ----------------> OCHandleRequests                            | -----------> |
 |                      |                    /                                          |   no ticks   | [[7.2]]
 |                            "/oic/ad"     /                                                          | STACK_PRESENCE_TIMEOUT
 |                           HandleVirtualResource                                                     |
 |   Trigger: CHANGE                 |                          DeleteClientCB  <--------------------- |
 +-----------------------------------+
                                                                               STACK_DELETE_TRANSACTION / KEEP
```
