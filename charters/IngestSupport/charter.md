# Ingest Support

## Description

The Ingest Support team is the liaison between the Data Coordination Platform (DCP) and labs contributing data to the Human Cell Atlas (HCA). This team is:

* Working with data contributors who are generating new data types to develop a deep understanding of their data
* Assisting data contributors to describe their experiments in a manner that supports findability, reusability, and reproducability within the DCP ecosystem
* Helping data contributors to submit their data to the DCP, resolving any emerging issues during the process

## Definitions

*Data contributor* - any user who uploads data and metadata into the DCP

*Data wrangler* -  the Ingest Support team member who assists labs contributing data to the HCA

*Data* - raw data files from an experiment such as [fastq files](https://en.wikipedia.org/wiki/FASTQ_format) for sequencing experiments or tiff stacks for imaging experiments

*Metadata* - descriptive information about projects, samples, and experiments and how they were collected or processed

*New data types* - assay data types which either can't currently be ingested by the DCP or have only recently (~6m - 1yr) started to be ingested by the system

*Established data types* - assay data types which have a history of submission and downstream use in the DCP and for which there are already many datasets of this type represented in the DCP

*Submission* - the grouping of data and metadata that makes sense to a data contributor to submit at one time. For example, a data contributor might upload all the data at once for a publication they have in preparation. This grouping does not necessarily correspond to how data is stored or displayed in the DCP nor how it should be displayed to a data consumer

*The PM team* - the team responsible for the oversight of the DCP as the product, developing the shared understanding of the overall product vision, priorities, and release schedule with the community

## Objectives

- Assisting data contributors uploading data and metadata to the DCP
- Helping data contributors address errors with data and metadata formatting before it is submitted to the DCP
- Ensuring data contributors' experiments can be accurately represented in the DCP
- Working with data contributors to understand if new metadata fields are needed to represent their data
- Proposing enhancements to the metadata schema following the process defined by the Metadata Schema team
- Working with the User Experience (UX) team to collect and provide feedback from data contributors to the Ingestion Service team about how the infrastructure can be improved
- Working with the UX team to ensure the submission process provides a positive user experience to data contributors

## In-scope

### For new data types

- Working with data contributors to define a spreadsheet template to collect metadata
- Collecting metadata from data contributors in HCA ingest spreadsheet form
- Collecting data files from data contributors
- Reviewing collected data files and metadata for completeness and correctness
- Submitting data files and metadata to the DCP on behalf of data contributors

### For established data types

- Assisting data contributors uploading data files to the DCP
- Assisting data contributors using the ingest user interface to generate a metadata template spreadsheet
- Assisting data contributors uploading metadata to the DCP
- Assisting data contributors who wish to use the Ingest API to submit data

### For all data contributors

- Assisting data contributors in fixing errors in data files and metadata 
- Responding to data contribution requests on Zendesk
- Writing/delivering training material about how to submit data and metadata to the DCP
- Writing/maintaining documentation about how to submit data and metadata to the DCP

### User Experience

- Collecting feedback about the submission process from data contributors
- Working with the UX team to use feedback to define new system requirements to be prioritized by the PM team

### Scientific "guardrails" 

The Ingest Support team will identify data type priorities in collaboration with the DCP PM team and the HCA Science Leadership and communicate these to potential data contributors.

## Out-of-scope

- Actively submitting data on behalf of data contributors with established data types
- Consistently reformatting data for a data contributor with an established data type when it does not meet HCA requirements
- Updating the HCA metadata schemas 
- Answering HCA helpdesk questions that come to data-help@humancellatlas.org about non-data contribution topics

## Milestones and Deliverables

* Contributor targeted training: How to populate spreadsheets.
  - Delivery: **Q4 2018**
* Requirements for imaging and scNuc-Seq metadata.
  - Delivery: **Q1 2019**
* Requirements for ingest infrastructure self-service phase; working with UX team to develop a requirements set in order to deliver full-featured user interfaces to support spreadsheet-based submissions.
  - Delivery: **Q2 2019**
* Requirements for ingest infrastructure review and repair phase; working with UX team to develop a requirements set in order to deliver user interfaces to support the review and repair of potential errors or inconsistencies in submissions.
  - Delivery: **Q3 2019**

## Roles

### Project Lead

[Clay Fischer](mailto:clmfisch@ucsc.edu)

### Product Owner

[Norman Morrison](mailto:norman@ebi.ac.uk)

### Process Lead

[Mallory Freeberg](mailto:mfreeberg@ebi.ac.uk)

## Communication

### Slack Channels

[HumanCellAtlas/ingestion-service](https://humancellatlas.slack.com/messages/ingestion-service) for discussions of the Ingestion service

[HumanCellAtlas/data-wrangling](https://humancellatlas.slack.com/messages/data-wrangling) for discussion about the data wrangling process

### Mailing list

Wrangler Team email: wrangler-team@data.humancellatlas.org

## Github repositories

[https://github.com/HumanCellAtlas/ingest-central](https://github.com/HumanCellAtlas/ingest-central) is the hub repository for the Ingestion Service - use it to report issues or request new features in the Ingestion Service
