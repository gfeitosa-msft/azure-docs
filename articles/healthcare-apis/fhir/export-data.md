---
title: Export your FHIR data by invoking the $export command on the FHIR service
description: This article describes how to export FHIR data by using the bulk $export operation.
author: expekesheth
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 08/03/2022
ms.author: kesheth
---
# Export your FHIR data

By using the bulk `$export` operation in the FHIR service, you can export data as described in the [HL7 FHIR Bulk Data Access specification](https://hl7.org/fhir/uv/bulkdata/export/index.html). 

Before you attempt to use `$export`, make sure that your FHIR service is configured to connect with an Azure Data Lake Storage Gen2 account. To configure export settings and create a Data Lake Storage Gen2 account, refer to [Configure settings for export](./configure-export-data.md).

## Call the `$export` endpoint

After you set up the FHIR service to connect with a Data Lake Storage Gen2 account, you can call the `$export` endpoint, and the FHIR service will export data into an Azure Blob Storage container inside the storage account. The following example request exports all resources into a container, which is specified by name (`{{containerName}}`). Note that you must create the container in the Data Lake Storage Gen2 account beforehand if you want to specify the `{{containerName}}` in the request.

```
GET {{fhirurl}}/$export?_container={{containerName}}
```

If you don't specify a container name in the request (for example, by calling `GET {{fhirurl}}/$export`), a new container with an autogenerated name will be created for the exported data.

For general information about the FHIR `$export` API spec, see the [HL7 FHIR Export Request Flow](https://hl7.org/fhir/uv/bulkdata/export/index.html#request-flow) documentation.

The FHIR service supports `$export` at the following levels:
* [System](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---system-level-export): `GET {{fhirurl}}/$export`
* [Patient](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---all-patients): `GET {{fhirurl}}/Patient/$export`
* [Group of patients](https://hl7.org/Fhir/uv/bulkdata/export/index.html#endpoint---group-of-patients)\*: `GET {{fhirurl}}/Group/[ID]/$export`  
    \*The FHIR service exports all referenced resources but doesn't export the characteristics of the group resource itself.

Data is exported in multiple files. Each file contains resources of only one type. No individual file will exceed 100,000 resource records. The result is that you might get multiple files for a resource type, and they'll be enumerated (for example, `Patient-1.ndjson`, `Patient-2.ndjson`).

> [!Note] 
> `Patient/$export` and `Group/[ID]/$export` can export duplicate resources if a resource is in multiple groups or in a compartment of more than one resource.

In addition to checking the presence of exported files in your storage account, you can check your `$export` operation status through the URL in the `Content-Location` header that's returned in the FHIR service response. For more information, see the [Bulk Data Status Request](https://hl7.org/fhir/uv/bulkdata/export/index.html#bulk-data-status-request) documentation from HL7.

### Export your FHIR data to Data Lake Storage Gen2

Currently, the FHIR service supports `$export` to Data Lake Storage Gen2 accounts, with the following limitations:

- Data Lake Storage Gen2 provides [hierarchical namespaces](../../storage/blobs/data-lake-storage-namespace.md), yet there isn't a way to target `$export` operations to a specific subdirectory within a container. The FHIR service can specify only the destination container for the export, where a new folder for each `$export` operation is created.
- After an `$export` operation is complete and all data has been written inside a folder, the FHIR service doesn't export anything to that folder again, because subsequent exports to the same container will be inside a newly created folder.

To export data to a storage account behind a firewall, see [Configure settings for export](configure-export-data.md).

## Settings and parameters

### Headers
Two required header parameters must be set for `$export` jobs. The values are set according to the current HL7 [$export specification](https://hl7.org/Fhir/uv/bulkdata/export/index.html#headers). 
* **Accept**: `application/fhir+json`
* **Prefer**: `respond-async`

### Query parameters
The FHIR service supports the following query parameters for filtering exported data. All these parameters are optional.

|Query parameter        | Defined by the FHIR specification?    |  Description|
|------------------------|---|------------|
| `_outputFormat` | Yes | Currently supports three values to align to the FHIR specification: `application/fhir+ndjson`, `application/ndjson`, or just `ndjson`. All export jobs will return `.ndjson` files and the passed value has no effect on code behavior. |
| `_since` | Yes | Allows you to export only resources that have been modified since the specified time. |
| `_type` | Yes | Allows you to specify which types of resources will be included. For example, `_type=Patient` would return only patient resources.|
| `_typeFilter` | Yes | To request finer-grained filtering, you can use `_typeFilter` along with the `_type` parameter. The value of the `_typeFilter` parameter is a comma-separated list of FHIR queries that further limit the results. |
| `_container` | No |  Specifies the name of the container in the configured storage account where the data should be exported. If a container is specified, the data will be exported into a folder in that container. If the container isn't specified, the data will be exported to a new container with an autogenerated name. |

> [!Note]
> Only storage accounts in the same subscription as the FHIR service are allowed to be registered as the destination for `$export` operations.
    
## Troubleshoot

The following information can help you resolve problems with exporting FHIR data.

### Jobs stuck in a bad state

In some situations, there's a potential for a job to be stuck in a bad state while the FHIR service is attempting to export data. This can occur especially if the Data Lake Storage Gen2 account permissions haven't been set up correctly.

One way to check the status of your `$export` operation is to go to your storage account's *storage browser* and see whether any `.ndjson` files are present in the export container. If the files aren't present and no other `$export` jobs are running, it's possible that the current job is stuck in a bad state. In this case, you can cancel the `$export` job by calling the FHIR service API with a `DELETE` request. Later, you can requeue the `$export` job and try again. 

For more information about canceling an `$export` operation, see the [Bulk Data Delete Request](https://hl7.org/fhir/uv/bulkdata/export/index.html#bulk-data-delete-request) documentation from HL7. 

> [!NOTE] 
> In the FHIR service, the default time for an `$export` operation to idle in a bad state is 10 minutes before the service stops the operation and moves to a new job.

## Next steps

In this article, you've learned about exporting FHIR resources by using the `$export` operation. For information about how to set up and use additional options for export, see:
 
>[!div class="nextstepaction"]
>[Export de-identified data](de-identified-export.md)

>[!div class="nextstepaction"]
>[Copy data from the FHIR service to Azure Synapse Analytics](copy-to-synapse.md)

FHIR&#174; is a registered trademark of [HL7](https://hl7.org/fhir/) and is used with the permission of HL7. 
