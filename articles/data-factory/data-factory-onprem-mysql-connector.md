<properties 
    pageTitle="Move data from MySQL | Azure Data Factory" 
    description="Learn about how to move data from MySQL database using Azure Data Factory." 
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/20/2016" 
    ms.author="jingwang"/>

# <a name="move-data-from-mysql-using-azure-data-factory"></a>Move data From MySQL using Azure Data Factory

This article outlines how you can use the Copy Activity in an Azure data factory to move data to from MySQL to another data store. This article builds on the [data movement activities](data-factory-data-movement-activities.md) article, which presents a general overview of data movement with copy activity and supported data store combinations.

Data Factory service supports connecting to on-premises MySQL sources using the Data Management Gateway. See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article to learn about Data Management Gateway and step-by-step instructions on setting up the gateway. 

> [AZURE.NOTE] Gateway is required even if the MySQL database is hosted in an Azure IaaS virtual machine (VM). You can install the gateway on the same VM as the data store or on a different VM as long as the gateway can connect to the database.  

Data factory currently supports only moving data from MySQL to other data stores, but not for moving data from other data stores to MySQL.

## <a name="installation"></a>Installation 
For Data Management Gateway to connect to the MySQL Database, you need to install the [MySQL Connector/Net 6.6.5 for Microsoft Windows](http://go.microsoft.com/fwlink/?LinkId=278885) on the same system as the Data Management Gateway.

> [AZURE.NOTE] See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshoot-gateway-issues) for tips on troubleshooting connection/gateway related issues. 

## <a name="copy-data-wizard"></a>Copy data wizard
The easiest way to create a pipeline that copies data from a MySQL database to any of the supported sink data stores is to use the Copy data wizard. See [Tutorial: Create a pipeline using Copy Wizard](data-factory-copy-data-wizard-tutorial.md) for a quick walkthrough on creating a pipeline using the Copy data wizard. 

The following example provides sample JSON definitions that you can use to create a pipeline. For a complete walkthrough with step-by-step instructions, see [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article. Data can be copied to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores) using the Copy Activity in Azure Data Factory.   

## <a name="sample-copy-data-from-mysql-to-azure-blob"></a>Sample: Copy data from MySQL to Azure Blob
This sample shows how to copy data from an on-premises MySQL database to an Azure Blob Storage. However, data can be copied **directly** to any of the sinks stated [here](data-factory-data-movement-activities.md#supported-data-stores) using the Copy Activity in Azure Data Factory.  

> [AZURE.IMPORTANT] This sample provides JSON snippets. It does not include step-by-step instructions for creating the data factory. See [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article for step-by-step instructions. 
 
The sample has the following data factory entities:

1.  A linked service of type [OnPremisesMySql](data-factory-onprem-mysql-connector.md#mysql-linked-service-properties).
2.  A linked service of type [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties).
3.  An input [dataset](data-factory-create-datasets.md) of type [RelationalTable](data-factory-onprem-mysql-connector.md#mysql-dataset-type-properties).
4.  An output [dataset](data-factory-create-datasets.md) of type [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
4.  A [pipeline](data-factory-create-pipelines.md) with Copy Activity that uses [RelationalSource](data-factory-onprem-mysql-connector.md#mysql-copy-activity-type-properties) and [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

The sample copies data from a query result in MySQL database to a blob hourly. The JSON properties used in these samples are described in sections following the samples. 

As a first step, setup the data management gateway. The instructions are in the [moving data between on-premises locations and cloud](data-factory-move-data-between-onprem-and-cloud.md) article. 

**MySQL linked service**

    {
      "name": "OnPremMySqlLinkedService",
      "properties": {
        "type": "OnPremisesMySql",
        "typeProperties": {
          "server": "<server name>",
          "database": "<database name>",
          "schema": "<schema name>",
          "authenticationType": "<authentication type>",
          "userName": "<user name>",
          "password": "<password>",
          "gatewayName": "<gateway>"
        }
      }
    }

**Azure Storage linked service**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**MySQL input dataset**

The sample assumes you have created a table “MyTable” in MySQL and it contains a column called “timestampcolumn” for time series data.

Setting “external”: ”true” informs the Data Factory service that the table is external to the data factory and is not produced by an activity in the data factory.
    
    {
        "name": "MySqlDataSet",
        "properties": {
            "published": false,
            "type": "RelationalTable",
            "linkedServiceName": "OnPremMySqlLinkedService",
            "typeProperties": {},
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true,
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }



**Azure Blob output dataset**

Data is written to a new blob every hour (frequency: hour, interval: 1). The folder path for the blob is dynamically evaluated based on the start time of the slice that is being processed. The folder path uses year, month, day, and hours parts of the start time.

    {
        "name": "AzureBlobMySqlDataSet",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties": {
                "folderPath": "mycontainer/mysql/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                },
                "partitionedBy": [
                    {
                        "name": "Year",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "yyyy"
                        }
                    },
                    {
                        "name": "Month",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "MM"
                        }
                    },
                    {
                        "name": "Day",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "dd"
                        }
                    },
                    {
                        "name": "Hour",
                        "value": {
                            "type": "DateTime",
                            "date": "SliceStart",
                            "format": "HH"
                        }
                    }
                ]
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }



**Pipeline with Copy activity**

The pipeline contains a Copy Activity that is configured to use the input and output datasets and is scheduled to run every hour. In the pipeline JSON definition, the **source** type is set to **RelationalSource** and **sink** type is set to **BlobSink**. The SQL query specified for the **query** property selects the data in the past hour to copy.
    
    {
        "name": "CopyMySqlToBlob",
        "properties": {
            "description": "pipeline for copy activity",
            "activities": [
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "RelationalSource",
                            "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                        },
                        "sink": {
                            "type": "BlobSink",
                            "writeBatchSize": 0,
                            "writeBatchTimeout": "00:00:00"
                        }
                    },
                    "inputs": [
                        {
                            "name": "MySqlDataSet"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobMySqlDataSet"
                        }
                    ],
                    "policy": {
                        "timeout": "01:00:00",
                        "concurrency": 1
                    },
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "MySqlToBlob"
                }
            ],
            "start": "2014-06-01T18:00:00Z",
            "end": "2014-06-01T19:00:00Z"
        }
    }



## <a name="mysql-linked-service-properties"></a>MySQL linked service properties

The following table provides description for JSON elements specific to MySQL linked service.

| Property | Description | Required |
| -------- | ----------- | -------- | 
| type | The type property must be set to: **OnPremisesMySql** | Yes |
| server | Name of the MySQL server. | Yes |
| database | Name of the MySQL database. | Yes | 
| schema  | Name of the schema in the database. | No | 
| authenticationType | Type of authentication used to connect to the MySQL database. Possible values are: Anonymous, Basic, and Windows. | Yes | 
| username | Specify user name if you are using Basic or Windows authentication. | No | 
| password | Specify password for the user account you specified for the username. | No | 
| gatewayName | Name of the gateway that the Data Factory service should use to connect to the on-premises MySQL database. | Yes |

See [Setting Credentials and Security](data-factory-move-data-between-onprem-and-cloud.md#set-credentials-and-security) for details about setting credentials for an on-premises MySQL data source.

## <a name="mysql-dataset-type-properties"></a>MySQL dataset type properties

For a full list of sections & properties available for defining datasets, see the [Creating datasets](data-factory-create-datasets.md) article. Sections such as structure, availability, and policy of a dataset JSON are similar for all dataset types (Azure SQL, Azure blob, Azure table, etc.).

The **typeProperties** section is different for each type of dataset and provides information about the location of the data in the data store. The typeProperties section for dataset of type **RelationalTable** (which includes MySQL dataset) has the following properties

| Property | Description | Required |
| -------- | ----------- | -------- |
| tableName | Name of the table in the MySQL Database instance that linked service refers to. | No (if **query** of **RelationalSource** is specified) | 

## <a name="mysql-copy-activity-type-properties"></a>MySQL copy activity type properties

For a full list of sections & properties available for defining activities, see the [Creating Pipelines](data-factory-create-pipelines.md) article. Properties such as name, description, input and output tables, are policies are available for all types of activities. 

Properties available in the **typeProperties** section of the activity on the other hand vary with each activity type. For Copy activity, they vary depending on the types of sources and sinks.

When source in copy activity is of type **RelationalSource** (which includes MySQL) the following properties are available in typeProperties section:

| Property | Description | Allowed values | Required |
| -------- | ----------- | -------------- | -------- |
| query | Use the custom query to read data. | SQL query string. For example: select * from MyTable. | No (if **tableName** of **dataset** is specified) | 

[AZURE.INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

### <a name="type-mapping-for-mysql"></a>Type mapping for MySQL

As mentioned in the [data movement activities](data-factory-data-movement-activities.md) article, Copy activity performs automatic type conversions from source types to sink types with the following two-step approach:

1. Convert from native source types to .NET type
2. Convert from .NET type to native sink type

When moving data to MySQL, the following mappings are used from MySQL types to .NET types.

| MySQL Database type | .NET Framework type |
| ------------------- | ------------------- | 
| bigint unsigned | Decimal |
| bigint | Int64 |
| bit | Decimal |
| blob | Byte[] |
| bool |  Boolean | 
| char | String |
| date | Datetime |
| datetime | Datetime |
| decimal | Decimal |
| double precision | Double |
| double | Double |
| enum | String |
| float | Single |
| int unsigned | Int64 |
| int | Int32 |
| integer unsigned | Int64 |
| integer | Int32 | 
| long varbinary | Byte[] |
| long varchar | String |
| longblob | Byte[] |
| longtext | String | 
| mediumblob |  Byte[] | 
| mediumint unsigned | Int64 |
| mediumint | Int32 | 
| mediumtext | String |
| numeric | Decimal |
| real |  Double |
| set | String |
| smallint unsigned | Int32 |
| smallint | Int16 |
| text | String |
| time | TimeSpan |
| timestamp | Datetime |
| tinyblob | Byte[] |
| tinyint unsigned |  Int16 | 
| tinyint | Int16 |
| tinytext | String | 
| varchar | String | 
| year | Int | 


[AZURE.INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[AZURE.INCLUDE [data-factory-type-repeatability-for-relational-sources](../../includes/data-factory-type-repeatability-for-relational-sources.md)]

## <a name="performance-and-tuning"></a>Performance and Tuning  
See [Copy Activity Performance & Tuning Guide](data-factory-copy-activity-performance.md) to learn about key factors that impact performance of data movement (Copy Activity) in Azure Data Factory and various ways to optimize it.

