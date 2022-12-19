We can find here distinctive architectures to suit different requirements, based on Azure Cloud services
Hence, they address the following scenarios

# Index

[1 - Data Engineering as a Service for document-image container](#data-engineering-as-a-service-for-document-image-container) </br>
[2 - Azure Log Analytics Workspace for Multi-cloud monitoring](#azure-log-analytics-workspace-for-multi-cloud-monitoring)

## Data Engineering as a Service for document-image container

![DEaas for document-image container](https://raw.githubusercontent.com/paulo-jsilva/data-architectures/master/DEaaS-4document-image-2.png?raw=True)

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
orchestrate the request from HTTP client </br></br>
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


## Azure Log Analytics Workspace for Multi-cloud monitoring

![Azure Log Analytics Workspace](https://raw.githubusercontent.com/paulo-jsilva/data-architectures/master/AzureLAW-multi-cloud-monitoing.png?raw=True)
