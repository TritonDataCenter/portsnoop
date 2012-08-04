# portsnoop: trace event port activity

portsnoop is a DTrace script to trace event port activity on illumos-based
systems.  It traces:

    * port\_get or port\_getn when either one fails or returns at least 1 event
    * the first 30 events returned by each port\_get or port\_getn invocation
    * all calls to port\_associate and port\_dissociate
    * calls to read, write, accept, or close between a returned event and a call
      to port\_associate for the same file descriptor

There are probably useful cases that are missing.  Contributions welcome.

Here's an example of watching Node.js:

    TIME        TID  SYSCALL             FD  DETAILS                    ERROR      
     12.208422    1  port_associate       6  0x01
     12.242194    1  port_associate       9  0x01
     12.255736    1  port_getn            -  1 events
     12.274277    1   PORT_SOURCE_FD      9  fdevents 0x01
     12.328980    1      read             9  = 385                    
     12.271295    1  port_associate       9  0x01
     12.288007    1  port_getn            -  1 events
     12.298038    1   PORT_SOURCE_FD      4  fdevents 0x01
     12.328153    1      read             4  = 1                      
     12.629188    1  port_associate       4  0x01
     12.645012    1  port_getn            -  1 events
     12.654589    1   PORT_SOURCE_FD      4  fdevents 0x01
     12.682737    1      read             4  = 1                      
     12.416725    1  port_associate       4  0x01
     12.432715    1  port_getn            -  1 events
     12.442648    1   PORT_SOURCE_FD      4  fdevents 0x01
     12.471463    1      read             4  = 1                      
     12.508922    1  port_associate       4  0x01
     12.521278    1  port_getn            -  1 events
     12.528865    1   PORT_SOURCE_FD      4  fdevents 0x01
     12.553993    1      read             4  = 1                      
     12.574697    1  port_associate       4  0x01
     12.822090    1  port_getn            -  1 events
     12.831173    1   PORT_SOURCE_FD      6  fdevents 0x01
     12.869254    1      accept           6  = 11                     
     12.088036    1      accept           6  = -1                       (errno = 11)
     12.125804    1  port_associate       6  0x01
     12.149611    1  port_associate      11  0x01
     13.033215    1  port_getn            -  1 events
     13.043168    1   PORT_SOURCE_FD      6  fdevents 0x01
     13.078822    1      accept           6  = 12                     
     13.145472    1      accept           6  = 23                     
     13.194127    1      accept           6  = -1                       (errno = 11)
     13.225361    1  port_associate       6  0x01
     13.248414    1  port_associate      12  0x01
     13.269476    1  port_associate      23  0x01
     13.310552    1  port_getn            -  1 events
     13.320322    1   PORT_SOURCE_FD      6  fdevents 0x01
     13.353921    1      accept           6  = 30                     
     13.431817    1      accept           6  = 37                     
     13.478750    1      accept           6  = -1                       (errno = 11)
     13.510167    1  port_associate       6  0x01
     13.533580    1  port_associate      30  0x01
     13.554498    1  port_associate      37  0x01

