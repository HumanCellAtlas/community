### DCP PR:

[dcp-community/rfc4](https://github.com/HumanCellAtlas/dcp-community/pull/57)

# RFC Deletion of data in the DCP

## Summary
Establish a secure and controlled process for deleting data from the Data Store.


## Author(s)

 [Trent Smith](mailto:trent.smith@chanzuckerberg.com)

 [Hannes Schmidt](mailto:hannes@ucsc.edu)

## Shepherd
 [Rhian Anthony](mailto:ranthony@broadinstitute.org)

## Motivation

One of the DCP's primary objectives is to ensure the durability of the
(meta)data it contains and to maintain a complete record of the changes made to
it. The DCP achieves this by never overwriting a (meta)data file but instead
treating an update to such a file as the addition of a new version of that
file. Instead of physically removing a file version during a logical deletion, the DCP hides that
file version behind a deletion marker. Physical deletion is only used as a last resort.

Under certain circumstances however, the **physical deletion** of a file version
is required. The first section of this specification describes the
circumstances and mechanisms for logical and physical deletion of files and the
bundles containing them. The second section details the changes to the API required to manage the deletion process.
The final section details the process of deleting data from the DSS, and design details.

### User Stories

* As an operator of a secondary index, I would like to know what bundles have been deleted, so I can keep my personal index up-to-date.
* As a data contributor, I need to remove data from the DSS as I have realised the consent it was collected under does not allow it to be hosted by the DCP.
* As a data consumer of the DSS, I would like the API to return a status indicating bundle deletion, so that I know what happened to a bundle that I used previously.


### Terminology

**Tombstone** - a marker placed in the DSS to indicate data that previously existed has been removed.

**Logical Deletion** - data is made unavailable to users through the API.

**Physical Deletion** - data is removed from the DSS.

## Reasons

Possible reasons for deletion of (meta)data are

* consent to share (meta)data derived from a donor's specimens is withdrawn by
  that donor after the (meta)data was submitted to DCP

* that consent was never given but this fact is revealed only after the
  (meta)data was submitted to the DCP

* storing or processing the (meta)data causes a serious disruption of service
  within the DCP

* the (meta)data was submitted to the DCP by a malicious party in violation of
  the DCP's data sharing agreement. This includes (meta)data that violates the
  laws or regulations in a jurisdiction applicable to the DCP or the (meta)data
  it stores

This is not an exhaustive list, and more reasons may be discovered later.

Whether any of the reasons require a physical or logical deletion is at the
discretion of the administrator performing the deletion. That discretion is
guided by a standard operating procedure (SOP).

## API Changes

The following are changes proposed to the DSS API to facilitate the deletion and
restoration of data from the DCP.

### File Deletion API

This is a new addition to the DSS API.

While a user can only opt to **physically delete** a file with the file deletion
API, a file can be considered **logically deleted** in the grace period before
the file is erased.

The file deletion API is a two part process. The first request returns the bundles
that will be **logically deleted** as a side effect and a confirmation
code. The second request must include the confirmation code in order to begin the
deletion process. The endpoint follows this API specification:

**Path**: `DELETE /files/{uuid}`

| Parameter | Description |
|-----------|-------------|
| uuid      | An RFC4122-compliant ID for the file.|
| version   | Timestamp of file creation in DSS_VERSION format.|
| reason    | The reason for the deletion.|
| details   | User-friendly reason for the file deletion.|
| confirmation_code | A code used to confirm a deletion operation.|

| Response Code | Description |
|---------------|-------------|
| 200 | Deletion pending. A confirmation code is returned to confirm the deletion in the second request. A list of affected files and bundles is also returned. |
| 202 | Confirmation code accepted, physical deletion pending. |
| 403 | Unauthorized user is attempting this action.|
| 404 | The file does not exist. |
| 409 | The confimation code used is invalid. Either the confirmation code is incorrect, or an affected bundle has changed state since the original request.|
| 410 | The file has already been deleted or is queued for deletion.

A user must have explicit permission to perform the file deletion operation.

### Bundle Deletion API

The bundle deletion API is a two part process. The first request will return the bundle and files that will be affected
as a consequence of this operation and a confirmation code. Both logical and physical deletions can be performed through
this API. For **logical deletions** only a list of bundles is returned in the response since files are unaffected
by the logical deletion of a bundle. For a **physical deletion** a list of affected files and bundles is returned.
The second request must include the confirmation code in order to begin the deletion process. A deletion request applies
across all replicas. The following table describes the request:

**Path**: `DELETE /bundles/{uuid}`

| Parameter | Description |
|-----------|------------|
|uuid| An RFC4122-compliant ID for the bundle.|
|version|Timestamp of bundle creation in DSS_VERSION format.|
|physical| Determines if the deletion is physical or logical.|
|reason| the reason for the deletion.|
|details| User-friendly reason for the bundle or timestamp-specfic bundle deletion.|
|confirmation_code| A code used to confirm a deletion operation.|

|Response Code| Description|
|--------------|------------|
|200| Deletion pending. A confirmation code is returned to confirm the deletion in the second request. A list of affected files and bundles is also returned. |
|201| The requested logical deletion has been completed. |
|202| The requested physical deletion order has been confirmed and is pending. |
|403| Unauthorized user is attempting this action.|
|404| The bundle does not exist.|
|409| The confirmation code used is invalid. Either the confirmation code is incorrect, or an affected bundle has changed state since the original request.|
|410| The bundle has already been deleted. |

A user must have explicit permission to perform a **logical deletion** of a bundle. For a **physical deletion** of a
bundle, the user must have explicit permission to **physically delete** files and bundles.

(!) The deletion of a bundle does not handle the deletion of secondary analysis bundles referencing this bundle via the
links metadata field.

### Restore File API

Restores a file that has been deleted from the DSS if possible.
This is a new endpoint added to the DSS API. This will restore a file that has been physically
deleted from the DSS. The restoration will apply across all replicas.

**Path**: `PUT /restore/file/{uuid}`

| Parameter | Description |
|-----------|------------|
|uuid| An RFC4122-compliant ID for the file.|
|version|Timestamp of file creation in DSS_VERSION format.|
|confirmation_code| A code used to confirm a deletion operation.|

|Response Code| Description|
|--------------|------------|
|200| Restore delete file possible. A confirmation code is returned to confirm the deletion in the second request.|
|201| Restoration confirmed with confirmation code. |
|403| Unauthorized user is attempting this action.|
|404| The file does not exist. |
|409| The file cannot be restored because it still exists. |
|410| The file cannot be restored because it has been permanently deleted.|


A user must have explicit permission to perform a restore files request. This endpoint does not restore
the bundles that were associated with that file.

### Restore Bundle API

Restore a physically deleted bundle with the given UUID and version. The restoration is applied across all replicas.
Bundles that have been logically deleted should be restored by uploading a new version of the bundle. This is a new
endpoint added to the DSS API.

**Path**: `PUT /restore/bundle/{uuid}`

| Parameter | Description |
|-----------|-------------|
| uuid      | An RFC4122-compliant ID for the bundle. |
| version   | Timestamp of bundle creation in DSS_VERSION format. |
| confirmation_code | A code used to confirm a restoration operation. |

|Response Code| Description|
|--------------|------------|
|200| The deleted bundle and associated files can be restored. A confirmation code is returned to confirm the deletion in the second request.|
|201| Restoration confirmed with confirmation code. |
|403| Unauthorized user is attempting this action.|
|404| The bundle does not exist.|
|409| The bundle cannot be restored because it still exists. This can also occur if the restoration cannot be completed succesfully because some of the files associated with the bundle have been deleted. |
|410| The bundle cannot be restored because it was physically deleted. |

A user must have explicit permission to restore file and bundles. This endpoint will attempt
to restore the file associated with this bundle and fail if it cannot.

### Get Deleted Bundle API

The following table proposes modification of the existing path
[/bundles/{uuid}](https://github.com/HumanCellAtlas/data-store/blob/3d73935692f2030e7c19e4ec3b074a15361d33ca/dss-api.yml#L1189-L1449).
to communicate the deletion of a bundle:

**Path**: `GET /bundles/{uuid}`

|Response Code| Description|
|-------------|------------|
|410| The bundle has been deleted. The reason and details for the deletion are returned. |

**Path**: `PUT /bundles/{uuid}`

|Response Code| Description|
|-------------|------------|
|409| A bundle or bundle tombstone with the same UUID and version already exists. |

### Get Deleted File API

The following proposes modification of the existing path
[/files/{uuid}](https://github.com/HumanCellAtlas/data-store/blob/3d73935692f2030e7c19e4ec3b074a15361d33ca/dss-api.yml#L237-L547)
to communicate the deletion of a file:

**Path**: `HEAD /files/{uuid}`

|Response Code| Description|
|--------------|------------|
|410| The file has been deleted. The reason and details for the deletion are returned. |

**Path**: `GET /files/{uuid}`

|Response Code| Description|
|--------------|------------|
|410| The file has been deleted. The reason and details for the deletion are returned. |

**Path**: `PUT /files/{uuid}`

| Response Code | Description |
|---------------|-------------|
| 409 | Returned when a file or file tombstone with the same UUID and/or version already exists. |


## Deletion Process

The following describes the process for deleting data from the DSS. The same process is shared for both file and
bundle deletes. It will be explicitly mentioned in areas where the processes diverge.

Once it is determined that (meta)data needs to be deleted –
the details of this determination are the scope of an SOP – the following
procedures takes place:

1) A user identifies the Data Store file or bundle to be deleted.

2) The user makes a request to the appropriate API to retrieve the confirmation code. A list of bundles and/or files
   affected is also returned in the request.

3) The users makes a second request that includes the confirmation code. This starts the deletion process.

4) After the deletion request has been confirmed, the DSS places **tombstones** in the
   underlying storage system (S3 or GCP bucket). For a **logical deletion** request,
   tombstones are placed for all bundles mentioned in the response to the deletion request.
   For **physical deletion** requests, tombstones are placed for all files and bundles
   mentioned in the response to the deletion request. The tombstone's content is as follows:

   ```JSON
   { "reason": "<reason>",
     "details": "<additional info>",
     "email": "<requester's email>"
   }
   ```

   A versioned bundle tombstone will cause `GET /bundles/{uuid}` to return a 410 status code for that bundle version. If the tombstoned
   bundle is the latest version then a request for `GET /bundles/{uuid}` without a version will
   also return a 410 status code. If bundle tombstone does not specify a bundle version then all requests for `GET /bundles/{uuid}` will
   return 410 status code even if version is specified in the request. The same applies to `HEAD /bundles/{uuid}` requests, both versioned and
   unversioned. The same applies for `GET /files/{uuid}` and `HEAD /files/{uuid}`. Bundles that have an
   associated tombstone are inaccessible through the API and are considered **logically deleted**. Files with an
   associated tombstone are not considered logically deleted, because user with a direct URL to the file can still access the data.

   A notification is sent out to subscribers of datastore deletions when a new bundle tombstone is created, so they can
   update their index appropriately. If the the bundle was logically deleted and later physically deleted, a second
   identical notification will be sent to subscribers. The subscribers must be able to handle these notifications
   idempotently.

   A **physical deletion** request of a bundle results in the bundle associated files being added to
   the deletion queue for regularly-scheduled **Physical Deletion**. A **physical deletion** request of a file results in
   that file being added to the to deletion queue

5) On a regular schedule the deletion queue is processed by code running within a secure boundary. An administrator
   may manually invoke the deletion process or wait for it to run at its scheduled time once daily.
   The deletion daemon is responsible for performing **physical deletes** of data from the deletion queue.
   A physical delete makes all files associated with a bundle or bundle version inaccessible. This will cause direct
   urls to a deleted file to fail.

   A log entry is produced when ever the deletion daemon is run. It contains information on what bundles and files
   have been logically deleted, the reason, the administrator who executed the deletion, and info about the deletion markers.
   The same information is stored in the **Deletion Table**. The deletion table tracks all bundles and files currently
   pending permanent deletion. The Deletion Table contains enough information to reverse Physical Deletes before
   the data is Permanently deleted.

   A daily digest of bundles and files that are scheduled for deletion are sent to DCP administrator 24 hours prior to the data
   being permanently deleted.

6) Data that has been physically deleted remains in the bucket for 7 days before being permanently deleted from the bucket.
   This grace period allows the deletion to be reversed within that time period. After the grace period the data will
   be permanently deleted.

### Tombstones

Tombstones are markers placed in the DSS to indicate previously existing data has been removed. Tombstones exist for
files and bundles stored in the DSS. Tombstones come in two varieties: versioned tombstones, and unversioned tombstones. A
versioned tombstone marks the deletion of a specific version of a file or bundle. This prevents that same version of data
from being reuploaded or retrieved. An unversioned tombstone does not specify a version and prevents a file or bundle
using the same UUID from being reuploaded or retrieved. Whether a tombstone is versioned or unversioned is determined
during a deletion request. If the version parameter is present in the request, then a versioned tombstone is created. If
no version is specified then an unversioned tombstone is created that is applied to all the bundles with that UUID. A
versioned tombstone is created for all bundles and files that are logically or physically deleted as a side affect of a
deletion operation. Unversioned tombstones should be used with caution for they effectively retire an existing UUID,
preventing users from uploading a corrected version of the data in the future.

### Primary indexes

Bundle tombstones are indexed verbatim by the Data Store. The JSON schema
for deletion markers is defined such that there are no conflicts with the
top-level keys in the index documents for regular bundles (`files`, `manifest`,
`uuid`, `version` etc). Bundle tombstones are therefore compatible with
regular bundle documents with respect to in the same index even if
Elasticsearch's dynamic mapping is enabled.

### Replication

When a bundle or file is deleted it is removed from all replicas.

### Consistency

Race conditions with Elasticsearch can arise when deleting files or bundles. The confirmation code is used to ensure
that the state of the index has not changed between deletion requests. If the state has changed another request to delete
should be made and the affected files and bundles should be verified before sending the confirmation to delete.

### Secondary indexes

The Data Store can optionally notify subscribers about the creation of a bundle
tombstone. Secondary index services need to register a subscription using
a query that matches deletion markers. When they are notified of a deletion
marker, they need to take the necessary actions to remove the data from their
database or index.

### Derived data

The metadata in bundles containing derived data must reference the input bundle
versions. Administrators are responsible for tracking down data bundles with
data derived from data in deleted bundles and to issue deletion requests for
them.

### Tiered storage

Physical deletion of blobs is effective across all tiered storage classes in
the underlying blob store. Additional cost may be incurred for deleting data recently moved to a
slower access storage tier. There is no additional time delay caused when deleting from a
tiered storage system such as AWS Glacier.

### Undoing Deletions

A bundle can be restored using `PUT /restore/bundle/{uuid}` if bundle has been logically
deleted. Physically deleted bundles and files can be restored using the restore API if the grace period for the data
has not elapsed. Subscribers of bundles will receive a notification that a new bundle has arrived when a bundle is restored.

Within the DSS the process for restoring data uses the info from the deletion table to remove tombstones and prevent
permanent deletion. Deletion Table Entry Example:

|Bundle.Version|admin|reason|AWS Deletion Markers (key,ID)|GCP Previous Generations (key, previous generation)| Expiration|
|--------------|-----|------|--------------------|------------------------|--------------------|
|1234-2134-3145|admin@email.com|consent|(file.obj, 1234), ...| (file.obj, 4321), ...| 01-01-01T120000|

A bundle is restored in AWS by deleting the deletion marker and the tombstone for each file in the bundle.
In GCS a bundle can be restored by by storing a copy of the orginal files in the bucket with the same name, and deleting
the tombstones.

### Monitoring and Alerts

Deletion operations will be strictly logged and monitored to detect and prevent the rapid destruction of data from the
DSS. An alert system will be created to monitor the number of deletion operations executed and the scope of a deletion
request. A large number of deletion requests over a short period of time would raise an alarm. A single deletion request
that affects a large number of files and bundles will also raise an alarm.

A deletion digest log will be produced once a day when files or bundles have been or will be deleted from the DSS. This
includes **logical deletes**, **physical deletes**, data that is 24 hours aways from being permanently deleted, and
data that was permanently deleted over the previous 24 hours.

### Resources

* [AWS S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/dev/Versioning.html),
* [AWS S3 Deleting Object Versions](https://docs.aws.amazon.com/AmazonS3/latest/dev/DeletingObjectVersions.html),
* [AWS S3 Restoring Previous Versions](https://docs.aws.amazon.com/AmazonS3/latest/dev/RestoringPreviousVersions.html),
* [AWS Object Lifecycle Management](https://docs.aws.amazon.com/AmazonS3/latest/dev/object-lifecycle-mgmt.html),
* [Amazon DynamoDB TTL](https://aws.amazon.com/about-aws/whats-new/2017/02/amazon-dynamodb-now-supports-automatic-item-expiration-with-time-to-live-ttl/)
* [GCS Object Versioning](https://cloud.google.com/storage/docs/object-versioning),
* [GCS Object Lifecycle Management](https://cloud.google.com/storage/docs/lifecycle)
* [HumanCellAtlas/data-store#1662 Explore Bucket Versioning for Deletion](https://github.com/HumanCellAtlas/data-store/issues/1662)
* [HumanCellAtlas/data-store#1663 Deletion Daemon Design](https://github.com/HumanCellAtlas/data-store/issues/1663)

### Open Issues

* TODO: implement object deletion and restoration for AWS and GCP
* TODO: implement tombstones for files
* TODO: enable bucket versioning and lifecycle rules for replicas.

### Acceptance Criteria [optional]

* When a bundle is tombstoned it is made immediately unavailable to users using search.
* Maintainers of external indices shall remove deleted data from its index when a bundle tombstone notification is received.
* Bundle files are still available until the deletion process has run.
* Bundles and files are not completely removed from the DSS until after the grace period has elapsed.
* Deleting a file results in the deletion of all bundles referencing that file.

### Unresolved Questions

- *What aspects of the design do you expect to clarify further through the RFC review process?*
- *What aspects of the design do you expect to clarify later during iterative development of this RFC?*

### Drawbacks and Limitations [optional]

* This introduces the ability to permanently destroy data from the DSS.
* There is no limit on the amount of data that can be rapidly permanently deleted.
* Permanent deletion does not occur until after the grace period has elapsed. This could
  be problematic if files need to be removed sooner.
* If two bundles share files and only one bundle is deleted, this causes the remaining bundle to be incomplete.
* the ability to search for bundles that contain a files is depended on maintaining a current index of bundles and files.
  Presently Elasticsearch provides this indexing.
