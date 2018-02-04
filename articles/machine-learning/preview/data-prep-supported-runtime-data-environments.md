---
title: Ondersteunde combinaties van omgevingen worden uitgevoerd en de gegevens voor Azure Machine Learning gegevens voorbereidingen | Microsoft Docs
description: Dit document bevat een volledige lijst met ondersteunde combinaties van andere runtimes en gegevensbronnen voor Azure Machine Learning gegevens voorbereidingen
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: 
ms.devlang: 
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 9025982d2a350941ef82733292ebbf15ab48d7fe
ms.sourcegitcommit: eeb5daebf10564ec110a4e83874db0fb9f9f8061
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/03/2018
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

Ondersteunt Azure SQL Database of SQL Server die dat de database kan worden bereikt vanaf de compute-omgeving. 
