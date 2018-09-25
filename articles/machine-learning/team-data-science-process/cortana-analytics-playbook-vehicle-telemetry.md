---
title: Voorspellen voertuigstatus en stimuleren gewoonten - Azure | Microsoft Docs
description: De mogelijkheden van Cortana Intelligence gebruiken om te krijgen van realtime en voorspellende inzichten op en winkelgedrag.
services: machine-learning
documentationcenter: ''
author: deguhath
ms.author: deguhath
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
ms.openlocfilehash: 02a12e917ed36367ffac1ac2e7a1fef1c6098ea7
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985364"
---
# <a name="vehicle-telemetry-analytics-solution-playbook"></a>Vehicle Telemetry Analytics-oplossing playbook
Dit bevat menukoppelingen naar de hoofdstukken in deze playbook: 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

## <a name="overview"></a>Overzicht
Super computers buiten de testomgeving hebt verplaatst, maar staan geparkeerd in garages. Deze worden nu in uw auto's met allerlei sensoren wordt geplaatst. Dit soort sensoren geven ze de mogelijkheid om te volgen en controleren van miljoenen gebeurtenissen per seconde. 2020, zullen de meeste van deze voertuigen zijn verbonden met Internet. Toegang te krijgen tot deze grote hoeveelheid gegevens biedt meer veiligheid, betrouwbaarheid, en dus een betere aangedreven ervaring. Microsoft biedt deze droom werkelijkheid met Cortana Intelligence.

Cortana Intelligence is een volledig beheerde big data en geavanceerde analyses waarmee u gebruiken kunt voor uw gegevens omzetten in slimme acties. Cortana Intelligence Vehicle Telemetry Analytics sjabloon van de oplossing toont aan hoe autodealers, autofabrikanten en verzekeringsmaatschappijen kunnen verkrijgen van realtime en voorspellende inzichten en rijgewoonten.

De oplossing wordt geïmplementeerd als een [lambda-architectuurpatroon](https://en.wikipedia.org/wiki/Lambda_architecture), geeft het volledige potentieel van de Cortana Intelligence-platform voor real-time en batch-verwerking.

## <a name="architecture"></a>Architectuur
De architectuur Vehicle Telemetry Analytics-oplossing wordt weergegeven in dit diagram:

![Diagram voor oplossingsarchitectuur](./media/cortana-analytics-playbook-vehicle-telemetry/fig1-vehicle-telemetry-annalytics-solution-architecture.png)


Deze oplossing omvat de volgende onderdelen van de Cortana Intelligence en brengt de integratie:

* **Azure Event Hubs** neemt miljoenen voertuigtelemetriegebeurtenissen in Azure.
* **Azure Stream Analytics** biedt in realtime inzichten in voertuigstatus en bewaart die gegevens in langdurige opslag voor rijkere batchanalyse.
* **Azure Machine Learning** afwijkingen in realtime gedetecteerd en gebruikt voor batchverwerking voorspellende statistieken aanleveren.
* **Azure HDInsight** gegevens op schaal worden getransformeerd.
* **Azure Data Factory** indeling, planning, resourcebeheer en controle van de batchverwerkingspijplijn worden verwerkt.
* **Power BI** biedt deze oplossing een uitgebreid dashboard toegevoegd voor gegevens in realtime en voorspellende analyses visualisaties.

Deze oplossing toegang heeft tot twee verschillende gegevensbronnen: 

* **Gesimuleerde vehicle signalen en diagnostische gegevens**: een vehicle telematicssimulator verzendt de diagnostische gegevens en signalen die overeenkomen met de status van het voertuig en het sparen patroon op een bepaald tijdstip. 
* **Vehicle catalogus**: deze referentiegegevensset VIN getallen toegewezen aan modellen.

