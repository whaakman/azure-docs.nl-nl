---
title: Overzicht van Azure Event raster
description: Beschrijving van Azure Event raster en de concepten.
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 12/14/2017
ms.author: babanisa
ms.openlocfilehash: e6665b3b0c6e92ed462f18dbd41d62ccd9304928
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="an-introduction-to-azure-event-grid"></a>Een inleiding tot Azure gebeurtenis raster

Azure Event raster kunt u eenvoudig om toepassingen te bouwen met architectuur op basis van gebeurtenissen. U selecteert de Azure resource die u wilt abonneren op en geef de gebeurtenis-handler of WebHook-eindpunt om de gebeurtenis te verzenden. Gebeurtenis raster bevat ingebouwde ondersteuning voor gebeurtenissen die afkomstig zijn van de Azure-services, zoals de storage-blobs en resourcegroepen. Gebeurtenis raster heeft ook aangepaste ondersteuning voor de toepassing en gebeurtenissen van derden, met behulp van aangepaste onderwerpen en aangepaste webhooks. 

U kunt filters gebruiken voor het doorsturen van specifieke gebeurtenissen naar verschillende eindpunten, multicast bij meerdere eindpunten en zorg ervoor dat uw gebeurtenissen op betrouwbare wijze worden bezorgd. Gebeurtenis raster ook met ingebouwde ondersteuning voor aangepaste en derden gebeurtenissen.

Op dit moment ondersteunt raster gebeurtenis de volgende gebieden:

* VS - midden
*   VS - oost
*   VS - oost 2
*   West-centraal VS
*   VS - west
*   VS - west 2

Andere regio's worden toegevoegd.

Dit artikel bevat een overzicht van Azure Event raster. Als u aan de slag met Event raster wilt, Zie [maken en route aangepaste gebeurtenissen met Azure Event raster](custom-event-quickstart.md). De volgende afbeelding toont hoe gebeurtenis raster uitgevers en handlers verbinding maakt, maar biedt geen een uitgebreide lijst met ondersteunde opties.

![Gebeurtenis raster functionele model](./media/overview/event-grid-functional-model.png)

## <a name="event-publishers"></a>Gebeurtenisuitgevers

De volgende Azure-services hebt op dit moment wordt de uitgever van de ingebouwde ondersteuning voor de gebeurtenis raster:

* Azure-abonnementen (beheerbewerkingen)
* Aangepaste-onderwerpen
* Event Hubs
* Resourcegroepen (beheerbewerkingen)
* Storage-Blob

Andere Azure-services worden van dit jaar met toegevoegd.

## <a name="event-handlers"></a>Gebeurtenis-handlers

De volgende Azure-services hebt op dit moment handler ingebouwde ondersteuning voor gebeurtenis raster: 

* Azure Automation
* Azure Functions
* Event Hubs
* Logic Apps
* Microsoft-stroom
* Webhooks.

Andere Azure-services worden van dit jaar met toegevoegd.

## <a name="concepts"></a>Concepten

Er zijn vijf concepten in Azure gebeurtenis raster waarmee u aan de slag:

* **Gebeurtenissen** -wat is er gebeurd.
* **Gebeurtenisuitgevers bronnen** - waar de gebeurtenis heeft plaatsgevonden.
* **Onderwerpen over** -het eindpunt waar de gebeurtenissen voor het verzenden van uitgevers.
* **Abonnementen voor gebeurtenissen** -eindpunt of de ingebouwde mechanisme route gebeurtenissen, soms aan meerdere handlers. Abonnementen worden ook gebruikt door handlers op intelligente wijze binnenkomende om gebeurtenissen te filteren.
* **Gebeurtenis-handlers** -de app of de service reageert op de gebeurtenis.

Zie voor meer informatie over deze concepten [concepten in Azure gebeurtenis raster](concepts.md).

## <a name="capabilities"></a>Functionaliteit

Hier volgen enkele van de belangrijkste functies van Azure Event raster:

* **Eenvoud** -punt en klik op om de gebeurtenissen van uw Azure-resource met een gebeurtenis-handler of het eindpunt.
* **Geavanceerde filters** -Filter op de gebeurtenis type of gebeurtenis publicatiepad om te controleren of gebeurtenis-handlers alleen relevante gebeurtenissen ontvangen.
* **Fan-out** -abonneren meerdere eindpunten op dezelfde gebeurtenis kopieën van de gebeurtenis verzenden naar plaatsen waar nodig.
* **Betrouwbaarheid** -gebruikmaken van 24 uur opnieuw proberen met exponentieel uitstel om te controleren gebeurtenissen worden geleverd.
* **Betalen per gebeurtenis** : betaal alleen voor het bedrag u gebeurtenis raster.
* **Hoge doorvoersnelheid** -hoog volume werkbelastingen voor gebeurtenis raster bouwen met ondersteuning voor miljoenen gebeurtenissen per seconde.
* **Ingebouwde gebeurtenissen** - slag snel gebruiksklaar met ingebouwde gebeurtenissen resource gedefinieerd.
* **Aangepaste gebeurtenissen** -gebeurtenis raster route, filter en betrouwbaar afleveren aangepaste gebeurtenissen in uw app gebruiken.

Zie voor een vergelijking van gebeurtenis raster, Event Hubs en Service Bus [Kies tussen Azure-services die berichten bezorgen](compare-messaging-services.md).

## <a name="what-can-i-do-with-event-grid"></a>Wat kan ik doen met gebeurtenis raster

Azure Event raster biedt verschillende mogelijkheden die aanzienlijk zonder server verbeteren, ops automatisering en integratie werk: 

### <a name="serverless-application-architectures"></a>Architecturen voor serverloze toepassingen

![Toepassing zonder server](./media/overview/serverless_web_app.png)

Event Grid verbindt gegevensbronnen en gebeurtenis-handlers. Gebruik Event Grid bijvoorbeeld om direct een serverloze functie te triggeren voor het uitvoeren van beeldanalyse zodra er een nieuwe foto wordt toegevoegd aan de container voor blob-opslag. 

### <a name="ops-automation"></a>OPS Automation

![Ops-automatisering](./media/overview/Ops_automation.png)

Met Event Grid kunt u sneller automatiseren en makkelijker beleid afdwingen. Zo kan Event Grid een melding sturen naar Azure Automation wanneer er een virtuele machine is gemaakt of wanneer er een SQL-database in gebruik wordt genomen. Deze gebeurtenissen kunnen worden gebruikt om automatisch te controleren of serviceconfiguraties compatibel zijn, metagegevens aan te bieden aan tools voor bewerkingen, virtuele machines te taggen of werkitems te archiveren.

### <a name="application-integration"></a>Integratie van toepassingen

![Integratie van toepassingen](./media/overview/app_integration.png)

Event Grid verbindt uw app met andere services. Bijvoorbeeld, een eigen onderwerp om uw app gebeurtenisgegevens verzenden naar Event raster en te profiteren van de betrouwbare levering, geavanceerde routering, maken en directe integratie met Azure. U kunt Event Grid ook gebruiken met Logic Apps om op elke locatie gegevens te verwerken, zonder dat u hiervoor code hoeft te schrijven. 

## <a name="how-much-does-event-grid-cost"></a>Wat kost gebeurtenis raster?

Azure gebeurtenis raster maakt gebruik van een prijsmodel voor betalen per gebeurtenis, zodat u alleen betaalt voor wat u gebruikt.

Gebeurtenis raster kost $0,60 per miljoen operations ($0,30 tijdens de preview) en de eerste 100.000 bewerking per maand zijn gratis. Bewerkingen worden gedefinieerd als gebeurtenis inkomend, geavanceerde overeen, levering poging en management aanroepen.  Meer informatie vindt u op de [pagina met prijzen](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Volgende stappen

* [Route Storage-Blob-gebeurtenissen](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)  
  Reageer op gebeurtenissen die storage blob met behulp van de gebeurtenis raster.
* [Maken en zich abonneren op aangepaste gebeurtenissen](custom-event-quickstart.md)  
  Beginnen met het verzenden van uw eigen aangepaste gebeurtenissen naar een willekeurig eindpunt met behulp van de Azure gebeurtenis raster Quick Start gaan.
* [Met Logic Apps als een gebeurtenis-Handler](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Een zelfstudie over het bouwen van een app met Logic Apps om te reageren op gebeurtenissen die door gebeurtenis raster gepusht.
* [Big data streamen naar een datawarehouse](event-grid-event-hubs-integration.md)  
  Een zelfstudie die gebruikmaakt van Azure Functions stroom gegevens uit Event Hubs met SQL Data Warehouse.
* [Gebeurtenis raster REST API-referentiemateriaal](/rest/api/eventgrid)  
  Biedt meer technische informatie over het raster Azure-gebeurtenis en een referentie voor het beheer van abonnementen, doorsturen en filteren.