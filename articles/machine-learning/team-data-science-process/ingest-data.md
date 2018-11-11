---
title: Gegevens laden in Azure storage-omgevingen voor analyses | Microsoft Docs
description: Gegevens verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: a5db14b99a81c373fbc72f523798e1f3bbdf9285
ms.sourcegitcommit: 96527c150e33a1d630836e72561a5f7d529521b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51344498"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Gegevens voor analysedoeleinden in opslagomgevingen laden

Het Team Data Science Process is vereist dat gegevens worden opgenomen of geladen in tal van verschillende opslagomgevingen worden verwerkt of geanalyseerd in de meest geschikte manier in elke fase van het proces. Gegevensbestemmingen gebruikte voor de verwerking van bevatten Azure Blob Storage, SQL Azure-databases, SQL Server op virtuele machine van Azure, HDInsight (Hadoop) en Azure Machine Learning. 

De volgende artikelen wordt beschreven hoe u gegevens opnemen in verschillende doelomgevingen waar de gegevens worden opgeslagen en verwerkt.

* Naar/van [Azure Blob-opslag](move-azure-blob.md)
* Naar [SQL Server op Azure VM](move-sql-server-virtual-machine.md)
* Naar [Azure SQL-database](move-sql-azure.md)
* Naar [Hive-tabellen](move-hive-tables.md)
* Naar [SQL gepartitioneerde tabellen](parallel-load-sql-partitioned-tables.md)
* Van [On-premises SQL Server](move-sql-azure-adf.md)

Formatteer technische en zakelijke behoeften, evenals de oorspronkelijke locatie en grootte van uw gegevens, bepaalt de doelomgevingen waarin de gegevens worden opgenomen moeten voor het bereiken van de doelstellingen van uw analyse. Het is niet ongebruikelijk dat voor een scenario dat gegevens worden verplaatst tussen verschillende omgevingen voor de verschillende taken die zijn vereist om te maken van een Voorspellend model. Deze reeks taken kan bijvoorbeeld bevatten gegevensverkenning, vooraf verwerken, opruimen, down-sampling en modeltraining.
