# Monitoring Tools: Observing Grafana for Near Misses

### The following provides a detailed description of the UI elements that make up the Grafana dashboard that are important for monitoring site health. This will be your main tool for discovering near misses as a TSE, Dev, or SRE and will assist you in resolving site issues proactively. 


**Grafana Sections**
---

ℹ️ The Grafana monitoring UI provides a comprehensive dashboard for checking and monitoring the health of each site. It is accessible from your Applications and Monitoring dashboard at https://hooli.ai/if/user/#/library. Once logged in, select the Prometheus Grafana tile.

![image](https://github.com/user-attachments/assets/cfd3a1bb-9ed5-4f77-9452-c5919fae291c)

There are four major sections that need to be monitored in the main dashboard:

1. **Rabbit MQ and Solr**
2. **Mirth**
3. **Server Services**
4. **Windows Disk Space, CPU, and Memory**

While exploring the different sections, the appropriate response to any issues discovered should be a site assessment. Let's take a look at the different sections.


**Rabbit MQ and Solr**
---

![image](https://github.com/user-attachments/assets/f8d25f1c-a1ba-4006-b07d-495ad3b257e1)

**Rabbit MQ ready messages:**

Rabbit MQ monitors incoming HL7 and other documents that are waiting to process. The RabbitMQ ready messages view pane will represent all queues of incoming documents into the system. This means that if a back-up is occurring within any internal Rabbit queue, it will display this in Grafana in the ready messages view pane. If there are back-ups occurring in more than one queue, the number of backed-up messages will combine in the RabbitMQ ready messages view pane. Significant spikes or trends upward in the documents waiting to process may indicate a back-up issue (setting the timeframe for granularity in the top right corner of the UI is important, default is usually 30 minutes). Back-up issues can occur for a number of reasons.  
- Failure to process records in the incoming queue manager.
- All of the allocated RAM in the application-production server is being used by other processes.
- Important services are impaired or not running.

![image](https://github.com/user-attachments/assets/6e7f52ad-4c8a-44e3-aeb6-715eed63ec76)

**RabbitMQ unacked messages**

Unacknowledged messages may vary between 50 to 300 messages at any given time. Because this process of acknowledgement doesn’t occur immediately, these small back-ups can occur. This can become an issue when the back-up exceeds the normal average and begins to escalate.  Should a larger back-up occur, the site will need to be investigated. Unacknowledged messages backing up will typically indicate an issue with Mirth which should be investigated.

![image](https://github.com/user-attachments/assets/2225c76a-adc5-4324-bc3e-95634af95512)

**RabbitMQ queued messages**

This queue will function very much the same as the unacked queue but you may see slightly larger back-ups (as pictured). What should be observed in this queue is if back-ups are progressing larger and larger, or trending upwards. 

![Solr Scrub redact](https://github.com/user-attachments/assets/ad0dff43-c8d8-4b26-a521-34a26ce63863)

**Solr Ping**

Prometheus pings the different Solr nodes to determine whether or not they are up and functioning. The results of these are then displayed in the Grafana dashboard. If drops are observed here, this could mean that Solr is crashing. The Solr Admin console will need to be investigated at the affected site for more information.


**Mirth**
---

![image](https://github.com/user-attachments/assets/436affb9-d150-44f0-a340-87dc5fa110ec)

**Mirth**

For a number of sites, the data transform begins with a queueing program that ingests and processes incoming documents entering the system. At older sites this is done through a custom created program. Many newer sites have now transitioned to Mirth. As a support engineer, SRE, or dev the main responsibility is to monitor the Mirth queue and make sure that there is not a back-up of incoming messages. A large back-up of messages within any Mirth queue can indicate larger issues with incoming documents at the site (Ex. Issues in Solr). This may also indicate larger issues within Mirth and its configuration.

**Windows and Custom Services**
---

Windows and custom configured services on the application production server should be running at all times to ensure the integrity of Hooli applications. There are more services that will be site dependent, but these first four services are shared between all sites. If you are in a site and notice that one the these first four services are down, please restart the service and troubleshoot any potential issues. 

![image](https://github.com/user-attachments/assets/adc21f3e-46a6-4418-b45f-fe3536b82d61)

**Auditor Status**

The Auditor Service is used at certain times by a number of applications and will affect their functionality if down. When data is viewed, dumped or transferred, status updates will post to this service and application changes will occur based on the updates provided. Auditor primarily affects alerting in the search application, the functionality of the Webhooks service, and processes within application A. This service needs to be running to ensure the integrity of Hooli applications. When it is running it will display the following.

![NWH Scrub](https://github.com/user-attachments/assets/1ef188b1-9485-425a-a3c8-bd241949d595)

**Webhooks Service**

The Webhooks service gathers status changes via webhooks. The alerted changes are then stored and used by Hooli applications. Working in tandem with the Auditor service, Hooli applications then make updates based on the status changes. As with the previous screenshot, the service will display in Grafana as running if it is up. 

![image](https://github.com/user-attachments/assets/cbd28e52-01c5-4320-9cd1-ee52d752fd23)

**Sessions Status**

The Session service will give information on what processes are running(active) under a user’s login session and who is using that session. The service will display in Grafana as running if it is up. 

![image](https://github.com/user-attachments/assets/d111b333-0e54-4ac5-b3b5-283594711880)

**IIS Service**

The are several applications and their dependent services that use Windows Internet Information Services (IIS). IIS is a flexible, general-purpose web server from Microsoft that runs on Windows systems to serve requested HTML pages or files. An IIS web server accepts requests from remote client computers and returns the appropriate response. The service will display in Grafana as running if it is up. It is integral to the functionality of Illuminate’s web apps. 


**Storage and CPU**
---

![image](https://github.com/user-attachments/assets/df331e17-9654-4ec3-bd4a-03170b1b76f4)

**Disk Space Storage**

It is important to monitor the storage of specifically the C Drive as it reflective of the current storage available in the application production server. Other drives can be monitored, but their storage usage and their overages will not affect production applications. 

Of the Linux nodes, the main node storage to monitor will be the nodes that contains disk space for the Postgres database. This will typically be listed under the directory of /var/lib/pg_data:

![image](https://github.com/user-attachments/assets/0ec21b94-602b-4785-9644-20a83175d9a0)

![image](https://github.com/user-attachments/assets/37ff5748-cd5f-4191-a107-826e00741238)

**CPU and Memory Utilization**

CPU spikes may not always indicate poor site health, but continued high CPU usage can potentially cause disruption of service. If this occurs at a site, then its root cause will need to be investigated. Memory can cause much more of a concern immediately. Once memory has been allocated by a number of potential programs and services, it will not allow applications to launch. Also, services on the server can allocate too much memory, restricting resources on the server. It is important to monitor these and validate the use of these resources through the Task Manager on the application production server.

