---

title: IOT之规则引擎
date: 2018-05-27 21:04:19
tags: [ IOT ]
categories: [ Note ]

---

Framework Design
================

```
v0.0.1  
                             ╔═════════════════╦════════════════════════════════════════╗                    
                             ║                 ║                                        ║                       
                             ║                 ║ Log / MQ / Json / DataChannel / Time   ║                       
                             ║                 ║                                        ║                       
                             ║     Rules       ╠════════════════════════════════════════╣                       
                             ║   Translate     ║                                        ║                       
                             ║                 ║         Rule Engine Manage             ║                       
                             ║                 ║                                        ║                       
                             ║                 ║                                        ║                       
                             ╠════════════╦════╩══════╦═╦═══════════════════════════════╣                       
                             ║            ║           ║ ║                               ║                       
                             ║   Global   ║  Devices  ║ ║      Clips C++ Interface      ║                       
                             ║            ║           ║ ║                               ║                       
                             ╠═══════════CLP══════════╣ ╠═══════════════════════════════╣                       
                             ║            ║           ║ ║                               ║                       
                             ║    Rules   ║   Utils   ║ ║         Clips Core            ║                       
                             ║            ║           ║ ║                               ║                       
                             ╚════════════╩═══════════╩═╩═══════════════════════════════╝                       
                                                                                                                                      
```

Class Diagram
=============

```
                                                                                                                                  
                                                                                                                                  

Message         MessageQueue
                                                                                                                                  
arg1/arg2/Obj
target

 next 
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                          MessageHandler                                                                                                        
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
                                                                                                                                  
```



Detail Tasks
============

1. Common Modules: Message Queue and Message Handle, Log

2. C++ Clips Interface

3. Rule Engine Manage

4. CLP files: Global, Devices, Utils, Rules

5. Rules Translate


Test Supported
==============

  Items  |  Supported 
:-------:|:----------:
and|yes
or|yes 
timer|no
update trigger|yes
state trigger|no
 
