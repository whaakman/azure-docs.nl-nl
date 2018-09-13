---
title: Ondersteunde combinaties van uitvoering en omgevingen voor Azure Machine Learning Data Preparations | Microsoft Docs
description: Dit document bevat een volledige lijst van ondersteunde combinaties van andere runtimes en gegevensbronnen voor Azure Machine Learning Data Preparations
services: machine-learning
author: euangMS
ms.author: euang
manager: lanceo
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.custom: ''
ms.devlang: ''
ms.topic: article
ms.date: 02/01/2018
ms.openlocfilehash: 07b9ac8389a2bf06e0356f65d816d75d49cc0cf5
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35646430"
---
# <a name="supported-matrix-for-this-release"></a>Ondersteunde matrix voor deze release 
Wanneer uw code met behulp van Azure Machine Learning-gegevensbronnen of Azure Machine Learning, Data Preparations, aan een van beide een Pandas gegevens worden geladen of Spark dataframe, de volgende combinaties van experiment compute-omgevingen en locaties worden ondersteund:

|     |Lokale bestanden  |Azure Blob Storage  |SQL Server-database ***  |
|---------|---------|---------|---------|---------|
|Lokale Python    |     Ondersteund    |Niet ondersteund         | Niet ondersteund        |         |
|Docker (Linux-VM) Python     |In de project-bestanden ondersteund *         | Niet ondersteund        |        Niet ondersteund |         |
|PySpark docker (virtuele Linux-machine)     |In de project-bestanden ondersteund *     |Ondersteund         | Ondersteund**        |         |
|Azure Data Science Virtual Machine-Python     |In de project-bestanden ondersteund *         |Niet ondersteund         |Niet ondersteund         |         |
|Azure Data Science Virtual Machine PySPark     | In de project-bestanden ondersteund *        |Niet ondersteund         |Niet ondersteund         |         |
|Azure HDInsight PySpark     | Niet ondersteund        |Ondersteund         |Ondersteund**         |         |
|Azure HDInsight-Python     | Niet ondersteund        | Niet ondersteund        | Niet ondersteund        |         |

Azure Data Lake Store is momenteel niet ondersteund voor een compute-doel.

* Wanneer lokale paden worden gebruikt, worden bestanden in het project gekopieerd naar de compute-omgeving en er vervolgens worden gelezen. Bestanden buiten het project zijn niet gekopieerd en de paden niet meer wordt opgelost in de compute-omgeving. Overweeg het gebruik van Data Source vervanging zodat uw code een lokaal bestand gebruiken kan bij lokale uitvoering. Ga vervolgens naar een Azure Storage-blob voor een andere configuratie uitvoeren. U kunt ook steekproeven ondersteuning op gegevensbronnen gebruiken voor het beheren van wordt uitgevoerd op grote hoeveelheden gegevens alleen in bepaalde configuraties uitvoeren.

** Maven JDBC SQL Server-stuurprogramma 6.2.1 wordt gebruikt. U moet ervoor zorgen dat dit pakket (of een compatibele versie) is opgenomen in het bestand spark_dependencies.yml voor de compute-omgeving.

Ondersteunt Azure SQL Database of SQL Server die dat de database kan worden bereikt vanaf de compute-omgeving. 
