---
title: Vehicle health voorspellen en die zorg draagt gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om inzicht in real-time en voorspeld op vehicle status en verkeer te krijgen gewoonten.
services: machine-learning
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2018
ms.author: deguhath
ms.openlocfilehash: fc8dfbbfc40db33f19d2b183546ed9c6df0159fa
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34836399"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Vehicle telemetrie Analytics-oplossing playbook
Dit menu is gekoppeld aan de hoofdstukken in deze playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Overzicht
Super computers worden buiten de testomgeving hebt verplaatst en nu in garages. Deze worden nu in geavanceerde auto's met allerlei sensoren wordt geplaatst. Deze sensoren bieden ze de mogelijkheid te volgen en controleren van miljoenen gebeurtenissen per seconde. De meeste van deze voertuigen wordt door 2020 verbonden met Internet. Toegang te krijgen tot deze schat van gegevens biedt meer veiligheid, betrouwbaarheid, en dus beter groot optreden. Microsoft is dit een situatie met Cortana Intelligence aan het einde.

Cortana Intelligence is een volledig beheerde big data en geavanceerde analyses suite kunt u uw gegevens transformeren naar intelligent actie. De sjabloon Cortana Intelligence Vehicle telemetrie Analytics-oplossing wordt gedemonstreerd hoe auto dealerbedrijven, auto fabrikanten en ondernemingen kunnen verkrijgen realtime zijn en gewoonten voorspellende insights op de drager gezondheid en groot.

De oplossing is geïmplementeerd als een [lambda architectuur patroon](https://en.wikipedia.org/wiki/Lambda_architecture), waarin de volledige potentiële van de Cortana Intelligence-platform voor realtime en batchverwerking.

## <a name="architecture"></a>Architectuur
De architectuur Vehicle telemetrie Analytics-oplossing wordt weergegeven in dit diagram:

![Architectuurdiagram van de oplossing](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Deze oplossing omvat de volgende onderdelen van de Cortana Intelligence en gepresenteerd hun integratie:

* **Azure Event Hubs** miljoenen vehicle telemetrische gebeurtenissen opgenomen in Azure.
* **Azure Stream Analytics** biedt realtime-inzichten op vehicle health en dat de gegevens zich blijft voordoen in langdurige opslag voor uitgebreidere batch analyses.
* **Azure Machine Learning** afwijkingen gedetecteerd in realtime en batchverwerking wordt gebruikt om voorspellende insights.
* **Azure HDInsight** gegevens op schaal worden omgezet.
* **Azure Data Factory** verwerkt orchestration, planning, bronbeheer en bewaking van de pipeline batch verwerkt.
* **Power BI** biedt deze oplossing een uitgebreide dashboard voor realtime-gegevens en visualisaties predictive analytics.

Deze oplossing gebruikt twee verschillende gegevensbronnen: 

* **Vehicle signalen en diagnostische gegevens in de simulatie**: een vehicle telematica simulator verzendt de diagnostische gegevens en signalen die overeenkomen met de status van de drager en het aangedreven patroon op een bepaald tijdstip. 
* **Vehicle catalogus**: deze gegevensset verwijzing VIN cijfers toegewezen aan modellen.

