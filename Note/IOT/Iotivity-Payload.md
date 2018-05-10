---

title: Iotivity之Payload
date: 2018-05-10 12:41:16
tags: [ IOT ]
categories: [ Note ]

---

```
                            
        +-----------------+                                                 +-------------------+
        |OCSecurityPayload|----------------------+                        / | OCPresencePayload |                                 
        |-----------------|                      |                       /  |-------------------|                                 
        |      base       |                      e2                     /   |       base        |                                 
        |                 |               +-------------+              /    |                   |                                
        | secureData/size |               |  OCPayload  | e3-----------     |  sequenceNumber   |                                
        +-----------------+               |-------------|                   |  maxAge/trigger   |                                
                                          |    type     |                   |   resourceType    |                                 
                                          +-------------+                   +-------------------+                                
                                             e1      e1                                                                           
                                             |       |                                                                            
                                    /--------+       |                                                                            
                                   /                 |                                                                            
          +---------------------+ /                  |           +----------------------+                                         
          | OCDiscoveryPayload  |/                   +---------- |    OCRepPayload      |                                         
          |---------------------|                                |----------------------|                                             
          |       base          |                                |        base          |                                             
          |                     |                                |                      |                                                            
          | sid/name/type/iface |                                | uri/types/interfaces |                                              
   +----c1|     resources       |                                |       values         |                                         
   |      |---------------------|                                |----------------------|                                             
   |      |       next          |                                |        next          |                                         
   |      +---------------------+                                +----------------------+                                         
   |                                                                                                                              
   v                                                                                                                              
 +------------------------+                                                                                                       
 |  OCResourcePayload     |                                                                                                       
 |------------------------|                                                                                                       
 | uri/types/Interfaces   |                                                                                                       
 | anchor/port/secure/rel |                                                                                                            
 |------------------------|                                                                                                            
 |        next            |                                                                                                                          
 +------------------------+                                                                                                                         
                                                                                                                                                        
                                                                                                                                       
                                                                                                                                         
                                           
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                      
                                                                                                                                      
                                                                                                                                                 
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
           OCDiscoveryPayloadCreate                                                                                               
                                                                                                                                      
           OCDiscoveryPayloadAddNewResource                                                                                       
                                                                                                                                                 
                                                                                                                                                
                                                                                                                                 
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
```
