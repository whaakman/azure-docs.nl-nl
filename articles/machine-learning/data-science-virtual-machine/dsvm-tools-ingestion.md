---
title: Data Science Virtual Machine gegevensopnamehulpprogramma's - Azure | Microsoft Docs
description: Meer informatie over de gegevensopnamehulpprogramma's en hulpprogramma's vooraf geïnstalleerd in de Data Science Virtual Machine.
keywords: hulpprogramma's voor datatechnologie, virtuele machine voor datatechnologie, hulpprogramma voor datatechnologie, linux-datatechnologie
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2017
ms.author: gokuma
ms.openlocfilehash: 824793c9897d7d728f621182a02a6092edc69e89
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55452376"
---
# <a name="data-science-virtual-machine-data-ingestion-tools"></a>Data Science Virtual Machine gegevensopnamehulpprogramma 's

Een van de eerste technische stappen in een data science of AI-project is het identificeren van de gegevenssets die worden gebruikt en deze weer in uw omgeving voor analyse. De Data Science Virtual Machine (DSVM) biedt hulpprogramma's en bibliotheken het binnenhalen van gegevens uit verschillende bronnen in een analytische gegevensopslag lokaal op de DSVM of in een gegevensplatform in de cloud of on-premises. 

Hier volgen enkele we hebt opgegeven op de DSVM verkeer hulpprogramma's. 

## <a name="adlcopy"></a>AdlCopy

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor het kopiëren van gegevens uit Azure storage-blobs in Azure Data Lake Store. Het kan ook gegevens tussen twee Azure Data Lake Store-accounts kopiëren.      |
| Ondersteunde DSVM-versies      | Windows      |
| Wordt doorgaans gebruikt      | Meerdere blobs van Azure storage te importeren in Azure Data Lake Store.      |
|  Hoe kan ik gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `adlcopy` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [AdlCopy gebruiken](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob)      |
| Meer hulpprogramma's op de DSVM      | AzCopy, Azure-opdrachtregel     |

## <a name="azure-command-line"></a>Azure-opdrachtregel

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor Azure. Het bevat ook bewerkingen van de opdracht voor het verplaatsen van gegevens uit Azure-gegevensplatforms zoals Azure storage-blobs, Azure Data Lake Storage     |
| Ondersteunde DSVM-versies      | Windows, Linux     |
| Wordt doorgaans gebruikt      | Importeren, exporteren van gegevens naar en van Azure storage, Azure Data Lake Store      |
|  Hoe kan ik gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `az` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [Azure CLI gebruiken](https://docs.microsoft.com/cli/azure)     |
| Meer hulpprogramma's op de DSVM      | AzCopy, AdlCopy      |


## <a name="azcopy"></a>AzCopy

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor het kopiëren van gegevens naar en van lokale bestanden, Azure storage-blobs, bestanden en tabellen.      |
| Ondersteunde DSVM-versies      | Windows      |
| Wordt doorgaans gebruikt      | Bestanden zijn gekopieerd naar blob-opslag, te kopiëren van BLOB's tussen accounts.      |
|  Hoe kan ik gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `azcopy` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [AzCopy in Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy)      |
| Meer hulpprogramma's op de DSVM      | AdlCopy     |


## <a name="azure-cosmos-db-data-migration-tool"></a>Azure Cosmos DB Data Migration tool

|    |           |
| ------------- | ------------- |
| Wat is het?   | Hulpprogramma voor het importeren van gegevens uit verschillende bronnen, met inbegrip van JSON-bestanden, CSV-bestanden, SQL, MongoDB, Azure Table storage, Amazon DynamoDB en Azure Cosmos DB SQL API verzamelingen in Azure Cosmos DB.      |
| Ondersteunde DSVM-versies      | Windows      |
| Wordt doorgaans gebruikt      | Bestanden van een virtuele machine importeren in CosmosDB, gegevens importeren uit Azure table storage in CosmosDB of importeren van gegevens uit een SQL Server-database in CosmosDB.     |
|  Hoe kan ik gebruik / uitvoeren?    |   Gebruik van de opdrachtregel versie, Open een opdrachtprompt, typ `dt`. Gebruik van het GUI-hulpprogramma, open een opdrachtprompt, typ `dtui`.    |
| Koppelingen naar voorbeelden      | [Gegevens importeren in CosmosDB](https://docs.microsoft.com/azure/cosmos-db/import-data)      |
| Meer hulpprogramma's op de DSVM      | AzCopy, AdlCopy      |


## <a name="bcp"></a>BCP

|    |           |
| ------------- | ------------- |
| Wat is het?   | SQL Server-hulpprogramma om gegevens tussen SQL Server en een bestand te kopiëren.      |
| Ondersteunde DSVM-versies      | Windows      |
| Wordt doorgaans gebruikt      | Een CSV-bestand importeren in een SQL Server-tabel, een SQL Server-tabel te exporteren naar een bestand.      |
|  Hoe kan ik gebruik / uitvoeren?    |   Open een opdrachtprompt, typ `bcp` om hulp te krijgen.    |
| Koppelingen naar voorbeelden      | [Hulpprogramma voor bulksgewijs kopiëren](https://docs.microsoft.com/sql/tools/bcp-utility)      |
| Meer hulpprogramma's op de DSVM      | SQL Server, sqlcmd      |

## <a name="blobfuse"></a>Blobfuse

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor het koppelen van een Azure blob-container in het Linux-bestandssysteem.      |
| Ondersteunde DSVM-versies      | Linux      |
| Wordt doorgaans gebruikt      | Lezen en schrijven naar blobs in een container      |
|  Hoe kan ik gebruik / uitvoeren?    |   Voer _blobfuse_ op een terminal.    |
| Koppelingen naar voorbeelden      | [blobfuse op GitHub](https://github.com/Azure/azure-storage-fuse)      |
| Meer hulpprogramma's op de DSVM      | Azure-opdrachtregel      |


## <a name="microsoft-data-management-gateway"></a>Microsoft Data Management Gateway

|    |           |
| ------------- | ------------- |
| Wat is het?   | Een hulpprogramma voor het verbinding maken met on-premises gegevensbronnen cloud services voor gebruik.      |
| Ondersteunde DSVM-versies      | Windows      |
| Wordt doorgaans gebruikt      | Een VM wordt verbonden met een on-premises gegevensbron.      |
|  Hoe kan ik gebruik / uitvoeren?    |   Start "Microsoft Data Management Gateway" in het startmenu.    |
| Koppelingen naar voorbeelden      | [Gegevensbeheergateway](https://msdn.microsoft.com/library/dn879362.aspx)      |
| Meer hulpprogramma's op de DSVM      | AzCopy, AdlCopy, bcp    |
