## This readme is an overview of the design process and architecture of our iot system in the cloud

# Fahamu Africa
We designed this IoT system to help solve human wildlife conflict in several game reserves and parks around Africa.

### Main Features
An alert system that will signal the villagers and response team when an animal has invaded the village.
Ability to detect animals with image captions and identify the level of threat they pose.
Easy access to data that will help the response teams to learn about certain patterns.
Ability to obtain real time data as the event occurs.
UI interface that displays changes in heat signatures and other telemetry.
Messaging service for location to location communication

### Roles of typical users
Villagers
Response Team
Data analyst or related proffesions
Site facilitator

## user personas
### Persona 1
Murkomen is a resident of Ol Tukai which neighbors amboseli national park. He owns a herd of cattle which he takes to the park to graze but the herd is sometimes attacked by a hyena. He wants to protect his cattle from dangerous animals. He recieves signals from the watch team through an alarm system to signify whenever a dangerous animal is approaching. This allows him to relocate his animals to safety while the response team deals with the matter.

### Persona 2
Kenya Wildlife Service is responsible for managing Amboseli national park and other parks in Kenya. Their main responsibility is protection of wildlife. They recently recieved reports of locals killing animals for bushmeat. The want to use the system to recieve real time alerts when hunters are around in order to save the animals. The also want insights from our system on how they can better protect wildlife.

### Persona 3
Beryl is a watch assistant for Amboseli national park. She is responsible for reading data from the user interface as it comes in and notify the appropriate team to take action. Sometimes data comes in late therefore she is not able to respond in real time. This is because of poor internet connection since the area is remote. She wants to use the system to recieve data even when she is offline.

## User Stories

As a cattle herder, I want to be notified immediately when a dangerous animal is approaching, so that I can take my animals to safety.
As a cattle herder, I want the response team to take action immediatley, so that I can feel safe when grazing.
As a site facilitator, I want to recieve data immediately, so that I can send notifications before any damage is done.
As the KWS data analyst, I want to analyze real time insights, so that I can help the organization make better decisions.
As the KWS representative, I want to obtain all data related to animal attacks, so that we can better protect wildlife.

## Service Level Indicators and Service Level Objectives
The metrics defined below are not real values but what we aim to achieve when this solution is implemented 

| User Story        | SLI                       |SLO                                                                                           |
| -----------       | ----------------------- |----------------------------------------------------------------------------------------------  |
| Data queries      | Available 99.95%                         |   Fraction of 200 vs 500 HTTP responses from API endpoint measured per day    |
| Data Inquiry      | 95% of requests complete in under 300 ms |Time to last byte GET requests measured every 10 seconds aggregated per minute|
| Image captioning  | Error rate <0.0001% | Upload errors measured as a percentage of bulk uploads per day by custom metric. |
| Alert creation    | Error rate < 0.00001% | Upload errors measured as a percentage of bulk uploads per day by custom metric.|
| UI data           | Available 99.95%     |  Fraction of 200 vs 500 HTTP responses from API endpoint measured per day |


## Microservice Architecture
Docker containers will be used to encapsulate discrete services within individual containers.

![image info](/images/Miro.PNG)

### Rest APIs Design
|Service Name |  Collections | Methods   |
|-------------|--------------|-----------|
|Image Capture Services | Images | List, Add, Get |
|Telemetry Service | Heat signatures, Geolocation | Get, Search, List |
|Blob Storage | telemetry, all data | Search, List, Get, Add |
|Analytics Service | Locations, heat signatures, images | Analyze, Get, List|
## Storage Characteristics
| Service | Structured/ UnStructured| SQL or NoSQL | Consistency | Amount of Data | Read or Write|
|---------|-------------------------|--------------|-------------|----------------|------------|
|Images service| Unstructured |          N/A       | Eventual    | GB             | Read/Write |
|Telemetry Data Service  |  Structured |   SQL  | Strong | TB | Read/Write|
|Analytics |  Both | Both | Eventual |  PB | Read|


### Choosing Microsoft Azure Data Services
| Service           | Storage Solution    |
|-------------------|---------------------|
|Image Service      | Blob Storage        |
|Telemetry Data Service |  Azure SQL Database |
|Analytics              |  Azure SQL Database   |   

## Disaster Recovery plan
### Scenario 1
Suppose while trying to retrieve some data from the database, the site regulator accidentally deletes data partaing to a certain period of time.
### Solution 
All of the data has to be recovered and this can be done through a failover replica and regular backups. We will then have to restrict user access policies to ensure this incident does not occur again.

### Scenario 2
Suppose the project is successful and we have a number of devices registered to our solution. Requesting for services will be difficult and also controlling and managing the devices would be impossible.
### Solution 
We implement our solution on the cloud IoT Hub that will help us to easily manage these devices.

