We can find here distinctive architectures to suit different requirements, based on Azure Cloud services
Hence, they address the following scenarios

# Index

[1 - Data Engineering as a Service for document-image container](#data-engineering-as-a-service-for-document-image-container) </br>
[2 - Azure Log Analytics Workspace for Multi-cloud monitoring](#azure-log-analytics-workspace-for-multi-cloud-monitoring)

## Data Engineering as a Service for document-image container

![DEaaS-4document-image-container](https://user-images.githubusercontent.com/60786166/208467766-de9789d0-166a-441f-8526-82be866500d0.png)

### Requirements
- Scan and Extract content from within a table in an document-image (example `pdf`) stored in a data lake, whether document's path is a public or private endpoint
- Table length might be broken across several pages
- Output content as a json dataset along with pre-defined fields names
- Table format and image quality may vary regarding font, borderline and gray scale
- Concerning where file is located, whether it's a public or private endpoint, respectively data must be read directly from the source location or streamed out
- Azure services used to process the image, are deployed to a public endpoint
- Ideally, no AI model should be trained with the document to submit, in order to scan and read the content successfully

### Services
**HTTP client**</br>
sw client which makes the call to read the table's content. One type of client can be Azure Data Factory (ADF) </br></br>
**Azure Function** (C#) </br>
orchestrate the request from HTTP client and renders the json file with extracted data</br></br>
**Cognitive Services (Form Recognizer)** </br> 
AI service specialized to scan and read content from images </br></br>
**Storage account** </br>
endpoint where file is stored </br></br>
\* these services and other details are illustrated in architecture diagram 
</br></br>
#### The key parameters to be passed, in the payload, by the client are the following: </br>
**document-path** document-image location </br>
**page** page where the table is </br>
**table-position**: table position within the page as there can exist more than one </br>
**outbound-fieldset**: fields names associated to the dataset to be generated

### Data Flow
Partial flow with black arrows is the common path for the two mutual exclusive alternative flows (green | blue) to read file content
Blue path - file content is streamed out to Cognitive Services - Form Recognizer endpoint
Green path - file contend is read directly from data lake by Cognitive Services - Form Recognizer endpoint

------
## Azure Log Analytics Workspace for Multi-cloud monitoring

![AzureLAW-multi-cloud-monitoing](https://user-images.githubusercontent.com/60786166/208469581-9964df87-e0e1-4907-a899-95be83d2989e.png)

### Requirements
- monitor different types of log data, from different sources, regardless of cloud provider
- data in the repository must be queried through a standard language or API, platform must be scalable and cost effective
- analytics and notifications must be provided through a built-in solution on the market 

### Sources of Logging
There could be more, but here in the example were identifed 3 inbound sources:</br>
1- **spark clusters**: set up Init config with two libraries - spark-listeners_<Spark Version>_<Scala Version>-<Version>.jar and spark-listeners-loganalytics_<Spark Version>_<Scala Version>-<Version>.jar - to push logs to Azure LAW </br>
2- **application logs**: add logs to your code as you wish to push them through Azure LAW API </br>
3- **diagnose settings (Azure)**: in Azure services, where available, set up services' Diagnose Settings to push audit logs to Azure LAW
  
### Data preparation
Use of a service such as Azure Runbook, to pull data out of Log Analytics Workspace, (make transformations and) save the logs to file format of your choice. It will be ready later to be consumed by an analytical tool.
  
### Serve data
There are a few out of the shelf options to present and notify events-related data </br>
Azure's built-in Notifications and Dashboards are effective options, easily integrated with Azure LAW to create meaningful dashboards based gon RBAC roles and set up relevant notifications based on queries (w/ time series) to the data existing in Azure LAW
