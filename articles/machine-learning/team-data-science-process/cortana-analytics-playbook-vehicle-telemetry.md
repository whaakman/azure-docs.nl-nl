---
title: Vehicle health voorspellen en die zorg draagt gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om inzicht in real-time en voorspeld op vehicle status en verkeer te krijgen gewoonten.
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 09fad60b-2f48-488b-8a7e-47d1f969ec6f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: af8b3d5bf891c93c30a05c5f02d86639a466dde5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Draaiboek met oplossingen voor voertuigtelemetrieanalyse
Dit **menu** koppelingen naar de hoofdstukken in deze playbook. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Overzicht
Super computers worden buiten de testomgeving hebt verplaatst en nu in onze garage! Deze geavanceerde auto's bevatten een groot aantal sensoren, waardoor ze in staat te volgen en controleren van miljoenen gebeurtenissen per seconde. We verwachten dat door 2020, de meeste van deze auto's wordt hebt is verbonden met Internet. Stel dat u toegang te krijgen tot deze schat aan gegevens voor meer veiligheid, betrouwbaarheid en een beter aangedreven ervaring! Microsoft heeft gesteld dat dit een situatie met Cortana Intelligence aan het einde.

Microsoft Cortana Intelligence is een volledig beheerde big data en geavanceerde analyses suite waarmee u uw gegevens transformeren naar intelligent actie. We willen u kennis met de sjabloon Cortana Intelligence Vehicle telemetrie Analytics-oplossing. Deze oplossing wordt gedemonstreerd hoe auto dealerbedrijven, auto fabrikanten en ondernemingen de mogelijkheden van Cortana Intelligence gebruiken kunnen om te krijgen realtime en gewoonten voorspellende insights op de drager gezondheid en groot. 

De oplossing is geïmplementeerd als een [lambda architectuur patroon](https://en.wikipedia.org/wiki/Lambda_architecture) met de volledige potentiële van de Cortana Intelligence-platform voor realtime en batchverwerking. De oplossing: 

* biedt een simulator Vehicle telematica
* maakt gebruik van Event Hubs voor het opnemen van miljoenen gesimuleerde vehicle telemetrische gebeurtenissen in Azure 
* Stream Analytics gebruikt om te verkrijgen van realtime-inzichten op vehicle health
* de gegevens persistente in langdurige opslag voor uitgebreidere batch analytics. 
* maakt gebruik van Machine Learning voor afwijkingsdetectie in realtime en batchverwerking om voorspellende inzicht krijgen.
* maakt gebruik van HDInsight voor het transformeren van gegevens op grote schaal en Data Factory om af te handelen orchestration, planning, bronbeheer en bewaking van de pipeline batch verwerkt 
* een uitgebreide dashboard van deze oplossing biedt voor realtime-gegevens en predictive analytics visualisaties met Power BI

## <a name="architecture"></a>Architectuur
![Architectuurdiagram van de oplossing](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)
*afbeelding 1 – architectuur Vehicle telemetrie Analytics-oplossing*

Deze oplossing omvat het volgende **Cortana Intelligence-onderdelen** en gepresenteerd hun end-to-end-integratie:

* **Event Hubs** voor het opnemen van miljoenen vehicle telemetrische gebeurtenissen in Azure.
* **Stream Analytics** voor realtime-inzichten op vehicle gezondheid en dat de gegevens zich blijft voordoen in langdurige opslag voor uitgebreidere batch analyses.
* **Machine Learning** voor afwijkingsdetectie in realtime en batchverwerking om voorspellende inzicht te krijgen.
* **HDInsight** wordt gebruikt voor het transformeren van gegevens op grote schaal
* **Data Factory** orchestration, planning, bronbeheer en controle van de pijplijn batchverwerking verwerkt.
* **Power BI** biedt deze oplossing een uitgebreide dashboard voor realtime-gegevens en visualisaties predictive analytics.

Deze oplossing gebruikt twee verschillende **gegevensbronnen**: 

* **Vehicle signalen en diagnostische gegevens in de simulatie**: een vehicle telematica simulator verzendt de diagnostische gegevens en signalen die overeenkomen met de status van de drager en het aangedreven patroon op een bepaald tijdstip. 
* **Vehicle catalogus**: een verwijzingsgegevensset met een Chassisnummer toewijzing van het model.

