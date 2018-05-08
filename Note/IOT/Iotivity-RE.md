---

title: Iotivity之Resource-Encapsulation
date: 2018-05-08 12:58:38
tags: [ IOT ]
categories: [ Note ]

---               
                                                                              
                                                                              
```                                                                             
                                                                              
                                                                                                               
                                                                                                               
                                                                                                               
                                                                                                               
                                                                                                               
                                                                                                               
                                       +---------------------------+ +--------------------------+              
                                       | ResourceAttributesBuilder | |  OCRepresentationBuilder |      
          +----------------------+     |---------------------------| |--------------------------|     +--------------------+
          | RCSResourceAttributes|<----|      m_target             | |      m_target            |---->|OC::OCRepresentation| 
          +----------------------+     |---------------------------| |--------------------------|     +--------------------+
                                       |       extract()           | |       extract()          |               ^
                                       +---------------------------+ +--------------------------+               |
                                                       \                       /                                |
                                                        \                     /                                 |
                                                         \                   /                                  c1           
                                                    +-------------------------------+                +--------------------+ 
                                                    |  ResourceAttributesConverter  |                |   RequestHandler   | 
                                                    |-------------------------------|                |--------------------| 
                                                    |      fromOCRepresentation     |                |    m_errorCode     | 
                                                    |      toOCRepresentation       |            --> |    m_customRep     | 
                                                    +-------------------------------+           /    |    m_ocRep         | 
                                                                                               /     |--------------------| 
                                                                                              /      |  getRepresentation |                              
                                                                                             /       +--------------------+                              
                                                                m_handler                    |                  e1                                        
                                                               +-----------------------------+                  |                                        
                                                               |                                                |                                        
   +------------------+                +-----------------+     |        +-----------------+         +-----------------------+                            
   |  RCSRequest      |                |  RCSGetResponse |     |        |  RCSSetResponse |         |   SetRequestHandler   |                                                                                                                        
   |------------------|                |-----------------|     |        |-----------------|         |-----------------------|                                                                                                                        
   | m_resourceObject |                |    m_handler    |c1---+        |    m_handler    |c1------>|                       |                                                                                                                        
   | m_ocRequest      |                |-----------------|              |-----------------|         | applyAcceptanceMethod |                                                                                                                        
   +------------------+                |  defaultAction  |              |  defaultAction  |         +-----------------------+                                                                                                                        
           |                           |    create       |              |     create      |                                                                                                                                                          
           | m_ocRequest               +-----------------+              +-----------------+                                                                                                                                                          
           v                                                                                                                                                                                                                                         
   OCResourceRequest                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                     
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                                         
                                                                                                                                  
                                                                                                              
                                                                                RCSResourceAttributes         
                                                                                                              
       OCResourceHandle                                                               m_values                
                                                                                                              
                                                                                      visit                   
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                                                              
                                                                              
                                                                              
                                                                              
                                                                              
                                                                              
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                               RCSResourceObject                                                                              
                                                                                                                                              
                                              m_resourceAttributes                                                                            
                                              m_interfaceHandlers                                                                                            
                                              m_getRequestHandler                                                                            
                                              m_setRequestHandler                                                                             
                                                                                                                                              
                                           set[Get/Set]RequestHandler                                                                
                                               [get/set]Attribute                                                                             
                                                   notify                                                                                     
                                                 sendResponse                                                                                 
                                                 entityHandler                                                                                
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                                                                                                                              
                                                      |                                   handleRequest                                       
                                                      |                                        |                                             
                                                      | RequestFlag                 "get"      |      "post"                                
                                                      |                             -----------+-------------                                       
                                                      |                             |                       |                                        
          handleObserve                               |                             v                       v                                        
                                                      | ObserverFlag         handleRequestGet        handleRequestSet                           
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             

                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                         
                SampleResourceServer                                                                                  SampleResourceClient                                   
                      |                                                                                                                                            
                      |--> startPresence                                                                                                                           
                      |                  
                      |--> initServer 
                      |                            
                      |     g_resource (RCSResourceObject)
                      |         |                                                                                                                                            
                      |         |--> setAutoNotifyPolicy                                                                                                                      
                      |         |--> setSetRequestHandlerPolicy                                                                                                                       
                      |         |                                                                                                                                            
                      |         |--> setAttribute("Brightness")                                                                                                                      
                      |         |--> setGetRequestHandler ------------+                                                                                                         
                      |         |--> setSetRequestHandler ------------+                                                                                                         
                      |         |                                     |                                                                                    
                      |                                               |                                                                                   
                 loop |--> updateAttribute                            |                                                                                                      
                      |                                               |                                                                                            
                      |     g_resource (RCSResourceObject)            |
                      |         |                                     |                                                                                             
                      |         |--> getAttributes                    |                                                                                                           
                      |         |                                     |                                                                                             
                      |         |--> getAttributeValue                |                                                                                                                    
                      |         |                                     |                                                                                            
                                                                      |                                                                                            
             +----------------------------------------+---------------+                                                                                            
             |                                        |
             v                                        v                                                                                                            
   requestHandlerForGet                     requestHandlerForSet                                                                                              
     |                                                                                                                                                             
     |--> printAttributes(req->attrs)                                                                                                                                          
     |
     |--> printAttributes(res->attrs)                                                                                                                                          
     |                                                                                                                                                             
     |                                                                                                                                                             
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   

                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   

                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                                                                                                                                                                   
                 +--------------------+                                                                                                                           
                 |   RequestHandler   |                                        
                 |--------------------|                                                                                                                            
                 |    m_errorCode     |            +-----------------------+                                                                                       
                 |    m_customRep     |            |   SetRequestHandler   |                                                                                      
                 |    m_ocRep         | e3-------- |-----------------------|                                                                                       
                 |--------------------|            |                       |                                                                                       
                 |  getRepresentation |            | applyAcceptanceMethod |                                                                                       
                 +--------------------+            +-----------------------+                                                                                        
                           |                                   |                                                                                                    
                           |  m_handler             m_handler  |                                                                                                   
                           c1                                  c1                                                                                                  
                  +-----------------+                +-----------------+                           +------------------+                                      
                  |  RCSGetResponse |                |  RCSSetResponse |                           |  RCSRequest      |                                      
                  |-----------------|                |-----------------|                           |------------------|                                      
                  |    m_handler    |                |    m_handler    |                           | m_resourceObject |                                      
                  |-----------------|                |-----------------|                           | m_ocRequest      |                                      
                  |  defaultAction  |                |  defaultAction  |                           +------------------+                                      
                  |    create       |                |     create      |                                   |                                                 
                  +-----------------+                +-----------------+                                   | m_ocRequest                                                
                                                                                                           v                                                 
                                                                                                   OCResourceRequest                                         
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
      +------------------------+                                                                                                                             
      |       Builder          |                                                                                                                             
      |------------------------|                                                                                                                             
      |  m_uri                 |                                                                                                                             
      |  m_types               |                                                                                                                             
      |  m_interfaces          |                                                                                                  
      |  m_defaultInterface    |      OC_DISCOVERABLE                                                                             
      |  m_properties          |----> OC_OBSERVABLE                                                                               
      |  m_resourceAttributes  |      OC_SECURE                                                                                   
      |------------------------|                                                                                                  
      |  build()               |                                                                                                  
      +------------------------+                                                                                                  
          |                                                                                                                       
          +-> server = RCSResourceObject()                                                                                        
          |                                                                                                                       
          +-> registerResource()                                                                                                  
          |                                                                                                                       
          +-> bindInterfaceToResource()                                                                                           
          |                                                                                                                       
          +-> bindTypeToResource()                                                                                                
          |                                                                                                                       
          +-> server->init()                                                                                                      
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
                                                                                                                                                             
```                                                                                                                         
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                                                                                            
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
                                                        
