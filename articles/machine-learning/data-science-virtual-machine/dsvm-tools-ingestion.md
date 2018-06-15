---
title: Extra gegevens opname in de gegevens wetenschappelijke Virtual Machine - Azure | Microsoft Docs
description: Gegevens wetenschappelijke virtuele Machine gegevens opname hulpprogramma 's
keywords: hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, hulpprogramma's voor gegevenswetenschap, gegevenswetenschap linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 7aeb0476fffb8c9e5cf2b0b5d89a2a387bd6364a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31411342"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Gegevens wetenschappelijke virtuele Machine gegevens opname hulpprogramma 's

Een van de eerste technical stappen in een gegevenswetenschap of AI-project is het identificeren van de gegevenssets die worden gebruikt en zorgen dat ze in uw omgeving analytics. De gegevens wetenschappelijke virtuele Machine (DSVM) biedt hulpprogramma's en op te zetten in de gegevens van verschillende gegevensbronnen in de opslag van een analytische gegevens lokaal op de DSVM of in een gegevensplatform in de cloud of on-premises-bibliotheken. 

Hier volgen enkele we hebt opgegeven op de DSVM verkeer hulpprogramma's. 

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma om gegevens te kopiëren van Azure storage-blobs in Azure Data Lake Store. Het kan ook gegevens tussen twee Azure Data Lake Store-accounts kopiëren.      |
| Ondersteunde DSVM versies      | Windows      |
| Doorgaans worden gebruikt      | Meerdere blobs van Azure storage te importeren in Azure Data Lake Store.      |
|  Hoe gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `adlcopy` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [AdlCopy gebruiken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| Bijbehorende hulpprogramma's op de DSVM      | AzCopy, Azure vanaf de opdrachtregel     |

## <a name="azure-command-line"></a>Azure vanaf de opdrachtregel

|    |           |
| ------------- | ------------- |
| Wat is het?   | Beheerprogramma voor Azure. Het bevat ook termen van de opdracht voor het verplaatsen van gegevens van Azure data platforms, zoals Azure storage-blobs, Azure Data Lake Storage     |
| Ondersteunde DSVM versies      | Windows, Linux     |
| Doorgaans worden gebruikt      | U importeert, exporteren van gegevens naar en van Azure storage, Azure Data Lake Store      |
|  Hoe gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `az` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [Azure CLI gebruiken](https://docs.microsoft.com/cli/azure)     |
| Bijbehorende hulpprogramma's op de DSVM      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma om gegevens te kopiëren naar en van lokale bestanden, Azure storage-blobs, bestanden en tabellen.      |
| Ondersteunde DSVM versies      | Windows      |
| Doorgaans worden gebruikt      | Kopiëren van bestanden naar blob storage, te kopiëren van BLOB's tussen accounts.      |
|  Hoe gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `azcopy` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [AzCopy in Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)      |
| Bijbehorende hulpprogramma's op de DSVM      | AdlCopy     |


## <a name="azure-cosmos-db-data-migration-tool"></a>Azure-hulpprogramma voor gegevensmigratie Cosmos-DB

|    |           |
| ------------- | ------------- |
| Wat is het?   | Hulpprogramma voor het importeren van gegevens uit diverse bronnen, met inbegrip van JSON-bestanden, CSV-bestanden, SQL, MongoDB, Azure Table storage, Amazon DynamoDB en SQL-API van Azure Cosmos DB verzamelingen naar Azure Cosmos DB.      |
| Ondersteunde DSVM versies      | Windows      |
| Doorgaans worden gebruikt      | Bestanden van een virtuele machine importeren in CosmosDB, importeren van gegevens uit Azure-tabelopslag in CosmosDB of importeren van gegevens uit een SQL Server-database in CosmosDB.     |
|  Hoe gebruik / uitvoeren?    |   De opdrachtregel te gebruiken versie, Open een opdrachtprompt, typ `dt`. De GUI-hulpprogramma gebruiken, opent u een opdrachtprompt, typ `dtui`.    |
| Koppelingen naar voorbeelden      | [Gegevens CosmosDB importeren](https://docs.microsoft.com/azure/cosmos-db/import-data)      |
| Bijbehorende hulpprogramma's op de DSVM      | AzCopy, AdlCopy      |


## <a name="bcp"></a>BCP

|    |           |
| ------------- | ------------- |
| Wat is het?   | SQL Server-hulpprogramma om gegevens tussen SQL Server en een gegevensbestand te kopiëren.      |
| Ondersteunde DSVM versies      | Windows      |
| Doorgaans worden gebruikt      | Een CSV-bestand importeren in een SQL Server-tabel, een SQL Server-tabel te exporteren naar een bestand.      |
|  Hoe gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `bcp` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [Hulpprogramma voor bulksgewijs kopiëren](https://docs.microsoft.com/sql/tools/bcp-utility)      |
| Bijbehorende hulpprogramma's op de DSVM      | SQL Server, sqlcmd      |

## <a name="blobfuse"></a>blobfuse

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor het koppelen van een Azure blob-container in het bestandssysteem van Linux.      |
| Ondersteunde DSVM versies      | Linux      |
| Doorgaans worden gebruikt      | Lezen en schrijven naar de blobs in een container      |
|  Hoe gebruik / uitvoeren?    |   Voer _blobfuse_ op een terminal.    |
| Koppelingen naar voorbeelden      | [blobfuse op GitHub](https://github.com/Azure/azure-storage-fuse)      |
| Bijbehorende hulpprogramma's op de DSVM      | Azure vanaf de opdrachtregel      |


## <a name="microsoft-data-management-gateway"></a>Microsoft Data Management Gateway

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor het verbinding maken met lokale gegevensbronnen cloudservices voor verbruik.      |
| Ondersteunde DSVM versies      | Windows      |
| Doorgaans worden gebruikt      | Een VM wordt verbonden met een on-premises gegevensbron.      |
|  Hoe gebruik / uitvoeren?    |   'Microsoft Data Management Gateway' starten vanuit het startmenu.    |
| Koppelingen naar voorbeelden      | [Gegevensbeheergateway](https://msdn.microsoft.com/library/dn879362.aspx)      |
| Bijbehorende hulpprogramma's op de DSVM      | AzCopy, AdlCopy, bcp    |
