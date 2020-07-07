# RPA UIPath - ElasticSearch

### ElasticSearch Environment Setup For RPA Uipath Orchestrator Logs


Orchestrator stores logs just for viewing but not for  analysis. These logs analysis can be performed from ELK.

Orchestrator can push logs to both database and elastic search.

These configurations can be done from web.config file. This file available in (C:\Program Files (x86)\UiPath\Orchestrator) only for Enterprise edition but not for installed community edition.

```xml 
<logger name="Robot.*" writeTo="database,robotElasticBuffer" final="true" />
<target name="robotElasticBuffer" xsi:type="BufferingWrapper" flushTimeout="5000">
<target xsi:type="ElasticSearch" name="robotElastic" uri="uritoelasticsearchnode" index="${event-properties:item=indexName}-${date:format=yyyy.MM}" documentType="logEvent" includeAllProperties="true" layout="${message}" excludedProperties="agentSessionId,tenantId,organizationId,indexName" />
</target>
</target>
```

uri - The ElasticSearch URL; note that you have to include the protocol and port, such as http://elastic_server:9200.

excludedProperties - Data that you do not want to be saved to ElasticSearch.

If you maintain more than two million logs in the SQL database, you might have some performance issues. For more than that, we recommend using ElasticSearch.

If we are using community edition, RPA Uipath genereated logs available in location:(C:\Users\jagadheeswarReddy\AppData\Local\UiPath\Logs)
![robot_logs](/robot_logs.PNG) 

These logs are sent to Orchestrator from our local once Robot Agent connected to cloud uipath.

Robot-Agent:

![Robot-Agent](/ui-robot-agent-connected.PNG)   


We can push these robot generated logs to Elasticsearch with help of Filebeat and Logstash. 

![workflow](/workflow.PNG) 

Kibana URL: http://localhost:5601/app/kibana

Dashboard:
![Uipath-Executions-Dashboard](/Uipath-Executions-Dashboard.PNG) 

Discover:
![Uipath-Executions-Discover](/Uipath-Executions-Discover.PNG)   


ElasticSearch URL: http://localhost:9200/
![ElasticSerachHost](/ElasticSerachHost.PNG)   

Logstash URL: http://localhost:9600/
![LogstashHost](/LogstashHost.PNG)

### ELK Environment Setup

ELK Homepage: https://www.elastic.co/what-is/elk-stack

Before setup of ELK first need to setup JAVA_HOME. Better to install latest version of Java.

1) Download JAVA : https://www.oracle.com/in/java/technologies/javase/javase-downloads.html
2) Download Elasticsearch : https://www.elastic.co/downloads/elasticsearch
3) Download Kibana : https://www.elastic.co/downloads/kibana
4) Download Logstash : https://www.elastic.co/downloads/logstash
5) Download Filebeat : https://www.elastic.co/downloads/beats/filebeat

![ELK-env-setup](/ELK-env-setup.PNG) 


Once download Elasticsearch, Kibana, Logstash and Filebeat, just replace the files elasticsearch.yml,kibana.yml, logstash.yml,filebeat.yml in the config folder of each of them.

Run the below windows batch script files to start ELK.
1) elastic_search_start.bat - Start Elasticsearch
2) kibana_start.bat - Start Kibana
3) logstash_start_generic.bat - Start Logstash (just for testing )
4) logstash_start_uipath_execution.bat - Start Logstash to push uipath robot execution logs to Elasticsearch
5) filebeat_start.bat - Start filebeat to update changes to Logstash


### Useful Links

[Elasticsearch MyBook](https://github.com/nrkreddy94/elasticsearch-mybook)

[ELK MyCollection](https://github.com/nrkreddy94/elasticsearch-repo)

[ELK Paris Accidentholgy](https://github.com/nrkreddy94/elasticsearch-paris-accidentholgy)

[ELK Apache Logs](https://github.com/nrkreddy94/elasticsearch-apache-logs)

[ELK Json Logs](https://github.com/nrkreddy94/elasticsearch-logstash-jsonlog)
