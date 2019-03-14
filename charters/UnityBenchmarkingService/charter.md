# Unity Benchmarking Service

## Description

The Unity Benchmarking Service automates and standardizes the process of running and benchmarking data processing pipelines.

## Objectives

The Unity Benchmarking Service automates benchmarking for data processing pipelines, enforces standards for consistent reporting for historic comparison and creates opportunity to demonstrate improvements to data processing pipelines using a common benchmarking infrastructure. The Unity Benchmarking Service is used by developers of data processing pipelines for the Human Cell Atlas (HCA) data processing service and is publicly hosted; so anyone can benchmark their [data processing pipelines](https://github.com/HumanCellAtlas/dcp-community/tree/master/charters/DataProcessingPipelines/charter.md) against benchmarked pipelines in the Data Coordination Platform (DCP).

## Definitions

__Community pipeline:__ A pipeline that is not officially used as a standard in the HCA project, but runs in the DCP and is operated by the Data Processing Service to enable community members to work with a diversity of submitted data until they have a standardized pipeline. These pipelines are not as vetted as standardized pipelines.  
__HCA Analysis Working Group (AWG):__ A group of domain specialists that advise on and monitor scientific choices made in the Human Cell Atlas Project.  
__Standardized Pipeline:__ An official pipeline of the HCA project.  

## In-scope

* Benchmarking for standardized HCA data processing pipelines is automated and included in Unity
* Data used to run pipelines during benchmarking is standardized and enforced to promote comparable analysis runs
* When multiple pipelines processing the same assay are collected, leaderboards are created to compare performance
* Documentation for the requirements of pipelines to be benchmarked by the Unity Benchmarking Service
* Documentation on benchmarking process and outputs

### Scientific "guardrails"

* Pipelines benchmarking standardized HCA data processing pipelines must be reviewed by HCA scientific leadership (Analysis Working Group) and must be treated as standardized data processing pipelines. Changes to these specific benchmarking pipelines will directly affect how standardized HCA data processing pipelines are optimized and comment on what are better scientific results produced by the data processing pipeline they benchmark.
* Metrics and their use in leaderboards must be reviewed by the HCA scientific leadership (Analysis Working Group).

## Out-of-scope

* Educational training or user assistance on how to create pipelines
* Recommendations on how to improve pipelines based on benchmarking
* Pipelines that benchmark Community Pipelines are not required to be included in the Unity benchmarking service (but are allowed). If a Community Pipeline is related to an assay that is neither currently processed by a standardized HCA pipeline nor is being scoped by the Data Processing Pipelines team, then setting up the benchmark will require significant investments which are evaluated based on team priorities. Otherwise, community members may also benchmark Community Pipelines using data and file format standards in the system.

## Milestones and Deliverables

__2018:__ Infrastructure operational and secure, HCA Data Processing Pipelines team has started using the system with a standardized HCA data processing pipeline.   
__2019:__ System updated to include all data processing pipelines operational as standardized HCA pipelines.   
__2019:__ Leaderboards are created for standardized HCA pipelines.   

## Roles

### Project Leads

[Kylee Degatano](mailto:kdegatano@broadinstitute.org)   
[Timothy Tickle](mailto:ttickle@broadinstitute.org)   

### Product Owner

[Vicky Horst](mailto:vicky@broadinstitute.org)

### Technical Lead

[Jonathan Bistline](mailto:bistline@broadinstitute.org)

## Communication

### Slack Channels

[HumanCellAtlas/unity-benchmarking](https://humancellatlas.slack.com/messages/unity-benchmarking): To answer questions and get feedback on pipeline benchmarking   

## Github repositories
[Unity](https://github.com/HumanCellAtlas/unity): Source code for the Unity service.
