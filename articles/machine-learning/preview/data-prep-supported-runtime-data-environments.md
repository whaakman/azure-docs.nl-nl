---
title: Ondersteunde combinaties van omgevingen worden uitgevoerd en de gegevens voor Azure Machine Learning gegevens voorbereidingen | Microsoft Docs
description: Dit document bevat een volledige lijst met ondersteunde combinaties van andere runtimes en gegevensbronnen voor Azure Machine Learning gegevens voorbereidingen
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: 
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 413bc8a0e0347498c004b93fb37f51d86ad029f5
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="supported-matrix-for-this-release"></a>Ondersteunde matrix voor deze release 
Wanneer uw code worden gegevens geladen met behulp van Azure Machine Learning-gegevensbronnen of Azure Machine Learning gegevens voorbereidingen, ofwel een Pandas ophalen of Spark dataframe, de volgende combinaties van proef compute-omgevingen en gegevens locaties worden ondersteund:

|     |Lokale bestanden  |Azure Blob Storage  |SQL Server-database ***  |
|---------|---------|---------|---------|---------|
|Lokale Python    |     Ondersteund    |Niet ondersteund         | Niet ondersteund        |         |
|Python docker (Linux VM)     |In de project-bestanden ondersteund *         | Niet ondersteund        |        Niet ondersteund |         |
|PySpark docker (Linux VM)     |In de project-bestanden ondersteund *     |Ondersteund         | Ondersteund**        |         |
|Azure Data wetenschappelijke virtuele Machine Python     |In de project-bestanden ondersteund *         |Niet ondersteund         |Niet ondersteund         |         |
|Azure Data wetenschappelijke virtuele Machine PySPark     | In de project-bestanden ondersteund *        |Niet ondersteund         |Niet ondersteund         |         |
|Azure HDInsight PySpark     | Niet ondersteund        |Ondersteund         |Ondersteund**         |         |
|Azure HDInsight Python     | Niet ondersteund        | Niet ondersteund        | Niet ondersteund        |         |

Azure Data Lake Store is momenteel niet ondersteund voor een compute-doel.

* Als lokale paden worden gebruikt, worden bestanden in het project gekopieerd naar de compute-omgeving en er vervolgens worden gelezen. Bestanden buiten het project zijn niet gekopieerd en de paden wordt niet langer worden omgezet in de compute-omgeving. Overweeg het gebruik van Data Source vervanging zodat uw code een lokaal bestand gebruiken kunt bij lokale uitvoering. Vervolgens overschakelen naar een Azure Storage-blob voor een andere configuratie uitvoeren. U kunt ook steekproeven ondersteuning op gegevensbronnen gebruiken voor het beheren van wordt uitgevoerd op grote hoeveelheden gegevens alleen in bepaalde configuraties uitvoeren.

** Maven JDBC SQL Server-stuurprogramma 6.2.1 wordt gebruikt. U moet ervoor zorgen dat dit pakket (of een compatibele versie) is opgenomen in het bestand spark_dependencies.yml voor de compute-omgeving.

Ondersteunt Azure SQL Database, Azure SQL Data Warehouse of SQL Server die dat de database kan worden bereikt vanaf de compute-omgeving. 
