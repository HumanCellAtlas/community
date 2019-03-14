# Data Portal


## Description
The Data Portal is the landing site for the Human Cell Atlas (HCA) Data Coordination Platform (DCP). It is hosted at https://data.humancellatlas.org/ with an access link from the main HCA website at https://www.humancellatlas.org/. It provides users interested in the HCA with an easy and efficient way to explore and use data at cellular resolution.

## Definitions
*DCP* The Data Coordination Platform is the name given to the entire system used to ingest, validate, store, analyze, and make available the data in the Human Cell Atlas project.

*Data Browser* A DCP application which provides a faceted search capability.

## Objectives
* Provide the access point for users to search, contribute to, or learn about, the cellular resolution and other data which is part of the HCA collection, at petabyte scale on public clouds.
* Work with the other DCP teams to provide a positive user experience for all users of the DCP website.
* Provide a common look and feel across the entire DCP website.
* Provide easy-to-use methods for users to identify a subset of the DCP data for download and/or further analysis.

## In-scope

### Core capabilities
* Implement a visual design which addresses the needs and goals of the identified main user personas. The visual design is driven by the user experience research insights and interaction design guidelines provided by the User Experience (UX) team.
* Implement a visual design which is in line with the HCA brand guidelines.
* Maintain a content management system that meets the needs of Data Portal content developers.
* Integrate DCP applications (e.g. the Data Browser, Ingestion Service, etc.) into a seamless UI with a common look and feel.
* Implement frequent (minimum daily) updates of the summary statistics (e.g. cells, organs, donors, projects, etc.) on the Data Portal home page.
* Coordinate the development of user guide documentation content (e.g. the UI 'Learn' section) from multiple contributors.
* Coordinate the development of developer documentation content (e.g. the UI 'Develop' section) from multiple contributors. This includes API documentation and development guides.
* Coordinate the wording on website pages that require written content but aren't specifically documentation (e.g. the 'About' section).
* Click through to the Data Browser UI from the search box and human body image on the home page.
* Click through to the Ingestion Service UI from the Contribute section.
* Provide a registration form for analysis portals and list of portals available, with click through to the hosting web site (e.g. Dockstore).

### Security
* Leverage the authentication and authorization implementation libraries from the DevSecOps team to implement user authentication and access control for the Data Portal.
* Implement and configure tools to facilitate the operation of the Data Portal in a production environment.
* Provide day-to-day operation of the Data Portal production deployment including but not limited to monitoring of errors and health of intrinsic components, fielding help requests, monitoring security, and assisting with the roll-out of new components into the deployment.

### Community engagement
* Provide triage of Data Portal feature requests from the community and integration of such requests into the Data Portal roadmap.
* Provide a support plan for the escalation of user reported issues and feature requests to the relevant component maintainers.
* Integration with help desk, discussion forum, and user feedback applications.
   
## Out-of-scope
* Production operation of applications and services linked to from the Data Portal (e.g. the Data Store, Ingestion Service, Upload Service, Data Processing Pipelines, etc.).
* An Ingestion Service application which may replace or augment the "Contribute" tab of the Data Portal.
* A Methods Registry providing management, searching, filtering, sorting, visualization, and methods packages in the 'Analyze' section of the Data Portal.

## Milestones and Deliverables
* Nov 2018:  Deploy a minimum viable product (MVP) feature set as part of the HCA DCP beta test phase.
* Q1  2019:  Implement as needed improved scaling/hardening to address any gaps identified during the HCA DCP beta test phase.
* 1H  2019:  Discussion forums support. User feedback application support. Click through to the Ingestion Service UI. Integration with authentication and authorization facilities.

## Roles

### Project Lead
[Brian O’Connor](mailto:brocono@ucsc.edu) 

### Product Owner
[Trevor Heathorn](mailto:theathor@ucsc.edu) 

### Technical Lead
[Dave Rogers](mailto:dave@clevercanary.com)

## Communication

### Slack Channels
* [HumanCellAtlas/data-portal](https://humancellatlas.slack.com/messages/data-portal): general data portal discussions
* [HumanCellAtlas/data-portal-dev](https://humancellatlas.slack.com/messages/data-portal-dev): data portal development discussions
* [HumanCellAtlas/content-team](https://humancellatlas.slack.com/messages/content-team): data portal content development discussions

## Github repositories
* https://github.com/HumanCellAtlas/data-portal - the repository for the UI implementation. Use it to report issues and request new features for the Data Portal.
* https://github.com/HumanCellAtlas/data-portal-content - the repository for the content pages of the Data Portal.
