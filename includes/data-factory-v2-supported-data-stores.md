---
title: ta med fil
description: ta med fil
services: data-factory
author: linda33wj
ms.service: data-factory
ms.topic: include
ms.date: 03/19/2018
ms.author: jingwang
ms.custom: include file
ms.openlocfilehash: 4a47b30b30e15bbd3df4f70b2a6f63b4ab167aea
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/20/2018
---
| Kategori | Datalager | Stöds som en källa | Stöds som en mottagare | Stöds av [Azure IR](../articles/data-factory/concepts-integration-runtime.md#azure-integration-runtime) | Stöds av [lokal IR](../articles/data-factory/concepts-integration-runtime.md#self-hosted-integration-runtime) |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **Azure** |[Azure Blob Storage](../articles/data-factory/connector-azure-blob-storage.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Cosmos DB](../articles/data-factory/connector-azure-cosmos-db.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Data Lake Store](../articles/data-factory/connector-azure-data-lake-store.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Database for MySQL](../articles/data-factory/connector-azure-database-for-mysql.md) |✓ | |✓ |✓  |
| &nbsp; |[Azure Database for PostgreSQL](../articles/data-factory/connector-azure-database-for-postgresql.md) |✓ | |✓ |✓  |
| &nbsp; |[Azure File Storage](../articles/data-factory/connector-azure-file-storage.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure SQL Database](../articles/data-factory/connector-azure-sql-database.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure SQL Data Warehouse](../articles/data-factory/connector-azure-sql-data-warehouse.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Azure Search-index](../articles/data-factory/connector-azure-search.md) | |✓ |✓ |✓  |
| &nbsp; |[Azure Table Storage](../articles/data-factory/connector-azure-table-storage.md) |✓ |✓ |✓ |✓  |
| **Databas** |[Amazon Redshift](../articles/data-factory/connector-amazon-redshift.md) |✓ | |✓ |✓  |
| &nbsp; |[DB2](../articles/data-factory/connector-db2.md) |✓ | |✓ |✓  |
| &nbsp; |[Drill (Beta)](../articles/data-factory/connector-drill.md) |✓ | |✓ |✓  |
| &nbsp; |[Google BigQuery](../articles/data-factory/connector-google-bigquery.md) |✓ | |✓ |✓  |
| &nbsp; |[Greenplum (Beta)](../articles/data-factory/connector-greenplum.md) |✓ | |✓ |✓  |
| &nbsp; |[HBase](../articles/data-factory/connector-hbase.md) |✓ | |✓ |✓  |
| &nbsp; |[Hive](../articles/data-factory/connector-hive.md) |✓ | |✓ |✓  |
| &nbsp; |[Apache Impala (Beta)](../articles/data-factory/connector-impala.md) |✓ | |✓ |✓  |
| &nbsp; |[Informix](../articles/data-factory/connector-odbc.md#ibm-informix-source) |✓ | | |✓  |
| &nbsp; |[MariaDB](../articles/data-factory/connector-mariadb.md) |✓ | |✓ |✓  |
| &nbsp; |[Microsoft Access](../articles/data-factory/connector-odbc.md#microsoft-access-source) |✓ | | |✓  |
| &nbsp; |[MySQL](../articles/data-factory/connector-mysql.md) |✓ | | |✓  |
| &nbsp; |[Netezza (Beta)](../articles/data-factory/connector-netezza.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle](../articles/data-factory/connector-oracle.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Phoenix](../articles/data-factory/connector-phoenix.md) |✓ | |✓ |✓  |
| &nbsp; |[PostgreSQL](../articles/data-factory/connector-postgresql.md) |✓ | | |✓  |
| &nbsp; |[Presto (Beta)](../articles/data-factory/connector-presto.md) |✓ | |✓ |✓  |
| &nbsp; |[SAP Business Warehouse](../articles/data-factory/connector-sap-business-warehouse.md) |✓ | | |✓  |
| &nbsp; |[SAP HANA](../articles/data-factory/connector-sap-hana.md) |✓ |✓ | |✓  |
| &nbsp; |[Spark](../articles/data-factory/connector-spark.md) |✓ | |✓ |✓  |
| &nbsp; |[SQL Server](../articles/data-factory/connector-sql-server.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Sybase](../articles/data-factory/connector-sybase.md) |✓ | | |✓  |
| &nbsp; |[Teradata](../articles/data-factory/connector-teradata.md) |✓ | | |✓  |
| &nbsp; |[Vertica (Beta)](../articles/data-factory/connector-vertica.md) |✓ | |✓ |✓  |
| **NoSQL** |[Cassandra](../articles/data-factory/connector-cassandra.md) |✓ | |✓ |✓  |
| &nbsp; |[Couchbase (Beta)](../articles/data-factory/connector-couchbase.md) |✓ | |✓ |✓  |
| &nbsp; |[MongoDB](../articles/data-factory/connector-mongodb.md) |✓ | |✓ |✓  |
| **Fil** |[Amazon S3](../articles/data-factory/connector-amazon-simple-storage-service.md) |✓ | |✓ |✓  |
| &nbsp; |[Filsystem](../articles/data-factory/connector-file-system.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[FTP](../articles/data-factory/connector-ftp.md) |✓ | |✓ |✓  |
| &nbsp; |[HDFS](../articles/data-factory/connector-hdfs.md) |✓ | |✓ |✓  |
| &nbsp; |[SFTP](../articles/data-factory/connector-sftp.md) |✓ | |✓ |✓  |
| **Generiskt protokoll** |[Generisk HTTP](../articles/data-factory/connector-http.md) |✓ | |✓ |✓  |
| &nbsp; |[OData (allmän)](../articles/data-factory/connector-odata.md) |✓ | |✓ |✓  |
| &nbsp; |[ODBC (allmän)](../articles/data-factory/connector-odbc.md) |✓ |✓ | |✓  |
| **Tjänster och appar** |[Amazon Marketplace Web Service (Beta)](../articles/data-factory/connector-amazon-marketplace-web-service.md) |✓ | |✓ |✓  |
| &nbsp; |[Common Data Service för appar](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Concur (Beta)](../articles/data-factory/connector-concur.md) |✓ | |✓ |✓  |
| &nbsp; |[Dynamics 365](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Dynamics CRM](../articles/data-factory/connector-dynamics-crm-office-365.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[GE Historian](../articles/data-factory/connector-odbc.md#ge-historian-source) |✓ | | |✓  |
| &nbsp; |[HubSpot (Beta)](../articles/data-factory/connector-hubspot.md) |✓ | |✓ |✓  |
| &nbsp; |[Jira (Beta)](../articles/data-factory/connector-jira.md) |✓ | |✓ |✓  |
| &nbsp; |[Magento (Beta)](../articles/data-factory/connector-magento.md) |✓ | |✓ |✓  |
| &nbsp; |[Marketo (Beta)](../articles/data-factory/connector-marketo.md) |✓ | |✓ |✓  |
| &nbsp; |[Oracle Eloqua (Beta)](../articles/data-factory/connector-oracle-eloqua.md) |✓ | |✓ |✓  |
| &nbsp; |[Paypal (beta)](../articles/data-factory/connector-paypal.md) |✓ | |✓ |✓  |
| &nbsp; |[QuickBooks (Beta)](../articles/data-factory/connector-quickbooks.md) |✓ | |✓ |✓  |
| &nbsp; |[Salesforce](../articles/data-factory/connector-salesforce.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[Salesforce Service Cloud](../articles/data-factory/connector-salesforce.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[SAP Cloud for Customer (C4C)](../articles/data-factory/connector-sap-cloud-for-customer.md) |✓ |✓ |✓ |✓  |
| &nbsp; |[SAP ECC](../articles/data-factory/connector-sap-ecc.md) |✓ | |✓ |✓ |
| &nbsp; |[ServiceNow](../articles/data-factory/connector-servicenow.md) |✓ | |✓ |✓  |
| &nbsp; |[Shopify (Beta)](../articles/data-factory/connector-shopify.md) |✓ | |✓ |✓  |
| &nbsp; |[Square (Beta)](../articles/data-factory/connector-square.md) |✓ | |✓ |✓  |
| &nbsp; |[Webbtabell (HTML-tabell)](../articles/data-factory/connector-web-table.md) |✓ | | |✓  |
| &nbsp; |[Xero (Beta)](../articles/data-factory/connector-xero.md) |✓ | |✓ |✓  |
| &nbsp; |[Zoho (Beta)](../articles/data-factory/connector-zoho.md) |✓ | |✓ |✓  |

> [!NOTE]
> Du kan testa alla anslutningsappar som har markerats som *Beta* och ge oss feedback. Om du vill skapa ett beroende på betaanslutningsappar i din lösning kan du kontakta [Azure-supporten](https://azure.microsoft.com/support/).