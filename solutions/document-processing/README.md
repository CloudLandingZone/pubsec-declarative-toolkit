# Document Processing

## Description
This packge contains the minimal set of infrastructure needed to help with a document processing environment.

## Quickstart
### Cloning the Repository
Run the following to open the shell and auto clone the repo into the cloudshell_open directory off your shell user directory. 

[![Open this project in Google Cloud Shell](http://gstatic.com/cloudssh/images/open-btn.png)](https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/GoogleCloudPlatform/pubsec-declarative-toolkit&page=editor&tutorial=README.md)

Navigate to the [Usage section](#usage)

## Architecture
- paraphrasing from original architecture diagram from internal AI CE team under S.A.
### Current Use Case
```mermaid
graph LR;
  style GCP-Services-Flow fill:#44f,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
  %% mapped and documented

  User-Trigger-->Cloud-Storage
  Cloud-Storage-->CloudRun
  CSR-->CloudBuild
  
  ArtifactRegistry-->DockerImage
  CloudBuild-->ArtifactRegistry
  DockerImage-->CloudRun
  CloudRun-->DocAI-Form-Parser
  CloudRun-->DocAI-Custom-ML-Model-0
  CloudRun-->DocAI-Custom-ML-Model-1
  DocAI-Form-Parser-->NLP-API
  NLP-API-->BigQuery
  DocAI-Custom-ML-Model-0-->BigQuery
  DocAI-Custom-ML-Model-1-->BigQuery
  BigQuery-->UI
  UI-->BigQuery
  
```


### Alternate Use Case - deprecated
```mermaid
graph LR;
  style GCP-Services-Flow fill:#44f,stroke:#f66,stroke-width:2px,color:#fff,stroke-dasharray: 5 5
  %% mapped and documented


  Cloud-Storage-0-->Document-AI
  Cloud-Storage-0-->BigQuery-0
  Document-AI-->DocAI-Warehouse
  DocAI-Warehouse-->Cloud-Storage-1
  Cloud-Storage-1-->Cloud-Functions
  Cloud-Functions-->Data-Loss-Prevention
  Data-Loss-Prevention-->BigQuery-1
  BigQuery-1-->UI?
  UI?
  CSR
  CloudBuild
  CloudRun
  ArtifactRegistry
  
```


## Usage
- see https://github.com/GoogleCloudPlatform/pubsec-declarative-toolkit/issues/220
- clone the repo from https://github.com/GoogleCloudPlatform/pubsec-declarative-toolkit or use the cloud shell button above
- navigate/cd to the directory /solutions/document-processing

### Prerequisites
- You must have GCP Organization Administrator or Owner role level privileges
- Your GCP account must have increased quotas for [billing/project](https://github.com/GoogleCloudPlatform/pbmm-on-gcp-onboarding/blob/main/docs/google-cloud-onboarding.md#quota-increase) association - if generating more than 5 projects in the organization
- Cloud Identity accounts that will receive the provisioned project must have been created by the Super Admin already

### KCC - via Kubernetes Config Controller

### Gcloud - via sh script

#### CD into the solutions folder
```
cd cloudshell_open/pubsec-declarative-toolkit/solutions/document-processing/gcloud
```

#### Switch to the canary branch
```
git checkout canary
```
#### Create a project

As an admin level user...

Run the following and substitute the following for each user being provisioned a project.
- ie: pdt-tgz (use your own previously created bootstrap project id instead)

```
export BOOT_PROJECT_ID=pdt-tgz
export UNIQUE_PREFIX=user-mo
gcloud config set project $BOOT_PROJECT_ID
./deployment.sh -b $BOOT_PROJECT_ID -u UNIQUE_PREFIX -c true -l false -e user@domain.com -d false
```
3 min


<img width="1335" alt="Screen Shot 2022-12-20 at 1 54 25 PM" src="https://user-images.githubusercontent.com/94715080/208744247-b3c520c5-532e-4b28-b8bc-e131a5916297.png">


#### Restricted user navigates to the project


<img width="1327" alt="Screen Shot 2022-12-20 at 1 55 07 PM" src="https://user-images.githubusercontent.com/94715080/208744385-1d63f3dd-9eaf-4336-ac2f-0411f2e12a23.png">


#### Deleting a project
- where kcc-lz-883 is the last created project id
```
./deployment.sh -b pdt-tgz -u pdt3 -c false -l false -d true -p kcc-lz-325
```


## Refereneces
- https://cloud.google.com/config-connector/docs/overview
- https://cloud.google.com/config-connector/docs/how-to/getting-started
- CFT - https://github.com/GoogleCloudPlatform/cloud-foundation-toolkit/tree/master/config-connector/solutions