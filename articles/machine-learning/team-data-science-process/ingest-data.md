---
title: Gegevens laden in Azure storage-omgevingen voor analyses | Microsoft Docs
description: Gegevens verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: 7d7da4f6dfed03d470c5b5706aaf412c07096120
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Gegevens voor analysedoeleinden in opslagomgevingen laden
Het Team gegevens wetenschap proces vereist dat gegevens worden ingenomen of in verschillende omgevingen voor andere opslag worden verwerkt of geanalyseerd in het meest geschikt in elke fase van het proces van geladen. Gegevensbestemmingen meestal gebruikt voor verwerking bevatten Azure Blob Storage, Azure SQL-databases en SQL Server op virtuele machine in Azure HDInsight (Hadoop) en Azure Machine Learning. 

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

Dit **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u opnemen van gegevens in deze omgevingen waarin de gegevens worden opgeslagen en verwerkt als doel.

Technische en zakelijke behoeften, evenals de initiële locatie opmaken en de doel-omgevingen waarin de gegevens worden ingenomen moeten zodat de doelstellingen van uw analyse wordt bepaald door de grootte van uw gegevens. Het is niet ongewoon is voor een scenario dat gegevens worden verplaatst tussen de verschillende omgevingen voor de verschillende taken die zijn vereist om een Voorspellend model samen te stellen. Deze reeks taken kan bijvoorbeeld bevatten gegevensverkenning, vooraf verwerken, opruimen, omlaag steekproeven en training model.

