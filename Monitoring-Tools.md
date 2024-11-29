# Monitoring Tools: Observing Grafana for Near Misses

### The following provides a detailed description of the UI elements that make up the Grafana dashboard that are important for monitoring site health. This will be your main tool for discovering near misses as a TSE, Dev, or SRE and will assist you in resolving site issues proactively. 

#### Grafana Sections

ℹ️ The Grafana monitoring UI provides a comprehensive dashboard for checking and monitoring the health of each site. It is accessible from your applications and Monitoring dashboard at https://hooli.ai/if/user/#/library . Once logged in, select the Prometheus Grafana tile.

![image](https://github.com/user-attachments/assets/cfd3a1bb-9ed5-4f77-9452-c5919fae291c)

There are four major sections that need to be monitored in the main dashboard:

1. **Rabbit MQ and Solr**
2. **Mirth**
3. **Server Services**
4. **Windows Disk Space, CPU, and Memory**

While exploring the different sections, the appropriate response to any issues discovered should be a site assessment. 

#### Rabbit MQ and Solr

![image](https://github.com/user-attachments/assets/f8d25f1c-a1ba-4006-b07d-495ad3b257e1)

**Rabbit MQ ready messages:**

Rabbit MQ monitors incoming HL7 and other documents that waiting to process. The RabbitMQ ready messages view pane will represent all queues of incoming documents into the system. This means that if a back-up is occurring within any internal Rabbit queue, it will display this in Grafana in the ready messages view pane. If there are back-ups occurring in more than one queue, the number of backed-up messages will combine in the RabbitMQ ready messages view pane. Significant Spikes or trends upward in the documents waiting to process may indicate a back-up issue (setting the timeframe for granularity in the top right corner of the UI is important, default is usually 30 minutes). Back-up issues can occur for a number of reasons.  
- Failure to process records in incoming queue manager.
- All of the allocated RAM in the application-production server is being used by other processes.
- Important services are impaired or not running.

![image](https://github.com/user-attachments/assets/6e7f52ad-4c8a-44e3-aeb6-715eed63ec76)

**RabbitMQ unacked messages**

Unaknowledged messages may vary between 50 to 300 messages at any given time. Because this process of acknowledgement doesn’t occur immediately, these small back-ups can occur. This can become an issue when the back-up exceeds the normal average and begins to escalate.  Should a larger back-up occur, the site will need to be investigated. Unacknowledged messages backing up will typically indicate an issue with Mirth which should be investigated.

![image](https://github.com/user-attachments/assets/2225c76a-adc5-4324-bc3e-95634af95512)

**RabbitMQ queued messages**

This queue will function very much the same as the unacked queue but you may see slightly larger back-ups (as pictured). What should be observed in this queue is if back-ups are progressing larger and larger, or trending upwards. 

