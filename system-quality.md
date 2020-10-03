### System Quality Attributes

### Availability

System is not 100% available if:
- it sometimes fails to respond to an input
- it repeatedly fails to respond to an input
- it responds but the response is early or late
- it responds with an incorrect value

System availability should be guaranteed in all these states:
- normal operation
- startup
- shutdown
- repair mode
- degraded operation
- overloaded operation

In case of fault, these steps are recommended:
- Log the fault (so that it's possible to understand what happened)
- Notify appropriate entities (people or systems)  

Recover from the fault:  
- Disable source of events causing the fault
- Be temporarily unavailable while repair is being effected
- Fix or mask the fault/failure or contain the damage it causes
- Operate in a degraded mode while repair is being effected

Availability depends on:
- Time to detect the fault
- Time to repair the fault
- Time or time interval in which system can be in degraded mode

#### Availability strategies:

- Ping/echo (with timeout)
- Monitoring of system state
- Heartbeat
- Redundancy: voting, replication, functional redundancy
- Self-test
